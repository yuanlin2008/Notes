```puml
!theme black-knight

class UBlueprint
note left:Blueprint Asset

UObject<|-- UBlueprintCore
UBlueprintCore<|-- UBlueprint
UBlueprint<|-- UAnimBlueprint
UBlueprint<|-- UControlRigBlueprint
UBlueprint<|-- ULevelScriptBlueprint

class UBlueprintGeneratedClass
note right:Blueprint Asset对应的UClass

UObject<|-- UClass
UClass<|-- UBlueprintGeneratedClass
UBlueprintGeneratedClass<|-- UAnimBlueprintGeneratedClass
UBlueprintGeneratedClass<|-- UControlRigBlueprintGeneratedClass

```

## Level Blueprint

# References
* [Functions vs Events](https://pixelsapiens.com/functions-vs-events-in-unreal-engine-4/#:~:text=Events%20cannot%20have%20return%20values,essential%20for%20any%20multiplayer%20game)