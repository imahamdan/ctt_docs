# Introduction and Architecture

## Ultimate Goals
1. TestProgram and Speed flow Simplicity and Dependency free design
2. Converged, Scalable and Generic Vmin-Search template with CTT native support
3. PDE Productivity enhancements, up-level tp abstraction, automations and assistive technologies

## Problem Statement
1. Complex vmin-search template/test-method implementation with dozens of features
2. high count of input parameters and configurations with internal dependency
3. Diverged NonCtt & CTT VminSearch templates implementation 
4. No Code Sharing between products in same Org or outside the org
5. Limited Unit tests and low coverage, lack of modules/services standalone testing and integration testing
6. heavy reliance on Si-results for code & module validity 
7. Manual test-program module crafting and sustain processes.
8. Lack of coding methodologies and quality assurance 

## High Level Arch
the Dependency term means that each test-instance should include all required settings and the acutal test-templates to perform the testing, 
in order to fullfill this request and based on existing building blocks in current testing echo-system, we are proposing building a test-composite/test-unit that 
will include test-instances of search to bring the Vmin to a working point before the check is being executed, as well as, will be place holders for user-defined instances 
that is required for a valid testing or any further actions later in the test-program stream.

we will use the term "**test-unit**": a group of test-instances required to perform testing of an ip or more at a specefic frequency.

## Current TP Architecture
* Current Test program built from a predefined flow of test-composites
* Each composite include other test-composites for a specific IP
* Each IP test-composite include other test-composites per frequency corner
* Each Frequency corner include test-instances performing actual testing as well as control instances to set a PATMOD, update corner and other instrumental actions.

Below you can find an illustration on the hierarchies and the structure of the current TP Flow:
```mermaid
graph LR
    T10["START"] --> T0["..."] --> T1["PRE"] --> T2["SEARCH"] --> T3["CHECK"]-->T4["POST"] --> TN["..."]--> TE["END"]

```

Going one Level Deeper:
```mermaid
graph LR
  T10["START"] --> T0["..."] --> T1["PRE"] --> T2["SEARCH"] 
  T2 --> IP-A 
  IP-A --> FA1["F1"] --> FA2[".."] --> FA3["F10"]
  FA2 --> FA2
  FA3 --> FA3
  FA1 --> FA1
  IP-B --> FB1["F1"] --> FB2[".."] --> FB3["F8"]
  FB1 --> FB1
  FB2 --> FB2
  FB3 --> FB3
  FA3 --> IP-B
  FB3 --> T3["CHECK"]
  T3 --> CIP["IP-A"]
  CIP-->CF1["F1"]
  CF1 --> CF1
  CF1 --> CF2[".."]
  CF2 -->CF2
  CF2 --> T4["POST"] --> TN["..."]--> TE["END"]
```
And in more detailed View:
## Reminder to Current TP Flow arch
![current_tp_flow.png](current_tp_flow.png)

## Future TP Flow Architecture Guidelines
We are proposing 3 major changes to the test-program architecture
1. Consolidate the SEARCH and CHECK test-composites into a single test-block
2. Flatten BinMatrix and design a SKU based system
3. Group test-instances and the required dependencies into a single testing block

## Test Program Flow

```mermaid
graph LR
  T10["START"] --> T0["..."] --> T1["PRE"] --> T2["SPEED"] --> T4["POST"] --> TN["..."]--> TE["END"]
```

**"SPEED"** is a test composite that will include a new flows of test-composites as follows:

```mermaid
graph LR
    T1["SKU_SELECTOR"] 
    PRE --> T1 --> B1["SKU #1"] --> TN["POST"]
  T1 --> B2["SKU #2"] --> TN["POST"]
  T1 --> B3["SKU #3"] --> TN["POST"] 
```

* SKU Selector - is an entity that we will develop to select the relevant SKU based on the units params and DFF data from previous sockets
* each SKU will include list of IPs & test-units or bundles incase of CTT.

### SKU Design illustration
each SKU will consist test-units based on YBS SKU spec definition. 

```mermaid
graph LR
    T1["Test-unit #1"] --> T2["Test-unit #2"] --> T3["Test-unit #3"] --> T4["..."] --> TN["Test-unit #N"]
```
Example:
```mermaid
graph LR
    T1["saq_f1_mbist_lsa_list"] --> T2["core_f1_mbist_lsa_list"] --> T3["core_f1_scan_s@_list"] --> T4["..."] --> TN["atom_f1_sbft_list"]
```

Now we would like to define what is a "test-unit" and what it includes.
will start from a definition:

**Test-unit Def** : a group of composites/test-instances grouped in a single composite achieving a full isolated/standalone testing for
a specific IP/FREQ.

**Test-unit internal Structure:**
```mermaid 
graph LR
    PRE["User Defined - PRE"] --> SRH["Search Composite"] --> MID["User Defined - Mid"] -->  CHK["Check Composite"] --> POST["User Defined - Post"] 
```
above is a super-set of a test-unit, based on TP flows and requirements several options are available and will be mostly common

