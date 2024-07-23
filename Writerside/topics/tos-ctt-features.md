# About TOS4 CTT FEATURES
a document that defines a new requirements for TOS4 aiming to simplify CTT debuggability on tester, scalability of vmin-search search-algorithms and contribute to ease of use efforts

# Requirements
1. Full backward compatibility supporting CTT current bundles plists. 
2. Native support for CTT SafeZone & Ultimate Search approaches
2. Flexible fabric & Ip transition definitions 
3. Scalable for any number of IPs/Configurations
4. Reduction of extra PLIST supporting collaterals and simplify PRIME code
5. UserDefined Custom Pattern Attributes 
6. Well defined API including Set/Get APIs to allow user code to query and modify the plist Image
7. Performance wise, extracting safezones from a PLIST via PRIME & HDMT-API should be fast and cached making sure each query is not exceeding more than a few ms of time. 

# Expected Benefits
1. Native CTT bundles MOW support
2. Eliminate the need for extra collateral files handling including offline generation & release to patterns area.
3. Reduce TP Init time and the need to parse the configuration files and decorate the plists in runtime
4. Product agnostic code handling CTT Searches SafeZone & Ultimate Search approach
5. Eliminate the need for full-list duplication by using different setup & full reuse of the plist (mainly for serial content)
6. IFPM & Subroutine elimination is a key for future TP simplicity 
7. Enriched & Smaller plists will ease the debug and contribute to the EaseOfUse (WYSWYG). 
 
# Proposal
in below proposal you can find a concurrency of 3 Resources / 2 IPs in a single bundle plist. (MEMSS, CORE_SBFT & CORE_ARR)
``` 
options(width = 300 )
PList preburst_default
{
	Pat ctt_vbump_dps_trig0; [VBUMP_ON]
	Pat preamble;
	Pat ctt_vbump_dps_trig1; [VBUMP_OFF]
}

PList pre_plist_memss
{
    Pat ijtag_config;
	Pat memss_precat;
}

PList pre_plist_core_sbft
{
    Pat mclkgating;
	Pat core_SBFT_LOAD_ENABLE;
}

PList pre_plist_core_array
{
    Pat mclkgating;
	Pat CORE_MEM_TESTING_ENABLE;
}

PListSetupBlock setup_lfm
{
    IdlePattern delay_1k;
    DelayScaleFactor    [CORE_ARR] [RefRatio 800] [NewRatio 400]
    ExitPattern ijtag_reset          [MEMSS_IJTAG];
    EntryPlist pre_plist_memss       [MEMSS_IJTAG];
    EntryPlist pre_plist_core_sbft    [CORE_SBFT];
    EntryPlist pre_plist_core_array   [CORE_ARR];
}
PListSetupBlock setup_f1
{
    IdlePattern delay_1k;
    DelayScaleFactor    [CORE_ARR] [RefRatio 800] [NewRatio 1600]
    ExitPattern resotre_to_root     [MEMSS_IJTAG];
    EntryPlist pre_plist_memss       [MEMSS_IJTAG];
    EntryPlist pre_plist_core_sbft    [CORE_SBFT];
    EntryPlist pre_plist_core_array   [CORE_ARR];
}

BundlePList bundle1_ctf400 [ConcurrentSetup setup_lfm] 
{
    PreBurstPlist=preburst_default
    safezone 1:   
        Pat t0_core_p0_test0    [CORE_SBFT]             [HEAD][START_OF_BULK][SAFEZONE1]     [WaitAfter 3000];
        Pat t8_memss_p0_test8   [MEMSS_IJTAG]           [HEAD]                 [WaitAfter 9000];
        Pat t0_core_p1_test0    [CORE_SBFT]                                    [WaitAfter 2000];
        Pat t8_memss_c0_test8   [MEMSS_IJTAG]           [TAIL];
        Pat t0_core_c0_test0    [CORE_SBFT]             [TAIL];

    safezone 2:
        Pat t18_memss_p0_test18    [MEMSS_IJTAG]        [HEAD][START_OF_BULK][SAFEZONE2]                 [WaitAfter 9000] ;
        Pat t10_core_p0_test10     [CORE_SBFT]          [HEAD]                 [WaitAfter 2000];
        Pat t10_core_p1_test10     [CORE_SBFT]                                 [WaitAfter 1500];
        Pat t14_core_p0_test10     [CORE_ARR]           [HEAD]                 [WaitAfter 7500];
        Pat t10_core_c0_test10     [CORE_SBFT]          [TAIL]                                        [PreEntryPlist=REPLACE_DEFAULT][ExitEntryPlist=pat2];
        Pat t11_core_p0_test11     [CORE_SBFT]          [HEAD]                 [WaitAfter 2000];
        Pat t18_memss_c0_test18    [MEMSS_IJTAG];       [TAIL]               
        Pat t10_core_p1_test10     [CORE_SBFT]                                 [WaitAfter 1500];
        Pat t10_core_c0_test10     [CORE_SBFT];         [TAIL]
        Pat t14_core_c0_test10     [CORE_ARR] ;         [TAIL]    

    safezone 3: 
        Pat t20_core_p0_test20  [CORE_SBFT]             [HEAD][START_OF_BULK][SAFEZONE3]                 [WaitAfter 2000]; 
        Pat t20_core_p1_test20  [CORE_SBFT]                                    [WaitAfter 1500];
        Pat t20_core_c0_test20  [CORE_SBFT]             [TAIL];
        Pat t21_core_p0_test21  [CORE_SBFT]             [HEAD][START_OF_BULK][SAFEZONE4]                 [WaitAfter 4000];
        Pat t21_core_p1_test21  [CORE_SBFT]                                    [WaitAfter 3500];
        Pat t21_core_c0_test21  [CORE_SBFT]             [TAIL];
}


```



