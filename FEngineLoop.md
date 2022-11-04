[toc]

# GuardedMain
```puml
@startmindmap
!theme black-knight
* GuardedMain()
** EnginePreInit()
*** FEngineLoop::PreInit()
** EngineInit()
*** FEngineLoop::Init()
** EngineTick()
*** FEngineLoop::Tick()
@endmindmap
```
# FEngineLoop::PreInit()
```puml
@startmindmap
!theme black-knight
* FEngineLoop::PreInit()
** FEngineLoop::PreInitPreStartupScreen()
*** FShaderCodeLibrary::PreInit()
*** FTaskGraphInterface::Startup()
*** GThreadPool 初始化
*** GBackgroundPriorityThreadPool 初始化
*** GIOThreadPool 初始化
***[#red] RHIInit
*** FShaderCodeLibrary::InitForRuntime()
** FEngineLoop::PreInitPostStartupScreen()
*** FShaderCodeLibrary::OpenLibrary()
*** FShaderPipelineCache::OpenPipelineFileCache()
***[#red] RHIPostInit()
***[#blue] StartRenderingThread()

@endmindmap
```

# FEngineLoop::Init()
```puml
@startmindmap
!theme black-knight
* FEngineLoop::Init()
** 创建GEngine
** GEngine->Init()
** GEngine->Start()
@endmindmap
```

# FEngineLoop::Tick
```puml
!theme black-knight

participant "GameThread" as GT
participant "RenderThread" as RT
'participant =RHIThread" as RHIT

'EnqueueRC
!procedure $enqueue_render_command($cmd, $desc)
GT-[#red]>RT:<b><font color=red>$cmd\n$desc
!endprocedure

RT->RT:FRenderingThreadTickHeartbeat\nTickRenderingTickables()

loop All world
$enqueue_render_command(UpdateScenePrimitives,"FScene::UpdateAllPrimitiveSceneInfo()")
end
$enqueue_render_command(BeginFrame,"BeginFrameRenderThread()")
loop All world
$enqueue_render_command(SceneStartFrame,"FScene::StartFrame()")
end
$enqueue_render_command(ResetDeferredUpdates, \
"handle some per-frame tasks on the rendering thread \n\
FDeferredUpdateResource::ResetNeedsUpdate()\n\
FlushPendingDeleteRHIResources_RenderThread()")

GT-[#yellow]>GT:GEngine->Tick()
GT-[#yellow]>GT:FSlateApplication::Tick()

$enqueue_render_command(WaitForOutstandingTasksOnly_for_DelaySceneRenderCompletion, \
"FRHICommandListExcutor::GetImmediateCommandList().ImmediateFlush()")

GT-[#yellow]>GT:RHITick()

GT-[#orange]>RT:FFrameEndSync::Sync()

$enqueue_render_command(EndFrame, "EndFrameRenderThread()")
```

## TickRenderingTickables()
## FScene::UpdateAllPrimitiveSceneInfo()
同步primitive添加删除和修改.
## UGameEngine::Tick()
```puml
@startmindmap
!theme black-knight
* UGameEngine::Tick()
**: Ticking Worlds
    https://www.cnblogs.com/kekec/p/14781454.html;
*** RunTickGroup(TG_PrePhysics)
*** RunTickGroup(TG_StartPhysics)
*** RunTickGroup(TG_DuringPhysics, false)
*** RunTickGroup(TG_EndPhysics)
*** RunTickGroup(TG_PostPhysics)
***[#red] Tick TimerManager
***[#red] FTickableGameObject::TickObjects
***[#red] Update cameras and streaming volumes
*** RunTickGroup(TG_PostUpdateWork)
*** RunTickGroup(TG_LastDemotable)
** RedrawAllViewports
@endmindmap
```
## FFrameEndSync::Sync()
???

* [RenderDoc](https://docs.unrealengine.com/4.26/en-US/TestingAndOptimization/PerformanceAndProfiling/RenderDoc/)
* [Unreal Insight](https://docs.unrealengine.com/4.26/en-US/TestingAndOptimization/PerformanceAndProfiling/UnrealInsights/)