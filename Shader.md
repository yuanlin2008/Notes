[toc]

# Shader parameters
## FShaderParamterMetadata
* Shader参数结构体的描述数据
* 通过GetLayout获得对应FRHIUniformBufferLayout
## Macros
### Struct
```cpp
BEGIN_SHADER_PARAMETER_STRUCT(FMyParameterStruct, RENDERER_API)
END_SHADER_PARAMETER_STRUCT()
```
```cpp
// header
BEGIN_UNIFORM_BUFFER_STRUCT(FMyParameterStruct, RENDERER_API)
END_UNIFORM_BUFFER_STRUCT()
// cpp
IMPLEMENT_UNIFORM_BUFFER_STRUCT(FMyParameterStruct, "MyShaderBindingName");
```
* SHADER_PARAMETER与UNIFORM_BUFFER都是用来定义一个结构体，区别是对应的FShaderParamterMetadata定义在哪里。
* SHADER_PARAMETER不需要额外的FShaderParameterMetadata定义，只能用在一个cpp中，放到都文件中会有问题
* UNIFORM_BUFFER通过IMPLEMENT_UNITFORM_BUFFER...定义FShaderParameterMetadata。
* ***UNIFORM_BUFFER可以通过调用StructTypeName::CreateUniformBuffer创建对应的FUniformBufferRHIRef，SHADER_PARAMETER不可以。***

```cpp
IMPLEMENT_UNIFORM_BUFFER_STRUCT(StructTypeName,ShaderVariableName)
```
定义StructTypeName对应的FShaderParameterMetadata，并指定Shader变量名

```cpp
IMPLEMENT_STATIC_UNIFORM_BUFFER_STRUCT_EX(StructTypeName,ShaderVariableName,StaticSlotName,BindingFlagsEnum) 
IMPLEMENT_STATIC_AND_SHADER_UNIFORM_BUFFER_STRUCT(StructTypeName,ShaderVariableName,StaticSlotName)
IMPLEMENT_STATIC_UNIFORM_BUFFER_STRUCT(StructTypeName,ShaderVariableName,StaticSlotName)
```
定义StructTypeName对应的FShaderParameterMetadata，并指定Shader变量名，StaticSlotName和BindingFlagsEnum

```cpp
IMPLEMENT_STATIC_UNIFORM_BUFFER_SLOT(SlotName)
```
定义一个静态槽，配合EUniformBufferBindingFlags::Static

### Parameter

# TODO
* [ ] /Engine/Generated/GeneratedUniformBuffers.ush
* [ ] ShaderCompiler.h cpp

# References
* [***RDG Programming Guide***](https://docs.unrealengine.com/5.1/en-US/render-dependency-graph-in-unreal-engine/)
* [Unreal RenderGraph](https://zhuanlan.zhihu.com/p/101149903)
* [***UE5 Shader***](https://scahp.tistory.com/78)
* [UE4 Shader Introduction](https://logins.github.io/graphics/2021/03/31/UE4ShadersIntroduction.html)
* [Adding a Global Uniform Shader Parameter](https://medium.com/@solaslin/learning-unreal-engine-4-adding-a-global-shader-uniform-1-b6d5500a5161)
* https://alain.xyz/research/realtime-celestial-rendering
* http://www.valentinkraft.de/compute-shader-in-unreal-tutorial/