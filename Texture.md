# FTexturePlatformData
```puml
!theme black-knight

class FTexturePlatformData{
    +TIndirectArray<struct FTexture2DMipMap> Mips
    +bool TryLoadMips(int32 FirstMipToLoad, void** OutMipData, FStringView DebugContext)
}
```
# Texture Loading
贴图的初始loading数据.
- UTexture2D::Serialize(FArchive& Ar)
    - UTexture::BeginCachePlatformData()
        - UTexture::CachePlatformData

# Texture StreamIn

# UTexture::UpdateResource()

* [ ] RHIAsyncCreateTexture2D与RHICreateTexture2D的区别
RHIAsyncCreateTexture2D可以在非RT中调用