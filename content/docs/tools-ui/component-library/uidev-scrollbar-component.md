---
linktitle: 滚动条 
title: O3DE UI 滚动条样式
description: 了解 O3DE 工具和 Gem 中滚动条样式的 O3DE UI 自定义。
toc: true
---

O3DE 为您的滚动条提供了多种样式选择。使用默认样式时，滚动条始终可见。但是，您可以选择设置滚动条显示模式，以便仅当用户将鼠标悬停在滚动区域上时显示该模式。您还可以将深色样式应用于滚动条，使其在浅色背景上更加明显。

以下示例演示如何应用这些样式。

## 滚动条显示模式

![component scrollbar display modes](/images/tools-ui/component-scrollbar-display-modes.gif)

使用`AzQtComponents::ScrollBar::setDisplayMode` 设置滚动条显示模式。默认模式为 `AlwaysShow`。

### 示例

```cpp
#include <AzQtComponents/Components/Widgets/ScrollBar.h>
#include <QScrollArea>

using namespace AzQtComponents;

// Use a scroll area that was created earlier in your code or .ui file.
QScrollArea* scrollArea;

// Set the scrollbar to appear only when users hover over the scroll area.
ScrollBar::setDisplayMode(scrollArea, ScrollBar::ScrollBarMode::ShowOnHover);

// Set it back to default so that it always appears.
ScrollBar::setDisplayMode(scrollArea, ScrollBar::ScrollBarMode::AlwaysShow);
```

## 深色样式的滚动条

使用 `AzQtComponents::ScrollBar::applyDarkStyle` 使滚动条在浅色背景上更明显。

 `ScrollBar::applyLightStyle`将 ScrollBar 恢复为默认样式。

### 示例

```cpp
#include <AzQtComponents/Components/Widgets/ScrollBar.h>
#include <QScrollArea>

// Use a scroll area that was created earlier in your code or .ui file.
QScrollArea* scrollArea;

// Set the scrollbar style to dark, to make the scrollbar more visible on light backgrounds.
AzQtComponents::ScrollBar::applyDarkStyle(scrollArea);

// Revert the scrollbar style back to the default light style.
AzQtComponents::ScrollBar::applyLightStyle(scrollArea);
```

## C++ API 参考

有关 **滚动条** API 的详细信息，请参阅 [O3DE UI 扩展 C++ API 参考](/docs/api/frameworks/azqtcomponents/namespace_az_qt_components.html) 中的以下主题：
+  [AzQtComponents::ScrollBar](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_scroll_bar.html)

相关的 Qt 文档包括以下主题：
+  [QScrollArea Class](https://doc.qt.io/qt-5/qscrollarea.html)
