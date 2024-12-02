---
title: 命令行参数和设置注册表
linkTitle: 命令行参数
description: 了解如何通过命令行参数修改设置，以及如何访问存储在 Open 3D Engine （O3DE） 的“设置注册表”中的命令行参数。
weight: 200
---

您可以在 Open 3D Engine （O3DE） 中使用命令行参数编辑设置注册表。命令行参数存储在 Settings Registry 中。用于存储和检索命令行参数的函数位于`SettingsRegistryMergeUtilities` API 中。

## 使用`--regset`覆盖设置

最常见的是，命令行参数用于设置注册表中的值。任何设置注册表值都可以通过指定 JSON 指针路径和要使用`--regset`选项设置的新值来覆盖。在以下示例中，当启动 **O3DE 编辑器** 时，项目路径将被覆盖并设置为 `AutomatedTesting`：

```bash
${CMAKE_BINARY_DIR}/bin/profile/Editor.exe --regset="/Amazon/AzCore/Bootstrap/project_path=AutomatedTesting"
```

## 命令行支持

您可以使用以下几个命令行选项通过 JSON 指针路径查询和修改设置注册表。此功能在通过`AZ::ComponentApplication`创建全局设置注册表的任何应用程序中都可用。

以下应用程序支持“设置注册表”命令行选项：

* **MaterialEditor**
* **AssetProcessor**
* **AssetProcessorBatch**
* **AssetBuilder**
* **AssetBundler**
* **Editor**
* **${project}.GameLauncher**
* **${project}.ServerLauncher**
* **SerializeContextTools**
* **ShaderManagementConsole**
* **PythonBindingsExample**

Settings Registry 支持以下命令行选项。
2
|命令行选项 |描述 |
| --- | --- |
| `--regset="<JSON pointer path>=<value>"` | 在 Settings Registry 的指定 JSON 指针路径处设置一个值。 `--regset` 按从左到右的顺序与其他 `--regset` 和 `--regremove` 选项内联计算。 |
| `--regremove="<JSON pointer path>"` | 从设置注册表中指定的 JSON 指针路径处删除一个值。 `--regremove` 按从左到右的顺序计算，与其他`--regremove` 和 `--regset`选项内联。 |
| `--regdump="<JSON pointer path>"` | 以递归方式将 Settings Registry 中的值从指定的 JSON 指针路径转储到 stdout。 |
| `--regdumpall` | 将整个 JSON 文档从 Settings Registry 转储到 stdout。这相当于传入命令行`--regdump=""`。 |
| `--regset-file="<file-path>::<JSON anchor path>"` | 将 JSON 格式的文件合并到 Settings Registry 中。通过省略 JSON 锚点路径，可以在根空 string（“”） 键下合并文件 (for example, `--regset-file="Registry/bootstrap.setreg"`)。（可选）可以在文件路径之后提供 [JSON 指针路径](https://datatracker.ietf.org/doc/html/rfc6901#section-5)，用两个冒号(`::`)分隔。锚点路径表示锚定设置的 JSON 对象。<br><br>**例如:** 在 “/O3DE/Bootstrap” 键下合并 JSON 文件<br>`--regset-file="Registry/bootstrap.setregpatch::/O3DE/Bootstrap"`<br>如果文件路径以 `.setregpatch` 结尾，则使用 JSON Patch 合并文件，否则使用 JSON Merge Patch 算法。<br><br>{{< note >}}此命令支持合并任何 JSON 格式的文件。文件扩展名仅在确定是否将在 `.setregpatch` 情况下使用 JSON Patch 算法时才重要。{{< /note >}} {{< note >}}使用文件路径 `-` 将允许从 stdin 读取 JSON 格式的内容。<br><br>**例如:** `C:\path\to\o3de>echo '{"Test": {} }' | Editor.exe --regset-file=-::/O3DE/Settings`<br>将  `{ "O3DE": {"Settings": {"Tests": {}}} }` 的 JSON 内容合并到设置注册表中。{{< /note >}} ||

### 命令行评估

`--regset`, `--regset-file` 和 `--regremove` 选项按从左到右的顺序进行评估。
当命令行中多次提供相同的选项或同时删除和设置设置时，这一点很重要。

|命令行选项 |评估结果 |
| --- | --- |
| `Editor.exe --regset="/My/Setting/value=false" --regset="/My/Setting/value=true"`| “/My/Setting/value” 字段将设置为`true` ，因为最后一个选项获胜。 |
| `Editor.exe --regset="/My/Setting/value=false" --regremove="/My/Setting/value"`| 设置注册表将没有 “/My/Setting/value” 字段，因为`--regremove`是最后一个选项。 |
| `Editor.exe --regremove="/My/Setting/value" --regset="/My/Setting/value=true"`| 设置注册表将有一个 “/My/Setting/value” 字段，该字段被设置为`true` ，因为 `--regset`是最后一个选项。 |
| `Editor.exe --regdump="/My/Setting" --regdump="/Your/Setting"`| 输出 “/My/Setting” 字段和 “/Your/Setting” 字段的值。 |


## 命令行存储

启动应用程序的命令行被解析为 *optional* 和 *positional* 参数。然后，它们将作为 `ComponentApplication` 初始化的一部分存储在 Settings Registry 中。

可选参数具有与之关联的选项名称。在可选参数 `--project-path /path/to/project/root` 中，选项是`project-path` ，值是`/path/to/project/root`。如果多个值与单个选项关联，则这些值将存储在数组中。可选参数存储在“/O3DE/Runtime/CommandLine/Switches”JSON 指针路径中。

位置参数不包含选项名称（或键），并根据它们在命令行中的位置进行解析。位置参数存储在“/O3DE/Runtime/CommandLine/MiscValues”JSON 指针路径中。

## `SettingsRegistryMergeUtilities` API

`SettingsRegistryMergeUtilities` 提供了 `StoreCommandLineToRegistry` 和 `GetCommandLineFromRegistry`函数。这些函数可用于缓存和检索提供给 O3DE 应用程序的命令行参数。

```c++
//! Stores the command line settings into the Setting Registry
//! The arguments can be used later anywhere the command line is needed
void StoreCommandLineToRegistry(SettingsRegistryInterface& registry, const AZ::CommandLine& commandLine);

//! Query the command line settings from the Setting Registry and stores them
//! into the AZ::CommandLine instance
bool GetCommandLineFromRegistry(SettingsRegistryInterface& registry, AZ::CommandLine& commandLine);
```

请参阅: [SettingsRegistryMergeUtils.h](https://github.com/o3de/o3de/blob/development/Code/Framework/AzCore/AzCore/Settings/SettingsRegistryMergeUtils.h#L265-L271)
