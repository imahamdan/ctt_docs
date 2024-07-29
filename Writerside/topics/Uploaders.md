# Uploaders


```plantuml
@startmindmap
 {...}
@startmindmap
* UploadPatch (MGT/RevB0.0/p17)
    * UploadPlist
        * UploadPattern
        * asada
                abc
                
    


@endmindmap
```

inside Loop:
**Voltage Object:** 
Start=0.7, End=0.73 0.01
0.7, 0.71, 0.72, 0.73

Frequency Range:
Start=400Mhz End=500Mhz Step-10Mhz

```C#
foreach Multipass (0..3)
    if (!isHealthy(CurrentRecovery)){
        continue;
    }
    foreach freq (400..400)
        do {
        while (isHealthy()) { // Recovery Loop
                foreach (0.7..0.73)
                {
                    yield new SearchSpec(Voltage,Freq,Multipass,Recovery) 
                }
            if (FAIL) --> YieldRecovery() // Move to a new recovery option/settings
            if (Pass) --> break;
            
        }
```
Recovery: 0000
MP=1 Core 0&1
MP=2 Core 2&3
Freq=400
Voltage,Freq,Recovery,Multipass
0.7,400,000,MP1

| voltage | freq | Recovery | Multipass | Result |  
|---------|------|----------|-----------|--------|
| 0.7     | 400  | 0000     | MP1       | Fail   |
| ...     | 400  | 0000     | MP1       | Fail   |
| 0.73    |      |          | MP1       | Fail   |
| 0.7     | 400  | 0011     | MP1       | FAIL   |
| 0.7     | 400  | 0011     | MP2       | FAIL   |
| 0.71    | 400  | 0011     | MP2       | PASS   |

Core0&Core1 = DEAD
Core2&Core3 = 0.71

List Of SearchPoint
{
    Voltage: 0.7
    Freq = 400
    MultiPass = 1
    RecoverySetting = NA
}
...
{
Voltage: 0.73
Freq = 400
Mask = NA
RecoverySetting = NA
}
...
{
Voltage:0.7
Freq = 410
Mask = NA
RecoverySetting = NA
}



TestProgram Upload
