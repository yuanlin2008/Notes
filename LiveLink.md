```puml
!theme black-knight

interface IModularFeature
interface IModuleInterface

package LiveLinkInterface{
	interface ILiveLinkClient
	class ULiveLinkRole
	class ULiveLinkBaseRole
	class ULiveLinkAnimationRole
	class ULiveLinkTransformRole
	class ULiveLinkCameraRole
	class ULiveLinkLightRole
	ULiveLinkRole<|-- ULiveLinkBaseRole
	ULiveLinkBaseRole<|-- ULiveLinkAnimationRole
	ULiveLinkBaseRole<|-- ULiveLinkTransformRole
	ULiveLinkTransformRole<|-- ULiveLinkCameraRole
	ULiveLinkTransformRole<|-- ULiveLinkLightRole
	class FLiveLinkSubjectFrameData{
		+FLiveLinkStaticDataStruct StaticData
		+FLiveLinkFrameDataStruct FrameData
	}
}
package LiveLink {
	interface ILiveLinkModule
	class FLiveLinkModule
	class FLiveLinkClient
	ILiveLinkModule<|--FLiveLinkModule
}
package LiveLinkAnimationCore {
	class FAnimNode_LiveLinkPose
	class ULiveLinkRetargetAsset
	class UARLiveLinkRetargetAsset
	class ULiveLinkRemapAsset
	ULiveLinkRetargetAsset<|-- UARLiveLinkRetargetAsset:ARKit
	ULiveLinkRetargetAsset<|-- ULiveLinkRemapAsset
}

FLiveLinkModule*..FLiveLinkClient
IModularFeature<|--ILiveLinkClient
IModuleInterface<|--ILiveLinkModule
ILiveLinkClient<|--FLiveLinkClient
LiveLinkInterface....*LiveLinkAnimationCore

```
* 应用LiveLink的逻辑主要集中在FAnimNode_LiveLinkPose中
# Refenrences
* https://docs.unrealengine.com/4.26/en-US/AnimatingObjects/SkeletalMeshAnimation/LiveLinkPlugin/LiveLinkPluginDevelopment/