---
linktitle: 上下文菜单
title: O3DE UI 上下文菜单组件
description: 使用 O3DE UI 上下文菜单组件显示一个弹出菜单，其中包含适合上下文的操作列表。
toc: true
---

上下文菜单（也称为上下文菜单或弹出菜单）在用户交互（如右键单击鼠标操作）时显示。使用上下文菜单可为用户提供一组与菜单所属组件的当前状态或上下文相关的有限选项。通常，可用选项是与所选对象相关的操作。

![component context menu style](/images/tools-ui/component-context-menu-style.png)

## 使用指南

在使用上下文菜单设计 UI 时，请遵循以下准则：

1. 仅包含适用于当前上下文的最常用命令。例如，在所选文本的上下文菜单中，可以包含编辑命令，但不能包含保存或打印命令。

1. 将上下文菜单的分层深度限制为一个或两个级别。上下文菜单中的子菜单可能很难导航，而不会意外关闭父菜单。如果必须包含子菜单，请将它们限制为单个级别。

1. 使上下文菜单项在菜单栏中可用（如果可用）。默认情况下，上下文菜单是隐藏的，用户可能不知道它的存在，因此它不应该是访问命令的唯一方式。特别是，请避免使用上下文菜单作为访问高级功能的唯一方式。

1. 使用弹出按钮提升上下文菜单功能。您可以使用弹出按钮在工具栏中提供应用程序范围的上下文菜单功能。例如，**Finder** 窗口的工具栏包括一个弹出按钮，该按钮允许用户访问在按住 Control 键单击所选项时上下文菜单中显示的相同命令。

1. 上下文菜单也可以在 **列表视图** 中使用。您的 UI 应向用户提供提示，表明他们可以使用右键单击来显示上下文菜单。

使用上下文菜单时，请避免以下设计选择：
+ 不要在上下文菜单中设置默认项。如果用户打开菜单并关闭它而没有选择任何内容，则不应执行任何操作。

## 基本上下文菜单

![component context menu basic](/images/tools-ui/component-context-menu-basic.png)

上下文菜单基于 `QMenu` Qt 小部件。您可以在 Qt Designer 或代码中创建一个。您还可以在运行时在代码中修改它们。

### 示例

```cpp
#include <QMenu>

// Create the menu.
QMenu* menu = new QMenu(parent);

// Add some actions.
menu->addAction(QStringLiteral("Load"));
menu->addAction(QStringLiteral("Save"));

// Add an action with a search icon.
menu->addAction(QIcon(QStringLiteral(":/stylesheet/img/search.svg")), QStringLiteral("Search"));

// Add a separator.
menu->addSeparator();

// Add a submenu.
auto submenu = menu->addMenu(QStringLiteral("Submenu"));

// Add some checkable actions in the submenu.
auto appleAction = submenu->addAction(QStringLiteral("Show Apples"));
appleAction->setCheckable(true);
appleAction->setChecked(true);

auto orangeAction = submenu->addAction(QStringLiteral("Show Oranges"));
orangeAction->setCheckable(true);

auto pearAction = submenu->addAction(QStringLiteral("Show Pears"));
pearAction->setCheckable(true);

// Add an action that is disabled.
auto disabledAction = menu->addAction(QStringLiteral("Disabled"));
disabledAction->setEnabled(false);
```

## C++ API 参考

有关用于设置上下文菜单样式的 **menu** API 的详细信息，请参阅 [O3DE UI 扩展 C++ API 参考](/docs/api/frameworks/azqtcomponents/namespace_az_qt_components.html) 中的以下主题：
+  [AzQtComponents::Menu](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_menu.html)

相关的 Qt 文档包括以下主题：
+  [QMenu Class](https://doc.qt.io/qt-5/qmenu.html)
