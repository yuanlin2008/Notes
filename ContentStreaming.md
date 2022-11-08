[toc]
```puml
!theme black-knight

interface IStreamingManager
interface IRenderAssetStreamingManager
class FRenderAssetStreamingManager

IStreamingManager<|--IRenderAssetStreamingManager
IRenderAssetStreamingManager<|--FRenderAssetStreamingManager

```
---
```puml
!theme black-knight

class UObject
class UStreamableRenderAsset
class UTexture
class USkeletalMesh

class FStreamableRenderResourceState
class FRenderAssetUpdate

UObject<|-- UStreamableRenderAsset
UStreamableRenderAsset<|-- UTexture
UStreamableRenderAsset<|-- USkeletalMesh

```
---
```puml
!theme black-knight

class FRenderResource
class FTexture
class FStreamableTextureResource
class FTexture2DResource
class FTexture2DArrayResource
class FTexture3DResource

FRenderResource<|--FTexture
FTexture<|--FStreamableTextureResource
FStreamableTextureResource<|--FTexture2DResource
FStreamableTextureResource<|--FTexture2DArrayResource
FStreamableTextureResource<|--FTexture3DResource

```

# Texture
```puml
!theme black-knight

class FTexturePlatformData{
    +TIndirectArray<struct FTexture2DMipMap> Mips
    +bool TryLoadMips(int32 FirstMipToLoad, void** OutMipData, FStringView DebugContext)
}
```
## Texture Loading
- UTexture2D::Serialize(FArchive& Ar)
    - UTexture::BeginCachePlatformData()
        - UTexture::CachePlatformData