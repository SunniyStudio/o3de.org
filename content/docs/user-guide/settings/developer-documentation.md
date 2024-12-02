---
linkTitle: 开发人员指南
title: 设置注册表开发人员指南
description: 介绍 O3DE 的 Settings Registry 功能以及开发人员如何与之交互。
weight: 700
---

*设置注册表* 是所有 **Open 3D Engine （O3DE** 应用程序的全局设置的中央存储库。可以使用 [JSON 指针语法](https://tools.ietf.org/html/rfc6901)定义、查询和更改设置。设置注册表可以读取任何具有`.setreg`扩展名的有效 JSON 文件。

设置注册表通过使用 [JSON Patch](https://tools.ietf.org/html/rfc6902) 格式或 [JSON Merge Patch](https://tools.ietf.org/html/rfc7386) 格式合并值，将多个 JSON 值合并到一个文档中。这允许通过加载使用 JSON 合并补丁策略合并的`.setreg`文件或使用 JSON 补丁策略合并的`.setregpatch`文件来覆盖设置注册表中的值。

可以将设置注册表视为 JSON DOM 的包装器，该包装器可以使用 JSON 指针查询和设置该 DOM 中的值。还可以通过访问 JSON 文档中的路径来查询值，该路径可用于从 JSON DOM 构造对象，而无需反射。

## JSON Patch和 JSON Merge Patch

有两种格式可用于修改设置注册表：[JSON Patch](https://tools.ietf.org/html/rfc6902) 和 [JSON Merge Patch](https://tools.ietf.org/html/rfc7386)。

*JSON Patch* 是一种格式，用于描述要在特定 JSON 路径上对 JSON 值执行的一组操作。

*JSON Merge Patch* 是一种格式，它允许指定 JSON 文档，该文档主要用于通过使用简单算法合并对象来修改 JSON 对象。与 JSON Patch 不同，它不指定在 JSON 文档中编码的一组操作。它充当更改补丁，以应用于特定路径中的 JSON 值。JSON Merge Patch 不明确允许将键值设置为 “null”，也不允许删除 JSON 数组或向 JSON 数组添加值。必须替换整个阵列。

### `.setreg` 示例

`.setreg` 文件在合并到设置注册表时使用 JSON 合并修补程序算法。格式为 JSON。

**~/.o3de/Registry/sample.setreg**

```json
{
    "Amazon": {
        "AzCore": {
            "Bootstrap": {
                "project_path": "D:/o3de/AtomSampleViewer"
            }
        }
    }
}
```

### `.setregpatch` 示例

`.setregpatch`文件在合并到设置注册表时使用 JSON 补丁算法。格式为 JSON，其中 JSON 数组的元素描述对 JSON 文档的一组 patch 操作。

**~/.o3de/Registry/sample.setregpatch**

```json
[
    { "op": "replace", "path": "/Amazon/AzCore/Bootstrap/project_path", "value": "D:/o3de/AtomSampleViewer" },
    { "op": "add", "path": "/Amazon/AzCore/Bootstrap/engine_path", "value": "D:/o3de/o3de" },
    { "op": "add", "path": "/Amazon/AzCore/Bootstrap/bin_directories/-", "value": "D:/o3de/AtomSampleViewer/AppendToEndOfJsonArray" },
    { "op": "copy", "from": "/Amazon/AzCore/Bootstrap/bin_directories/0", "path": "Amazon/AzCore/Bootstrap/default_bin_directory" },
    { "op": "move", "from": "/Amazon/AzCore/Bootstrap/windows_assets", "path": "/Amazon/AzCore/Bootstrap/assets" },
    { "op": "remove", "path": "/Amazon/AzCore/Bootstrap/bin_directories/1" }
]
```

## 开始使用 Settings Registry

每个 O3DE 应用程序都使用`AZ::ComponentApplication` 类，该类为应用程序创建全局设置注册表。使用设置注册表的最简单方法是通过其`Get` 和 `Set` API（`AZ::SettingsRegistry::Get` 和 `AZ::SettingsRegistry::Set`）。 `Get` 和 `Set`函数支持查询和修改`bool`, `integer`, `double`, 和 `string`类型。

### 使用 `Get` 函数

```c++
auto settingsRegistry = AZ::SettingsRegistry::Get();
AZ::SettingsRegistryInterface::FixedValueString projectName;

if (settingsRegistry && settingsRegistry->Get(projectName, "/Amazon/AzCore/Bootstrap/project_path"))
{
// Do stuff with current Project Path
}
```

### 使用 `Set` 函数

```c++
auto settingsRegistry = AZ::SettingsRegistry::Get();
if (settingsRegistry && settingsRegistry->Set("/Amazon/AzCore/Bootstrap/project_path", "AutomatedTesting"))
{
// Successfully set the Project Path, do other stuff
}
```

## Create a Settings Registry

创建 `AZ::ComponentApplication` 或从它派生的类会创建一个全局设置注册表。创建`AZ::ComponentApplication`的实例时，`AZ::SettingsRegistryImpl`实例将注册到 `AZ::Interface`。它可以通过名称`AZ::SettingsRegistry`访问。为清楚起见，创建全局设置注册表所需的 `AZ::ComponentApplication` 的默认构造就是全部。注册单例`AZ::SettingsRegistryInterface`时，不需要调用`AZ::ComponentApplication::Create()`函数。

要创建可由 Gem 或库在本地使用的空设置注册表，请创建`AZ::SettingsRegistryImpl`类的实例。

### 填充设置注册表

设置将按以下顺序合并到 Settings 注册表中：

1. 首先，扫描全局用户 `~/.o3de/Registry` 目录，并将其内容合并到 Settings Registry 中。在合并所有设置注册表路径（引擎、Gem 和项目）后，全局用户`Registry`目录也会再次合并，以确保用户设置覆盖除命令行设置之外的所有其他设置。
2. 接下来，读取命令行上指定的任何`--regset`/`--regremove` 选项并将其合并到设置注册表中。
3. 然后，`ComponentApplication` 请求设置注册表根据引导设置填充运行时文件路径。运行时文件路径都位于“/O3DE/Runtime/FilePaths”的 JSON 路径下。

以下实用程序函数有助于检索重要路径和工程名称：

* `AZ::Utils::GetEnginePath()` - 查询引擎根的绝对路径。
* `AZ::Utils::GetProjectPath()` - 查询项目根目录的绝对路径。
* `AZ::Utils::GetProjectName()` - 查询 `<ProjectRoot>/project.json` 文件中指定的项目名称。
* `AZ::Utils::GetExecutableDirectory()` - 查询包含可执行文件的目录的绝对路径。
* `AZ::Utils::GetO3deManifestDirectory()` - 查询用户 home 内的绝对路径 .o3de 目录： `~/.o3de`.
* `AZ::Utils::GetGemPath(gemName)` - 查询 gem.json “gem_name” 条目匹配的 Gem 的绝对路径 .to。

### 搜索位置

`ComponentApplication` 全局设置注册表尝试按以下顺序加载设置注册表文件：

1. 首先，扫描`~/.o3de/Registry`目录以查找全局用户设置。它位于用户的主目录中。默认情况下，这在 Windows 上是`C:\Users\<user>\.o3de\Registry`，在 Linux 上是 `/home/<user>/.o3de/Registry`，在 Mac 上是`/Users/<user>/.o3de/Registry`。
2. 命令行选项已合并。命令行允许通过以下方法在设置注册表中设置值：
   * `--regset` 通过将值指定为`<path>=<value>`来切换。
   * `--regset-file` 开关允许将`.setreg`文件 （`--regset-file=<file>`) 或 `stdin` (`--regset-file=-`)  合并到设置注册表中。
   * `--regremove` 切换允许删除注册表中的键。
    {{< note >}}
        可以使用`--regdump`开关提供的 JSON 指针路径将注册表转储到 `stdout`。整个设置注册表可以使用 `--regdumpall` 开关转储到  `stdout`。
    {{< /note >}}
3. `<EngineRoot>/Registry`目录中搜索设置。
4. 每个Gem的`<GemRoot>/Registry`目录中搜索设置。
5. 项目路径中的注册表目录，`<ProjectRoot>/Registry`中搜索设置。
6. 位于 `~/.o3de/Registry` 的全局用户设置注册表将再次合并，以确保全局用户设置覆盖任何插件、Gem 或项目设置。
7. 项目特定的用户目录 `<ProjectRoot>/user/Registry`被合并。此目录不应位于源代码控制中，因为它是特定于用户的。
8. 最后，通过检查`--regset`, `--regset-file`, 和 `--regremove`选项来覆盖任何其他设置，再次合并命令行。

### 启动器搜索位置

项目的 GameLauncher 和 ServerLauncher 应用程序在项目的根缓存目录`<ProjectRoot>/Cache/<platform>`中有一个额外的搜索位置，用于查找 **Asset Processor ** 生成的聚合`bootstrap.*.setreg` 文件。

### 设置注册表生成器

Asset Processor 包含一个生成器，用于将存储在文件和命令行中的所有设置聚合到一个`bootstrap.game.<config>.setreg`文件。这些设置按以下顺序加载，并合并到一个本地设置注册表中，然后序列化为`bootstrap.game.\*.setreg`文件。

* `<engine-root>/Registry`
* `<gem-root>/Registry` 对于 Asset Processor 中加载的每个 Gem
* `<project-root>/Registry`
* (Non-Release Only) `<user-home>/.o3de/Registry`
* (Non-Release Only) `<project-root>/user/Registry`
* (Non-Release Only) `--regset`/`--regremove` 提供给 Asset Processor 的命令行参数

例如，在 Android 上，以下文件将输出到项目的资产缓存`Android`目录：

* `bootstrap.game.debug.setreg`
* `bootstrap.game.profile.setreg`
* `bootstrap.game.release.setreg`

当应用程序以非整体模式加载时，将在`<ExeDirectory>/Registry`中搜索包含要加载的 Gem 列表的`cmake_dependencies.*.setreg`。对于非主机平台（如 Android 或 iOS），其中文件是根据特定布局部署的，还会搜索`<ProjectCachePlatformRoot>/Registry`。这将在下面的 [加载 Gem](./gem-loading#loading-gems) 中更详细地解释。

{{< note >}}
如果在 Asset Processor Settings Registry Builder 更新`bootstrap.game.<config>.setreg`文件，则在下次运行 Launcher 应用程序之前，最新设置可能不可用。
{{< /note >}}

### `<EngineRoot>/Gems`外的Gem

Gem 根目录路径列表由 CMake 在为平台生成构建文件时填充。因为 CMake 知道每个 Gem 的 `CMakeLists.txt` 位置，所以它可以生成一个 `.setreg` 文件，其中包含每个 CMake 目标的 Gem 列表，该目标设置要加载的 “Gem 变体”。这是通过使用 `ly_set_gem_variant_to_load` 命令完成的。此列表包括 Gem 的文件名和基于配置期间提供给 CMake 的源目录的 Gem 目录的相对路径。

好处是，如果使用[`cmake add_subdirectory`](https://cmake.org/cmake/help/v3.18/command/add_subdirectory.html)  命令在`<EngineRoot>`位置之外添加 Gem，则设置注册表可以加载 `<GemRoot>/Registry` 目录中的任何 `.setreg`文件。

这可用于在 O3DE `<EngineRoot>` 目录之外包含特定 Gem，如以下示例所示：

```cmake
add_subdirectory(<AbsolutePathToMoleculeGem> <AbsolutePathToMoleculeGem>) # This doesn't have to be in the O3DE engine root
add_subdirectory(../<RelativePathToElectronGem> ../<RelativePathToElectronGem>)
```

### Settings 注册表项层次结构

由于设置注册表是从一个或多个 JSON 文档构建的，因此来自两个单独的“.setreg”文件的 JSON 值的路径可能具有根于同一路径的键。因此，始终建议在设置注册表中的任何键前加上前缀，特别是“/O3DE”下的键（如果它是特定于引擎的设置）。此外，还建议将项目、Gem 或库名称附加到键中，以减少键之间发生冲突的可能性（例如，“/O3DE/AzCore/Key1”、“/O3DE/Gems/EMotionFX/Key1”）。

而不是像以下示例这样的设置层次结构：

```json
{
    "useExistingAllocator": false,
    "grabAllMemory": false
}
```

更好的方法是将设置根植于层次结构（如“/O3DE/AzCore”），如以下示例所示：

```json
{
    "O3DE": {
        "AzCore": {
            "useExistingAllocator": false,
            "grabAllMemory": false
        }
    }
}
```

此方法有助于避免与来自其他库或组织的 JSON 密钥发生冲突。

## Settings Registry API

为设置注册表提供了多个 API，用于查询、修改和合并设置键和值。 这些 API 在 [设置注册表开发人员 API](./developer-api).

|主题 |描述 |
| - | - |
| [Settings Registry Developer API](./developer-api) | 了解如何使用 C++ API 与设置注册表交互。|

## Setting Registry specializations

特化是一组标签，用于过滤将从特定目录合并的 `.setreg` 和 `.setregpatch` 文件。这些标签是  `*.setreg`文件可以包含的文件名的一部分。并非标记集中的所有标记都需要作为文件名的一部分进行匹配，但文件名中与 specializations 对象中的标记不匹配的任何部分都将导致匹配失败。以下是`.setreg`文件的示例规范：

`<filename stem>.<tag1>.<tag2>...<tagN>.setreg`

名称为`bootstrap.game.windows.profile.setreg`的设置注册表文件的文件，文件名为 `bootstrap`，标签为 `game`, `windows`, 和 `profile`。 要使用`AZ::SettingsRegistry::MergeSettingsFolder`函数合并文件，需要带有 `game`, `windows`, 和 `profile`标签的 specializations 对象。 specializations 对象可以具有除列出的标记以外的其他标记。 只要它包含 `game`, `windows`, 和 `profile`标签，`bootstrap.game.windows.profile.setreg`文件就会被合并。

specializations 对象可以被视为 `.<tag>` 值，这些值可以是 Settings Registry 文件名的一部分。

以下示例使用 specialization 标记合并`<ExecutableDirectory>/Registry`目录中的`.setreg`文件：

```c++
SettingsRegistryInterface::Specializations specializations{
     "automatedtesting",
     "automatedtesting_gamelauncher",
     "randomtag"
};
AZ::SettingsRegistryInterface* registry = AZ::SettingsRegistry::Get();
registry->MergeSettingsFolder(AZ::Utils::GetExecutableDirectory() / "Registry", specializations, AZ_TRAIT_OS_PLATFORM_CODENAME);
```

下表详细说明了将使用前面的示例专用化合并哪些类型的文件：

| `<ExeDirectory>/Registry`中的文件  | 被`MergeSettingsFolder`合并 |
| --- | --- |
| `cmake_dependencies.automatedtesting.automatedtesting_gamelauncher.setreg` | **Yes** - 都包含`automatedtesting` 和 `automatedtesting_gamelauncher` tags |
| `cmake_dependencies.automatedtesting.setreg` | **Yes** - 包含 `automatedtesting` tag |
| `cmake_dependencies.automatedtesting_gamelauncher.setreg` | **Yes** - 包含 `automatedtesting_gamelauncher` tag |
| `cmake_dependencies.automatedtesting.automatedtesting_gamelauncher.setregpatch` | **Yes** - 都包含 `automatedtesting` 和 `automatedtesting_gamelauncher` tags, 和 `.setregpatch` 文件也可以合并 |
| `cmake_dependencies.automatedtesting.automatedtesting_gamelauncher` | **No** - 缺少 `.setreg` 或 `.setregpatch` 扩展名 |
| `automatedtesting.cmake_dependencies.setreg` | **No** - `cmake_dependencies` 不是标签集的一部分 |
| `cmake_dependencies.setreg` | **Yes** - 不包含专用化标签|
| `automatedtesting.setreg` | **Yes** - “automatedtesting” 是文件名的词干，而不是标签。 |

## 合并设置文件

使用专用化系统的另一种方法是使用`AZ::SettingsRegistryInterface::MergeSettingsFile`函数合并每个文件，如下所示：

.使用`AZ::IO::SystemFile::FindFiles`搜索`.setreg` 和 `.setregpatch`文件。要收集特定平台的文件，请通过将`"Platform" / AZ_TRAIT_OS_PLATFORM_CODENAME`的目录附加到正在搜索的目录（例如 `<ExeDirectory>/Registry/Platform/Windows`）来再次调用`AZ::IO::SystemFile::FindFiles`。
1. 将每个文件传递到 `AZ::SettingsRegistryInterface::MergeSettingsFile`。

## 合并平台抽象层 （PAL） 目录

`AZ::SettingsRegistry::MergeSettingsFolder`允许用户提供可选的平台名称，以指示用于搜索`.setreg` 和 `.setregpatch`文件的操作系统平台目录。这允许通过提供`platform` 参数来合并特定于平台的设置。如果不需要特定于平台的设置，则应将此参数设置为空字符串。要合并当前操作系统的特定于平台的设置，请使用 [AZ_TRAIT_OS_PLATFORM_CODENAME](https://github.com/o3de/o3de/blob/37b1216015567fb7faa49fe7e3f6f7a73379e06a/Code/Framework/AzCore/Platform/Linux/AzCore/AzCore_Traits_Linux.h#L41) 定义，该定义将扩展到当前平台。

## 从命令行添加专用化

Settings Registry 支持存储要在注册表本身中使用的特定化。可以将`/Amazon/AzCore/Settings/Specialization/<TagName>`键设置为 true，以添加<TagName>从设置注册表合并文件时要使用的专业化。这可以通过`--regset`命令行选项来完成。

以下示例使用自定义专用化启动 **O3DE Editor**：

```bash
Editor.exe --project-path=<Path-To-Project> --regset="/Amazon/AzCore/Settings/Specialization/custom=true"
```

{{< note >}}
Settings Registry 中的专用化不区分大小写。
{{< /note >}}

## 深入的专业化系统

专用化标记系统可用于按专用化标记筛选`.setreg` 和 `.setregpatch`文件。设置注册表 `MergeSettingsFolder` 函数接受 `specializations` 参数，该参数根据位于合并目录中的文件名中找到的所有标签进行检查。如果文件名包含未作为 specialization 参数的一部分提供的标记，则会将其从要合并的 Settings Registry 文件列表中删除。

请考虑以下合并硬件设置的示例：

例如，给定一组专用化标记`mobile` 和 `core_count_16`，以及包含以下设置注册表文件的目录：

* `hardware_settings.core_count_16.mobile.setreg`
* `hardware_settings.mobile.setreg`
* `hardware_settings.pc.setreg`
* `hardware_settings.core_count_16.pc.setreg`
* `hardware_settings.core_count_4.mobile.setreg`
* `hardware_settings.core_count_16.setreg`
* `Platform/Android/hardware_settings.mobile.setreg`
* `a_hardware_settings.core_count_16.mobile.setreg`

根据 [SettingsRegistryImpl::ExtractFileDescription](https://github.com/o3de/o3de/blob/02846cf44347cbf4fae0faacc4a2ba74284908ff/Code/Framework/AzCore/AzCore/Settings/SettingsRegistryImpl.h#L113)函数中文件名中的每个专用化来检查专用化标记。文件名中作为  `*.setreg` 或 `*.setregpatch` 文件中的专用部分是第一个句点之后和文件扩展名之前的部分。

例如， `<filename stem>.<tag1>.<tag2>.<tag3>.setreg` 是具有三个标签的 Settings Registry 文件。

`.setreg`文件的每个标记都必须与提供给专用化结构的标记匹配，该标记将传递到 [SettingsRegistryInterface::MergeSettingsFolder](https://github.com/o3de/o3de/blob/02846cf44347cbf4fae0faacc4a2ba74284908ff/Code/Framework/AzCore/AzCore/Settings/SettingsRegistry.h#L357-L375) 函数。

在这种情况下，硬件设置将从以下文件中合并，因为 specializations 参数包含  `core_count_16` 和 `mobile`。

* `hardware_settings.core_count_16.mobile.setreg`
* `hardware_settings.mobile.setreg`
* `hardware_settings.core_count_16.setreg`
* `Platform/Android/hardware_settings.mobile.setreg`
* `a_hardware_settings.core_count_16.mobile.setreg`

## 合并顺序

[SettingsRegistryImpl::IsLessThan](https://github.com/o3de/o3de/blob/02846cf44347cbf4fae0faacc4a2ba74284908ff/Code/Framework/AzCore/AzCore/Settings/SettingsRegistryImpl.h#L110-L112) 函数用于根据以下规则对 Settings Registry 文件列表进行排序：

1. `<filename stem>.<tag1>.<tag2>...<tagN>.setreg` 文件根据文件的 `<filename>` 部分的字典顺序进行排序。
2. 接下来，具有相同 `<filename>` 的文件将按标签数量升序排序。
3. 接下来，对于具有相同 `<filename>` 和相同数量的标记的文件，将根据 Specialization 结构中标记的顺序对文件进行排序。在前面的示例中，由于 `core_count_16` 标记是在 `mobile`之前添加的，因此 `core_count_16` 标记在 `mobile`  标记之前排序。
4. 最后，对于具有相同`<filename>`标签、编号和顺序的文件，如果其中一个文件位于平台抽象层目录中，例如 `Registry/Platform/Android/hardware_settings.mobile.setreg`，则特定于平台的文件将排在非特定于平台的文件之后。

下表说明了示例文件的合并顺序：

|设置 注册表文件加载顺序 |原因 |
| --- | --- |
| `a_hardware_settings.core_count_16.mobile.setreg` | 文件名 `a_hardware_settings` 在`hardware_settings` 之前排序。|
| `hardware_settings.core_count_16.setreg` | 文件名 `hardware_settings` 与下一个条目匹配并包含相同数量的标签，但 `core_count_16` 这个条目标签是在 `mobile` 之前添加的。 |
| `hardware_settings.mobile.setreg` | `mobile`标记已添加到 specialization 数组中 `core_count_16`标记之后。 |
| `Platform/Android/hardware_settings.mobile.setreg` | 特定于 Android 平台的`hardware_settings.mobile.setreg`版本在非平台版本之后加载。 |
| `hardware_settings.core_count_16.mobile.setreg` | 文件名 `hardware_settings`与前四个条目匹配，但此条目包含两个 specialization 标签，而不是前几个条目中的一个。 |

## 命令行支持

您可以使用多个命令行选项来查询、创建、修改`.setreg`文件并将其合并到 Settings Registry。
命令行选项在 [命令行参数](./command-line-arguments)中详细说明。

| 主题 |描述 |
| - | - |
| [命令行参数](./command-line-arguments) | 了解如何通过命令行修改 Settings Registry。 |


## 转换 `.ini`文件为 JSON

为了帮助转换 Windows 风格的`.ini`设置以便在设置注册表中使用，`SerializeContextTools` 公开了一个 `convert-ini` 选项，它可以接受一个或多个`.ini`样式的文件，并将它们转换为`.setreg`文件，然后可以加载到设置注册表中。以下示例演示了将`AssetProcessorPlatformConfig.ini`和`user.cfg`转换为`.setreg`文件的过程。

**convert-ini**

```bash
<CMakeBinaryDir>/bin/profile/SerializeContextTools.exe convert-ini --files AssetProcessorPlatformConfig.ini;user.cfg --ext setreg --json-prefix="/O3DE/Settings"
```

## MSVC 调试

设置注册表将其设置维护在`rapidjson::Document`中，该文档是`AZ::SettingsRegistryImpl`中的成员变量。

rapidjson 3rdParty 库为 MSVC 调试器提供本机可视化工具 (`.natvis`) 文件，使其可以通过`rapidjson::Value` 和 `rapidjson::Document`实例轻松递归。此`.natvis` 文件是`AzCore`的一部分，可以在 Visual Studio 中可视化。展开`AZ::SettingsRegistryImpl::m_settings` 可提供 Settings Registry 中值的视图。这允许向下钻取到 Settings Registry 中的特定 JSON 条目，以确定条目及其值是否存在。
