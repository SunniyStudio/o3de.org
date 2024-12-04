---
linkTitle: 数据类
title: 数据类
description: 描述项目管理器中维护的各种数据的类
weight: 200
---

{{< note >}}
本信息面向**项目管理器**工具的开发人员。如果您是使用项目管理器的用户，请参阅[项目管理器用户指南](/docs/user-guide/project-config/project-manager)。
{{< /note >}}

{{< note >}}
其中包含各种类的功能和作用的简要概述。如果有，目录中还会提供其他链接，包括扩展信息或 github 链接。所有文档反映的代码截至提交 [(b79bd3df1f)](https://github.com/o3de/o3de/tree/b79bd3df1fe5d4c2a639d3921a29bd0d95712f6c) 
{{< /note >}}




## 概述

| 类                     | 源代码                                                                                                                                                                                                             | 说明                                                                            |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| TemplateInfo          | [头文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/TemplateInfo.h)  [cpp 文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/TemplateInfo.cpp) | 描述封装通用模板的数据字段。                                                                |
| ProjectTemplateInfo   | [头文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/ProjectTemplateInfo.h)                                                                                                     | 项目专用模板的子类。唯一不同的是，该模板包含Gem。                                                    |
| SettingsInterface     | [头文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/SettingsInterface.h)                                                                                                       | 与 O3DE 设置注册表交互的界面。                                                            |
| [Settings](#settings) | [头文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/Settings.h)  [cpp 文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/Settings.cpp)         | 实现与 O3DE 设置注册表交互的`SettingsInterface`。                                         |
| ProjectInfo           | [头文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/ProjectInfo.h)  [cpp 文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/ProjectInfo.cpp)   | 包含描述项目的数据结构，记录在 `ProjectManager` 内的 project.json 中。还包含远程项目的元数据，或用于检查项目是否需要重建。 |
| EngineInfo            | [头文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/EngineInfo.h)  [cpp 文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/EngineInfo.cpp)     | 包含描述当前引擎的数据结构，记录在 `ProjectManager` 内的 engine.json 中。还包含引擎注册的元数据。              |
| ProjectManagerDefs    | [头文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/ProjectManagerDefs.h)                                                                                                      | 包含各种常量和预设字符串。                                                                 |






### 设置

我们指定`SettingsInterface`作为一个契约，以确保我们实现了适当的方法，并能从一个简洁的接口（[一个示例](https://github.com/o3de/o3de/blob/b79bd3df1fe5d4c2a639d3921a29bd0d95712f6c/Code/Tools/ProjectManager/Source/ExternalLinkDialog.cpp#L88))对设置进行全局访问。它还是访问项目管理器设置的单例接口。

`SettingsInterface` 指定以下功能作为与 O3DE 设置注册表交互的合约：

| 函数 | 说明 | 实现 |
| - | - | - |
| [`SettingsInterface::Get`](https://github.com/o3de/o3de/blob/69dbcd08a56539315bfb0472984daf0f46e7a966/Code/Tools/ProjectManager/Source/SettingsInterface.h#L31-L44) | 使用 `QString` 键，可以返回字符串或 bool 值。 | [代码](https://github.com/o3de/o3de/blob/7d716a4a21afd217444d91043ea810f6c8a38f21/Code/Tools/ProjectManager/Source/Settings.cpp#L65-L79) |
| [`SettingsInterface::Set`](https://github.com/o3de/o3de/blob/69dbcd08a56539315bfb0472984daf0f46e7a966/Code/Tools/ProjectManager/Source/SettingsInterface.h#L46-L59) | 使用 `QString` 键，可以为条目设置字符串或 bool 值。 | [代码](https://github.com/o3de/o3de/blob/7d716a4a21afd217444d91043ea810f6c8a38f21/Code/Tools/ProjectManager/Source/Settings.cpp#L81-L99) |
| [`SettingsInterface::Remove`](https://github.com/o3de/o3de/blob/69dbcd08a56539315bfb0472984daf0f46e7a966/Code/Tools/ProjectManager/Source/SettingsInterface.h#L61-L66) | 从注册表中删除带有 `QString` 键的条目。 | [代码](https://github.com/o3de/o3de/blob/7d716a4a21afd217444d91043ea810f6c8a38f21/Code/Tools/ProjectManager/Source/Settings.cpp#L101-L109) |
| [`SettingsInterface::Copy`](https://github.com/o3de/o3de/blob/69dbcd08a56539315bfb0472984daf0f46e7a966/Code/Tools/ProjectManager/Source/SettingsInterface.h#L68-L75) | 将数值从一个键复制到另一个键。也可以选择删除原始条目，起到 “移动 ”命令的作用。 | [代码](https://github.com/o3de/o3de/blob/7d716a4a21afd217444d91043ea810f6c8a38f21/Code/Tools/ProjectManager/Source/Settings.cpp#L111-L132) |
| [`SettingsInterface::GetProjectKey`](https://github.com/o3de/o3de/blob/69dbcd08a56539315bfb0472984daf0f46e7a966/Code/Tools/ProjectManager/Source/SettingsInterface.h#L77-L82) | 给定一个 `ProjectInfo` 实例，为该项目在设置注册表中的位置提供键前缀。 | [代码](https://github.com/o3de/o3de/blob/7d716a4a21afd217444d91043ea810f6c8a38f21/Code/Tools/ProjectManager/Source/Settings.cpp#L134-L137) |
| [`SettingsInterface::GetProjectBuiltSuccessfully`](https://github.com/o3de/o3de/blob/69dbcd08a56539315bfb0472984daf0f46e7a966/Code/Tools/ProjectManager/Source/SettingsInterface.h#L84-L90) | 查询给定项目的最新构建状态（成功与否）。| [代码](https://github.com/o3de/o3de/blob/7d716a4a21afd217444d91043ea810f6c8a38f21/Code/Tools/ProjectManager/Source/Settings.cpp#L144-L163) |
| [`SettingsInterface::SetProjectBuiltSuccessfully`](https://github.com/o3de/o3de/blob/69dbcd08a56539315bfb0472984daf0f46e7a966/Code/Tools/ProjectManager/Source/SettingsInterface.h#L91-L97) | 设置给定项目的最新构建状态（成功与否）。 | [代码](https://github.com/o3de/o3de/blob/7d716a4a21afd217444d91043ea810f6c8a38f21/Code/Tools/ProjectManager/Source/Settings.cpp#L165-L187) |

所有这些函数都是纯虚函数，需要在基本实现类中实现，否则会导致编译错误，从而强制执行契约。

[`Settings`](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/Settings.h) 类[继承](https://github.com/o3de/o3de/blob/69dbcd08a56539315bfb0472984daf0f46e7a966/Code/Tools/ProjectManager/Source/Settings.h#L23-L24)`SettingsInterface`，它在引擎盖下使用 [`AZ::SettingsRegistryInterface`](https://github.com/o3de/o3de/blob/b79bd3df1fe5d4c2a639d3921a29bd0d95712f6c/Code/Tools/ProjectManager/Source/Settings.h#L48) 与 O3DE 设置注册表交互。

这与 JSON 类似，因此可以通过键路径检索值。例如，以 .setreg 为例：
```json
{
    "key":{
        "subkey":{
            "subsubkey":"value"
        }
    }
}
```
然后可使用字符串键 `key/subkey/subsubkey` 检索该值。

`ProjectManager::SettingsInterface` 与 `AZ::SettingsRegistryInterface`类似，只是它覆盖了一些行为，以查找 Project Manager 的特定配置，而不是整个 O3DE。此外，它还能与 `QString` 配合使用，而 `AZ` 的对应程序却不能处理这种情况。

`Settings` 类继承自`SettingsInterface::Registrar`。 [`Registrar`](https://github.com/o3de/o3de/blob/b79bd3df1fe5d4c2a639d3921a29bd0d95712f6c/Code/Framework/AzCore/AzCore/Interface/Interface.h#L100-L106) 是`AzCore`中`AZ::Interface`的一部分。

`Registrar` 的[实现](https://github.com/o3de/o3de/blob/b79bd3df1fe5d4c2a639d3921a29bd0d95712f6c/Code/Framework/AzCore/AzCore/Interface/Interface.h#L202-L212)在全局范围内注册接口，这表明了单例模式。

以下是 `Settings` 也提供的其余功能：

| 函数 | 说明 |
| - | - |
| [`Settings::Save`](https://github.com/o3de/o3de/blob/69dbcd08a56539315bfb0472984daf0f46e7a966/Code/Tools/ProjectManager/Source/Settings.cpp#L25-L55) | 将设置注册表转为字符串流。然后尝试打开 O3DE 用户路径中的 `ProjectManager.setreg` 文件，并将流数据写入该文件。 |
| [`Settings::GetBuiltSuccessfullyPaths`](https://github.com/o3de/o3de/blob/69dbcd08a56539315bfb0472984daf0f46e7a966/Code/Tools/ProjectManager/Source/Settings.cpp#L139-L142) | 针对请求的每个项目路径，获取构建状态。结果以集合形式返回。 |

[返回概述](#overview)
