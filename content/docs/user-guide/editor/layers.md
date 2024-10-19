---
description: ' 使用 Open 3D Engine 中的实体大纲管理器管理实体。 '
title: 使用图层
draft: true
---

使用 O3DE 图层系统将关卡数据组织成离散文件。图层系统可以分割关卡内容，这样开发团队的成员就可以异步处理关卡的不同方面。

图层是标准的 O3DE 实体，带有一个特殊的编辑器专用图层组件。在游戏中，该组件在层次结构中显示为一个空实体。当您导出游戏时，您设计的行为将保持不变，关卡中的所有实体都会添加到导出数据中。

**主题**
+ [创建图层](#creating-layers)
+ [修改图层](#modifying-layers)
+ [将实体添加到图层](#adding-entities-to-layers)
+ [保存图层](#saving-layers)
+ [恢复图层](#recovering-layers)
+ [图层相关组件](#layer-specific-components)

## 创建图层

创建图层后，您可以向该图层添加实体。这可以帮助您组织游戏中的内容。例如，您可以为所有角色实体创建一个图层，为植被创建另一个图层。

**要创建图层**

1. 在O3DE编辑器中，选择**Tools**, **Entity Outliner**。

1. 在**Entity Outliner**中，右击选择**Create layer**。

![Right-click in the Entity Outliner and choose Create layer.](/images/user-guide/component/entity_system/creating-layers.png)

1. 在**Entity Outliner**中选中图层后，您可以在**Entity Inspector**中修改其属性。

![Right-click in the Entity Outliner and choose Create layer.](/images/user-guide/component/entity_system/modifying-layers-inspector.png)
****


## 修改图层

创建图层后，可以通过添加实体、重组层次结构、添加嵌套图层、重命名图层等方式进行修改。

**要显示图层的上下文菜单**

1. 在**Entity Outliner**中，右击图层。

1. 您可以在右键菜单中执行以下操作。

   黄色高亮显示的操作会影响所选图层。其他选项是标准的上下文菜单操作，不会影响所选图层。

   ![Right-click in the Entity Outliner and choose Create layer.](/images/user-guide/component/entity_system/modifying-layers.png)

   右键菜单中的以下选项可对所选图层执行操作。
****


### 图层层次结构

您可以在其他图层中嵌套图层。如果您想组织层中的实体，这将非常有用。这种行为类似于为父实体和子实体创建层次结构。

![Right-click in the Entity Outliner and choose Create layer.](/images/user-guide/component/entity_system/layer-hierarchies.png)

{{< note >}}
不能将图层作为非图层实体的子实体，也不能在Slice中保存图层。
{{< /note >}}

您可以嵌套图层，将关卡分成更小、更可行的部分。例如，如果您正在创建一个大型关卡，您可能只有一个植被层。如果只有一个植被层，那么一次只能有一个环境美工编辑这个层。为了让多个美工同时编辑植被层，您可以在植被层中嵌套其他层，并将每个嵌套层分配给不同的美工。这有助于建立一个井然有序的层次结构，保持游戏结构的高效性。

## 向图层添加实体

图层可以包含独立的（非切片）实体和Slice。

**要将实体添加到图层**
+ 在**Entity Outliner**中，完成以下一个操作：

1. 选择并拖动一个实体到图层或图层层次结构中。

1. 右键单击一个实体，在**Assign to layer**上暂停，然后选择一个图层。

## 保存图层

组件实体系统会在图层数据中保存对图层及其层次结构的引用。当您从水平仪中添加或删除图层时，必须先保存水平仪，然后再做更多更改。如果您不保存您的图层，下次打开该图层时，图层及其内容将无法正确加载。

O3DE 图层以`.layer`文件形式保存在`level_name/layers`目录下。图层的文件名保存为 `layer_name.layer`。如果一个图层嵌套在另一个图层中，则父图层名称会被添加到图层文件名前。

当一个图层包含未保存的更改时，图层名称旁边会出现一个星号 (\*) 。保存层或图层后，星号将被删除。

![Right-click in the Entity Outliner and choose Create layer.](/images/shared/shared-saving-layers.png)

同一层级的图层名称必须是唯一的。名称重复的同一层次的图层会显示警告 (**\!**) ，并且在重新命名之前无法保存。

![Right-click in the Entity Outliner and choose Create layer.](/images/user-guide/component/entity_system/saving-layers-duplicate.png)

**要保存关卡和所有图层**
+ 在O3DE编辑器中，选择**File**, **Save** 或按下 **Ctrl+S**。

**只保存特定图层**

1. 选择要保存的图层，或按下 **CTRL**，然后选择多个图层。

1. 在**Entity Outliner**中，右击选中项，选择**Save**。

## 恢复图层

如果您在 O3DE 编辑器中删除了一个图层，您可以重新导入它。

**重新导入已删除的图层**

1. 使用文件浏览器，将要恢复的图层的图层文件复制到桌面上，例如 `level_name\layer\layer_name.layer`。

1. 在 O3DE 编辑器中，在您的图层中创建一个新图层，并输入与已删除图层相同的名称。

1. 保存关卡并关闭 O3DE 编辑器。

1. 将桌面上的图层文件复制到 O3DE 的图层目录中，例如 `level_name/layers`。

1. 重命名复制的图层文件，以匹配并替换您在 O3DE 编辑器中创建的图层。

1. 重新打开关卡。新创建的图层现在会引用已恢复的图层信息。

## 图层特定组件

图层只是一个具有特殊规则的实体。因此，您可以为图层添加特定于图层的组件。默认情况下，O3DE 不包含任何图层专用组件，但您可以创建自己的组件，例如用于流媒体或标签的特殊图层组件。

任何给定的组件只能出现在一个上下文菜单中。默认情况下，O3DE 有**Game**、**System**和**Layer**的组件上下文菜单。

您可以通过编辑 **Comment** 组件来测试创建特定图层的组件。

**要修改Comment组件**

1. 在文本编辑器中，打开`EditorCommentComponent.cpp`文件。

1. 修改`AZ_CRC`熟悉为**Layer**，并删除CRC值。

1. 保存文件。

1. 在O3DE编辑器中，将**Comment**组件添加到一个图层。
