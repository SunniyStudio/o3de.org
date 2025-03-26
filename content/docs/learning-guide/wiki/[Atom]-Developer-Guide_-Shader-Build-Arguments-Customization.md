---
title: "[Atom]-Developer-Guide_-Shader-Build-Arguments-Customization"
description: ""
toc: false
---

# [WARNING]
Changing factory settings for shader build arguments should be done sparingly and only for debugging/experimental purposes. Keep reading at your own risk ;-).  

***
 
The shader build arguments are data driven and customizable at the global, Platform, RHI, .shader and .shader.Supervariants levels.

The user can customize build arguments under the following .json property keys:

> `"Definitions"`: Available for **convenience**. Used to pass macro definitions to the C-Preprocessor. Accepts an array of string where each string is of the form: "MACRO", or `"MACRO=VALUE"`. Either way these macro definitions will be passed to the `"AddBuildArguments"."preprocessor"` list as "-DMACRO" or "-DMACRO=VALUE".  
`"AddBuildArguments"` / `"RemoveBuildArguments"`:  
> > `"debug"`: A Boolean, used for convenience as some tools require a combination of arguments to enable debug symbols, etc.  
> > `"preprocessor"`: Arguments for the C-Preprocessor (mcpp.exe).  
> > `"azslc"`: Arguments for AZSLc.  
> > `"dxc"`: Arguments for DirectXShaderCompiler.  
> > `"spirv-cross"`: Arguments for SpirvCross (Metal RHI only).  
> > `"metalair"`: Arguments used for the command `xcrun -sdk <ios/macosx> metal` (Metal RHI only). REMARK: The arguments ["-sdk", "<ios/macosx>", "metal"] must be explicitly specified by the user.  
> > `"metallib"`: Arguments used for the command `xcrun -sdk <ios/macosx> metallib` (Metal RHI only). REMARK: The arguments ["-sdk", "<ios/macosx>", "metallib"] must be explicitly specified by the user.  
  
Atom provides the default, aka factory, arguments in the following directory structure:  
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
  
The factory files, mentioned above, are always loaded first. Subsequently the shader builders will check if the user chose to customize (replace) zero or more files. The value of the registry key `"/O3DE/Atom/Shaders/Build/ConfigPath"` can be customized to the path of a root directory that has the same structure as shown above. From that root directory the shader builders check which files are supposed to be replaced. Examples:

**Example one**: Assume the user only cares to replace the global build arguments, leaving the per-Platform and per-RHI as the factory default. To accomplish this the user creates a directory, let's say: "/my/directory/with/arguments/". The user customizes the registry key `"/O3DE/Atom/Shaders/Build/ConfigPath"` with the value `"/my/directory/with/arguments/"`. The user creates a single `shader_build_options.json` file in the chosen directory:
```
/my/directory/with/arguments/
    shader_build_options.json
```

**Example two**: In this example the user will replace the global build arguments and the vulkan arguments when compiling for Windows. This should be the file structure:
```
/my/directory/with/arguments/
    shader_build_options.json
    Platform/
        Windows/
            vulkan/
                shader_build_options.json
```
**Example three**: In this example the user will replace only the build arguments for dx12 when compiling for Windows:
```
/my/directory/with/arguments/
    Platform/
        Windows/
            dx12/
                shader_build_options.json
```
**Example four**: In this example the user will replace only the build arguments for all RHIs supported for Windows:
```
/my/directory/with/arguments/
    Platform/
        Windows/
            shader_build_options.json
```
  
**In summary**: All factory settings are loaded first, optionally some of the files are replaced if the user chooses to do so.  
  
### Inside shader_build_options.json

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
All properties; `"Definitions"`, `"AddBuildArguments"` and `"RemoveBuildArguments"` are optional; and the sub-properties, which are the array of arguments for each application are also optional. The property `"RemoveBuildArguments"`  is meaningless for the global `shader_build_options.json`. All properties are also accepted in .shader files, and also inside each `"Supervariant"` in the .shader files.

Build arguments work like a stack of arguments, each time a set or arguments is pushed to the stack, a removal is done first using the arguments in  `"RemoveBuildArguments"`, and the arguments in `"AddBuildArguments"`, along with the `"Definitions"` are added next. Here is the order in which arguments are pushed to the stack:

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

See the unit tests in [ShaderBuildArgumentsTests.cpp](https://github.com/o3de/o3de/blob/25a5d7f41d81e5f89bc39c7622fc48ee7cf447e6/Gems/Atom/Asset/Shader/Code/Tests/ShaderBuildArgumentsTests.cpp) for examples.
