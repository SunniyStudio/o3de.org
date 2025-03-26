---
title: "Shader-Build-Pipeline_-How-To-Use-A-Different-Version-Of-AZSLc"
description: ""
toc: false
---

Whether you need to validate a new AZSLc package, or simply try a different version of AZSLc for your local engine build, there's a way to do so without having to hack the CMake 3rdParty Package Fetch system. Set the registry key:  
`"/O3DE/Atom/AzslCompilerOverridePath"`  
to the absolute path to the AZSLc binary you may want to work with.  
  
Registry keys can be defined within any `*.setreg` file under one of the `Registry/` subfolders of O3DE. For the sake of organization there's already a `setreg` file that consolidates property keys related with the Shader Build Pipeline:  
**o3de/Gems/Atom/Asset/Shader/Registry/atom_shaders.setreg**, with the following default content:  
```json
{
    "O3DE": {
        "Atom": {
            "Shaders": {
                "BuildVariants": true,
                "Build": {
                    "ConfigPath": "@gemroot:AtomShader@/Config"
                }
            }
        }
    }
}
```
As an example let's assume you checkout, modify and compile the latest version of AZSLc from [AZSLc on Github](https://github.com/o3de/o3de-azslc) in the following local directory `/mnt/data/GIT/o3de-azslc`. The Release version of the executable would be located at `/mnt/data/GIT/o3de-azslc/bin/linux/release/azslc`. To use this version of AZSLc, this would be the required changes in the setreg file mentioned above:  
```json
{
    "O3DE": {
        "Atom": {
            "Shaders": {
                "BuildVariants": true,
                "Build": {
                    "ConfigPath": "@gemroot:AtomShader@/Config"
                }
            },
            "AzslCompilerOverridePath": "/mnt/data/GIT/o3de-azslc/bin/linux/release/azslc"
        }
    }
}
```

