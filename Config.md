## ConfigRedirects.ini
section和key的redirect.

## DataDrivenPlatformInfo.ini
ini查找路径
* Engine/Config/[Platform]/DataDrivenPlatformInfo.ini (FPaths::EngineConfigDir())
* Engine/Platforms/[Platform]/DataDrivenPlatformInfo.ini (FPaths::EnginePlatformExtensionsDir())
* Project/Platforms/[Platform]/Config/DataPlatformInfo.ini (FPaths::ProjectPlatformExtensionsDir())

|Section|C++|
|-|-|
|DataDrivenPlatfromInfo|FDataDrivenPlatformInfo|
|PreviewPlatform|FPreviewPlatformMenuItem|
|ShaderPlatform|FGenericDataDrivenShaderPlatformInfo|

### FDataDrivenPlatformInfoRegistry
保存每个platform对应的FDataDrivenPlatformInfo和所有的FPreviewPlatformMenuItem

* [ ] FConfigCacheIni::AsyncInitializeConfigForPlatforms()