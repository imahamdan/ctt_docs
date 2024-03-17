# CTB Integration as part of FLASH

at this document we will define how CTB will be integrated as part of FLASH TRACE CONVERSION FLOW

# Introduction
to scale CTT and natevly creating atomic patterns for all content type, integrating CTB as part of trace convert flow & TRC2ATE is the best approach.
to accomplish this, CTB tool need to be integrated and called as part of each TRC2ATE call.

# Flow Diagram
CTB will be integrated at the pattern conversion tools and will handle all the traces regardless of the trace-generated flow (iTraceMgr,FlashScan, other..)

![high level flow.png](high level flow.png)



# CTB integration in details
```Shell
ctb-wrapper mark --cfg [CFG_FILE JSON] --stil [STIL] --metadata [METADATA JSON] --rundir [RUNDIR] --result_name [ctb_mark_result.json]
```
## Inputs
### CFG File
the configuration file will include all parameters required for CTB to be executed, as follows:
```JSON
{
 "product" : "LNL",
 "step"    : "B0",
 "version_override" : null
}
```

### STIL
full path to the stil/itpp file to be marked

### METADATA File
the metadata file will contain the pattern name dissassemble information in both bit & full value
```JSON
{
  "naming_metadata": {
    "namefield1": {
      "bit": "bit_value",
      "full": "full_value"
    },
    "namefield2": {
      "bit": "bit_value2",
      "full": "full_value2"
    }
  }
}
```
### RUNDIR
full path to a folder where CTB can create logs and other files required for the transformation

### Result Name
the name of the file that will hold the result information of the marking - default: ctb_mark_result.json
the file will be located at the supplied RUNDIR (e.g. RUNDIR/ctb_mark_result.json)

## Outputs
CTB will create a JSON file that contains full path to the marked STIL, exit-status and error message incase of any issue

Exit Codes:
0: SUCCESS
Other: FAILURE



Output Json structure:

**in-case of success run:**
```JSON
{
  "preMarkInputFile": "FULL_PATH_TO_INPUT_FILE",
  "postMarkInputFile": "FULL_PATH_TO_OUTPUT_FILE",
  "exitStatus" : 0,
  "errorMessage" : null
}
```

**in-case of failure run:**
```JSON
{
  "preMarkInputFile": "FULL_PATH_TO_INPUT_FILE",
  "postMarkInputFile": null,
  "exitStatus" : "4",
  "errorMessage" : "Failed to find a marker script for input File"
}
```

