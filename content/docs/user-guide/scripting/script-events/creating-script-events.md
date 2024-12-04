---
linktitle: 创建脚本事件
title: 在 Open 3D Engine 中创建脚本事件
description: 了解如何在 Open 3D Engine （O3DE） 中创建脚本事件。
weight: 100
---

使用 **O3DE 编辑器** 中的 **Asset Editor** 工具创建脚本事件。

**创建脚本事件**

1. 在 O3DE 编辑器中，选择**Tools**, **Asset Editor**。

    ![Choose Tools, Asset Editor.](/images/user-guide/scripting/script-events/creating-1.png)

1. 在 **Asset Editor** 中，选择 **File**, **New**, **Script Events**。

    ![Choose File, New, Script Events in the Asset Editor.](/images/user-guide/scripting/script-events/creating-2.png)

1. 在 **Asset Editor** 中，输入定义脚本事件的信息。有关每个字段的说明，请参阅下面的 [脚本事件属性](#script-event-properties) 表。

    ![Creating a script event in the Asset Editor.](/images/user-guide/scripting/script-events/creating-3.png)

1. 选择 **File**, **Save**, 或按下 **Ctrl+S**。

1. 在 **Save As** 对话框中，输入`.scriptevents` 文件的文件名，然后单击 **Save**。

    {{< note >}}
要确保脚本事件资源保存正确，请检查 Asset Editor 的底部是否有"<_`file_name`_>`.scriptevents` - Asset loaded！”消息。如果您看到错误消息 由于验证错误，无法保存资产，请检查 O3DE 编辑器 **控制台** 窗口或`Editor.log`文件以了解更多信息。
    {{< /note >}}

## 脚本事件属性

|脚本事件字段 |描述 |
| --- | --- |
| Name | 在 Lua 和 Script Canvas 编辑器 **Node Palette** 中输入标识脚本事件的名称。事件名称必须仅包含字母数字字符，并且不能以数字开头或包含空格。 |
| Category | 在显示脚本事件的 **Script Canvas 编辑器** **Node Palette** 中输入类别的名称。默认类别为 **Script Events**。要嵌套类别，请使用语法 _category/sub_category/sub_category_。 |
| Tooltip | 输入脚本事件的描述。当用户将指针暂停在 **Node Palette** 中的脚本事件上或图形中的事件节点上时，将显示描述。 |
| Address Type | （可选）为寻址此脚本事件的值选择数据类型。可能的类型包括 **String**, **Entity Id**, 或 **Tag**。 |
| Events | 指定脚本系统发送和接收的事件列表。定义事件的方式与在编程语言中定义函数的方式相同。您创建的事件可从 Script Canvas 的 Node Palette 中拖放。 |

### Event 属性

|事件字段 |描述 |
| --- | --- |
| Name | 事件的名称。 |
| Tooltip | 输入当用户将指针悬停在事件名称上时显示的事件的描述。 |
| Return value type | （可选）选择事件处理程序返回给事件发送方的值的数据类型。如果 Return Type 不是 **None**，则必须将指定类型的值从接收方节点连接到发送方节点。 |
| Parameters | 选择加号 （+） 图标以为事件函数添加一个或多个参数。 |

#### Event parameter 属性

| 参数字段 |描述 |
| --- | --- |
| Name | 输入函数参数的名称。 |
| Tooltip | 输入当用户将指针悬停在参数名称上时显示的参数的描述。 |
| Type | 选择参数的数据类型。 |
