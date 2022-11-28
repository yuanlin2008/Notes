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