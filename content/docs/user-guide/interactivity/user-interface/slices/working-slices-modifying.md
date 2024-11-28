---
linkTitle: 修改 UI 切片
description: ' 修改 UI 切片并在 Open 3D Engine 的 UI 编辑器中推送更改。 '
title: 修改 UI 切片并推送更改
weight: 500
---

在 **UI 编辑器 ** 中，您可以修改 UI 切片并推送更改，其方式与在主 O3DE 编辑器中的操作类似。

在 **UI 编辑器**中使用切片时，以下约定适用：
+ 如果您选择的实体是切片实例的一部分，则 **属性** 窗格将以橙色突出显示所选实体与其所属切片之间的任何不同属性。
+ UI 切片不能包含对不在切片中的任何实体的引用。UI 切片只能包含对切片中实体的引用。



在 **Hierarchy** 窗格中，属于切片的元素显示为蓝色。以蓝色粗体显示的元素表示该元素是切片的根。在切片的根中，以斜体显示的元素表示子切片。

**例**
+ 元素 **Background** 不在切片中.
+ **FontRenderingButton**是根元素及其子元素， **Text**是`submenubutton.slice`中的一个元素。

![The UI Editor's Hierarchy pane displays in blue any elements that are part of a slice.](/images/user-guide/interactivity/user-interface/slices/ui-editor-modifying-slices-submenubutton.png)

您可以在本地更改实例化切片的名称，这会隐藏从中实例化切片的名称。

**确定元素实例化的切片**
+ 在元素上暂停。切片名称及其路径将显示在工具提示中。

**将本地更改推送到切片**

1. 右击**Properties**面板，或在**Hierarchy**面板中选择UI元素。

1. 在上下文菜单中，选择**Push to Slice**。
