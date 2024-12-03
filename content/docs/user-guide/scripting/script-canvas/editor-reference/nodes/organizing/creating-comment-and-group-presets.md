---
linktitle: 创建注释预设和组预设
title: 在 Script Canvas 中创建注释预设和组预设
description: 将自定义节点注释和组保存为预设，以便在 Open 3D Engine （O3DE） Script Canvas 可视化脚本编辑器中快速重用。
weight: 400
---

您可以将自定义节点注释和节点组设置保存为预设，以便于重复使用。在团队设置中，这些预设可以帮助您创建自己的着色和命名约定，以提高 Script Canvas 图形之间的清晰度和一致性。

## 自定义评论或组的外观

在基于现有节点注释或节点组创建预设之前，请通过在 **Node Inspector **中设置  **Background Color**, **Group Color**, 和 **Font Settings** 选项来自定义其颜色和字体。

![Using Script Canvas Node Inspector to customize node comments and node groups.](/images/user-guide/scripting/script-canvas/nodes-comment-and-group-presets-1.png)

有关各项设置的更多信息，请参阅 [自定义注释](commenting-nodes/#customizing-comments)  和 [自定义群组](grouping-nodes/#customizing-groups).

## 创建和使用预设

使用现有节点组或节点注释在 **Script Canvas 编辑器** 中创建预设。

**从组或评论创建预设**

1. 右键单击组或评论的标题栏，然后选择 **Create Preset From**。您创建的预设将保存原始组或注释的字体设置和颜色。它不会保存显示文本。

    ![Creating a preset from an existing comment in Script Canvas.](/images/user-guide/scripting/script-canvas/nodes-comment-and-group-presets-2.png)

1. 在 **Set Preset Name** 对话框中，输入预设的名称，然后单击**OK**.

    ![Naming a comment or group preset in Script Canvas.](/images/user-guide/scripting/script-canvas/nodes-comment-and-group-presets-3.png)

1. 要使用您创建的预设，请右键单击画布，选择 **Add Comment** 或 **Group**，然后选择您创建的预设注释或组。

    ![Choosing a preset comment](/images/user-guide/scripting/script-canvas/nodes-comment-and-group-presets-4.png)

## 配置默认预设

您可以将预设定义为节点组或注释的默认值。当您执行以下操作之一时，将创建默认组或评论：

+ 在 Script Canvas 编辑器工具栏上，选择新注释 {{< icon "comment.svg" >}}  或新建组 {{< icon "group.svg" >}} 图标。
+ 按 **Ctrl+Alt+M** 创建注释。
+ 按 **Ctrl+Alt+O** 创建一个组。

**配置默认预设**

1. 在 Script Canvas 编辑器中，选择 **Tools**, **Presets Editor**.

1. 对于 **Construct Type,** 选择 **Comment** 或 **Node Group**.

1. 在 **Is Default** 列中，选择要设为默认的预设，然后选择 **OK**.

    ![Setting a preset as the default in the Script Canvas Presets Editor.](/images/user-guide/scripting/script-canvas/nodes-comment-and-group-presets-5.png)

{{< note >}}
配置预设后，无法对其进行修改。使用 **Presets Editor** 删除预设。然后重新创建预设。

如果重新创建预设，则您所做的更改不会传播到使用早期版本的预设创建的注释或组。
{{< /note >}}

## 删除预设

要删除预设，请使用 Script Canvas 预设编辑器。

**删除预设**

1. 在 Script Canvas 编辑器中，选择 **Tools**, **Presets Editor**.

1. 对于 **Construct Type,** 选择 **Comment** 或 **Node Group**.

1. 选择要删除的预设。

    ![Using the Presets Editor in Script Canvas to remove a comment preset.](/images/user-guide/scripting/script-canvas/nodes-comment-and-group-presets-6.png)

1. 选择 **Remove**, 然后选择 **OK**.
