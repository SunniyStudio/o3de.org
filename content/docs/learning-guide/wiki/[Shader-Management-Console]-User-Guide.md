---
title: "[Shader-Management-Console]-User-Guide"
description: ""
toc: false
---

Shader Management Console is an editor for `.shadervariantlist` file, on Windows, located at 

`{Project folder}\{Build folder}\bin\profile\ShaderManagementConsole.exe`. 

You can check the content of a `.shadervariantlist` by double-clicking it in AssetBrowser, shader option values can be changed by clicking on each value in the table. Save your changes though `File > Save` or simply Ctrl-S.

When double-clicking on other file types, the default text editor is used to open them.

![image](https://user-images.githubusercontent.com/4178768/227850580-61281538-cca8-4b90-a630-3f549ed7e417.png)

Other than editing `.shadervariantlist`, you can extend SMC functionality with python scripts, some examples are in `Gems\Atom\Tools\ShaderManagementConsole\Scripts`, explained below:


# Generate shader variant list from single `.shader`
The function is using the script `GenerateShaderVariantListForMaterials.py`, which already has path setup in `setreg`, so we can use it directly by right-clicking on any `.shader` file, `Python Scripts > GenerateShaderVariantListForMaterials.py`.

The script search for all materials making use of this shader, then extract shader variants from them and generate the table.

When saving the table a window will pop up, asking about saving location, default is `Save to project`. Going to `{Project folder}\ShaderVariants`.
If `Save to engine` is selected, it will go to the same path as the selected `.shader` file.

![image](https://user-images.githubusercontent.com/4178768/227851738-8ae2bb34-cd9a-43b3-b0fd-9446e9f90835.png)

Note that you can modify the output of the script by providing a custom `.systemoptions` file. This is for changing and enumerating system option values, which are not accessible from materials alone.

To do that, create a `ShaderName.systemoptions` with the same path of the shader, the file should be a json looking like this
```
{
     "o_option1": "true",
     "o_option2": "2",
     "o_option3": "Modulate::None",
     "o_option4": ""     // value unset
}
```
If the value is set, it will replace the value retrieved from material; if it is unset, it will be automatically expanded with all the possible combinations.

Note that there currently is a duplicated mechanism in C++ and python for this function, because we can't get `ShaderOptionDescriptor` in the script if there is no material using the shader, so we have to handle this case in C++.


# Generate and save all shader variant list for all material
The function is using the script `GenerateShaderVariantListForAllShaders.py`, since it is not operating on one single shader, we can run it using `File > Python Scripts > Run Python Script...` and select the script on disk.

The script searches for all built material asset, and generate the shader variant list for all shaders being used. The lists are automatically saved in `{Project folder}\ShaderVariants`.

Note that this script is not yet using the `.systemoptions` feature.


# Shader variant statistic
This is an analysis feature to give an idea about how much impact a shader option has in the system.

You can run it by `File > Generate Shader Variant Statistic...`

![image](https://user-images.githubusercontent.com/4178768/227864840-20a57e3f-46db-4b59-91c3-61ab87e5971b.png)

One row represents one shader variant, the row title value is the number of shader items using this variant.

The number next to the shader option value is the number of shader items with this option value.

By right-clicking on each column you can see a context menu `See materials using option_name`, which will show you all the materials using this option, note the number is different from the table value, because there are multiple shader items in one material.

Note this feature is not done in python script, mostly because of the issue on `AZ::RPI::ShaderVariantId`
* It can not do key indexing in python dictionary, so we have to loop through the dictionary and compare the key instead of doing `dict[key]`
* AZStd::unordered_map will report errors when using `AZ::RPI::ShaderVariantId` as key








