---
linktitle: 实体和Prefab基础知识
title: 实体和Prefab基础知识
description: 了解在 Open 3D Engine （O3DE） 中创建和使用实体和Prefab的基础知识。
weight: 200
toc: true
---

本教程介绍实体和Prefab，并说明如何为 Open 3D Engine （O3DE） 项目创建、修改和保存它们。

|O3DE 体验 |完成时间 |功能聚焦 |最后更新 |
|--- |--- |--- |--- |
|初级 |25 分钟 |创建、修改和保存实体和Prefab。 | January 11, 2023 |

## 先决条件

* 具备使用 [O3DE 编辑器](/docs/user-guide/editor) 的基本知识。
* 从标准项目模板构建的项目，或包含标准模板中的 Gem 的项目。

## 什么是实体和Prefab？

实体和Prefab为在 O3DE 中构建项目提供了基础。实体是定义对象的组件的任意组合的集合。实体唯一需要的组件是将实体放置在关卡中的 [**Transform**](/docs/user-guide/components/reference/transform)组件。但是，实体是一个抽象概念，仅存在于关卡或Prefab的上下文中。将实体保存到磁盘需要保存包含它的关卡，或从实体创建Prefab。

Prefab是可以具有一个或多个子实体或Prefab实例的容器。Prefab是保存到磁盘的文件，可以在编辑时实例化或在运行时生成。存在于关卡中的Prefab是文件Prefab的 *实例*。对在 **O3DE Editor 中打开以供编辑的预设件所做的更改将自动传播到该预设件的实例。

放置在关卡中的实体和Prefab实例显示在 **Entity Outliner** 中。实体由 {{< icon "entity.svg" >}} 白色立方体图标表示。Prefab实例由{{< icon "prefab.svg" >}} 白色容器图标表示，背景为深灰色。Prefab实例还会在右侧显示Prefab文件名。

![An entity and a prefab in Entity Outliner.](/images/learning-guide/tutorials/entities-and-prefabs/entity-outliner.png)

本主题中的各节演示了使用实体、Prefab和Prefab实例的基础知识。

## 创建实体

您可以使用以下任一方法创建实体：

* 在 Entity Outliner 或视区中，右键单击并从上下文菜单中选择 **Create entity**。
* 聚焦 Entity Outliner 时，按 **Ctrl+Alt+N**。

![Creating a new entity in Entity Outliner.](/images/learning-guide/tutorials/entities-and-prefabs/create-entity.png)

要创建以资产为基础的实体，请将 `.azmodel` 或 `.actor` 等资产从 **Asset Browser** 拖到 Entity Outliner 或视区中。在以下视频中，将`.azmodel`从 Asset Browser 拖动到视区中，并在关卡中创建具有 **Mesh** 组件的新实体。

{{< video src="/images/learning-guide/tutorials/entities-and-prefabs/create-entity.mp4" info="Drag and drop to create an entity." autoplay="true" loop="true" width="900" >}}

## 创建Prefab

Prefab保存到磁盘中，并允许您通过在关卡中实例化它们或在运行时生成它们来轻松重用对象。Prefab是通过以下步骤从实体创建的：

1. 在 Entity Outliner 中，右键单击实体，然后从上下文菜单中选择 **Create Prefab...**。
1. 在 **Save As...** 窗口中，选择项目中的一个目录，例如`Prefabs`  目录，提供Prefab的名称，然后选择 **Save**。

![Creating a prefab from an entity in Entity Outliner.](/images/learning-guide/tutorials/entities-and-prefabs/create-prefab.png)

Prefab文件将保存到磁盘，并且Prefab的实例将替换 O3DE Editor 中的实体。Prefab实例在左侧的{{< icon "prefab.svg" >}} Prefab图标旁边显示实例的名称。Prefab文件的名称显示在右侧。在下图中，请注意关卡本身是一个Prefab。

![A new prefab instance in Entity Outliner.](/images/learning-guide/tutorials/entities-and-prefabs/prefab-instanced.png)

