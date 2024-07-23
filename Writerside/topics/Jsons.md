# Jsons

# Bin Matrix Input File

|            | Comments                                                                                          |
|------------|---------------------------------------------------------------------------------------------------|
| Definition | a product spec describing all available testing points and domains, frequencies and voltage specs |
| Scope      | Global per product                                                                                |
| Owner      | YBS Team                                                                                          |

```plantuml
@startjson
<style>
  .h1 {
    BackGroundColor green
    FontColor white
    FontStyle italic
  }
  .h2 {
    BackGroundColor red
    FontColor white
    FontStyle bold
  }
</style>

#highlight "Domains" / * /"Name" <<h1>>

{
  "Domains": [
    {
      "Name": "CR",
      "Description": "CORE Main Domain",
      "instancesCount": 4,
      "VoltageSpec": {
        "LowValue": 0.45,
        "HighValue": 1.0,
        "VMaxLimit": 1.05
      },
      "FreqSpec": {
        "Unit": "GHz",
        "TestPointsDef": [
          {
            "Name": "F1",
            "Freq": 0.4,
            "VoltageSpecOverride": {
              "LowValue": 0.45,
              "HighValue": 1.0
            }
          },
          {
            "Name": "F2",
            "Freq": 0.8
          },
          {
            "Name": "F3",
            "Freq": 1.2
          },
          {
            "Name": "F4",
            "Freq": 1.6
          },
          {
            "Name": "F5",
            "Freq": 2.0
          }
        ]
      }
    },
    {
      "Name": "GT",
      "Description": "GT Main Domain",
      "instancesCount": 1,
      "VoltageSpec": {
        "LowValue": 0.5,
        "HighValue": 0.60,
        "VMaxLimit": 1.05
      },
      "FreqSpec": {
        "Unit": "GHz",
        "TestPointsDef": [
          {
            "Name": "F1",
            "Freq": 0.4
          },
          {
            "Name": "F2",
            "Freq": 0.8
          },
          {
            "Name": "F3",
            "Freq": 1.2
          },
          {
            "Name": "F4",
            "Freq": 2.0
          }
        ]
      }
    },
    {
      "Name": "SAQ",
      "Description": "SA QCLK Main Domain",
      "instancesCount": 1,
      "VoltageSpec": {
        "LowValue": 0.5,
        "HighValue": 0.60,
        "VMaxLimit": 1.05
      },
      "FreqSpec": {
        "Unit": "GHz",
        "TestPointsDef": [
          {
            "Name": "F1",
            "Freq": 0.4
          },
          {
            "Name": "F2",
            "Freq": 0.8
          },
          {
            "Name": "F3",
            "Freq": 1.2
          }
        ]
      }
    }
  ]
}
@endjson
```

# Sku Definition

|            | Comments                                                                       |
|------------|--------------------------------------------------------------------------------|
| Definition | a testing definition spec describing the required frequencies/flows per domain |
| Scope      | Global per product                                                             |
| Owner      | YBS Team                                                                       |
```plantuml
@startjson
<style>
  .h1 {
    BackGroundColor green
    FontColor white
    FontStyle italic
  }
  .h2 {
    BackGroundColor yellow
    FontColor black
    FontStyle bold
  }
  .h3 {
    BackGroundColor orange
    FontColor black
    FontStyle bold
  }
</style>

#highlight "Skews" / * /"SkewName" <<h1>>
#highlight "Skews" / * / "Domains" / * / "DomainName" <<h2>>
#highlight "Skews" / 1 / "Domains" / 0 / "TestPoints" / 3 / "Name" <<h3>>
#highlight "Skews" / 1 / "Domains" / 0 / "TestPoints" / 4 / "Name" <<h3>>
#highlight "Skews" / 1 / "Domains" / 1 / "TestPoints" / 2 / "Name" <<h3>>

{
  "Skews": [
    {
      "SkewName": "Lean",
      "Domains": [
        {
          "DomainName": "CR",
          "TestPoints": [
            {
              "Name": "F1",
              "StartVoltage": "0.47"
            },
            {
              "Name": "F3"
            },
            {
              "Name": "F5"
            }
          ]
        },
        {
          "DomainName": "SA",
          "TestPoints": [
            {
              "Name": "F1"
            },
            {
              "Name": "F3"
            }
          ]
        }
      ]
    },
    {
      "SkewName": "HS",
      "Domains": [
        {
          "DomainName": "CR",
          "TestPoints": [
            {
              "Name": "F1"
            },
            {
              "Name": "F3"
            },
            {
              "Name": "F5"
            },
             {
              "Name": "F8"
            },
             {
              "Name": "F10"
            }
          ]
        },
        {
          "DomainName": "SA",
          "TestPoints": [
            {
              "Name": "F1"
            },
            {
              "Name": "F3"
            },
            {
              "Name": "F5"
            }
          ]
        }
      ]
    }
  ]
}
@endjson
```
# Generation Spec

|            | Comments                                                           |
|------------|--------------------------------------------------------------------|
| Definition | a crafting spec describing and mapping plists to frequency corner. |
| Scope      | per Module/SubModule                                               |
| Owner      | Module Owner                                                       |


```plantuml

@startjson
{
  "ModuleName": "SAQ",
  "Description": "this is the crafting spec for LNL SAQ Module",
  "TestingSpec": {
    "Timing": "saq_rom_chk_F1_mbist_list",
    "Levels": "IP_CPU_BASE::SBF_nom_lvl",
    "FreqTestingSpec": [
      {
        "FreqName": "F1",
        "SetupFlowItems": [ "IP_CPU::FLOW1_PRE" ],
        "FinalizeFlowItems": [ "IP_CPU::FLOW1_FINALIZE" ],
        "PlistsSpec": [
          {
            "Name": "saq_lsa_chk_F1_mbist_list"
          },
          {
            "Name": "saq_ssa_chk_F1_mbist_list",
            "TimingOverride": "saq_rom_chk_F1_mbist_list",
            "LevelsOverride": "IP_CPU_BASE::SBF_nom_lvl"
          },
          {
            "Name": "saq_rom_chk_F1_mbist_list"
          },
          {
            "Name": "saq_ssa_chk_F1_mbist_sbclk_list"
          },
          {
            "Name": "saq_lsa_chk_F1_mbist_sbclk_list"
          },
          {
            "Name": "saq_xsa_chk_F1_mbist_ddr_list"
          },
          {
            "Name": "san_lmax_ks_ddr_list"
          }
        ]
      },
      {
        "Freq": "F2",
        "SetupFlowItems": [ "IP_CPU::FLOW2_PRE" ],
        "FinalizeFlowItems": [ "IP_CPU::FLOW2_FINALIZE" ],
        "PlistsSpec": [
          {
            "Name": "saq_lsa_chk_F2_mbist_list"
          },
          {
            "Name": "saq_ssa_chk_F2_mbist_list"
          }
        ]
      }
    ]
  }
}
@endjson
```