| Option | PRE | SRH | MID | CHK | POST |                                      DESC                                       |
|--------|:---:|:---:|:---:|:---:|:----:|:-------------------------------------------------------------------------------:|
| #1     |     |  X  |     |  X  |      | no special handling required, search, check and set the Vmin of associated test |
| #2     |  X  |  X  |     |  X  |      |         a special PATMOD/preparations is required to perform SRH & CHK          |
| #3     |     |     |     |  X  |      |         Check is only required and no special preparations is required,         |
| #4     |  X  |     |     |  X  |      |         No Search is required but some custom preparations is required          |
| #5     |  X  |     |     |  X  |  X   |          No Search, custom preparations and post actions are required           |
| #6     |  X  |  X  |     |  X  |  X   |                custom preparations and post actions are required                |
| #7     |  X  |  X  |     |     |      |             search only corner and custom preparations is required              |
| #8     |  X  |  X  |     |     |  X   |           search only corner, both pre and post actions are required            |

* Mid is a placeholder to export search results or any custom action required between SRH & CHK - no use-case for now.

### USER-Defined PRE, MID & POST
a place-holders where users/MOs can introduce custom test-instances required to perform the actual IP/Freq testing
or for debug and analysis purposes.
examples:
1. applying a PAT-MOD instance for defeaturing
2. Corner Update
3. etc..

### Search Composite
a place holder to perform the search or the prediction - a cheaper test bringing the Vmin to a working zone to decrease full check test to tick
there are few possible combinations:

| Option | Search | Predict | Yield-Recovery | ReTest |
|--------|:------:|:-------:|:--------------:|:------:|
| #1     |   X    |         |                |        |
| #2     |        |    X    |                |        |
| #3     |   X    |         |       X        |        |
| #4     |   X    |         |       X        |   X    |



#### Predict/VminSearch with No Yield-Recovery
based on the SKU definition and the data per IP/Freq, autogen will place the relevant test-template (Search/Predict), incase of a pass the flow will progress to the next composite,
incase of a failure testing will stop and the associated Bin will be reported.

```mermaid
graph LR
    MODE["Is SRH or Predict?"]
    MODE -- SRH --> SRH["Execute Search"] --> ISPASS["Is Passed?"]
    MODE -- PREDICT --> PREDICT --> MOVE_NEXT
    ISPASS -- Yes --> MOVE_NEXT
    ISPASS -- No --> FAIL["Fail & Report Bin"]
```

#### VminSearch With Yield-Recovery and No-Retest
for low-yield products, the recovery is a must-have feature, therefore, a recovery flow must be well-defined and places incase of Search failures

in High Level
```mermaid
graph LR
    SRH_OR_PREDICT -- FAIL --> YLD-RECOVERY
    SRH_OR_PREDICT -- PASS --> MOVE_NEXT
    YLD-RECOVERY -- PASS -->  MOVE_NEXT
    YLD-RECOVERY -- FAIL --> BIN["Fail & Report Bin"]
```
in more Low Level diagram including Retest:

```mermaid
graph LR
    MODE["Is SRH or Predict?"]
    MODE -- SRH --> SRH["Execute Search"] --> ISPASS["Is Passed?"]
    MODE -- PREDICT --> PREDICT --> MOVE_NEXT
    ISPASS -- Yes --> MOVE_NEXT
    ISPASS -- No --> RECOVERY["Perform Yield Recovery"]
    RECOVERY --> ISRETEST["Retest?"]
    ISRETEST -- No --> MOVE_NEXT
    ISRETEST -- Yes --> RETEST["Re-test With Recovery"] --> ISPASSREC["Is Passed?"]
    ISPASSREC -- Yes --> MOVE_NEXT
    ISPASSREC -- No --> FAIL["Fail & Report Bin"]

```
### Check Composite
the check is the main component and will include 3 test-instances
1. VminSearch instance: locate the Lower Vmin for a set of conditions (PLIST, Voltage & Freq)
2. Recovery Instance: handles Recovery incase of failures and set a remede to continue the part testing.
3. Scoreboard Instance: handles data-logging to ituff providing insights about the limiting patterns

in High Level
```mermaid
graph LR
    VMIN_SEARCH  -- FAIL --> RECOVERY["Yield-Recovery & Retest"]
    VMIN_SEARCH -- PASS --> MOVE_NEXT
    RECOVERY -- PASS -->  MOVE_NEXT
    RECOVERY -- FAIL --> BIN["Fail & Report Bin"]
```

### Possible combinations

| Option | VminSearch | Recovery | ScoreBoard | ReTest |
|--------|:----------:|:--------:|:----------:|:------:|
| #1     |     X      |          |            |        |
| #2     |     X      |          |     X      |        |
| #3     |     X      |    X     |            |        |
| #4     |     X      |    X     |            |   X    |
| #5     |     X      |    X     |     X      |        |
| #6     |     X      |    X     |     X      |   X    |


# PROS of current design
1. standalone & dependency free testing
2. same PRE for both SRH & CHK
3. maintenance and sustain --> what ever goes to chk/srh goes for both
4. the combination of search & check into a single test-unit will prevent testing same corner and waste testtime
5. Ctt ready, to run a test-unit without any knowledge or PATMOD handling 