### 从实体和Prefab实例的集合创建Prefab

复杂Prefab可能需要多个实体和Prefab实例。您可以通过以下步骤从实体和Prefab实例的组合创建Prefab：

1. 选择要包含在新Prefab中的实体和Prefab实例。您可以通过按住 **SHIFT** 并单击每个实体和Prefab实例来选择多个实体和Prefab实例。或者，您可以在 Entity Outliner 或视区中围绕一组实体和Prefab实例 **拖动** 选择框。
1. 在 Entity Outliner 中，**右键单击**选定的实体或Prefab实例之一，然后从上下文菜单中选择 **Create Prefab...**。
1. 在 **Save as...** 窗口中，选择项目中的一个目录，例如`Prefabs`目录，提供Prefab的名称，然后选择 **Save**。

在以下视频中，为新Prefab选择了一个Prefab实例和一个具有层次结构的实体。当在 [Prefab Edit Mode](#edit-a-prefab) 中打开新的 Prefab 实例时，它显示层次结构已保留在新的 Prefab 中。

{{< video src="/images/learning-guide/tutorials/entities-and-prefabs/multiple-entity-prefab.mp4" info="Creating a prefab from a collection of entities and prefabs." autoplay="true" loop="true" >}}

## 实例化一个Prefab

Prefab可以作为Prefab实例添加到关卡或其他Prefab中。要实例化Prefab，请执行以下操作：

1. 在 Entity Outliner 或视区中 **右键单击**，然后从上下文菜单中选择 **Instantiate Prefab...**。
1. 在 **Pick Prefab** 窗口中，导航到要实例化的Prefab文件并将其选中。选择 **确定**。

![Instantiate a prefab in Entity Outliner.](/images/learning-guide/tutorials/entities-and-prefabs/instantiate-prefab.png)

或者，您也可以将 `.prefab` 从 Asset Browser 拖动到 Entity Outliner 或视区中以实例化它，如以下视频所示。

{{< video src="/images/learning-guide/tutorials/entities-and-prefabs/prefab-instance.mp4" info="Drag and drop to instantiate a prefab." autoplay="true" loop="true" width="900" >}}

## 命名实体和Prefab实例

Prefab实例的名称派生自Prefab文件名。但是，每个Prefab实例都可以具有唯一的名称。您可以使用以下任一方法编辑所选实体或Prefab实例的名称：

* **单击** Entity Outliner 中实体或Prefab实例的名称以对其进行编辑。
* 按 **F2** 编辑名称。
* 在 Entity Outliner 中，右键单击实体或Prefab实例，然后从上下文菜单中选择 **Rename**。
* 在 **Entity Inspector**中，编辑 name 字段。

![Renaming a prefab.](/images/learning-guide/tutorials/entities-and-prefabs/rename-prefab.png)

{{< note >}}
实体和Prefab实例不需要具有唯一名称。实例化Prefab或复制实体或Prefab实例时，实例或副本的名称与源的名称相同。每个实体和Prefab实例都有一个唯一的实体 ID，O3DE 使用该 ID 来标识它们。
{{< /note >}}

## 保存实体和Prefab

要保存实体，必须保存包含该实体的关卡（从文件菜单中选择 **Save** 或按 **CTRL+S**）。或者，您可以从实体 [创建Prefab](#creating-entities-and-prefabs)。

要保存Prefab，请在 Entity Outliner 或视区中右键单击Prefab实例，然后从上下文菜单中选择 **Save Prefab to file**。

![Saving a prefab.](/images/learning-guide/tutorials/entities-and-prefabs/save-prefab.png)

{{< note >}}
**Save Prefab to file** 选项仅在Prefab实例具有未保存的更改时显示在上下文菜单中。当Prefab实例具有未保存的更改时，它会在 Entity Outliner 中的Prefab文件名右侧显示一个 **\***。如果尝试保存或关闭包含已编辑的Prefab实例的关卡，则会显示一个窗口，警告未保存的Prefab。该警告将列出未保存的Prefab，并提供保存它们的机会。
{{< /note >}}

## 查看实体或Prefab实例

要将视区聚焦在特定实体或Prefab实例上，请使用以下任一方法：

* **右键单击** 实体或Prefab实例，然后从上下文菜单中选择 **Find in viewport**。
* 选择实体或Prefab实例后，按 **Z**。

{{< video src="/images/learning-guide/tutorials/entities-and-prefabs/find-in-viewport.mp4" info="Focus the viewport on a prefab instance." autoplay="true" loop="true" width="900" >}}

## 创建实体和Prefab层次结构

实体和Prefab可以具有嵌套层次结构。要在实体下或Prefab中创建层次结构，请将实体或Prefab实例 **拖动** 到 Entity Outliner 中的根实体或Prefab实例上。拖动Prefab或实体时，淡蓝色高亮显示标记它将在层次结构中的位置。

{{< note >}}
编辑预设件的层次结构要求在 [预设件编辑模式](#edit-a-prefab)中打开预设件进行编辑。
{{< /note >}}

在以下视频中，实体和Prefab实例被添加为实体的子项。

{{< video src="/images/learning-guide/tutorials/entities-and-prefabs/nested-entities.mp4" info="Nesting entities." autoplay="true" loop="true" >}}

实体和Prefab可以具有多个级别的嵌套。您可以拖放嵌套实体和Prefab来更改层次结构，如以下视频所示。

{{< video src="/images/learning-guide/tutorials/entities-and-prefabs/multiple-nested.mp4" info="Rearranging a nested entity hierarchy." autoplay="true" loop="true" >}}


## 编辑实体

基本实体具有定义对象的组件集合。您可以通过在 Entity Inspector 中选择 **Add Component** 并从列表中选择一个组件，将组件添加到所选实体。在以下视频中，将 **Material** 组件添加到包含 **Mesh** 组件的实体中。

{{< video src="/images/learning-guide/tutorials/entities-and-prefabs/basic-entity.mp4" info="Adding a component to an entity." autoplay="true" loop="true" >}}

## 编辑Prefab

要更改Prefab，必须通过输入 **Prefab Edit Mode** 打开它进行编辑。

### 进入Prefab编辑模式

使用以下任一方法进入 Prefab Edit Mode：

* **双击** Entity Outliner 或视区中的Prefab实例。
* 在 Entity Outliner 或视区中 **右键单击** Prefab实例，然后从上下文菜单中选择 **Open/Edit Prefab**。
* 选择Prefab实例后，按 **+**。

![Edit a prefab in Prefab Edit Mode.](/images/learning-guide/tutorials/entities-and-prefabs/edit-prefab.png)

在 Prefab Edit Mode 中，Prefab的内容显示在 Entity Outliner 中的蓝色框架内。视区将切换到单色模式，打开的Prefab将以全色渲染。视区上方的工具栏显示打开以供编辑的Prefab的路径。从工具栏中，您可以选择 **Prefab Edit Mode** 列表，将视口渲染模式从单色切换到彩色。当您选择Prefab中包含的实体时，该实体的组件将显示在 Entity Inspector 中。

{{< image-width "/images/learning-guide/tutorials/entities-and-prefabs/prefab-focus-mode.png" "1200" "A prefab opened for editing in Prefab Edit Mode" >}}

### 在 Prefab Edit Mode 中编辑Prefab

当 Prefab Edit Mode （预设件编辑模式） 处于活动状态时，您可以在预设件的上下文中使用任何实体和预设件编辑操作。您可以使用本主题中描述的方法创建、复制、删除和重命名实体和Prefab实例，编辑实体，创建和实例化Prefab，以及创建实体和Prefab实例的层次结构。任何修改的结果都会在 Prefab Edit Mode 中的Prefab的上下文中发生。

在以下视频示例中，当Prefab处于Prefab编辑模式时，将`.azmodel`从 Asset Browser 拖动到视区中。将为Prefab中的`.azmodel`创建一个新实体。

{{< video src="/images/learning-guide/tutorials/entities-and-prefabs/create-entity-focus-mode.mp4" info="Creating an entity within a prefab in Prefab Edit Mode." autoplay="true" loop="true" width="1060" >}}

当Prefab具有未保存的更改时，Prefab文件名旁边会显示一个 **\***。[保存Prefab](#saving-entities-and-prefabs) 将更改写入磁盘。

### 退出 Prefab 编辑模式

使用以下任一方法退出 Prefab Edit Mode：

* **双击** Entity Outliner 中的Prefab。
* 在 Entity Outliner 中 **右键单击** Prefab，然后从上下文菜单中选择 **Close Prefab**。
* 点击**-**。

### Prefab编辑模式和嵌套Prefab

您可以通过在 Prefab Edit Mode 中打开嵌套的Prefab来编辑嵌套的Prefab。在以下视频中，将打开嵌套Prefab进行编辑，并在嵌套Prefab的上下文中创建新实体。

{{< video src="/images/learning-guide/tutorials/entities-and-prefabs/nested-focus-mode.mp4" info="Open a nested prefab for edit in Prefab Edit Mode." autoplay="true" loop="true" >}}

## 分离Prefab实例

分离Prefab实例时，其与Prefab文件的链接将断开，并且Prefab实例将转换为实体。Prefab的层次结构由新实体维护。要分离Prefab实例，请 **右键单击** Prefab实例，然后从上下文菜单中选择 **Detach Prefab...**，如以下视频所示。

{{< video src="/images/learning-guide/tutorials/entities-and-prefabs/detach-prefab.mp4" info="Detach a prefab instance." autoplay="true" loop="true" >}}

{{< note >}}
当Prefab实例处于 Prefab Edit Mode 时，无法分离该实例。
{{< /note >}}

## 复制实体或Prefab实例

可以使用以下任一方法复制选定的实体和Prefab实例：

* **右键单击** 选定的实体或Prefab实例，然后从上下文菜单中选择 **Duplicate**。
* 选择实体或Prefab实例后，按 **CTRL+D**。

副本显示在源实体或Prefab实例的下方，其名称与源实体或Prefab实例相同。在 Entity Inspector 中检查重复项会显示该重复项具有唯一的 Entity ID。

选择多个实体和Prefab时，它们都将与其层次结构一起复制，如以下视频所示。

{{< video src="/images/learning-guide/tutorials/entities-and-prefabs/multiple-duplicates.mp4" info="Duplicating multiple entities and prefab instances." autoplay="true" loop="true" >}}

在以下视频中，嵌套实体和Prefab实例在Prefab的层次结构中就地复制。

{{< video src="/images/learning-guide/tutorials/entities-and-prefabs/nested-duplicate.mp4" info="Duplicating nested entities and prefab instances." autoplay="true" loop="true" >}}

## 删除实体和Prefab实例

您可以使用以下任一方法删除选定的实体和Prefab实例：

* **右键单击** 选定的实体或Prefab实例，然后从上下文菜单中选择 **Delete**。
* 选择实体或Prefab实例后，按 **DEL**。

{{< note >}}
从关卡中删除实体或Prefab实例时，必须保存关卡（按 **CTRL+S**）以提交更改。

从Prefab中删除实体或Prefab实例时，您必须 [保存Prefab](#saving-entities-and-prefabs)  以提交更改。

删除Prefab实例不会影响用于创建Prefab实例的Prefab文件。Prefab实例将被删除，但磁盘上的 `.prefab` 文件不会更改。
{{< /note >}}

## 结论和下一步

现在，您已经了解了创建和使用Prefab的基础知识，您可以学习如何将覆盖应用于单个Prefab实例。在下一个教程中学习 [覆盖Prefab](override-a-prefab)。
