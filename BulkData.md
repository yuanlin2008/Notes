# 单元测试
Engine/Source/Runtime/CoreUObject/Private/Tests/Serialization/BulkDataTests.cpp

# Overview
```puml
!theme black-knight

class FUntypedBulkData
class FByteBulkDataOld
class FWordBulkDataOld
class FIntBulkDataOld
class FFloatBulkDataOld

FUntypedBulkData<|--FByteBulkDataOld
FUntypedBulkData<|--FWordBulkDataOld
FUntypedBulkData<|--FIntBulkDataOld
FUntypedBulkData<|--FFloatBulkDataOld
```