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