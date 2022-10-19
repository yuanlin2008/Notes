```puml
left to right direction
!theme black-knight

package RenderCore{
	class FRenderResource #darkorange
}

package Engine {
	interface FSceneInterface
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

package RenderCore{
	class FRDGBuilder
	class FShader
	FShader<|--FGlobalShader
}

package Renderer{
	class FSceneRenderer{
		void Render(FRDGBuilder& GraphBuilder)
	}
	FSceneInterface<|--FScene
	FSceneRenderer<|--FDeferredShadingSceneRenderer
	FSceneRenderer<|--FMobileSceneRenderer
	FGlobalShader<|--FGlobalShaderXXX
	FShader<|--FMaterialShader
	FMaterialShader<|--FMeshMaterialShader
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
left to right direction
!theme black-knight

package RenderCore{
	class FRenderResource #darkorange
	FRenderResource<|--FTexture
	FRenderResource<|--FTextureReference
	FRenderResource<|--FVertexBuffer
	FRenderResource<|--FIndexBuffer
	FRenderResource<|--FVertexFactory
	FRenderResource<|--FVertexFactory
	FRenderResource<|--TUniformBuffer
}

package Engine {
	FRenderResource<|-- EngineRRXXX
	FRenderResource<|--FMaterialRenderProxy
}

package RHI{
}

package RHICore{
}

package RenderCore{
	FRenderResource<|-- RenderCoreRRXXX
}

package Renderer{
	FRenderResource<|-- RendererRRXXX
}

package D3D12RHI #green{
}

```
```puml
left to right direction
!theme black-knight

package RHI{
	class FRHIResource #darkred

	FRHIResource<|--FRHISamplerState
	FRHIResource<|--FRHIRasterizerState
	FRHIResource<|--FRHIDepthStencilState
	FRHIResource<|--FRHIBlendState
	FRHIResource<|--FRHIVertexDeclaration
	FRHIResource<|--FRHIBoundShaderState
	FRHIResource<|--FRHIShader
	FRHIShader<|--FRHIVertexShader
	FRHIShader<|--FRHIPixelShader
	FRHIShader<|--FRHIComputeShader
	FRHIResource<|--FRHIGraphicsPipelineState
	FRHIResource<|--FRHIComputePipelineState
	FRHIResource<|--FRHIUniformBufferLayout
	FRHIResource<|--FRHIUniformBuffer
	FRHIResource<|--FRHIBuffer
	FRHIResource<|--FRHITexture
	FRHITexture<|--FRHITexture2D
	FRHITexture2D<|--FRHITexture2DArray
	FRHITexture<|--FRHITexture3D
	FRHITexture<|--FRHITextureCube
	FRHITexture<|--FRHITextureReference
}

package D3D12RHI #green{
	FRHISamplerState<|--FD3D12SamplerState
	FRHIRasterizerState<|--FD3D12RasterizerState
	FRHIDepthStencilState<|--FD3D12DepthStencilState
	FRHIBlendState<|--FD3D12BlendState
	FRHIVertexDeclaration<|--FD3D12VertexDeclaration
	FRHIBoundShaderState<|--FD3D12BoundShaderState
	FRHIVertexShader<|--FD3D12VertexShader
	FRHIPixelShader<|--FD3D12PixelShader
	FRHIComputeShader<|--FD3D12ComputeShader
	FRHIGraphicsPipelineState<|--FD3D12GraphicsPipelineState
	FRHIComputePipelineState<|--FD3D12ComputePipelineState
	FRHIUniformBuffer<|--FD3D12UniformBuffer
	FRHIBuffer<|--FD3D12Buffer
	FRHITexture2D<|--FD3D12BaseTexture2D
	FRHITexture2DArray<|--FD3D12BaseTexture2DArray
	FRHITexture3D<|--FD3D12Texture3D
	FRHITextureCube<|--FD3D12BaseTextureCube
}

```

D3D12RHI-->Engine
D3D12RHI-->RHI
D3D12RHI-->RHICore
D3D12RHI-->RenderCore

Engine-->RenderCore
Engine-->RHI
Engine-->Renderer

RHI-->D3D12RHI #green:dynamic load

Renderer-->Engine
Renderer-->RenderCore
Renderer-->RHI

RHICore-->RHI
RHICore-->RenderCore

RenderCore-left->RHI