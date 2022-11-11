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
                    
初始加载结束后会通过CreateResource创建RHI资源。
**如果异步加载没有完成，就使用GetDefaultTexture2D来创建RHI资源.**

## FTextureCompilingManager何时结束UTexture的loading?
* 在FinishAllCompilation时
* 在FEngineLoop::Tick中调用ProcessAsyncTasks

异步加载完成后会调用Texture的UpdateResource

# UTexture::UpdateResource()
* 调用CreateResource()创建FTextureResource
* BeginInitResource()
* LinkStreaming()

# RHI资源创建

# Texture StreamIn


* [ ] RHIAsyncCreateTexture2D与RHICreateTexture2D的区别
RHIAsyncCreateTexture2D可以在非RT中调用