# D3D12
```puml
left to right direction
!theme black-knight

ID3D12Object<|--ID3D12Device
ID3D12Object<|--ID3D12DeviceChild
ID3D12DeviceChild<|--ID3D12CommandList
ID3D12CommandList<|--ID3D12GraphicsCommandList
ID3D12DeviceChild<|--ID3D12LifttimeTracker
ID3D12DeviceChild<|--ID3D12Pageable
ID3D12Pageable<|--ID3D12CommandAllocator
ID3D12Pageable<|--ID3D12CommandQueue
ID3D12Pageable<|--ID3D12CommandSignature
ID3D12Pageable<|--ID3D12DescriptorHeap
ID3D12Pageable<|--ID3D12Fence
ID3D12Pageable<|--ID3D12Heap
ID3D12Pageable<|--ID3D12PipelineState
ID3D12Pageable<|--ID3D12Resource
ID3D12Pageable<|--ID3D12StateObject
ID3D12DeviceChild<|--ID3D12RootSignature
```
---
```puml
!theme black-knight
package RHI {
	class FRHIResource
	class FRHIBuffer
	class FRHITexture
	class FRHITexture2D
	class FRHITexture2DArray
	class FRHITexture3D
	class FRHITextureCube
	FRHIBuffer--|>FRHIResource
	FRHITexture--|>FRHIResource
	FRHITexture2D--|>FRHITexture
	FRHITexture2DArray--|>FRHITexture2D
	FRHITexture3D--|>FRHITexture
	FRHITextureCube--|>FRHITexture
}
package D3D12RHI {
	class FD3D12Buffer #darkred
	class FD3D12Texture2D #darkred
	class FD3D12Texture2DArray #darkred
	class FD3D12TextureCube #darkred
	class FD3D12Texture3D #darkred

	FD3D12DeviceChild<|--FD3D12BaseShaderResource
	FD3D12BaseShaderResource<|--FD3D12Buffer
	FD3D12Buffer--|>FRHIBuffer
	FD3D12BaseShaderResource<|--FD3D12TextureBase
	FD3D12TextureBase<|--FD3D12Texture3D
	FD3D12Texture3D--|>FRHITexture3D
	FD3D12BaseTexture2D--|>FRHITexture2D
	FD3D12BaseTexture2DArray--|>FRHITexture2DArray
	FD3D12BaseTextureCube--|>FRHITextureCube

	FD3D12TextureBase<|--FD3D12Texture2D
	FD3D12Texture2D--|>FD3D12BaseTexture2D
	FD3D12TextureBase<|--FD3D12Texture2DArray
	FD3D12Texture2DArray--|>FD3D12BaseTexture2DArray
	FD3D12TextureBase<|--FD3D12TextureCube
	FD3D12TextureCube--|>FD3D12BaseTextureCube
}

```

# D3D12状态管理.
* [ ] D3D12State.h .cpp
