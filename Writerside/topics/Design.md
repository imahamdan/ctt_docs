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

    Note right of TestMethod: Verify @ INIT
    TestMethod ->> TestMethodParser: +Parse(this)
    TestMethodParser -->> TestMethod: List<TargetsSearchSpec>
    TestMethod ->> VminSearchDirector: +init()
    VminSearchDirector ->> TestPointsRangeMgr: +init()
    TestPointsRangeMgr -->> VminSearchDirector: Done.
    VminSearchDirector --> TestMethod: Done.
    VminSearchDirector ->> VminSearchDirector: Execute Handlers init()

  
    Note right of TestMethod: Pre-Execute    
    Note over TestMethod,VminSearchDirector: Pre Execute and get Prepared for Sku based configurations
    TestMethod ->> +VminSearchDirector: PreExecute()
    VminSearchDirector ->> TestPointsRangeMgr: ResolveTestingPoints()
    TestPointsRangeMgr ->> VminSearchDirector: Done.
    VminSearchDirector ->> VminSearchDirector: Execute Handlers Registered for Pre-Execute()
    VminSearchDirector -->> TestMethod: Services Init by Skew is Done.
    
    loop While Vmin is not Concluded
        Note over TestMethod,VminSearchDirector: Pre Point - Prepare Services for SinglePoint Execution
        VminSearchDirector ->> TestPointsRangeMgr: GetTestingPoint()
        TestPointsRangeMgr ->> VminSearchDirector: MinVoltage=X, MaxVoltage=Y, Pass=X()
        VminSearchDirector ->> VminSearchDirector: UpdateSinglePointContext()
        VminSearchDirector ->> VminSearchDirector: Execute Handlers Registered for Pre-Point()
        VminSearchDirector -->> TestMethod: All Set - Single-Point Test Can be Executed.
    
        Note over TestMethod,VminSearchDirector: Exec-Point - Execute FunctonalTest/SinglePoint on PreDefined Settings.
        TestMethod ->> TestMethod: Execute Single Point()
        Note over TestMethod,VminSearchDirector: Post-Point - Process Single-point result and act accordingly}
        TestMethod ->> VminSearchDirector: Analyze SinglePoint Execution and Advice Pass/Fail
        alt Plist Passed
            Note over TestMethod,VminSearchDirector: Single Point test passed at given Voltage & Frequency
            VminSearchDirector->>TestMethod: Pass - Exit VminSearch Loop.
        else Plist Failed
            Note over TestMethod,VminSearchDirector: Single Point test failed at given Voltage & Frequency, prepare to move for next point if any.
            VminSearchDirector->>TestMethod: Fail - need to move for the next point
            TestMethod ->> VminSearchDirector: HasNextPoint()
            VminSearchDirector ->> TestPointsRangeMgr: HasNextPoint()
            alt Yes - More Points To BE executed
                TestPointsRangeMgr -->>VminSearchDirector: Yes.
                VminSearchDirector ->> TestPointsRangeMgr: MoveForNextPoint()
                VminSearchDirector -->> TestMethod: All Set - StartPattern, TargetsToBeTicked, StepSize,etc... You Can Proceed.
            else No - No More points are available
                TestPointsRangeMgr -->>VminSearchDirector: No.
                VminSearchDirector -->> TestMethod: Terminate()
            end
        end
    end
    Note over TestMethod,VminSearchDirector: Post-Execute - Perform Search Finalization and Report Results.
    TestMethod ->> VminSearchDirector: Execute Handlers Registered for Post-Execute()
    TestMethod ->> TestMethod: Terminate()
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
    
    interface ISearchDirector {
        Init()
        PreExecute()
        PrePoint()
        PostPoint()
        PostExecute()
        Finalize()
    }
    
    interface IPreExecute {
         ExecutePreExecute() : ExecResult
    }
    interface IPrePoint {
         ExecutePrePoint() : ExecResult
    }
    interface IPostPoint {
        ExecutePostPoint() : ExecResult
    }
    interface IPostExecute {
        ExecutePostPoint() : ExecResult
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
    status : PassFail
    error : String
}

enum PassFail {
    Pass, Fail
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
