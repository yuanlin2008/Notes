# FTexturePlatformData
```puml
!theme black-knight

class FTexturePlatformData{
    +FTextureAsyncCacheDerivedDataTask* AsyncTask
    +TIndirectArray<struct FTexture2DMipMap> Mips
    +bool TryLoadMips(int32 FirstMipToLoad, void** OutMipData, FStringView DebugContext)
}
```
# Texture Loading
## 贴图的初始loading数据.
- UTexture2D::Serialize(FArchive& Ar)
    - UTexture::BeginCachePlatformData()
        - UTexture::CachePlatformData
            - FTexturePlatformData::Cache
            开始异步加载数据.
- UTexture2D::PostLoad()
    - UTexture::PostLoad()
        - UTexture2D::UpdateResource()
            - UTexture::UpdateResource()
                - UTexture2D::CreateResource()
                    - if(数据加载完成)
                        - FTexturePlatformData::FinishCache
                        结束加载数据.
                    - else
                        - 加入到FTextureCompilingManager中,等待完成.

## FTextureCompilingManager何时结束UTexture的loading?
* 在FinishAllCompilation时
* 在FEngineLoop::Tick中调用ProcessAsyncTasks

# UTexture::UpdateResource()
刷新渲染资源.

# Texture StreamIn


* [ ] RHIAsyncCreateTexture2D与RHICreateTexture2D的区别
RHIAsyncCreateTexture2D可以在非RT中调用