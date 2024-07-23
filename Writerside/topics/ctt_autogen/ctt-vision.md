# CTT TP AUTO-GENERATION VISION

## Introduction
Scaling TestTime reduction and integrating more and more CTT Bundles, requires a fast and smart automated
test-program generation infrastructure, it's crucial to keep the pace of the content changes, testing conditions, corner removal or addition, and more and produce a high quality and up-to-date test programs for Intel products.

in this document we will define CTT TP Vision and how in harmony with TORCH automated Test Program will be crafted for CTT result-in 
aggressive test time reduction. 

## CTT IN TP Evolution
CTT started from the CHK (speed) flow tht
 

![CttEVOL](CttEVOL)
## CTT Evolution

### Opportunistic CTT TP flow
at most of the test-programs till today, CTT is integrated in the CHK flow and CTT bundles are built from 
a content that exists in the same flow as well.
the first integration strategy that being adopted is "Opportunistic".

1. Un-Bypass all serial-instances associated with all bundles

2. Execute the bundles, passing bundles only will register and inform status 

3. iterate over all passing bundles, bypass all serial-instances associated with the bundle

#### CurrentFlow - PROS 
1. minimal changes to the test-program
2. simple (UNBYPASS, EXECUTE , BYPASS)
3. instances are kept at the same location

#### CurrentFlow - CONS
1. bundles are limited to CHK Flow since it can be bypass only instances that are executed afterwards
2. CTT Flow must be the first one in CHK flow, incase of no SEARCH flow or non-optimal one, bundles are ticked to find the voltages work points that increase the overhead and reduce savings.  
3. enforce bundles to work at instance level (not optimal)

below you can see an illustration on the flow:


![CurrentTpFlow](CurrentTpFlow)

### MidTerm Solution
Midterm solution change is driven by 2 major motivations:
1. move CTT to the end to reduce ticks and non-optimal search
2. Enable the ability to work with partial plists
3. unlimit the bundles to the CHK flow, but include content from PRE/BEGIN/VMAX and more.

The architecture is to build the bundle and it's content as a composite, bundle ingredients can be from different flows 
as well as sub-plists of a specific instance. 

the Bundle will be executed, in-case of a pass move to the next bundle, but, incase of failures, a serial instance 
flow exist in the same composite will be executed, where the serial instances represent the content in bundle.

Notes:
* Binning will happen by the serial instnaces as well as down-skew as done in the past
* Scoreobarding will be done by the bundle, where each VoltageDomain (AKA BundleFloor) will has it's 
own BASE_NUMBER.


#### PROS 
1. Extend CTT to all TP and not limited to CHK only
2. Enable a better optmizations by breaking long-time plists to smaller pieces consumed by several bundles
3. Each Bundle composite is standalone and no order-dependency between CTT & Legacy cotnent
4. CTT can be placed at the end of the flow reducing non optimal search overhead

#### CONS
1. Test-instances will be moved/duplicated to CTT Flow
2. Binning and DownSkew still done in legacy way using serial instances

below you can see an illustration on the midterm solution:

![Midterm](Midterm)

### LongTerm Solution
TODO: ADD comments
![LongTerm](LongTerm)

### Bundle Composite
TODO: ADD comments

![BundleComposite](BundleComposite)
## CTT TP Auto-Generation

### Stage 0 - CTB AutoGen - Bypass/Unbypass arch
TODO - add comments

### Stage 1 - CTB Autogen - Midterm arch:

### Stage 2 - Based on an existing TP & TORCH APIs:
#### Inputs
1. Plists
2. Test Program
3. Bundles 
4. Testing Collaterals (Timing, Levels,...)

#### Outputs
1. CTT Module MTPL
2. Bypass Instances Spec
3. Base Numbers Spec



```C#
    protected CTTconfig DeserializeCCRconfig()
        {
            var fullPath = Services.FileService.GetFile(this.CTTconfigFile);

            // var configFileContent = string.Join("\n", Services.FileService.ReadAllLines(fullPath));
            var configFileContent = System.IO.File.ReadAllText(fullPath);
            List<CTTconfig> config = JsonConvert.DeserializeObject<List<CTTconfig>>(configFileContent);
            return config[0];
        }
```
```JSON
[
  {
    "Bundle" : "B0",
    "module" : "MAarr",
    "major" : "RevB0.0",
    "minor" : "p17",
    "plistFile": "class_atom_all.plist",
    "plistName" : "fusa.plist"
  },
  {
    "Bundle" : "B17",
    "module" : "Mscn",
    "major" : "RevB1.0",
    "minor" : "p13",
    "plistFile": "class_scn_all.plist",
    "plistName" : "ca2all.plist"
  }
]
```

```#C#
public PlistCompareResults Compare(FileInfo reference, FileInfo current, List<string> plistsToCompare)

class PlistCompareResults()
{
    public bool Result {get; set;}
    public List<PlistCompareResult> results {get; set;}
}

class PlistCompareResult()
{
    public bool Result {get; set;}
    public string BundleName {get; set;}
    public string plistName {get; set;}
}

```
#### Stage2 - Pros:

#### Stage2 - Cons:
1. MTPL generation done out-o-torch
2. Manual integration by PDEs
3. Bundles OutOfSync hazard
4. Manual qualification process

#### Stage3:
TBD

#### Stage4:
TBD

TODO: ADD a table showing how all CONS are cleared by moving from stage to the another

## TORCH APIs


## Opens
* exclude MTT non-relevant instances incase of a known down-skew (midterm solution)
* 









