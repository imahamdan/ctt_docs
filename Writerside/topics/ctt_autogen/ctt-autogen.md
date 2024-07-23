# CTT TestProgram Module Auto Generatinon

## Introduction 
for CTT Scale and introducing more and more bundles, fast and seemless an automatic test-program
generation is developed and below you can find details how to invoke it and create CTT Module in matter of minutes

## High Level Flow Diagram

## Setup
CTT Module Generation is part of CTB2 tool.

**CTB2 Setup:**
```
TODO
```

# Inputs
## High Level Bundles CSV
a CSV with a predefined header populated with relevant information required for the CTT TP module generation
Header must have all below columns:

<table>
<tr><td>Column Name</td><td width="500">Description</td><td>Example</td></tr>
<tr><td>INSTANCE</td><td>Full name for the instance name as it appears in TP</td><td>SSA_X_CLRS_VMIN_CHKRINGF01_CBO</td></tr>
<tr><td>MODULE</td><td>Name of the Module as defined in TP hosting the Plist/Instnace</td><td>ARR_CCF</td></tr>
<tr><td>PLIST</td><td>Name of plist as defined in Module Plists</td><td>ring_ssa_CHKRING_F01_ks_tito_cbo_mbistgroup_list</td></tr>
<tr><td>MODULE_READY</td><td>a flag that indicates that the bundle/line is ready for module-auto generation, 1 or 0 are accepted</td><td>1</td></tr>
<tr><td>BUNDLE_NAME</td><td>the name of the bundle</td><td>b1</td></tr>
<tr><td>CTF</td><td>the CTF Frequency required for this bundles (affects timing), 100, 200 or 400 are permitted only</td><td>400</td></tr>
<tr><td>TAP</td><td>the tap frequency required for the bundle (affects timing), 80, 100 values are permitted</td><td>100</td></tr>
</table>


## Test Program Analyzer Dumps
We've introduced a tool called "TestProgramAnalyzer" that gets a TP folder and creates a set of metadata
files extracting TP information required for the autogen to function
LNL TP Analyzer Dump folder: 
```
/nfs/iil/disks/mfg_lnl_019/ctb_patterns_inventory/test_programs//
```


### Cmdline
```
ctb ctt_module_gen \
-hl_def /nfs/iil/proj/cdft/func_coe/users/ahamdan/sandbox/repo_lnl_m/lnl_m/tp_autogen/tp_autogen.csv \
-tps /nfs/iil/proj/cdft/func_coe/users/ahamdan/sandbox/b0_load/tp_analyzer_b0/tp_LNLM4H6B0H10E0BS411 \
-o /nfs/iil/proj/cdft/func_coe/users/ahamdan/sandbox/b0_load/module_gen/runs\
-tag 0p0\
-levels IP_CPU_BASE::SBF_SCAN_nom_lvl
```

### FLAGS
<table>
<tr><td width="100">Flag</td><td width="10" sorted="desc">Required</td><td width="500">Description</td></tr>
<tr><td>-hl_dev</td><td>true</td><td>full path to a High Level Definition of bundles, a well-defined CSV file defines the bundles group, see Inputs section for more details</td></tr>
<tr><td>-tps</td><td>true</td><td>full path to TP-Analzer Dump Area , see Inputs section for more details</td></tr>
<tr><td>-o</td><td>true</td><td>a path to output folder required for the tool</td></tr>
<tr><td>-tag</td><td>true</td><td>a string that will be added for bundle names and instances indicating Bundles Release drop</td></tr>
<tr><td>-levels</td><td>true</td><td>a string for a valid LEVELS file to override and set for all bundles</td></tr>
<tr><td>-timing</td><td>true</td><td>a string for a valid TIMINGS block, by default the tool generates a timing called IP_CPU_BASE::ctt_timing_ctf[CTF_FREQ]_tclk[TCLK_FREQ], example: IP_CPU_BASE::ctt_timing_ctf400_tclk100</td></tr>
<tr><td>--include</td><td>false</td><td>a flag that defines specific bundle names to include in module auto-gen, it gets commad seperated value, example: -include b1,b7,b9</td></tr>
<tr><td>--exclude</td><td>false</td><td>a flag that defines specific bundle names to exclude from processing. example: -exclude b11,b18 </td></tr>
<tr><td>--result_name</td><td>false</td><td>the name of the auto generated MTPL file, default: CTT_ALL.mtpl</td></tr>
<tr><td>-edc</td><td>false</td><td>set @EDC flag for all generated bundles</td></tr>
<tr><td>--gen_ing</td><td>false</td><td>instructs the tool to clone the originated test instances and add them as fail flow for the bundle</td></tr>
</table>

### Results
the module generator module in CTB generates several files and collaterals  
