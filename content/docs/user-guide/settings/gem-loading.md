---
linkTitle: Gem 加载和 Settings Registry
title: 设置注册表 Gem 加载
description: 了解如何使用 Settings Registry 加载和配置 Gem。
weight: 900
---

设置注册表与 CMake 链接，以便于根据项目中的活动 Gem 集自动加载 *Gem 模块*。 当为项目配置 CMake 时，它会生成一组`cmake_dependencies.<project-name>.<target>.setreg`文件，用于列出活动的 Gem 的名称。它包含任何共享的 Gem 模块文件名，这些文件名将 Gem 共享库加载到与 CMake 目标关联的 O3DE 应用程序中。


## 加载 Gem

CMake 用于输出标记的`.setreg`文件列表(`cmake_dependencies.<target>.setreg`)，其中包含通过 O3DE 模块系统加载的 CMake Target 的构建依赖项。
这用于根据 CMake Target 的构建依赖关系自动加载 Gem。本节演示如何生成包含 Gem 构建依赖项列表的“.setreg”文件。

以下是 *假设* 体素编辑器功能的 `CMakeLists.txt`：

**CMakeLists.txt (Voxel Editor)**

```cmake
# Associate the VoxelEditor CMake target with the ".Tools" variant of Gems to load.
# This will generate a "cmake_dependencies.voxeleditor.setreg" file that contains the path to the shared
# libraries that will need to be loaded by the VoxelEditor, as well as the list paths to the Gem source directory
ly_set_gem_variant_to_load(TARGETS VoxelEditor VARIANTS Tools)

# Adds the VoxelEditor target as a C preprocessor define so that it can be used as a Settings Registry
# specialization to look up the generated .setreg, which contains the dependencies
# specified for the target.
set_source_files_properties(
    Source/VoxelEditorApplication.cpp
    PROPERTIES
        COMPILE_DEFINITIONS LY_CMAKE_TARGET="VoxelEditor"
)
```

运行 CMake 命令会生成一个解决方案和一个`cmake_dependencies.voxeleditor.setreg`。它递归浏览 Gem 的`RUNTIME_DEPENDENCIES`列表，并查找`GEM_MODULE` 目标属性以确定所需 Gem 的列表。

例如，以下是 Atom Bridge Gem 在 CMake 中的配置方式：

**CMakeLists.txt (AtomBridge)**

```cmake
if(PAL_TRAIT_BUILD_HOST_TOOLS)
    ly_add_target(
        NAME Atom_AtomBridge.Editor ${PAL_TRAIT_MONOLITHIC_DRIVEN_MODULE_TYPE}
        NAMESPACE Gem
        ...
        RUNTIME_DEPENDENCIES
            Gem::Atom_RHI.Private
            Gem::Atom_RHI_DX12.Private
            Gem::Atom_RHI_DX12.Builders
            Gem::Atom_Asset_Shader.Builders
            Gem::ImageProcessingAtom.Editor
            Gem::ImguiAtom
            Gem::AtomToolsFramework.Editor
    )


    # Any 'tool' and 'builder' type applications should use Gem::Atom_AtomBridge.Editor:
    ly_create_alias(NAME Atom_AtomBridge.Builders NAMESPACE Gem TARGETS Gem::Atom_AtomBridge.Editor)
    ly_create_alias(NAME Atom_AtomBridge.Tools    NAMESPACE Gem TARGETS Gem::Atom_AtomBridge.Editor)
endif()
```

为体素编辑器配置 CMake 会在可执行目录`Registry`目录中生成以下`.setreg` 文件：

