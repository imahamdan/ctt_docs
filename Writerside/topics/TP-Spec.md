
# TP Spec

|            | Comments                                                        |
|------------|-----------------------------------------------------------------|
| Definition | a mapping between Corner to voltage target required for autogen |
| Scope      | per Module/SubModule                                            |
| Owner      | Module Owner                                                    |

1. need to support DPS & DLVR (For the same domainsCore)

```plantuml
@startjson
{
    "Product" : "LNL",
    "Step" : "B0",
    "TargetToCorners" : [
        {
            "VoltageTargetName" : "VCCGT_HC",
            "Corners" : [
                "GTRENDER",
                "SCNGTRENDER",
                "ARRGTRENDER",
                "FUNGTRENDER"
            ]
        },
        {
            "VoltageTargetName" : "VCCPCORE_HC",
            "Corners" : [
                "CR",
                "CR0",
                "CR1",
                "CR2",
                "CR3"
            ]
        }
    ]
}
@end

```