# Attributes Explination:
<table>
<tr><td width="50">TagName</td><td width="30">TagType</td><td width="200">Explination</td></tr>
<tr><td>[START_OF_BULK]</td><td>SYSTEM</td><td>a TAG to indicate a point that all resources are idle and can be used for a restart after tick (required for safezone aearch algorithm)</td></tr>
<tr><td>[SAFEZONE\d]</td><td>USER_DEFINED</td><td>a custom/user attribute to label the bulk for easy debug and ease-of-use</td></tr>
<tr><td>[HEAD]</td><td>SYSTEM</td><td>a TAG to indicate the first atomic-pattern for a specefic TID/TEST (required for Ultimate search algorithm)</td></tr>
<tr><td>[TAIL]</td><td>SYSTEM</td><td>a TAG to indicate the last atomic-pattern for a specefic TID/TEST  (required for Ultimate search algorithm)</td></tr>
<tr><td>[VBUMP_ON]</td><td>USER_DEFINED</td><td>a custom/user attribute to label the VBUMP trigger pattern - to validate configuration coherency between Test-Instance & PLIST</td></tr>
<tr><td>[VBUMP_OFF]</td><td>USER_DEFINED</td><td>a custom/user attribute to label the VBUMP trigger pattern - to validate configuration coherency between Test-Instance & PLIST</td></tr>
<tr><td>PreEntryPlist</td><td>SYSTEM</td><td>PreEntryPlist override</td></tr>
<tr><td>ExitEntryPlist</td><td>SYSTEM</td><td>ExitEntryPlist override</td></tr>
</table>

# Ultimate Search Incremental Support:
the Ultimate Search algorithm aims to set the "StartPattern" after a Vmin-Search failure to the first/closest [HEAD] of the non completed patterns. the [TAIL] attribute will help the algorithm to 
indicate a completion of pattern testing and approve the [SKIP] attribute at the next iteration (we may be able to do it by looking into the WaitAfter != null attribute, but it's not clean in my eyes.)

Example:
## Iteratoin #1:
1. failure at t10_core_p1_test10

``` 
                    Pat t18_memss_p0_test18    [MEMSS_IJTAG]        [HEAD][START_OF_BULK][SAFEZONE2]                 [WaitAfter 9000] ;
                    Pat t10_core_p0_test10     [CORE_SBFT]          [HEAD]                 [WaitAfter 2000];
                    Pat t10_core_p1_test10     [CORE_SBFT]                                 [WaitAfter 1500];
                    Pat t14_core_p0_test10     [CORE_ARR]           [HEAD]                 [WaitAfter 7500];
                    Pat t10_core_c0_test10     [CORE_SBFT]          [TAIL]                                        [PreEntryPlist=REPLACE_DEFAULT][ExitEntryPlist=pat2];
                    Pat t11_core_p0_test11     [CORE_SBFT]          [HEAD]                 [WaitAfter 2000];
                    Pat t18_memss_c0_test18    [MEMSS_IJTAG];       [TAIL]               
   FAILURE -->      Pat t10_core_p1_test10     [CORE_SBFT]                                 [WaitAfter 1500]; // <== FAIL AT FIRST TICK
                    Pat t10_core_c0_test10     [CORE_SBFT];         [TAIL]
```
## Iteration #2:
1. Set Start Pattern to t10_core_p0_test10
2. add [SKIP] to t18_memss_c0_test18 since it's completed testing
```
// plist after modification
                        Pat t18_memss_p0_test18    [MEMSS_IJTAG]        [HEAD][START_OF_BULK][SAFEZONE2]                 [WaitAfter 9000] ;
[StartPattern] -->      Pat t10_core_p0_test10     [CORE_SBFT]          [HEAD]                 [WaitAfter 2000];
                        Pat t10_core_p1_test10     [CORE_SBFT]                                 [WaitAfter 1500];
                        Pat t14_core_p0_test10     [CORE_ARR]           [HEAD]                 [WaitAfter 7500];
                        Pat t10_core_c0_test10     [CORE_SBFT]          [TAIL]                                        [PreEntryPlist=REPLACE_DEFAULT][ExitEntryPlist=pat2];
                        Pat t11_core_p0_test11     [CORE_SBFT]          [HEAD]                 [WaitAfter 2000];
                        Pat t18_memss_c0_test18    [MEMSS_IJTAG]  [SKIP][TAIL]               
                        Pat t10_core_p1_test10     [CORE_SBFT]                                 [WaitAfter 1500]; 
                        Pat t10_core_c0_test10     [CORE_SBFT];         [TAIL]
                            
```


# Opens/ToFollow-up:
1. what is the minimum size of the delay pattern? what is the resolution of the Wakeafter? 
2. Are the Domains are Forced to Be Aligned? No, they "should" be aligned by default 
3. Prime wise: what is the best way to identify the SafeZone pattern? Pattern Attribute?
Option #1 (inpolist label):
safezone 1:

Option #2 (pattern attribute):
Pat t0_core_p0_test0    [CORE_SBFT][HEAD][SAFEZONE]     [WaitAfter 3000];

4. What can be done to scale the number of IPs?
5. Skip by Tag feature, it will help skipping atomic-patterns of a single TID in a single command
      * custom attriobute: [TID] & SkipByTag? (**ASK PRIME AS WELL)**
6. how the DomainMultipler works? can we assume as example, that the same TAP content can be bundled with various CTF frequencies where the only change is the concurrentSetupBlock? 


# Rev

0p1 - establish REV0 of the document










