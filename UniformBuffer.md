```puml
!theme black-knight

class FRHIResource
class FRHIUniformBufferResource
class FRHIUniformBufferLayout{
    + TArray<FRHIUniformBufferResource> Resources
}
class FRHIUniformBuffer{
    -TRefCountPtr<const FRHIUniformBufferLayout> Layout
}
class FD3D12UniformBuffer

class FRenderResource
class "TUniformBuffer<<TBufferStruct>>" as TU{
    -TUniformBufferRef<TBufferStruct> UniformBufferRHI
}
note bottom:将TBufferStruct的Layout与FRHIUniformBuffer关联起来

FRHIResource<|--FRHIUniformBuffer
FRHIResource<|--FRHIUniformBufferLayout
FRHIUniformBuffer<|--FD3D12UniformBuffer
FRHIUniformBuffer*..FRHIUniformBufferLayout
FRHIUniformBufferResource..*FRHIUniformBufferLayout

FRenderResource<|--TU
FRHIUniformBuffer..*TU

```

* [虚幻4 高级数据传输UniformBuffer](https://zhuanlan.zhihu.com/p/36696626)