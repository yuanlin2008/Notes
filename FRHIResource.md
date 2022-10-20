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