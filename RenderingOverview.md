```puml
left to right direction
!theme black-knight
package Engine{
	interface FSceneInterface
	class UWorld
	class ULevel
	class USceneComponent{
		void MarkRenderStateDirty()
	}
	together {
		class UPrimitiveComponent
		class FPrimitiveSceneProxy{
			virtual FPrimitiveViewRelevance GetViewRelevance(const FSceneView* View) const
			virtual void DrawStaticElements(FStaticPrimitiveDrawInterface* PDI)
			virtual void GetDynamicMeshElements()
		}
		note right:Rendering Thread
	}
	together {
		class ULightComponent
		class FLightSceneProxy
		note right:Rendering Thread
	}
	class FSceneView
	interface FSceneViewStateInterface
	together {
		class UMaterialInterface
		class UMaterial
		class UMaterialInstance
		class UMaterialInstanceConstant
		class UMaterialInstanceDynamic
		interface FMaterial
		class FMaterialResource
		class FMaterialRenderProxy
		note right:Rendering Thread
	}

	USceneComponent<|-- UPrimitiveComponent
	USceneComponent<|-- ULightComponent
	FMaterial<|--FMaterialResource
	UMaterialInterface<|-- UMaterial
	UMaterialInterface<|-- UMaterialInstance
	UMaterialInstance<|-- UMaterialInstanceConstant
	UMaterialInstance<|-- UMaterialInstanceDynamic
}
package RenderCore {
	interface IRendererModule
	class FRenderResource #darkorange
	class FRDGBuilder
	class FShader

	FShader<|--FGlobalShader
}
package Renderer{
	class FRendererModule
	class FScene
	class FViewInfo
	class FPrimitiveSceneInfo
	class FLightSceneInfo
	class FSceneViewState
	class FSceneRenderer{
		void Render(FRDGBuilder& GraphBuilder)
	}

	IRendererModule<|--FRendererModule
	FSceneInterface<|--FScene
	FSceneView<|--FViewInfo
	FSceneViewStateInterface<|--FSceneViewState
	FSceneRenderer<|--FDeferredShadingSceneRenderer
	FSceneRenderer<|--FMobileSceneRenderer

	FGlobalShader<|--FGlobalShaderXXX
	FShader<|--FMaterialShader
	FMaterialShader<|--FMeshMaterialShader
}

package RHI{
	class FRHIResource #darkred
	class FDynamicRHI
	interface IRHIComputeContext
	interface IRHICommandContext
	interface IRHITransientResourceAllocator

	object GDynamicRHI#green

	FRHICommandBase<|--FRHICommand
	FRHICommand<|--FRHICommandXXX

	FRHICommandListBase<|--FRHIComputeCommandList
	FRHIComputeCommandList<|--FRHICommandList
	FRHIComputeCommandList<|--FRHIAsyncComputeCommandListImmediate
	FRHICommandList<|--FRHICommandListImmediate
	IRHIComputeContext<|--IRHICommandContext
}

package RHICore{
	class FRHIMemoryPool
	class FRHIPoolAllocator
	class FRHITransientHeap
	class FRHITransientHeapAllocator
	class FRHITransientPageSpanAllocator
	class FRHITransientPagePool

	interface IRHITransientMemoryCache
	IRHITransientResourceAllocator<|--FRHITransientResourceHeapAllocator
	IRHITransientResourceAllocator<|--FRHITransientResourcePageAllocator
	IRHITransientMemoryCache<|--FRHITransientHeapCache
	IRHITransientMemoryCache<|--FRHITransientPagePoolCache
}

package D3D12RHI #green{
	FRHICommand<|--FD3D12RHICommandXXX
	FDynamicRHI<|--FD3D12DynamicRHI
	IRHICommandContext<|--FD3D12CommandContextBase
	FD3D12CommandContextBase<|--FD3D12CommandContext
	FRHITransientResourceHeapAllocator<|--FD3D12TransientResourceHeapAllocator
	FRHIMemoryPool<|--FD3D12MemoryPool
	FRHIPoolAllocator<|--FD3D12PoolAllocator
	FRHITransientHeapCache<|--FD3D12TransientHeapCache
	FRHITransientHeap<|--FD3D12TransientHeap

	FRHIBuffer<|--FD3D12Buffer
}
```

```puml
!theme black-knight
title Add Primitive to FScene

activate UPrimitiveComponent
activate FScene

group Game Thread
UPrimitiveComponent -> FScene : AddPrimitive
FScene->UPrimitiveComponent:CreateScenenProxy
UPrimitiveComponent->FPrimitiveSceneProxy
activate FPrimitiveSceneProxy
FScene->FPrimitiveSceneInfo:new FPrimitiveSceneInfo
activate FPrimitiveSceneInfo
FScene-[#red]>AddPrimitiveCommand:ENQUEUE_RENDER_COMMAND
activate AddPrimitiveCommand
end

group Render Thread
AddPrimitiveCommand->FPrimitiveSceneProxy:SetTransform
AddPrimitiveCommand->FPrimitiveSceneProxy:CreateRenderThreadResources
AddPrimitiveCommand->FScene:AddPrimitiveSceneInfo_RenderThread
end

```

* [Unreal Engine 4 Rendering](https://medium.com/@lordned/unreal-engine-4-rendering-overview-part-1-c47f2da65346)
* [How Create A Custom Mesh Component](https://medium.com/realities-io/creating-a-custom-mesh-component-in-ue4-part-0-intro-2c762c5f0cd6)

* FRenderResource在GT创建，在RT调用
* UStaticMesh资源管理？与Nanite关系(Engine/Renderring)
