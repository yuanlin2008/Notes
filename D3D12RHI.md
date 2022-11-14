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
# Resource

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
	class FD3D12ResourceLocation {
		-FD3D12BaseShaderResource Owner
		-FD3D12Resource UnderlyingResource
	}
	class FD3D12BaseShaderResource {
		+FD3D12ResourceLocation ResourceLocation
	}

	FD3D12DeviceChild<|--FD3D12BaseShaderResource
	FD3D12DeviceChild<|--FD3D12Resource
	FD3D12DeviceChild<|--FD3D12ResourceLocation
	FD3D12Resource.*FD3D12ResourceLocation
	FD3D12ResourceLocation*.*FD3D12BaseShaderResource
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

# RHI Transient Resource & Allocator
* [ ] 为RDG服务.

```puml
left to right direction
!theme black-knight
package RHI {
	class FRHITransientResource #purple
	class FRHITransientBuffer #purple
	class FRHITransientTexture #purple

	interface IRHITransientResourceAllocator

	class FDynamicRHI {
		+IRHITransientResourceAllocator* RHICreateTransientResourceAllocator()
	}

	FRHITransientResource<|--FRHITransientBuffer
	FRHITransientResource<|--FRHITransientTexture
	IRHITransientResourceAllocator..*FDynamicRHI
}
package RHICore {
	class TRHITransientResourceCache
	class FRHITransientResourceHeapAllocator
	class FRHITransientResourcePageAllocator
	IRHITransientResourceAllocator<|--FRHITransientResourceHeapAllocator
	IRHITransientResourceAllocator<|--FRHITransientResourcePageAllocator
}

package D3D12RHI #green {
	class FD3D12TransientResourceHeapAllocator

	FRHITransientResourceHeapAllocator<|--FD3D12TransientResourceHeapAllocator
}
```

# D3D12Allocation
```puml
!theme black-knight

package RHICore {
	class FRHIMemoryPool
	class FRHIPoolAllocator
}

package D3D12RHI #green {
	class FD3D12ResourceAllocator
	class FD3D12BuddyAllocator
	class FD3D12BaseAllocatorType #blue
	class FD3D12MultiBuddyAllocator{
		#TArray<FD3D12BuddyAllocator> Allocators
	}
	class FD3D12BucketAllocator #green
	class FD3D12TextureAllocator #green
	class FD3D12UploadHeapAllocator{
		-FD3D12MultiBuddyAllocator SmallBlockAllocator
		-FD3D12PoolAllocator BigBlockAllocator
		-FD3D12MultiBuddyAllocator FastConstantPageAllocator
	}
	class FD3D12MemoryPool
	class FD3D12PoolAllocator
	class FD3D12BufferPool #blue
	class FD3D12DefaultBufferAllocator{
		-TArray<FD3D12BufferPool*> DefaultBufferPools;
	}
	class FD3D12FastAllocator
	class FD3D12FastConstantAllocator
	class FTransientUniformBufferAllocator
	class FD3D12SegListAllocator
	class FD3D12TextureAllocatorPool {
		-FD3D12PoolAllocator* PoolAllocators[(int)EPoolType::Count]
	}

	class FD3D12Adapter #darkred{
		#FD3D12UploadHeapAllocator* UploadHeapAllocator[MAX_NUM_GPUS]
		#TArray<FTransientUniformBufferAllocator*> TransientUniformBufferAllocators;
	}
	class FD3D12Device #darkred{
		#FD3D12DefaultBufferAllocator DefaultBufferAllocator
		#FD3D12FastAllocator DefaultFastAllocator
		#FD3D12TextureAllocatorPool TextureAllocator
	}
	class FD3D12ResourceLocation #darkred {
		-FD3D12BaseAllocatorType* Allocator;
		-FD3D12SegListAllocator* SegListAllocator;
		-FD3D12PoolAllocator* PoolAllocator;
	}

	FD3D12ResourceAllocator<|--FD3D12BuddyAllocator
	FD3D12ResourceAllocator<|--FD3D12MultiBuddyAllocator
	FD3D12ResourceAllocator<|--FD3D12BucketAllocator
	FD3D12MultiBuddyAllocator<|--FD3D12TextureAllocator

	FRHIMemoryPool<|--FD3D12MemoryPool
	FRHIPoolAllocator<|--FD3D12PoolAllocator
	FD3D12PoolAllocator..FD3D12BufferPool
	FD3D12BuddyAllocator..FD3D12BaseAllocatorType

	FD3D12UploadHeapAllocator..*FD3D12Adapter
	FD3D12BufferPool..*FD3D12DefaultBufferAllocator

	FD3D12MultiBuddyAllocator..*FD3D12UploadHeapAllocator
	FD3D12PoolAllocator..*FD3D12UploadHeapAllocator
	FD3D12DefaultBufferAllocator..*FD3D12Device
	FD3D12FastAllocator..*FD3D12Device
	FD3D12TextureAllocatorPool..*FD3D12Device

	FD3D12FastConstantAllocator<|--FTransientUniformBufferAllocator
	FTransientUniformBufferAllocator..*FD3D12Adapter

	FD3D12ResourceLocation*..FD3D12BaseAllocatorType
	FD3D12ResourceLocation*..FD3D12SegListAllocator
	FD3D12ResourceLocation*..FD3D12PoolAllocator
	FD3D12TextureAllocatorPool*..FD3D12PoolAllocator
	FD3D12MemoryPool..*FD3D12PoolAllocator
}
```

# D3D12状态管理.
* [ ] D3D12State.h .cpp
