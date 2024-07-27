#   DirectorToDecodersMgr

# Available Decoders Today

| DecoderName | Description                                                                                  |
|-------------|----------------------------------------------------------------------------------------------|
| LOG1        | Read Tos LOG1 register per tick and determine which IP is defective                          |
| DEFAULT     | No need further decoding, voltage-target can be directly defined by the failling pattern/Pin |
| CTV         | Read Capture Memory, decode bits-string and determine pass/failing targets                   |
| CTT         | Recognize the failing Pattern, map the required Decoder, initialize it and execute it        |

# Decoders Architecture


# how hte Decoder is being selected?
1. InstanceParameter
   2. FailDecoderName = "LOG1_DECODER"
   3. FailDecoderArgs = "11110000"


```C#
interface IDecoderInit {
   IDecoder CreateDecoder(Context ctx)
}

interface IDecoder {
   BitArray Decode()
   IDecoder Clone()
   IDecoder setContext(VminSearch current,Context ctx)
   void ApplyPreExecute(string plistName)
   void ApplyPostExecute(string plistName)
}

class DecoderInit : IDecoderInit {
   IDecoder CreateDecoder(Context ctx) {
      var map = new Dictionary<string,stromg>() {
              "cfgFile" : "./Modules/CTT_INFRA/....json",
           "Mode" : "MisrOff",
      }
      IDecoderInit init = new Log1DecoderInit(map)
      IDecoder decoder = init.CreateDecoder()
      Services.SharedMemory.AddRow<IDecoder>(decoder,Polci)
      return decoder;
   }
}
   
```



```mermaid
sequenceDiagram
    Note right of VminSearchDirector: BASE_INIT
   Note over VminSearchDirector,DecoderAInit: initialize the Decoder by supplying all requested data, and storing the IDecoder as Skeletong the SharedMemory
    DecoderInit ->> DecoderInit: verify()
    DecoderInit ->> DecoderInit: execute()

   Note right of VminSearchDirector: MAIN
   
   Note right of VminSearchDirector: Verify Of VminSearch instance
    Note over VminSearchDirector,DecodersMgr: Get the Target Decoder for the current instance, Clone it and Get Ready For Decoding
    VminSearchDirector ->> DecodersMgr: GetDecoderByName("LOG1_DECODER")
   DecodersMgr ->> DecodersMgr: GetDecoderFromSharedMemory("LOG1_DECODER")
   DecodersMgr ->> DecodersMgr: CloneDecoder("LOG1_DECODER")
   DecodersMgr ->> VminSearchDirector: IDecoder for the current VminSearch.

   Note over VminSearchDirector,DecodersMgr: GetFunctionalTest extension
   VminSearchDirector ->> DecodersMgr: ApplyChangesToFuncTest(FunctionalObject)
   DecodersMgr ->> Decoder: ApplyChangesToFuncTest()
   Decoder ->> Decoder: ApplyConnectLog1ToPlist(FunctionalObject)
   Decoder -->> DecodersMgr: FunctionalObject
   DecodersMgr -->> VminSearchDirector: FunctionalObject
   
    Note right of VminSearchDirector: Pre-Execute
   VminSearchDirector ->> VminSearchDirector: PreExecute()

   loop While Vmin is not Concluded
        Note over VminSearchDirector,VminSearchDirector: Pre Point - Prepare Services for SinglePoint Execution
        Note over VminSearchDirector,VminSearchDirector: Exec-Point - Execute FunctonalTest/SinglePoint on PreDefined Settings.
        Note over VminSearchDirector,VminSearchDirector: Post-Point - Process Single-point result and act accordingly}
        VminSearchDirector --> Decoder: Decode()
        Decoder --> VminSearchDirector: List Of Targets that Failed or Passed
        VminSearchDirector --> VminSearchDirector: ProcessPlistResults()
    end
    Note over VminSearchDirector,VminSearchDirector: Post-Execute - Perform Search Finalization and Report Results.
    VminSearchDirector --> Decoder: PostExecute()
    
```