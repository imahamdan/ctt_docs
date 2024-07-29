# Recovery

there are 2 config files by PRIME
1. Defaetature tracking: a json that define the group of ips to perform recovery
define family and number of instances
![vpu.png](vpu.png)

rule what is healthy recovery
1. minimum count of IPs
2. valid combinations

setPoint patmod infra to apply recovery settings (setpoints & patmod)
![image_2.png](image_2.png)
![image_1.png](image_1.png)

need to add RecoverySpec(Modulke, Group=Disabiling_slice, RulesGroup=VPU1_RUlE] at the Corenr Mapping file
need to define a mapping between a failling IP to the recovery String

```mermaid
sequenceDiagram
    Note right of VminSearchDirector: Execute  @ PRECPU
    VoltageConfigInitializer ->> VminFwInitlaizer: ReadDffAndPopulateSharedStorage()

    Note right of VminSearchDirector: Verify @ INIT

    Note over VminSearchDirector,RecoveryMgr: Create and Initialize VoltageHandlers Per TargetGroups & InitialVoltages
    VminSearchDirector ->> VoltageService: initDefeatureServiceAndTracking()
    Note over VminSearchDirector,RecoveryMgr: Apply defeature at both Tracking SharedStorage and Reset/...
    VminSearchDirector ->> VoltageService: applyAlreadyDoneDefaturesBasedOnDFFRead()
    
    Note over RecoveryMgr,VoltageConfigInitializer: runCtx is Process,OptType,...  (CLASSHOT,PBIS_DAB)

    Note right of VminSearchDirector: Pre-Execute    
    loop While Vmin is not Concluded
        Note over VminSearchDirector,VminSearchDirector: Pre Point - Prepare Services for SinglePoint Execution
        VminSearchDirector --> RecoveryMgr: ApplySearchVoltage(List<TargetAndVoltage> voltagesToSet)
        Note over VminSearchDirector,VminSearchDirector: Exec-Point - Execute FunctonalTest/SinglePoint on PreDefined Settings.
        VminSearchDirector ->> VminSearchDirector: Execute Single Point()
        Note over VminSearchDirector,VminSearchDirector: Post-Point - Process Single-point result and act accordingly}
    end
    Note over VminSearchDirector,VminSearchDirector: Post-Execute - Perform Search Finalization and Report Results.
    
    VminSearchDirector ->> RecoveryMgr: Finalize()
    RecoveryMgr --> RecoveryMgr: Iterate over VotlageDomain Handlers And Restore()
    RecoveryMgr --> VminFwInitlaizer: Iterate over VminFw Handlers And Store()
    VminSearchDirector -->> VminFwInitlaizer: StoreVoltages()
```


