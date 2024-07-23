# Design

# Modules
1. Voltage Module
2. Frequencuy Module
3. SCRBD?
4. Func BASE
5. PATMOD Module
6. CTT Module
7. Misc Module
8. DTS Module
9. Recovery Module


# VminSearch Test Template Methods
```mermaid
sequenceDiagram

    Note right of TestMethod: Verify @ **INIT** 
    TestMethod -->> TestMethod: +Parse input Params()
    TestMethod -->> TestMethod: +read and parse all relevant config files()
    TestMethod -->> TestMethod: +Build SearchSpec Per TargetGroup()
    TestMethod -->> VoltageService: +init() 
    VoltageService -> TestMethod: Done.
    TestMethod -->> FrequencyService: init() 
    TestMethod -->> CTTService: init() 
    TestMethod -->> PatModService: init() 
    TestMethod -->> MiscService: init() 
    TestMethod -->> RecoveryService: init() 
    
    Note right of TestMethod: Pre-Execute    
    Note right of VoltageService: StepSize,...    
    TestMethod -->> VoltageService: Initialize Voltage Object per sku()
    TestMethod -->> FrequencyService: Initialize Frequency per corner per spec()
    TestMethod -->> DecodersService: Initialize Decoders()
    TestMethod -->> CttService: setDefaultPlistOptionsAndStartPattern()
    TestMethod -->> RecoveryService: Apply any upstream recovery()
    
    Note right of TestMethod: Pre-Point
    TestMethod -->> VoltageService: resolveVotlagesAndApply()

    Note right of TestMethod: Post-Point
    TestMethod -->> DecodersService: GetExecutionStatus()
    DecodersService -->> TestMethod: Pass/Fail()

    Note right of TestMethod: relevant for both CTT Bundles and Serial Lists with ATomic patterns 
    TestMethod -->> CttService: HandleStartPatternTargetTicksAndPlistAndUpdateStepSize()

    Note right of TestMethod: Post-Execute
    
    TestMethod -->> TestMethod: POINT_EXECUTE()
    TestMethod -->> CVminSearch: ApplyPreSearchSetup()
    TestMethod -->> CVminSearch: GetStartVoltageValues()
    TestMethod -->> CVminSearch: GetLowerStartVoltageValues()
    TestMethod -->> CVminSearch: GetEndVoltageLimitValues()
    TestMethod -->> TestMethod: Execute()
    TestMethod -->> CVminSearch: ProcessPlistResults()
    TestMethod -->> CVminSearch: HasToContinueToNextSearch()
    
    Note right of TestMethod: Search Completed
    TestMethod -->> CVminSearch: PostProcessSearchResults()
    TestMethod -->> CVminSearch: ExecuteScoreboard()
    TestMethod -->> CVminSearch: HasToRepeatSearch()
    Note right of CVminSearch: iterate over all handlers and Init each
    HandlersMgr -->> CVminSearch: Done.
    CVminSearch -->> TestMethod: Done.
    
```

```plantuml
@startuml
left to right direction

' Split into 4 pages
page 2x2
skinparam pageMargin 10
skinparam pageExternalColor gray
skinparam pageBorderColor black

namespace cgen.interfaces #DDDDDD {

    interface ISearchEngine {
        Search() : void
    }
    
    interface ISearchExecuter {
        Execute() : void
    }
    
    interface IPrePoint {
         ExecutePrePoint() : ExecResult
    }
    interface IPostPoint {
        ExecutePostPoint() : ExecResult
    }
    
    interface IHandler {
        Handle() : 
    }


}
namespace cgen.enums #DDFFFF {
     enum Socket {
        CLASS, PHM
    }
    enum TestTemperature {
        HOT, COLD, ROOM
    }
}


class Context {
    socket : Socket
    temperature : TestTemperature
    locationCode : LocationCode
}

class ExecResult {
    status : Pass/Fail
    error : String
}


ISearchExecuter *-- IHandler

class ConvergedVminSearch {
+id : String
}

@enduml
```

```plantuml
@startuml
    interface IServicesHandler {
        init() : ExecResult
        getVoltageService() : IVoltageService
        getFreqService() : IFreqService
        getMiscService() : IMiscService
        getDecodersService() : IDecodersService
        getRecoveryService() : IRecoveryService
        getCttService() : ICttService
    }
    
    interface IVminSearchService {
        init(ctx : InitContext) : ExecResult
    }
    interface IVoltageService {
    }
    interface IFreqService {
    }
    interface IMiscService {
    }
    interface IDecodersService {
    }
    interface IRecoveryService {
    }    
    
    IVoltageService --|> IVminSearchService
    IFreqService --|>IVminSearchService
    IMiscService --|> IVminSearchService
    IDecodersService --|> IVminSearchService
    IRecoveryService --|> IVminSearchService
    ICttService --|> IVminSearchService
    
    IServicesHandler o-- IVoltageService
    IServicesHandler o-- IFreqService
    IServicesHandler o-- IMiscService
    IServicesHandler o-- IDecodersService
    IServicesHandler o-- IRecoveryService
    IServicesHandler o-- ICttService
    
    
    interface IPrePoint {
         ExecutePrePoint() : ExecResult
    }
    interface IPostPoint {
        ExecutePostPoint() : ExecResult
    }
    
    interface IHandler {
        Handle() : 
    }


class InitContext {
}

class ExecContext {
    socket : Socket
    temperature : TestTemperature
    locationCode : LocationCode
}

class ExecResult {
    status : Pass/Fail
    error : String
}


ISearchExecuter *-- IHandler

class ConvergedVminSearch {
+id : String
}

@enduml
```
