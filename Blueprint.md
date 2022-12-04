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
## Class Name
 If your blueprint reference looks like this:
Blueprint’/Game/Environments/AsteroidBaseMocks/AsteroidHall_L/Base_A_AsteroidHall_L_Straight.Base_A_AsteroidHall_L_Straight’
Then the string is:
/Game/Environments/AsteroidBaseMocks/AsteroidHall_L/Base_A_AsteroidHall_L_Straight.Base_A_AsteroidHall_L_Straight_C
Take off the Blueprint part and the single quotes and end it in _C

* [ ] AnimGraph derivation?
## Level Blueprint

# References
* [Functions vs Events](https://pixelsapiens.com/functions-vs-events-in-unreal-engine-4/#:~:text=Events%20cannot%20have%20return%20values,essential%20for%20any%20multiplayer%20game)
* [深入Unreal蓝图开发](https://www.zhihu.com/column/blueprints-in-depth)