
# Search Init Spec
|            | Comments                                                                          |
|------------|-----------------------------------------------------------------------------------|
| Definition | a configuration file of test-modes and a definition of the search mode per socket |
| Scope      | Global per product                                                                |
| Owner      | DIG_BASE Module Owner                                                             |

```plantuml
@startjson
{
    "EngIdsDef": [
        {
            "EngIds": [
                "QE",
                "QC",
                "QP",
                "QT"
            ],
            "SearchConfOverride": {
                "IsVminFwRequired": false,
                "IsLimitCheckValidation": false,
                "IsUseLimitCheckAsSrc": false
            }
        },
        {
            "EngIds": [
                "QQ"
            ],
            "SearchConfOverride": {
                "IsVminFwRequired": false,
                "IsLimitCheckValidation": false,
                "IsUseLimitCheckAsSrc": false,
                "IsRecoveryAllowed": true
            }
        },
        {
            "EngIds": [
                "MV"
            ],
            "SearchConfOverride": {
                "IsVminFwRequired": true,
                "IsLimitCheckValidation": false,
                "IsUseLimitCheckAsSrc": false
            }
        }
    ],
    "Flows": [
        {
            "SeachModeName": "SPEED",
            "SearchContexts": [
                {
                    "Process": "CLASSHOT",
                    "OpType": "PBIC_DAB",
                    "LocnCode": "*",
                    "SearchConf": {
                        "OperationMode": "SEARCH",
                        "IsVminFwRequired": true,
                        "IsLimitCheckValidation": false,
                        "IsUseLimitCheckAsSrc": true,
                        "IsRecoveryAllowed": true,
                        "IsGracefulChk": true
                    }
                },
                {
                    "Process": "CLASSCOLD",
                    "OpType": "PBIC_DAB",
                    "LocnCode": "*",
                    "SearchConf": {
                        "OperationMode": "SEARCH",
                        "IsVminFwRequired": true,
                        "IsLimitCheckValidation": false,
                        "IsUseLimitCheckAsSrc": true,
                        "IsRecoveryAllowed": true,
                        "IsGracefulChk": true
                    }
                },
                {
                    "Process": "PHM",
                    "OpType": "PBIC_DAB",
                    "LocnCode": "*",
                    "SearchConf": {
                        "OperationMode": "SEARCH/SINGLEPT",
                        "IsVminFwRequired": true,
                        "IsLimitCheckValidation": false,
                        "IsUseLimitCheckAsSrc": true,
                        "IsRecoveryAllowed": true,
                        "IsGracefulChk": true,
                        "vminGB" : 0.03
                    }
                }
            ]
        },
        {
            "SeachModeName": "DREC"
        }
    ]
}
@endjson
```


```plantuml
@startjson
{

    "ConfigSets" : [
        {
            "Name" : "SEARCH",
            "vminFwSpec" : {
                      "IsVminFwRequired": true,
                      "IsLimitCheckValidation": false,
                      "IsUseLimitCheckAsSrc": true,
                      "StoreVoltage" : true
                    },
        },
        {
            "Name" : "SEARCH_NO_FW",
            "vminFwSpec" : {
                      "IsVminFwRequired": false,
                      "IsLimitCheckValidation": false,
                      "IsUseLimitCheckAsSrc": false,
                      "StoreVoltage" : false
                    },
        },
        {
        "Name" : "SINGLE_PT",
            "vminFwSpec" : {
                      "IsVminFwRequired": false,
                      "IsLimitCheckValidation": true,
                      "IsUseLimitCheckAsSrc": true,
                      "StoreVoltage" : true
                    },
        },
        {
        "Name" : "SEARCH_AND_KILL",
            "vminFwSpec" : {
                      "IsVminFwRequired": true,
                      "IsLimitCheckValidation": true,
                      "IsUseLimitCheckAsSrc": false,
                      "StoreVoltage" : true
                    },
        }        
    ],
    
    "EngIdsDef": [
        {
            "EngIds": [
                "QE",
                "QC",
                "QP",
                "QT"
            ],
            "SearchConfOverride": {
                   "VminFwConfigSetName": "SEARCH_NO_FW",
            }
        },
        {
            "EngIds": [
                "QQ"
            ],
            "SearchConfOverride": {
                "VminFwConfigSetName": "SEARCH_NO_FW",
                "IsRecoveryAllowed": true
            }
        },
        {
            "EngIds": [
                "MV"
            ],
            "SearchConfOverride": {
              "VminFwConfigSetName": "SEARCH",
            }
        }
    ],
    "Flows": [
        {
            "SeachModeName": "SPEED",
            "SearchContexts": [
                {
                    "Process": "CLASSHOT",
                    "OpType": "PBIC_DAB",
                    "LocnCode": "*",
                    "SearchConf": {
                        "VminFwConfigSetName": "SEARCH",
                        "IsRecoveryAllowed": true,
                        "IsGracefulChk": true
                    }
                },
                {
                    "Process": "CLASSCOLD",
                    "OpType": "PBIC_DAB",
                    "LocnCode": "*",
                    "SearchConf": {
                        "VminFwConfigSetName": "SEARCH_AND_KILL",
                        "IsRecoveryAllowed": true,
                        "IsGracefulChk": true
                    }
                },
                {
                    "Process": "PHM",
                    "OpType": "PBIC_DAB",
                    "LocnCode": "*",
                    "SearchConf": {
                        "VminFwConfigSetName": "SINGLE_PT",
                        "IsRecoveryAllowed": true,
                        "IsGracefulChk": true,
                        "vminGB" : 0.03
                    }
                }
            ]
        },
        {
            "SeachModeName": "DREC"
        }
    ]
}
@endjson
```

