```puml
left to right direction
!theme black-knight

package RenderCore{
	class FRenderResource #darkorange {
		+void InitResource()
		+void InitRHI()
		+void InitDynamicRHI()
	}
	FRenderResource<|--FTexture
	FTexture<|--FTextureResource
	FTextureResource<|--FStreamableTextureResource
	FStreamableTextureResource<|--FTexture2DResource
	FStreamableTextureResource<|--FTexture2DArrayResource
	FStreamableTextureResource<|--FTexture3D
	FRenderResource<|--FTextureReference
	FRenderResource<|--FVertexBuffer
	FRenderResource<|--FIndexBuffer
	FRenderResource<|--FVertexFactory
	FRenderResource<|--FVertexFactory
	FRenderResource<|--"TUniformBuffer<TBufferStruct>"
}

package Engine {
	FRenderResource<|-- EngineRRXXX
	FRenderResource<|--FMaterialRenderProxy
}

package RenderCore{
	FRenderResource<|-- RenderCoreRRXXX
}

package Renderer{
	FRenderResource<|-- RendererRRXXX
}

```

FRenderResource代表一个RT拥有的渲染资源。FRenderResource一般在GT创建并填充数据，然后推到RT调用InitResource做RT方面的初始化。
FRenderResource一般会包含使用到的FRHIResource。