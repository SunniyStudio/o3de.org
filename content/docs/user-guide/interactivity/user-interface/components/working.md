---
linkTitle: 使用 UI 组件
description: 了解如何在 Open 3D Engine 的 UI 编辑器中添加和删除组件、引用 UI 元素作为组件属性以及管理组件。
title: 使用 UI 组件
weight: 100
---

您可以使用 **UI 编辑器** 添加和删除组件、为组件属性创建对 UI 元素的引用以及管理组件。

## 添加或删除组件

您可以在 [UI 编辑器](/docs/user-guide/interactivity/user-interface/editor).

**将组件添加到元素**

1. 在 **UI 编辑器**中，在 **Hierarchy** 窗格或视区中选择一个元素。

  要一次将组件添加到多个元素，请使用 **Ctrl** 选择多个元素。

1. 执行以下操作之一：
   + 在 **Properties** 面板顶部，点击 **Add Component**.
   + 在 **Properties** 面板中，右击并选择 **Add Component**.

1. 选择要添加到元素的组件：图像、文本、按钮、复选框、滑块、文本输入、滚动框、推子、蒙版、布局列、布局行或布局网格。

1. 请参阅您要添加的特定组件的说明。

**从元素中删除组件**

1. 在 **UI Editor**中，在 **Hierarchy** 窗格中选择一个元素。

1. 在 **Properties** 面板中，右击组件，点击 **Remove**。

## 引用 UI 元素

您可以为组件上的某些属性指定 UI 元素。例如，您可以为 **Lua Script** 属性指定 UI 元素。

![Properties in a Lua Script component.](/images/user-guide/interactivity/user-interface/components/ui-editor-referencing-ui-elements-1.png)

您可以通过将元素拖动到属性字段中或使用 pick 对象按钮将元素指定为属性。

**将 UI 元素拖动到属性中**
+ 在 **UI 编辑器**的 **Hierarchy** 面板中，选择一个元素并将其拖到 **Properties** 面板中的组件属性中。

**使用选取对象按钮引用 UI 元素**

1. 在 **Properties** 面板中，在要为其指定元素的属性旁边，单击 pick object 按钮。

    ![Properties in a Lua Script component.](/images/user-guide/interactivity/user-interface/components/ui-editor-referencing-ui-elements-2.png)

1. 从视区或 **Hierarchy** 面板中选择所需的元素。

    该属性将填充 UI 元素名称。

**从属性中清除元素名称**
+ 在 **Properties** 面板中，单击属性名称旁边的 **x**。

将 UI 元素指定为属性后，您可以从 **属性** 面板中选择该元素。

从 **Properties** 面板中选择 UI 元素
+ 在 **Properties** 面板中，双击元素名称。

## 管理 UI 组件

您可以使用 **Properties** 面板中的上下文菜单来添加、删除、剪切、复制和粘贴组件。

**从上下文菜单管理组件**

1. 在 **Hierarchy** 面板中，选择 UI 元素。

1. 在 **Properties** 面板中，右键单击组件或单击标题中的菜单按钮。然后选择以下选项之一：
    + **Add component** - 向元素添加组件
    + **Delete component** - 从元素中删除组件
    + **Cut component** - 从一个 UI 元素剪切组件以粘贴到另一个 UI 元素上
    + **Copy component** - 从一个 UI 元素复制组件以粘贴到另一个 UI 元素
    + **Paste component** - 将从一个 UI 元素复制的组件粘贴到另一个 UI 元素上

您还可以使用上下文菜单一次对多个组件执行操作。

**对多个组件执行操作**

1. 按 Ctrl 键选择多个组件，然后右键单击以打开上下文菜单。

1. 选择一项操作。

    {{< note >}}
**Element** 和 **Transform2D** 组件会自动添加到 UI 元素中，并且无法从组件列表中删除。
某些操作被禁用，具体取决于上下文。例如，如果您尚未复制组件，则无法粘贴组件。
{{< /note >}}