**cmake_dependencies.voxeleditor.setreg (Voxel Editor)**
```json
{
    "O3DE": {
        "Gems": {
            "Atom_AtomBridge": {
                "Targets": {
                    "Atom_AtomBridge.Editor": {
                        "Modules":["Atom_AtomBridge.Editor.dll"]
                    },
                    "Atom_AtomBridge.Builders": {
                    }
                }
            },
            "AtomShader": {
                "Targets": {
                    "Atom_Asset_Shader.Builders": {
                        "Modules":["Atom_Asset_Shader.Builders.dll"]
                    }
                }
            },
            "ImageProcessingAtom": {
                "Targets": {
                    "ImageProcessingAtom.Editor": {
                        "Modules":["ImageProcessingAtom.Editor.dll"]
                    }
                }
            },
            "AtomToolsFramework": {
                "Targets": {
                    "AtomToolsFramework.Editor": {
                        "Modules":["AtomToolsFramework.Editor.dll"]
                    }
                }
            },
            "Atom_RHI_DX12": {
                "Targets": {
                    "Atom_RHI_DX12.Builders": {
                        "Modules":["Atom_RHI_DX12.Builders.dll"]
                    },
                    "Atom_RHI_DX12.Private": {
                        "Modules":["Atom_RHI_DX12.Private.dll"]
                    }
                }
            }
        }
    }
}
```

然后，添加用于加载`cmake_dependencies.voxeleditor.setreg`文件的“设置注册表”专用化。这是通过 `SettingsRegistryMergeUtils::MergeSettingsToRegistry_AddBuildSystemTargetSpecialization` 函数在 C++ 代码中完成的，如以下示例所示：

```c++
    //! This function returns the build system target name
    AZStd::string_view GetBuildTargetName()
    {
#if !defined (LY_CMAKE_TARGET)
#error "LY_CMAKE_TARGET must be defined in order to add this source file to a CMake executable target"
#endif
        return AZStd::string_view{ LY_CMAKE_TARGET };
    }

    VoxelEditorApplication::VoxelEditorApplication(int* argc, char*** argv)
        : Application(argc, argv)
        , QApplication(*argc, *argv)
    {
        // The Settings Registry has been created at this point, so add the CMake target "voxeleditor"
        // as a specialization into the Settings Registry
        AZ::SettingsRegistryMergeUtils::MergeSettingsToRegistry_AddBuildSystemTargetSpecialization(
            *AZ::SettingsRegistry::Get(), GetBuildTargetName());
    }
```

`AZ::ComponentApplication`使用存储在 Settings Registry 中的 Gem 模块文件名从文件系统加载 Gem。它还会合并每个 Gem 的`<GemRootPath>/Registry/*.setreg` 文件。基于游戏项目的 CMake 目标也可以使用 `ly_enable_gems` 函数生成 `.setreg` 文件。

AtomTest 项目 Gem 使用 `ly_enable_gems`函数生成`cmake_dependencies.atomtest.<target>.setreg`文件。该文件包含要加载的共享库文件路径和 Gem 源目录的列表，如以下示例所示：

**CMakeLists.txt (AtomTest)**

```cmake
ly_add_target(
    NAME AtomTest ${PAL_TRAIT_MONOLITHIC_DRIVEN_MODULE_TYPE}
    NAMESPACE Gem
    FILES_CMAKE
        atomtest_files.cmake
    INCLUDE_DIRECTORIES
        PUBLIC
            Include
    BUILD_DEPENDENCIES
        PRIVATE
            AZ::AzGameFramework
            Gem::Atom_LyIntegration.Static
)

# if enabled, AtomTest Gem is used by the Client and Server Launchers as well as Tools
# But it isn't needed in Builders
ly_create_alias(NAME AtomTest.Clients NAMESPACE Gem TARGETS Gem::AtomTest)
ly_create_alias(NAME AtomTest.Servers NAMESPACE Gem TARGETS Gem::AtomTest)
ly_create_alias(NAME AtomTest.Tools   NAMESPACE Gem TARGETS Gem::AtomTest)

################################################################################
# Gem dependencies
################################################################################
# The GameLauncher uses "Clients" Gem variants:
ly_enable_gems(PROJECT_NAME AtomTest GEM_FILE enabled_gems.cmake)
```

