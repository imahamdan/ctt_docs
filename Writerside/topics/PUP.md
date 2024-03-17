# PUP

## Motivation
A spec file that describes the structure of a bundle, it's ingrediants and extra metadata required for CTT-Service swapping the plist on the fly
based on PUP prediction as well as a contract between CTT & PUP.

## Introduction
TBD

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
  "metadata": {
    "ctf": 400,
    "tap": 100,
    "tag": "0p0",
    "alias": "b17"
  },
  "structure": [
    {
      "resourceName": "GT",
      "plistName": "mctt_0p0_b17_400_gt_plist",
      "index": 0,
      "metadata": {
        "baseNumber": "20103"
      },
      "items": [
        {
          "plistName": "scn_gt_chk_f1_ca2_dito_noram_render"
        },
        {
          "plistName": "scn_gt_chk_f1_ca2_dito_sec_render"
        },
        {
          "plistName": "scn_gt_chk_f1_ca2_dito_ramonly_render"
        },
        {
          "plistName": "scn_gt_chk_f1_ca1_dito_sec_render"
        }
      ]
    },
    {
      "resourceName": "CCF",
      "index": 1,
      "plistName": "mctt_0p0_b17_400_ccf_plist",
      "metadata": {
        "baseNumber": "31014"
      },
      "items": [
        {
          "plistName": "ring_ssa_CHKRING_F01_ks_tito_cbo_mbistgroup_list"
        }
      ]
    }
  ],
  "permutations": {
    "SS": "mctt_0p0_b17_400_ss_plist",
    "SL": "mctt_0p0_b17_400_sl_plist",
    "LS": "mctt_0p0_b17_400_ll_plist",
    "LL": "mctt_0p0_b17_400_ll_plist"
  },
}

```