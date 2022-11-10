```puml
!theme black-knight

interface IStreamingManager
interface IRenderAssetStreamingManager
class FRenderAssetStreamingManager {
    +void AddStreamingRenderAsset(UStreamableRenderAsset* InAsset)
    +void RemoveStreamingRenderAsset(UStreamableRenderAsset* RenderAsset)
    -TArray<FStreamingRenderAsset> StreamingRenderAssets
}
class FStreamingRenderAsset
note top:The streaming system's version\n of UStreamableRenderAsset

IStreamingManager<|--IRenderAssetStreamingManager
IRenderAssetStreamingManager<|--FRenderAssetStreamingManager
FRenderAssetStreamingManager..*FStreamingRenderAsset

```
---
```puml
!theme black-knight

class FStreamableRenderResourceState
class FRenderAssetUpdate

''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''

class UObject
class UStreamableRenderAsset #red{
    # FStreamableRenderResourceState CachedSRRState
}
class UTexture {
    +FTextureResource* CreateResource()
}
class UTexture2D
class USkeletalMesh

UObject<|-- UStreamableRenderAsset
UStreamableRenderAsset<|-- UTexture
UTexture<|-- UTexture2D
UStreamableRenderAsset<|-- USkeletalMesh

''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
class FRenderResource
class FTexture
class FTextureResource
class FStreamableTextureResource #red
class FTexture2DResource
class FTexture2DArrayResource
class FTexture3DResource

FRenderResource<|--FTexture
FTexture<|--FTextureResource
FTextureResource<|--FStreamableTextureResource
FStreamableTextureResource<|--FTexture2DResource
FStreamableTextureResource<|--FTexture2DArrayResource
FStreamableTextureResource<|--FTexture3DResource

''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
FStreamableRenderResourceState*.. UStreamableRenderAsset

```
