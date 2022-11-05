[toc]
# RHITests
Engine\Plugins\Tests\RHITests\Source\RHITests
测试在FEngineLoop::PostInitRHI()中启动
命令行参数 "rhiunittest"

# Overview
```puml
!theme black-knight

package RHI{
	interface IRHIComputeContext
	interface IRHICommandContext
	interface IRHICommandContextPSOFallback
	interface IRHICommandContextContainer{
        IRHICommandContext* GetContext()
        void SubmitAndFreeContextContainer(int32 Index, int32 Num)
        void FinishContext()
    }
    class FDynamicRHI{
    	+IRHICommandContext* RHIGetDefaultContext()
        +IRHIComputeContext* RHIGetDefaultAsyncComputeContext()
        +IRHICommandContextContainer* RHIGetCommandContextContainer()
    }

	IRHIComputeContext<|--IRHICommandContext
	IRHICommandContext<|--IRHICommandContextPSOFallback
    FDynamicRHI--*IRHICommandContextContainer
}

package D3D11RHI #green{
    class FD3D11DynamicRHI{
    	+IRHICommandContext* RHIGetDefaultContext()
        +IRHIComputeContext* RHIGetDefaultAsyncComputeContext()
        +IRHICommandContextContainer* RHIGetCommandContextContainer()
    }
	IRHICommandContextPSOFallback<|--FD3D11DynamicRHI
	FDynamicRHI<|--FD3D11DynamicRHI
}

package D3D12RHI #green{
    class FD3D12DynamicRHI{
    	+IRHICommandContext* RHIGetDefaultContext()
        +IRHIComputeContext* RHIGetDefaultAsyncComputeContext()
        +IRHICommandContextContainer* RHIGetCommandContextContainer()
    }
    class FD3D12Device {
        uint32 GetNumContexts()
        FD3D12CommandContext& GetCommandContext(uint32 ThreadIndex)
        uint32 GetNumAsyncComputeContexts()
        FD3D12CommandContext& GetAsyncComputeContext(uint32 ThreadIndex)
        FD3D12CommandContext* ObtainCommandContext() 
        void ReleaseCommandContext(FD3D12CommandContext* CmdContext) 
        FD3D12CommandContext& GetDefaultCommandContext() 
        FD3D12CommandContext& GetDefaultAsyncComputeContext()

    }
	IRHICommandContext<|--FD3D12CommandContextBase
	FD3D12CommandContextBase<|--FD3D12CommandContext
	FDynamicRHI<|--FD3D12DynamicRHI
    IRHICommandContextContainer<|--FD3D12CommandContextContainer
    FD3D12Device--*FD3D12CommandContext
}
```
* [ ] D3D12有多个CommandContext，每个Context有自己的StateCache
* [ ] IRHICommandContextContainer与FRHICommandListBase::QueueParallelAsyncCommandListSubmit相关.
* [ ] FRHICommandListBase::QueueParallelAsyncCommandListSubmit与FParallelCommandListSet相关，在RDG和shadow中有使用.

# RHI命令执行
## 执行上下文.
命令列表的最终执行在FRHICommandLIstExecutor::ExcuteInner.
此函数有可能在GT,RT,RHIT上下文中执行，只有在RT中，命令会被推到RHIT中执行，否则就在本线程执行。

# FRHIResource
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
## ENQUEUE_RENDER_COMMAND

## FlushRenderingCommands()