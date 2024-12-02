---
title: 使用控制台命令访问设置注册表
linktitle: 控制台命令
description: 了解如何在 Open 3D Engine （O3DE） 中使用 AZ 控制台命令访问设置注册表。
weight: 400
---

设置注册表向 [AZ::Console](/docs/user-guide/programming/az-console/) 注册命令列表，这些命令允许您在设置注册表中添加、更新或删除密钥，并将密钥转储到控制台输出窗口。

## 控制台命令列表

设置注册表控制台命令具有前缀`sr_`，是 _Settings Registry_的缩写。

| <div style="width:150px">命令</div> | 说明 |
| :-- | :-- |
| `sr_regset` \<key> \<value> | 使用 value 参数在 Settings Registry 的指定键项处添加或修改值。可以指定多个键和值。 |
| `sr_regremove` \<key> | 从 Settings Registry 中删除每个键及其值。可以指定多个键。 |
| `sr_regdump` \<key> | 将 Settings Registry （设置注册表） 中的每个键值转储到控制台输出窗口。如果指定了多个键，则它们的值用换行符分隔。 |
| `sr_regdumpall` | 将整个 Settings Registry 转储到控制台输出窗口。 |
| `sr_regset_file` \<file path> \<anchor path> | 在 Settings Registry 的指定锚点路径处合并 Settings Registry 文件。锚点路径参数将加载的设置合并到特定设置对象下，例如，`"/O3DE/Settings"`. |
| `sr_dump_origin` \<key> | 输出负责 Settings Registry 中每个键的最新修改的文件路径。 如果上次在 C++ 或 Script 中更新了键，则输出值 “\<in-memory>”。 可以指定多个键。  |

## 示例

### 使用 `sr_regset` 更新或添加值

以下 Console 命令示例更新 Settings Registry 中的项目根路径。

| 命令 | 结果 |
| --- | --- |
| `sr_regset /Amazon/AzCore/Bootstrap/project_path TestProject` | 此示例将“/Amazon/AzCore/Bootstrap”JSON 对象下的“project_path”字段设置为“TestProject”。 |
| `sr_regset /Amazon/AzCore/Bootstrap/project_path ProjectPath With Spaces` | 此示例将“/Amazon/AzCore/Bootstrap”JSON 对象下的“project_path”字段设置为“带空格的 ProjectPath”。 |

![](/images/user-guide/settings/update-setting.png)

### 使用 `sr_regremove` 删除键值

以下 Console 命令示例从 Settings Registry 中删除键及其值。

| 命令 | 结果 |
| --- | --- |
| `sr_regremove /Amazon/AzCore/Bootstrap/project_path` | 这将删除“/Amazon/AzCore/Bootstrap”JSON 对象下的“project_path”字段。 |
| `sr_regremove /Your/Custom/Setting1 /My/Custom/Setting2` | 这将删除“/Your/Custom”JSON 对象下的“Setting1”字段和“/My/Custom”JSON 对象下的“Setting2”字段。 |

![](/images/user-guide/settings/remove-setting.png)

### 使用 `sr_regdump` 命令输出值

以下 Console 命令示例将指定键的值输出到 Console。

| 命令 | 结果 |
| --- | --- |
| `sr_regdump /Amazon/AzCore/Bootstrap/project_path` | 这会将当前 “project\_path” 输出到 Console。 |
| `sr_regdump /Your/Custom/Setting1 /My/Custom/Setting2` | 这会将“/Your/Custom”JSON 对象下的“Setting1”字段和“/My/Custom”JSON 对象下的“Setting2”字段输出到控制台。|

![](/images/user-guide/settings/dump-setting.png)

### 使用 `sr_regdumpall` 命令输出整个设置注册表

以下 Console 命令将整个 Settings Registry 输出到 Console。

| 命令 | 结果 |
| --- | --- |
| `sr_regdumpall` | 这会将完整的 Settings Registry 输出到 Console。|

![](/images/user-guide/settings/dump-all-settings.png)

### 使用 `sr_dump_origin` 命令输出上次修改键的 filepath

以下 Console 命令将负责对每个键进行最新修改的文件路径输出到 Console。 代码块展示了一个按顺序合并 2 个 JSON 文件并查询文件来源 `"/Your/Custom/Setting1"` 的示例。

**file1.setreg**
```json
{
    "Your": { "Custom": { "Setting1": "initialValue"} }
}
```

**file2.setreg**
```json
{
    "Your": { "Custom": { "Setting1": "overrideValue"} }
}
```

| 命令 | 结果 |
| --- |------------------------------------------------------------------|
| `sr_dump_origin /Your/Custom/Setting1` | 这会将修改 “/Your/Custom/Setting1” 键的最后一个文件输出到控制台。 返回值为`file2.setreg` |

![](/images/user-guide/settings/dump-file-origin.png)

### 使用 `sr_regset_file` 命令合并 Settings Registry 文件

以下 Console 命令将 Settings Registry 文件从 Console 合并到 Settings Registry 中。该文件必须采用 JSON 格式。该代码块显示了一个名为`engine.json`的示例 JSON 文件，其中包含`engine_name`键的示例值。

**engine.json**
```json
{
   "engine_name": "o3de"
}
```

| 命令                                                              | 结果                                                     |
|-----------------------------------------------------------------|--------------------------------------------------------|
| `sr_regset_file <engine-root>/engine.json`                      | 将 `engine.json` 文件合并到 “” 的 Settings Registry 锚点的根键 “”。 |
| `sr_regset_file <engine-root>/engine.json /O3DE/EngineSettings` | 将`engine.json`文件合并到锚定到“/O3DE/EngineSettings”对象的设置注册表中。 |

![](/images/user-guide/settings/merge-regset-file.png)