`ly_enable_gems` 函数为 AtomTest 项目添加构建和加载依赖项。

在 CMake 生成期间，设置注册表文件基于`ly_enable_gems` 函数在 `<CMakeBuildDir>/bin/$<CONFIG>`目录中生成。例如，在仅启用 AtomTest 项目的情况下为 Windows 配置 CMake 会生成以下`.setreg`文件：

```
<CMakeBuildDir>\bin\profile\Registry\cmake_dependencies.atomtest.editor.setreg
<CMakeBuildDir>\bin\profile\Registry\cmake_dependencies.atomtest.assetprocessor.setreg
<CMakeBuildDir>\bin\profile\Registry\cmake_dependencies.atomtest.atomtest_gamelauncher.setreg
...
```

生成的项目 `.setreg` 文件的格式为`cmake_dependencies.<ProjectNameLower>.<CMakeTargetNameLower>.setreg`。项目名称是生成的 `cmake_dependencies.*.setreg`文件名的一部分，因为 O3DE 允许一次配置多个项目。每个游戏项目都使用相同的应用程序，例如 O3DE Editor 和 Asset Processor，但应用程序需要根据活动游戏项目加载一组不同的 Gem，因此项目名称将作为 CMake 构建依赖项设置注册表文件的一部分添加。

例如，如果 CMake 配置了`-DLY_PROJECTS="./AutomatedTesting;D:/o3de/AtomSampleViewer"`，则会生成以下`.setreg` 文件：

```
D:\o3de\o3de\windows_vs2019\bin\profile\Registry\cmake_dependencies.atomsampleviewer.assetbuilder.setreg
D:\o3de\o3de\windows_vs2019\bin\profile\Registry\cmake_dependencies.atomsampleviewer.assetprocessor.setreg
D:\o3de\o3de\windows_vs2019\bin\profile\Registry\cmake_dependencies.atomsampleviewer.assetprocessorbatch.setreg
D:\o3de\o3de\windows_vs2019\bin\profile\Registry\cmake_dependencies.atomsampleviewer.atomsampleviewer_gamelauncher.setreg
D:\o3de\o3de\windows_vs2019\bin\profile\Registry\cmake_dependencies.atomsampleviewer.editor.setreg

D:\o3de\o3de\windows_vs2019\bin\profile\Registry\cmake_dependencies.automatedtesting.assetbuilder.setreg
D:\o3de\o3de\windows_vs2019\bin\profile\Registry\cmake_dependencies.automatedtesting.assetprocessor.setreg
D:\o3de\o3de\windows_vs2019\bin\profile\Registry\cmake_dependencies.automatedtesting.assetprocessorbatch.setreg
D:\o3de\o3de\windows_vs2019\bin\profile\Registry\cmake_dependencies.automatedtesting.automatedtesting_gamelauncher.setreg
D:\o3de\o3de\windows_vs2019\bin\profile\Registry\cmake_dependencies.automatedtesting.editor.setreg
```

### `<EngineRoot>/Gems`外的Gem

Gem 根目录路径列表由 CMake 在为平台生成构建文件时填充。由于 CMake 知道每个 Gem 的 `CMakeLists.txt` 位置，因此它能够生成一个 `.setreg` 文件，其中包含每个 CMake 目标的 Gem 列表，该目标设置要加载的 “Gem 变体”。这是通过使用 `ly_set_gem_variant_to_load` 命令完成的。此列表包括 Gem 的文件名和基于配置期间提供给 CMake 的源目录的 Gem 目录的相对路径。

