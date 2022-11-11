[toc]

# 数据描述
```puml
!theme black-knight

class FTexture2DMipMap {
    +FByteBulkData BulkData
}
note top:mipmap数据

class FTexturePlatformData{
    +FTextureAsyncCacheDerivedDataTask* AsyncTask
    +TIndirectArray<struct FTexture2DMipMap> Mips
    +bool TryLoadMips(int32 FirstMipToLoad, void** OutMipData, FStringView DebugContext)
}
note top:贴图数据

class UTexture2D{
    -FTexturePlatformData* PrivatePlatformData;
}

FTexture2DMipMap..*FTexturePlatformData
FTexturePlatformData.* UTexture2D

```

# UTexture2D::Serialize
- UTexture2D::Serialize(FArchive& Ar)
    - UTexture::BeginCachePlatformData()
        - UTexture::CachePlatformData
            - FTexturePlatformData::Cache

* 在UTexture2D::Serialize中开始异步加载FTexturePlatformData
* FTexturePlatformData的mipmap数据不一定都会被加载到内存，等待后面streaming

# UTexture2D::PostLoad

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
                    
* 会调用UpdateResource创建FTexture2DResource.
* 如果异步加载没有完成，就使用GetDefaultTexture2D的FTexture2DResource当作ProxyResource来创建FTexture2DResource
* 如果异步加载没有完成，将UTexture2D加入到FTextureComilingManager中，等待完成.
* FTextureCompilingManager何时结束UTexture的loading?
    * 在FinishAllCompilation时
    * 在FEngineLoop::Tick中调用ProcessAsyncTasks
* FTextureCompilingManager结束后会调用UpdateResource更新FTexture2DResource.


# UTexture2D::UpdateResource()
* 调用CreateResource()创建FTexture2DResource
* BeginInitResource()在通知RT初始化RenderResource
* LinkStreaming()

# FTexture2DResource
## FTexture2DResource创建两种模式
* 正常模式
* 代理模式:使用另一个贴图的FTexture2DResource,用来处理一部加在未完成状态，使用系统默认贴图.
## FTexture2DResource::CreateTexture
* RHICreateTexture2D
* 遍历所有mipmap
    * RHILockTexture2D
    * FTexture2DResource::GetData
    * RHILockTexture2D

# Texture StreamIn


* [ ] RHICreateTexture2D
* [ ] RHIAsyncCreateTexture2D与RHICreateTexture2D的区别
RHIAsyncCreateTexture2D可以在非RT中调用