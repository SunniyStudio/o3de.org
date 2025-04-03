---
title: "[Atom] 开发人员指南 着色器构建参数自定义"
description: ""
toc: false
---

# [WARNING]
应谨慎更改着色器构建参数的出厂设置，并且仅用于调试/实验目的。继续阅读，风险自负 ;-).  

***
 
着色器构建参数是数据驱动的，可在全局、Platform、RHI、.shader 和 .shader 上进行自定义。超变体级别。

用户可以在以下 .json 属性键下自定义构建参数：

> `"Definitions"`: Available for **convenience**. Used to pass macro definitions to the C-Preprocessor. Accepts an array of string where each string is of the form: "MACRO", or `"MACRO=VALUE"`. Either way these macro definitions will be passed to the `"AddBuildArguments"."preprocessor"` list as "-DMACRO" or "-DMACRO=VALUE".  
`"AddBuildArguments"` / `"RemoveBuildArguments"`:  
> > `"debug"`: A Boolean, used for convenience as some tools require a combination of arguments to enable debug symbols, etc.  
> > `"preprocessor"`: Arguments for the C-Preprocessor (mcpp.exe).  
> > `"azslc"`: Arguments for AZSLc.  
> > `"dxc"`: Arguments for DirectXShaderCompiler.  
> > `"spirv-cross"`: Arguments for SpirvCross (Metal RHI only).  
> > `"metalair"`: Arguments used for the command `xcrun -sdk <ios/macosx> metal` (Metal RHI only). REMARK: The arguments ["-sdk", "<ios/macosx>", "metal"] must be explicitly specified by the user.  
> > `"metallib"`: Arguments used for the command `xcrun -sdk <ios/macosx> metallib` (Metal RHI only). REMARK: The arguments ["-sdk", "<ios/macosx>", "metallib"] must be explicitly specified by the user.  
  
Atom 在以下目录结构中提供默认的 （aka factory） 参数：
> @gemroot:AtomShader@/Config/ (Which usually resolves to /o3de/Gems/Atom/Asset/Shader/Config/)  
> > shader_build_options.json  
> > Platform/  
> > > Android/  
> > > > shader_build_options.json  

> > > iOS/  
> > > > shader_build_options.json  

> > > Linux/  
> > > > shader_build_options.json  

> > > Mac/  
> > > > shader_build_options.json  

> > > Windows/  
> > > > shader_build_options.json  
> > > > dx12/  
> > > > > shader_build_options.json  

> > > > vulkan/  
> > > > > shader_build_options.json  
  
上面提到的工厂文件总是首先加载。随后，着色器生成器将检查用户是否选择自定义（替换）零个或多个文件。注册表项  `"/O3DE/Atom/Shaders/Build/ConfigPath"` 的值可以自定义为具有与上述相同结构的根目录的路径。着色器生成器从该根目录中检查哪些文件应该被替换。例子：

**示例 1**：假设用户只关心替换全局构建参数，将每个平台和每个 RHI 保留为出厂默认值。为了实现这一点，用户创建了一个目录，比如：`"/my/directory/with/arguments/"`。用户自定义注册表项 `"/O3DE/Atom/Shaders/Build/ConfigPath"` 的值为 `"/my/directory/with/arguments/"`。用户在所选目录中创建一个 `shader_build_options.json` 文件：
```
/my/directory/with/arguments/
    shader_build_options.json
```

**示例 2**: 在此示例中，用户在为 Windows 编译时将替换全局 build 参数和 vulkan 参数。这应该是文件结构：
```
/my/directory/with/arguments/
    shader_build_options.json
    Platform/
        Windows/
            vulkan/
                shader_build_options.json
```
**示例 3**: 在此示例中，用户在为 Windows 编译时将仅替换 dx12 的构建参数：
```
/my/directory/with/arguments/
    Platform/
        Windows/
            dx12/
                shader_build_options.json
```
**示例 4**: 在此示例中，用户将仅替换 Windows 支持的所有 RHI 的构建参数：
```
/my/directory/with/arguments/
    Platform/
        Windows/
            shader_build_options.json
```
  
**总结**: 首先加载所有出厂设置，如果用户选择替换某些文件，也可以选择替换某些文件。
  
### 在 shader_build_options.json 中

```json
{
    "Definitions": [], // array of strings
    "AddBuildArguments": {
        "debug": false,
        "preprocessor": [], // array of strings
        "azslc": [], // array of strings
        "dxc": [], // array of strings
        "spirv-cross": [], // array of strings
        "metalair": [], // array of strings
        "metallib": [] // array of strings
    },
    "RemoveBuildArguments": {
        "debug": false,
        "preprocessor": [], // array of strings
        "azslc": [], // array of strings
        "dxc": [], // array of strings
        "spirv-cross": [], // array of strings
        "metalair": [], // array of strings
        "metallib": [] // array of strings
    },
}
```
所有属性; `"Definitions"`, `"AddBuildArguments"` 和 `"RemoveBuildArguments"` 是可选的;和 sub-properties（每个应用程序的参数数组）也是可选的。属性 `"RemoveBuildArguments"` 对于全局 `shader_build_options.json` 没有意义。所有属性也包含在 .shader 文件中，以及 .shader 文件中的每个`"Supervariant"`中。

构建参数的工作方式类似于一堆参数，每次将一组或参数推送到堆栈时，首先使用 `"RemoveBuildArguments"` 中的参数进行删除，然后添加 `"AddBuildArguments"` 中的参数以及`"Definitions"`。以下是参数被推送到堆栈的顺序：

    Push "" (global) arguments.
        Push "<Platform>" arguments. First remove arguments in "RemoveBuildArguments", second add arguments in "AddBuildArguments" & "Definitions".
            Push "<rhi" arguments. First remove arguments in "RemoveBuildArguments", second add arguments in "AddBuildArguments"& "Definitions".
                Push .shader arguments. First remove arguments in "RemoveBuildArguments", second add arguments in "AddBuildArguments"& "Definitions".
                    Push supervariant arguments. First remove arguments in "RemoveBuildArguments", second add arguments in "AddBuildArguments"& "Definitions".
                    Pop (supervariants).
                Pop (shader arguments).
            Pop (rhi arguments).
        Pop (Platform arguments).
    Back at global arguments. Any Pop() at this level is a no-op because the global arguments are never removed from the top of the stack.

查看 [ShaderBuildArgumentsTests.cpp](https://github.com/o3de/o3de/blob/25a5d7f41d81e5f89bc39c7622fc48ee7cf447e6/Gems/Atom/Asset/Shader/Code/Tests/ShaderBuildArgumentsTests.cpp) 中的单元测试。