这样做的好处是，如果使用 [`cmake add_subdirectory`](https://cmake.org/cmake/help/v3.24/command/add_subdirectory.html) 命令在 `<EngineRoot>` 位置之外添加 Gem，则设置注册表可以加载`<GemRoot>/Registry`目录中的任何 `.setreg` 文件。

这可用于在 O3DE `<EngineRoot>` 目录之外包含特定 Gem，如以下示例所示：

```cmake
add_subdirectory(<AbsolutePathToMoleculeGem> <AbsolutePathToMoleculeGem>) # This doesn't have to be in the O3DE engine root
add_subdirectory(../<RelativePathToElectronGem> ../<RelativePathToElectronGem>)
```

### 特定于平台的 Gem 加载

通过使用平台抽象层 （PAL） 路径，为给定变体多次调用 `ly_enable_gems` 函数，可以基于每个平台构建和加载 Gem。
CMake 支持多个平台抽象变量，这些变量可用于包含基于当前平台（Windows、Linux、Android 等）的特定已启用 Gem。

以下示例演示了如何同时指定常规和特定于平台的 Gem 依赖项：

```cmake
o3de_pal_dir(pal_dir ${CMAKE_CURRENT_LIST_DIR}/Platform/${PAL_PLATFORM_NAME} "${gem_restricted_path}" "${gem_path}" "${gem_parent_relative_path}")
# Read a platform-specific CMake file that contains the names of Gems to activate
ly_enable_gems(PROJECT_NAME AtomTest GEM_FILE ${pal_dir}/enabled_gems.cmake)
```

### 显式 Gem 激活

如 [加载 Gem](#loading-gems) 中所述，Gem 构建由`enabled_gems.cmake`文件中的 Gem 列表决定。不应手动修改此文件。相反，请使用 `o3de.py`, `enable-gem`, 和 `disable-gem` 命令或 **Project Manager** 来添加或删除 Gem。本节介绍如何启用和禁用用于构建的 Gem，以及如何使用 Settings Registry 关闭 Gem 的自动加载。

以下示例演示了如何使用`o3de.py` Python 脚本命令在 AutomatedTesting 项目中为名为“Sponza”的 Gem 添加和删除显式 Gem 激活：

```bash
# The following command adds explicit activation of the Sponza Gem within the AutomatedTesting project
# It will modify the enabled_gems.cmake file and conditionally the project.json file if the Gem being activated is registered with the global o3de_manifest.json
[engine-root]> scripts\o3de.bat enable-gem --gem-name Sponza --project-path AutomatedTesting
# The following command removes explicit activation of the Sponza Gem within the AutomatedTesting project
# It will modify the enabled_gems.cmake file and conditionally the project.json file if the Gem being activated is registered with the global o3de_manifest.json
[engine-root]> scripts\o3de.bat disable-gem --gem-name Sponza --project-path AutomatedTesting
```

### 禁用 Gem 自动加载

在 CMake 项目生成步骤中，将生成一个包含要加载的 Gem 列表的`cmake_dependencies.*.setreg` 文件。为防止自动加载特定 Gem，请使用路径 `"/O3DE/Gems/${GemName}/${TargetModule}/AutoLoad"` 以 JSON 指针格式为 Gem 设置 JSON 布尔值 `false`。例如，您可以在`"<project-root>/Registry/"`目录中添加一个 `.setreg` 文件，用于设置 `"/O3DE/Gems/${GemName}/${TargetModule}/AutoLoad=false"`  值。

以下是生成的`cmake_dependencies.automatedtesting.assetproccessor.setreg`的代码片段，该代码是在为自动测试项目配置 O3DE 时生成的 （`-DLY_PROJECTS=AutomatedTesting`）。

**cmake_dependencies.automatedtesting.assetproccessor.setreg**

```json

{
    "O3DE": {
        "Gems": {
            "ChatPlay": {
                "ChatPlay": {
                    "Modules": ["ChatPlay.dll"]
                }
            },
            "QtForPython": {
                "QtForPython.Editor": {
                    "Modules": ["QtForPython.Editor.dll"]
                }
            },
            //...
        }
    }
}
```

通过将`.setreg`文件放置在 `<project_root>/User/Registry`（每个项目覆盖）或`~/.o3de/Registry`全局用户覆盖中，可以禁用 ChatPlay 客户端和 QtForPython 工具模块在*每个用户*的基础上自动加载，如以下示例所示：

**gem_autoload.setreg**

```json
{
    "O3DE": {
        "Gems": {
            "ChatPlay": {
                "ChatPlay": {
                    "AutoLoad": false
                }
            },
            "QtForPython": {
                "QtForPython.Editor": {
                    "AutoLoad": false
                }
            }
        }
    }
}
```

要在项目级别禁用 Gem 自动加载，可以将 `.setreg` 文件（如前面的示例）放置在项目的`<project_root>/Registry`目录`Registry`中。

要在 *平台* 级别禁用 Gem 自动加载，可以将 `.setreg` 文件（如前面的示例）放在 `<O3DE root>/Registry/Platform/${PAL_PLATFORM_NAME}` 目录中。

### 在 C++ 中加载 Gem

如果需要，您可以通过 C++ 在应用程序中手动加载 Gem。`SettingsRegistryMergeUtils.cpp`包含一个函数[MergeSettingsToRegistry_TargetBuildDependencyRegistry](https://github.com/o3de/o3de/blob/02846cf44347cbf4fae0faacc4a2ba74284908ff/Code/Framework/AzCore/AzCore/Settings/SettingsRegistryMergeUtils.h#L216-L220)，该函数加载  `cmake_dependencies.<tag1>.<tag2>.setreg`文件，其中包含要加载的 Gem 列表。Gem 是根据 “specialization” 标签结构中的值加载的。Gem 模块列表存储在 Setting Registry 中。

```c++
void MergeSettingsToRegistry_TargetBuildDependencyRegistry(SettingsRegistryInterface& registry, const AZStd::string_view platform,
    const SettingsRegistryInterface::Specializations& specializations, AZStd::vector<char>* scratchBuffer);
```

`ComponentApplication.cpp`负责通过 [LoadDynamicModule](https://github.com/o3de/o3de/blob/02846cf44347cbf4fae0faacc4a2ba74284908ff/Code/Framework/AzCore/AzCore/Component/ComponentApplication.h#L297-L299) 函数加载所需的 Gem，该函数读取`/O3DE/Gems/${GemName}/Modules`路径中所有数组键的设置注册表，并将它们聚合到要加载的 Gem 列表中。

```c++
void ComponentApplication::LoadDynamicModules()
{
    // Queries the Settings Registry to get the list of Gem modules to load
    struct GemModuleLoadData
    {
        AZ::OSString m_gemName;
        AZStd::vector<AZ::OSString> m_dynamicLibraryPaths;
        bool m_autoLoad{ true };
    };
    // ...
}
```

## 添加 Gems 扫描文件夹

Settings Registry 支持配置 Asset Processor 的设置。`AssetProcessorPlatformConfig.setreg`文件可用作可用设置的参考： [AssetProcessorPlatformConfig.setreg](https://github.com/o3de/o3de/blob/development/Registry/AssetProcessorPlatformConfig.setreg).

### Gem 资产扫描文件夹

要为活动 Gem 添加其他扫描文件夹，`.setreg`文件可以在<name>“/Amazon/AssetProcessor/Settings”字段下添加“ScanFolder”。“\<name>” 部分可以是任何内容，只要它不与其他扫描文件夹条目冲突即可。

以下示例将`<Blast Gem Root>/Editor/Scripts`文件夹添加为 Asset Processor 的 Scan 文件夹：

```json
{
    "Amazon": {
        "AssetProcessor": {
            "Settings": {
                "ScanFolder Blast/Scripts": {
                    "watch": "@GEMROOT:Blast@/Editor/Scripts",
                    "display": "Blast/Scripts",
                    "recursive": 1,
                    "order": 101
                }
            }
        }
    }
}
```
