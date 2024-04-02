# PUP

## Motivation
Test cost is one of the most important metrics for a successful product production ramp-up & profitable one, today, there are different and various
methods reducing test cost. at thie spec, we will define the contract between 2 solutions, CTT & PUP.

**CTT** is static using smart scheduling algorithms producing one solution fits all material

**PUP** is dynamic using ML and predict different content per unit

in order to work in harmony and get the best out of the 2 solutions, there are  constraints that need to enforced
and bi-directional data to be communicated.

## Introduction


## Flow & Assumptions
the POC will be executed at LNL product where
1. CTT is integrated before PRQ milestone
2. SCRDB is done by the bundle instance 
3. BaseNumber(BN) is associated with a bundle floor/resource
4. each bundle floor will include a single Voltage target and Content type
5. floor definition changes force allocation of a new BaseNumber
6. once pup is introduced, CTT can move floors from one bundle to another
7. CTT can stack several floors from different bundles into a single bundle


## Base Number Allocation
* adding or removing content plist will be transparent and base number change is not expected.
* any change to a floor ingredients will result-in re-allocating a new BN

# Release Process and Location
the ctt pup spec files will be released among other CTT files to the content release area.

**Location:**
/intel/

### Prediction Key

each resource in a bundle can get one of the possible predecitions:
None: Not targeted by PUP --> will be marked by "L"
Monitor: a mode used for PUP for prediction validation --> will be marked by "L" 
Short: a mode that indicate a good health and the short list can be used --> will be makred by "S" 


## Spec File Main Elements
1. Bundle ID: bundle Name and metadata required for humans and tools.
2. Bundle Structure: includes the resources and the bundles lists.
3. Bundle Permutations: a mapping from PredictionKey

### BundleName
BundleName, associated plist and hosting instance share the same naming structure.

MCTT _ [TAG] _ [BUNDLEALIAS] _ [CTFFREQ]

example: mctt_0p0_b17_400


## Example of a Spec File

```JSON
{
  "name": "mctt_0p0_b17_400",
  "bundlePlistName": "mctt_0p0_b17_400_plist",
  "metadata": {
    "ctf": "400",
    "tap": "100",
    "tag": "0p0",
    "alias": "b17"
  },
  "structure": [
    {
      "resourceName": "GT",
      "plistName": "mctt_0p0_b17_400_gt_plist",
      "metadata": {
        "misc": {
          "someKey": "someValue"
        }
      },
      "items": [
        {
          "plistName": "scn_gt_chk_f1_ca2_dito_noram_render",
          "plistFile" : "AHMAD TODO",
          "serialTTInMs": "200"
        },
        {
          "plistName": "scn_gt_chk_f1_ca2_dito_sec_render",
          "serialTTInMs": "450"
        },
        {
          "plistName": "scn_gt_chk_f1_ca2_dito_ramonly_render",
          "serialTTInMs": "800"
        },
        {
          "plistName": "scn_gt_chk_f1_ca1_dito_sec_render",
          "serialTTInMs": "923"
        }
      ]
    },
    {
      "resourceName": "CCF",
      "plistName": "mctt_0p0_b17_400_ccf_plist",
      "metadata": {
        "misc": null
      },
      "items": [
        {
          "plistName": "ring_ssa_CHKRING_F01_ks_tito_cbo_mbistgroup_list",
          "serialTTInMs": "923"
        }
      ]
    }
  ],
  "permutations": [
    {
      "key": "SS",
      "permuPlistName": "mctt_0p0_b17_400_ss_plist"
    },
    {
      "key": "SL",
      "permuPlistName": "mctt_0p0_b17_400_sl_plist"
    },
    {
      "key": "LS",
      "permuPlistName": "mctt_0p0_b17_400_ls_plist"
    },
    {
      "key": "LL",
      "permuPlistName": "mctt_0p0_b17_400_ll_plist"
    }
  ]
}

```


# Opens
* incase of "stacking" floors of same resource in a new bundle --> will require a definition and new base number
* log to ituff the selected permutation per bundle - @Slava
* 