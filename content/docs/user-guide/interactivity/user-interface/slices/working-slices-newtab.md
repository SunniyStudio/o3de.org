---
linkTitle: 编辑 UI 切片
description: ' 在 Open 3D Engine 的 UI Editor 的新选项卡中编辑 UI 切片。 '
title: 在新选项卡中编辑切片
weight: 600
---

在 UI 画布中编辑 UI 切片的实例时，可以使用上下文菜单 **Push to Slice**。这将推送您选择的本地更改。为简单起见，您可以改为在切片自己的上下文中对其进行更改。为此，您可以创建一个空白的 UI 画布，并在其中实例化切片以进行编辑。**UI 编辑器** 使用 **Edit slice in new tab** 功能自动执行此过程。

**在新选项卡中编辑切片**

1. 在 **Hierarchy** 窗格中，右键单击切片元素。

1. 选择 **Edit slice in new tab**并选择要编辑的切片。仅当所选元素是级联切片的实例时，才会显示多个选项。

    ![Right-click the slice element to display the context menu.](/images/user-guide/interactivity/user-interface/slices/ui-editor-working-slices-newtab.png)

    所选切片将显示在标记为 **Slice: **slice name****.

1. 在新选项卡中编辑切片。

    {{< note >}}
如果向切片添加元素，则元素必须是切片实例的子元素。切片实例之外的任何元素都不会被保存，因为这是用于编辑切片的临时画布。
{{< /note >}}

1. 当完成时，选择 **File**, **Save Slice** 以保存对切片的更改。
