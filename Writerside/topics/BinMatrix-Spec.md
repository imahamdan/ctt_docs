
# Bin Matrix Input File

|            | Comments                                                                                          |
|------------|---------------------------------------------------------------------------------------------------|
| Definition | a product spec describing all available testing points and domains, frequencies and voltage specs |
| Scope      | Global per product                                                                                |
| Owner      | YBS Team                                                                                          |

Opens:
1. number of TestInstances is not defined in BinMatrix and need to be added
2. sometime the StartVoltage is a calculation and not a absolute number --> make sure you have a way to handle it in the flow
2. Scoreboarding to use VoltageDelta instead of Ticks
3. can we add the GB as part of the VMinForwadring File??? we need to handle a file per operatino/socket

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
        "VMaxLimit": 1.05,
        "StepSize" : 0.01
      },
      "FreqSpec": {
        "Unit": "GHz",
        "TestPointsDef": [
          {
            "Name": "F1",
            "Freq": 0.4,
            "IsResetCanBeALimiter" : false, 
            "VoltageSpecOverride": {
              "LowValue": 0.65,
              "HighValue": 0.8,
              "StepSize" : 0.02
            }
          },
          {
            "Name": "F2",
            "Freq": 0.8,
            "IsResetCanBeALimiter" : false
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

#highlight "Skus" / * /"SkuName" <<h1>>
#highlight "Skus" / * / "Domains" / * / "DomainName" <<h2>>
#highlight "Skus" / 1 / "Domains" / 0 / "TestPoints" / 3 / "Name" <<h3>>
#highlight "Skus" / 1 / "Domains" / 0 / "TestPoints" / 4 / "Name" <<h3>>
#highlight "Skus" / 1 / "Domains" / 1 / "TestPoints" / 2 / "Name" <<h3>>

{
  "Skus": [
    {
      "SkuName": "Lean",
      "Domains": [
        {
          "DomainName": "CR",
          "TestPoints": [
            {
              "Name": "F1",
              "VoltageSpecOverride": {
                 "StartVoltage": 0.65,
                 "StepSize" : 0.02
                }
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
      "SkuName": "HS",
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