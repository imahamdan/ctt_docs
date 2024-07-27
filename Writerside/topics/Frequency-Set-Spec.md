# Frequency Set Spec

MODULE::RATIONAME::VALUE(GHz/MHz)

|            | Comments                                                                  |
|------------|---------------------------------------------------------------------------|
| Definition | a json to define voltage targets, frequency mapping to YBS spec and more. |
| Scope      | Per Module                                                                |
| Owner      | Module Owner                                                              |


```plantuml
@startjson
{
    "CORE": {
        "Targets": [
            "CR0",
            "CR1",
            "CR2",
            "CR3"
        ],
        "Module": "Marr",
        "PrimarySettings": {
            "ratioName": "core_ratio",
            "Unit": "GHz",
            "FreqOverride": [
                {
                    "CornerName": "F1",
                    "Source": "BinMatrix",
                    "Value": "CORE_F2_Ratio"
                },
                {
                    "CornerName": "F3",
                    "Source": "Custom",
                    "Value": "0.5"
                },
                {
                    "CornerName": "F4",
                    "Source": "Custom",
                    "Value": "0.7"
                }
            ]
        },
        "Followers": [
            {
                "ratioName": "ring_ratio",
                "Unit": "GHz",
                "FreqOverride": [
                    {
                        "CornerName": "F1",
                        "Source": "BinMatrix",
                        "Value": "RING_F1_Ratio"
                    },
                    {
                        "CornerName": "F4",
                        "Source": "Custom",
                        "Value": "0.9"
                    }
                ]
            },
            {
                "ratioName": "core_ws",
                "Unit": "GHz"
            }
        ]
    },
    "CCF": {
        "Targets": [
            "CLR",
            "CLRS"
        ],
        "Module": "Marr",
        "PrimarySettings": {
            "ratioName": "ring_ratio",
            "Unit": "GHz"
        },
        "Followers": [
            {
                "ratioName": "core_ratio",
                "Unit": "GHz",
                "DefaultFreq": {
                    "Source": "Custom",
                    "Value": "0.8"
                },
                "FreqOverride": [
                    {
                        "CornerName": "F1",
                        "Source": "BinMatrix",
                        "Value": "RING_F1_Ratio"
                    }
                ]
            },
            {
                "ratioName": "ring_ws",
                "Unit": "GHz"
            }
        ]
    }
}
@end
```

# Opens
NOte: RING will get 500Mhz at F3 following the Primary Overriden Freq