---
linktitle: 样式 Dock
title: O3DE UI 样式化的 Dock 构件
description: 了解如何将 O3DE UI 样式的停靠小部件与 Dock 主窗口组件结合使用，以便在 O3DE 工具和 Gem 中启用花哨的停靠。
toc: true
---

将 **styled dock widgets** 与`DockMainWindow`和`FancyDocking`组件结合使用，在 O3DE 中创建称为“fancy docking”的自定义停靠解决方案，它为用户提供了多种选项来安排他们的窗口布局。

Fancy 停靠在目标边缘周围提供四个停靠放置区域，一个位于中心，用于将窗口停靠为选项卡式窗格。将窗口或工具栏拖动到界面元素或窗口边缘上会导致停靠目标出现，以向您显示可以停靠的位置。您可以相对于任何打开的窗格停靠窗口，无论它是否已停靠、作为选项卡浮动，还是拆分为列或行。要了解更多花哨的停靠功能和控件，请参阅 [自定义 O3DE 编辑器](/docs/user-guide/editor/customizing/).

![component fancy docking editor](/images/tools-ui/component-fancy-docking-editor.gif)

## 使用带样式的停靠小部件进行花哨的停靠

![component fancy docking example](/images/tools-ui/component-fancy-docking-example.png)

花哨的停靠最多可以使用五个添加到`DockMainWindow`的样式停靠小部件。Setup 涉及以下实现步骤：

1.  使用`AzQtComponents::DockMainWindow` 构造主窗口。

1. 使用主窗口构造一个`AzQtComponents::FancyDocking`组件。

1. 为每个边缘构造一个`AzQtComponents::StyledDockWidget`并将其添加到主窗口。

1. 为中心构建一个`AzQtComponents::StyledDockWidget`并将其添加到主窗口。

以下代码显示了带样式的 Dock 构件的简单示例。

### 示例

```cpp
#include <AzQtComponents/Components/DockMainWindow.h>
#include <AzQtComponents/Components/FancyDocking.h>
#include <AzQtComponents/Components/StyledDockWidget.h>

// Construct a main window and apply fancy docking.
auto mainWindow = new AzQtComponents::DockMainWindow(this);
auto fancyDocking = new AzQtComponents::FancyDocking(mainWindow);

// Add one AzQtComponents::StyledDockWidget per edge.
auto leftDockWidget = new AzQtComponents::StyledDockWidget("Left Dock Widget", mainWindow);
leftDockWidget->setObjectName(leftDockWidget->windowTitle());
leftDockWidget->setWidget(new QLabel("StyledDockWidget"));
mainWindow->addDockWidget(Qt::LeftDockWidgetArea, leftDockWidget);

auto topDockWidget = new AzQtComponents::StyledDockWidget("Top Dock Widget", mainWindow);
topDockWidget->setObjectName(topDockWidget->windowTitle());
topDockWidget->setWidget(new QLabel("StyledDockWidget"));
mainWindow->addDockWidget(Qt::TopDockWidgetArea, topDockWidget);

auto rightDockWidget = new AzQtComponents::StyledDockWidget("Right Dock Widget", mainWindow);
rightDockWidget->setObjectName(rightDockWidget->windowTitle());
rightDockWidget->setWidget(new QLabel("StyledDockWidget"));
mainWindow->addDockWidget(Qt::RightDockWidgetArea, rightDockWidget);

auto bottomDockWidget = new AzQtComponents::StyledDockWidget("Bottom Dock Widget", mainWindow);
bottomDockWidget->setObjectName(bottomDockWidget->windowTitle());
bottomDockWidget->setWidget(new QLabel("StyledDockWidget"));
mainWindow->addDockWidget(Qt::BottomDockWidgetArea, bottomDockWidget);

QWidget* centralWidget = new QWidget;
QVBoxLayout* vl = new QVBoxLayout(centralWidget);
vl->addWidget(new QLabel("Central Widget"));
mainWindow->setCentralWidget(centralWidget);
```

## C++ API 参考

有关 **styled dock** API 和其他与花式停靠相关的组件的详细信息，请参阅 [O3DE UI 扩展 C++ API 参考](/docs/api/frameworks/azqtcomponents/namespace_az_qt_components.html) 中的以下主题:
+  [AzQtComponents::StyledDockWidget](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_styled_dock_widget.html)
+  [AzQtComponents::DockMainWindow](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_dock_main_window.html)
+  [AzQtComponents::FancyDocking](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_fancy_docking.html)

相关的 Qt 文档包括以下主题：
+  [QDockWidget Class](https://doc.qt.io/qt-5/qdockwidget.html)
+  [QMainWindow Class](https://doc.qt.io/qt-5/qmainwindow.html)
