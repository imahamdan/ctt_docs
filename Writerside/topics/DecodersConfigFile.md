
# Decoders Config


|            | Comments                                                                                                  |
|------------|-----------------------------------------------------------------------------------------------------------|
| Definition | a json to define which parameters and configuration files are required to initialize the Decoder skeleton |
| Scope      | Global Per Product                                                                                        |
| Owner      | TBD                                                                                                       |

```plantuml
@startjson
{
    "LOG1_DECODER" : {
        "TemplateName" : "Log1DecoderInitializer",
        "cfgFile" : "./Modules/CTT_INFRA/....json",
        "Mode" : "MisrOff"
     },
     "LOG1_DECODER_MISREN" : {
        "TemplateName" : "Log1DecoderInitializer",
        "cfgFile" : "./Modules/CTT_INFRA/....json",
        "Mode" : "MisrOn"
     },
    "LOG1_DECODER_2026" : {
        "TemplateName" : "EnhacnedLog1AndCtvTemplate",
        "cfgFile" : "./Modules/CTT_INFRA/Log1Ahmad...json",
        "Mode" : "MisrOn"
    }
}
@endjson
```