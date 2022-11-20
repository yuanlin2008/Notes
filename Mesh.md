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


# References
* [UE5 MeshDrawCommand](https://scahp.tistory.com/74)
* [Creating a Custom Mesh Component in UE4](https://medium.com/realities-io/creating-a-custom-mesh-component-in-ue4-part-0-intro-2c762c5f0cd6)
* [Mesh Drawing Pipeline](https://docs.unrealengine.com/4.27/en-US/ProgrammingAndScripting/Rendering/MeshDrawingPipeline/)