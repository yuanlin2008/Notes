[toc]
# 渲染过程
```puml
!theme black-knight
object UPrimitiveComponent
object "FPrimitiveSceneProxy/FPrimitiveSceneInfo" as P
object FMeshBatch
object FMeshDrawCommand

UPrimitiveComponent-->P
P-->FMeshBatch
FMeshBatch-->FMeshDrawCommand
```
# UPrimitiveComponent==>FPrimitiveSceneProxy/FPrimitiveSceneInfo
## FScene::AddPrimitive
* 创建FPrimitiveSceneProxy和FPrimitiveSceneInfo，并相互引用
* RT:SceneProxy->CreateRenderThreadResources()
* RT:Scene->AddPrimitiveSceneInfo_RenderThread

## FScene::AddPrimitiveSceneInfo_RenderThread
将FPrimitiveSceneInfo添加到AddedPrimitiveSceneInfos中，等待FScene::UpdateAllPrimitiveSceneInfos中处理.

## FScene::UpdateAllPrimitiveSceneInfos
处理FPrimitiveSceneInfo的添加、删除、修改。
* 将新添加的FPrimitiveSceneInfo放到AddedLocalPrimitiveSceneInfos中，并排序准备使用.
* 添加到FScene的Primitives中。 
* 调用FPrimitiveSceneInfo::AddToScene，填充各种Primitive...信息
***FScene中的各个Prmitive...数组包含了Primitive不同的信息。之所以好拆开是为了cache访问效率。FPrimitveSceneInfo->PackedIndex保存了这些数组的id***

# FPrimitiveSceneProxy/FPrimitiveSceneInfo==>FMeshBatch
## FPrimitiveSceneInfo::AddToScene
* 调用FPrimitiveSceneInfo::AddStaticMeshes为StaticMesh添加FMeshBatch和FMeshDrawCommand,并缓存起来

## FPrimitiveSceneInfo::AddStaticMeshes
* 调用FPrimitiveSceneProxy->DrawStaticElements,创建FMeshBatch(FStaticMeshBatch)并保存到FPrimitiveSceneInfo->StaticMeshes。
***StaticMesh会创建所有LOD层级的FMeshBatch***
* 将FMeshBatch保存到FScene->StaticMeshes中，并将此数组id保存到FStaticMeshBatch和FStaticMeshBatchRelevance的id中
* 调用CacheMeshDrawCommands缓存FMeshDrawCommand

# FMeshBatch==>FMeshDrawCommand
## EMeshPassFlags
* CachedMeshCommands
* MainView

## EMeshPass::Type
所有meshpass的id

## FMeshPassDrawListContext
```puml
left to right direction
!theme black-knight
class FMeshPassDrawListContext{
    +AddCommand()
    +FinalizeCommand()
}
note top:代表一个FMeshDrawCommand的list
note left of FMeshPassDrawListContext::"AddCommand()"
分配一个FMeshDrawCommand
end note
note left of FMeshPassDrawListContext::"FinalizeCommand()"
完成一个FMeshDrawCommand
end note

class FCachedPassMeshDrawListContext{}
note top:向FScene的CachedDrawLists中添加
class FCachedPassMeshDrawListContextImmediate
note top:立即添加.
class FCachedPassMeshDrawListContextDeferred
note top:延迟添加。用于多线程驱动的列表生成。

FMeshPassDrawListContext<|--FCachedPassMeshDrawListContext
FCachedPassMeshDrawListContext<|--FCachedPassMeshDrawListContextImmediate
FCachedPassMeshDrawListContext<|--FCachedPassMeshDrawListContextDeferred
FMeshPassDrawListContext<|--FDynamicPassMeshDrawListContext
FMeshPassDrawListContext<|--FNaniteDrawListContext
FNaniteDrawListContext<|--FNaniteDrawListContextImmediate
FNaniteDrawListContext<|--FNaniteDrawListContextDeferred
```
## FMeshPassProcessor
```puml
left to right direction
!theme black-knight
class FPassProcessorManager
note top:FMeshPassProcessor factory
class FMeshPassProcessor{ }
note top:将一个FMeshBatch转换为对应的FMeshDrawCommand
class FBasePassMeshProcessor{}
note top:BassPass

FMeshPassProcessor<|--FBasePassMeshProcessor
FMeshPassProcessor<|..OtherPassProcessor
```

* [ ] FPrimitiveSceneInfo::CacheMeshDrawCommands

# References
* [UE5 MeshDrawCommand](https://scahp.tistory.com/74)
* [Creating a Custom Mesh Component in UE4](https://medium.com/realities-io/creating-a-custom-mesh-component-in-ue4-part-0-intro-2c762c5f0cd6) * [Mesh Drawing Pipeline](https://docs.unrealengine.com/4.27/en-US/ProgrammingAndScripting/Rendering/MeshDrawingPipeline/)