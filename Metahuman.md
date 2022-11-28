# Assets
## Face
### Face包含的Curve类型
* RigLogic
* Arkit
* Material
* MorphTarget
### Face_AnimBP
从头body通过CopyPoseFromMesh拷贝动画姿势，处理livelink与身体动画的融合
### Face_PostProcess_AnimBP
应用Neck_CtrlRig
### Neck_CtrlRig
最后交给RigLogic
### mh_arkit_mapping_pose
Arkit curve names ==> RigLogic curve names

## Body

# Modules
* LiveLinkAnimationCore

# Plugins
## mh_arkit_mapping_anim needs
* Live Link
* Live Link Control Rig
* Apple ARKit Face Support
* Groom

# Tools
* https://github.com/endink/Mediapipe4u-plugin
* https://google.github.io/mediapipe/

# Skeleton
Epic skeleton

# Face
# Hair
# Body

# Mocap
https://www.bilibili.com/video/BV1FV4y1x7ZV/?spm_id_from=333.337.search-card.all.click
* [Live Link Face](https://apps.apple.com/us/app/live-link-face/id1495370836)
* [Rokoko](https://www.rokoko.com/)
* [Noitom](https://www.noitom.com.cn/)
* [VDSuit](http://www.vdsuit.com/)
* [IPI](https://ipisoft.com/)
* [Facegood](https://www.avatary.com/home)
# BP
* SetLeaderPoseComponent
* Post Animation Blueprint

# TODO
* [x] Animation Curve
* [x] Morph target
* [ ] Control Rig
* [ ] Pose Asset
* [ ] MH技术规格
* [ ] ARKit原理
* [ ] Body mocap


# References
* [Fix Mouth Gap in Lips](https://www.youtube.com/watch?v=zVDWAom65EI)
* https://www.brasilliant.com/post/what-is-facs-rigging
* https://stefanperales.com/blog/dynamic-ik-retargeting-in-ue5/
* https://cdn2.unrealengine.com/rig-logic-whitepaper-v2-5c9f23f7e210.pdf
