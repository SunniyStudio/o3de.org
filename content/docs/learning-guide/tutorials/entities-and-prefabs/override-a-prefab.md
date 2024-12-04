---
linktitle: 覆盖Prefab
title: 覆盖Prefab
description: 了解如何在 Open 3D Engine （O3DE） 中覆盖Prefab。
weight: 300
toc: true
---

在 [预设件编辑模式](/docs/learning-guide/tutorials/entities-and-prefabs/entity-and-prefab-basics#edit-a-prefab)中打开预设件时，对其内容的更改会自动传播到该预设件的所有实例。有时，在不影响其他实例的情况下自行更改Prefab实例非常有用。例如，您可能在关卡中拥有同一汽车Prefab的多个实例，但您希望每个汽车实例都是不同的颜色。Prefab overrides 允许您更改Prefab实例的属性和内容，使其唯一。

| O3DE 体验 |完成时间 |功能聚焦 |最后更新 |
| --- | --- | --- | --- |
| 初级 |15 分钟 |使用Prefab覆盖使Prefab实例唯一。 | November 20, 2023 |


## 先决条件

* 具备使用 [O3DE 编辑器](/docs/user-guide/editor) 的基本知识。
* 从标准项目模板构建的项目，或包含标准模板中的 Gem 的项目。


## 什么是Prefab覆盖？

Prefab覆盖是Prefab系统的主要组件之一。在覆盖之前，更改单个Prefab实例将影响所有实例。通过覆盖，我们可以使Prefab从其他兄弟姐妹中脱颖而出。覆盖为内容创建者提供了更大的灵活性来构建大型和复杂的场景。它还允许覆盖通过 `.fbx`文件导入的默认程序Prefab。

下图显示了关卡中 Car Prefab的两个实例。启用覆盖后，Car 实例是可扩展的，并且可以打开进行编辑：

{{< image-width src="/images/learning-guide/tutorials/entities-and-prefabs/level-prefab-edit.png" width="300" alt="Level in Prefab Edit Mode in Entity Outliner." >}}

{{< note >}}
如果禁用了Prefab覆盖，则无法扩展关卡中的Prefab实例进行编辑。换句话说，您将不会在实例名称左侧看到箭头图标。
{{< /note >}}

应用于Prefab实例的覆盖将为该单个实例注册，并存储在包含实例的正在编辑的Prefab中。在上面的示例中，Car 实例位于关卡（Prefab）中，因此在保存关卡时，应用于 Car 实例的Prefab覆盖将存储在关卡中。

{{< image-width src="/images/learning-guide/tutorials/entities-and-prefabs/level-prefab-edit-with-override.png" width="300" alt="Level in Prefab Edit Mode with Override Edit in Entity Outliner." >}}

将 Body 实体的位置修改为覆盖时，您会注意到 Entity Outliner 中将出现一个蓝色圆圈。这表示实体覆盖。现在，如果保存关卡Prefab，也会保存覆盖。

{{< note >}}
关卡是Prefab，打开时会自动进入 Prefab Edit Mode。这由 **Entity Outliner** 中关卡周围的蓝色胶囊表示。

覆盖不限于关卡。事实上，在 Prefab Edit Mode 中打开以供编辑的任何Prefab都负责存储对其嵌套Prefab所做的覆盖。
{{< /note >}}

如果您不想进行覆盖更改，则应改用旧的 Prefab Edit Mode 工作流程。在此示例中，您可以通过 Entity Outliner 中的 Car 实例进入Prefab编辑模式。现在，对 Car 的任何更改都将影响所有 Car 实例。

{{< image-width src="/images/learning-guide/tutorials/entities-and-prefabs/level-prefab-edit-enter-edit-mode.png" width="300" alt="Edit Mode Entered on Car Prefab in Entity Outliner." >}}

您可以将以下类型的覆盖应用于Prefab：

* 编辑组件属性
* 添加或删除组件
* 添加或删除实体
* 删除嵌套的Prefab实例


## 覆盖类型

### 编辑组件属性

要覆盖Prefab实例下实体的组件属性，请执行以下操作：

1. 在 Entity Outliner 中，通过单击实例名称左侧的箭头展开Prefab实例。
1. 选择实例下的一个实体，然后在 **Inspector**中编辑其属性之一。
1. 请注意，Entity Outliner 中的实体图标上会显示一个蓝色圆圈。这表示覆盖已应用于实体。

在下图中，第一个 Car 实例中的 Body 实体具有将汽车的默认颜色从红色更改为蓝色的覆盖：

{{< image-width src="/images/learning-guide/tutorials/entities-and-prefabs/prefab-override-property.png" width="750" alt="Overriding a component property." >}}

在 Inspector 中进行覆盖编辑时，您会注意到属性标签之前出现一个蓝色圆圈，并且标签文本以粗体显示。在组件标题中，文本附近还会显示一个蓝色圆圈，这表示拥有属性值已被覆盖。

{{< image-width src="/images/learning-guide/tutorials/entities-and-prefabs/prefab-override-component-property.png" width="450" alt="Overriding a component property." >}}

### 添加或删除组件

要在Prefab实例下添加实体的组件：

1. 在 Entity Outliner 中，选择未处于 Prefab Edit Mode 的实例下的实体。
1. 在 Inspector 中，右键单击空白区域，然后从上下文菜单中选择 **Hi**。
1. 请注意，组件名称左侧的组件图标上会显示一个蓝色的加号图标。

{{< image-width src="/images/learning-guide/tutorials/entities-and-prefabs/prefab-override-add-component-override.png" width="450" alt="Adding a component as an override." >}}

要删除组件作为覆盖，您可以改为从上下文菜单中选择 **Delete**。

在 Inspector 中，目前没有视觉指示组件作为覆盖删除。摆脱此覆盖编辑的唯一方法是在`.prefab`文件中手动编辑或正在编辑的Prefab实例。或者，您可以通过按 Ctrl+Z 来恢复删除，以返回到删除该组件的先前状态。但是，仅当编辑器尚未关闭并重新打开时，此撤消才有效。关闭编辑器后，您的撤消堆栈已被清除，您无法再以这种方式撤消。

### 添加或删除实体

要在Prefab实例下添加实体作为覆盖，请执行以下操作：
1. 在 Entity Outliner 中，右键单击Prefab实例，然后从上下文菜单中选择 **Create entity**。
1. 请注意，Entity Outliner 中新实体的图标上会显示一个蓝色加号。这表示该实体已添加为覆盖。

在下图中，已将 Antenna 实体添加到第一个 Car Prefab实例中。

{{< image-width src="/images/learning-guide/tutorials/entities-and-prefabs/prefab-override-add.png" width="750" alt="Adding an entity as an override." >}}

要删除实体或嵌套的Prefab实例作为覆盖，请执行以下操作：
1. 在 Entity Outliner 中，右键单击实体或嵌套Prefab实例，然后从上下文菜单中选择 **Delete**。
1. 请注意，在 Entity Outliner 中，父实体的图标上会显示一个蓝色圆圈。这表示已将子项作为覆盖删除。

同样，在 Entity Outliner 中，目前没有视觉指示表明实体或Prefab实例作为覆盖被删除。您可以按照上述步骤删除文件中的覆盖或在 Editor 中撤消覆盖。

{{< note >}}
您无法通过恢复父实体上的覆盖来恢复删除。有关更多详细信息，请参阅 GitHub issue [#13437](https://github.com/o3de/o3de/issues/13437)。
{{< /note >}}

### 删除嵌套的Prefab实例

在当前工作流程中，它不支持将嵌套的Prefab实例添加为覆盖。但是，您可以按照上述实体的步骤删除嵌套的Prefab实例作为覆盖。

{{< note >}}
删除或撤消Prefab实例不会从硬盘驱动器中删除实际的Prefab。

要永久删除Prefab，您必须从 Asset Browser 或 OS 文件夹操作中删除Prefab文件。我们建议您不要这样做，除非您确定 prefab 不存在于其他级别或引用它的其他项目中。
{{< /note >}}


## 回退覆盖编辑

注册覆盖后，它将一直存在，直到明确删除为止。有三种方法可以还原覆盖：
1. 还原属性的覆盖
1. 还原组件上的一些覆盖
1. 还原实体上的所有覆盖

### 还原属性的覆盖

您可以分别还原单个覆盖的属性值：
1. 在 Inspector 中，**右键单击**属性标签或蓝色圆圈**，然后从上下文菜单中选择 **还原覆盖**。
1. 请注意，该属性不再有任何具有覆盖的指示。

{{< image-width src="/images/learning-guide/tutorials/entities-and-prefabs/prefab-override-revert-component-property-override.png" width="450" alt="Reverting a component property override." >}}

### 恢复组件上的一些覆盖

此外，您还可以还原组件拥有的所有属性覆盖：
1. 在 Inspector 中，**右键单击**组件标题中的蓝色圆圈**，然后从上下文菜单中选择 **Revert override**。
1. 请注意，所有属性都不再具有任何具有覆盖的指示。

{{< image-width src="/images/learning-guide/tutorials/entities-and-prefabs/prefab-override-revert-component-override.png" width="450" alt="Reverting a component added as an override." >}}

对于作为覆盖添加的组件，您可以按照上述相同方式还原添加。或者，您可以使用以下任一方法删除组件：

1. 在 Inspector 中，右键单击组件，然后从上下文菜单中选择 **Delete component**。
1. 选择元件后，按 **DEL**。

对于作为覆盖删除的组件，您可以通过恢复所选实体上的所有覆盖来恢复删除。但是，请注意，如果存在其他覆盖，这将删除它们。

### 还原实体上的所有覆盖

最后但同样重要的是，您可以还原应用于实体的所有覆盖：
1. 在 Entity Outliner 中，**右键单击** 具有覆盖的实体，然后从上下文菜单中选择 **Revert Overrides**。
1. 请注意，该实体不再有任何具有覆盖的迹象。

{{< image-width src="/images/learning-guide/tutorials/entities-and-prefabs/prefab-override-revert.png" width="300" alt="Reverting overrides on an entity." >}}

{{< note >}}
如果你看到任何意外的覆盖显示，你可以尝试上述方法来恢复它们。如果不起作用，您可以打开拥有这些覆盖的Prefab文件（在`Instances`键下）并删除所需的覆盖补丁。
{{< /note >}}


## 禁用Prefab覆盖

Prefab覆盖是一项新功能。您可能会遇到减慢甚至阻碍工作流程的问题。您可以按照以下建议在项目中禁用该功能。

首先，我们建议您在 Inspector 中禁用Prefab覆盖功能，该功能应回滚到它在 23.05 版本中的工作方式。
1. 在**Tool**中打开**Console Variables (CVar)** 编辑器。
1. 关闭 `ed_enableInspectorOverrideManagement` 变量。
1. 请注意，您需要关闭和打开`ed_enableDPEInspector`变量才能使其生效。

{{< note >}}
Entity Inspector 中的Prefab覆盖管理是基于新的 *[Document Property Editor (DPE)](https://github.com/o3de/sig-content/issues/11)*开发的，旨在取代旧的 *Reflected Property Editor (RPE)*。因此，`ed_enableInspectorOverrideManagement`标志依赖于`ed_enableDPEInspector`标志。
{{< /note >}}

这将禁用 Inspector 中的该功能，但您仍然可以在 Entity Outliner 中操作覆盖编辑。

其次，如果您想在 O3DE 编辑器中完全禁用该功能，您可以按照以下步骤操作：
1. 在**Tool**中打开**Console Variables (CVar)** 编辑器。
1. 关闭 `ed_enableInspectorOverrideManagement` 变量。
1. 请注意，您需要重新启动 O3DE 编辑器才能使其生效。

或者，您可以通过包含以下内容的设置注册表文件更改上述标志：

```json
{
    "O3DE": {
        "Autoexec": {
            "ConsoleCommands": {
                "ed_enableDPEInspector": true,
                "ed_enableInspectorOverrideManagement": false,
                "ed_enableOutlinerOverrideManagement": false
            }
        }
    }
}
```

此类文件的示例是 AutomatedTesting 项目中特定于项目的覆盖： [`AutomatedTesting/Registry/editorpreferences.setreg`](https://github.com/o3de/o3de/blob/c8f19bbe664a89ad92007fb7674cb8c6aa165bd9/AutomatedTesting/Registry/editorpreferences.setreg)

{{< note >}}
建议将设置注册表文件作为用户特定的覆盖添加到`<project-path>/user/Registry` 目录。用户目录中的文件会被 git 忽略，并且不会跟踪更改。
{{< /note >}}

最后但并非最不重要的一点是，如果禁用该功能无法解决您的问题，您可以查看已知问题列表：
1. [Prefab覆盖的已知问题](https://github.com/o3de/o3de/issues?q=is%3Aopen+is%3Aissue+label%3Afeature%2Fprefabs+%22prefab+overrides%22)
1. [DPE Inspector 的已知问题](https://github.com/o3de/o3de/issues?q=is%3Aopen+is%3Aissue+%22DPE+inspector%22)

如果您找不到相关问题，请通过 [GitHub Issue](https://github.com/o3de/o3de/issues/new/choose)报告，并将相关标签（`sig/content`, `feature/prefabs`）附加到新问题上。


## 结论和下一步

现在，您已经了解了实体、Prefab和Prefab实例之间的区别，以及创建和使用它们的基础知识，可以将其付诸实践。在下一个教程中学习 [生成和取消生成Prefab](spawn-a-prefab) 。
