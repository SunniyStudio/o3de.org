---
linktitle: 按钮
title: O3DE UI按钮样式
description: 了解如何在 O3DE Gem 和工具中为按钮和工具按钮应用 O3DE UI 样式。
toc: true
---

使用按钮使用户能够进行选择，从而使 UI 执行操作。O3DE 中有四种类型的按钮样式：
+ 主按钮
+ 辅助按钮
+ 下拉按钮
+ 图标按钮

![component button types](/images/tools-ui/component-button-types.png)

## 使用指南

在设计带有按钮的 UI 时，请遵循以下准则：

1.  **按钮应触发事件**

    选择按钮应始终触发事件。例如，事件可能是提交表单、打开对话框或显示下拉菜单。

1.  **主按钮与辅助按钮**

    每页只有一个 **主按钮** 应接收主按钮颜色。我们建议您始终使用主要按钮颜色设置肯定按钮。页面上的所有其他操作都应使用 **secondary button** 颜色设置样式，这将帮助用户确定任何给定窗口或面板上的主要操作是什么。

1.  **对话框中的按钮放置**

    在 Windows 平台上，按钮应放置在对话框的右下角，肯定按钮位于左侧，不屑一顾按钮位于右侧。这减少了用户的认知负载，并阐明了页面上可用操作的层次结构。

    在 iOS 平台上时，按钮放置应遵循 iOS 平台准则，即右侧的肯定按钮和左侧的不屑一顾按钮。

1.  **卡片上的按钮放置**

    由于卡片具有灵活的布局，因此我们建议您将按钮放置在适合内容和上下文的位置，同时保持产品内的一致性。当按钮未附加到任何标签时，建议将按钮放在该区域的中心。

1.  **下拉菜单**

    使用下拉菜单，其中有多个值与按钮关联。当用户选择按钮并选择所需的值时，按钮应立即触发事件。

1.  **图标按钮**

    当没有足够的空间在按钮上显示完整文本时，请使用图标按钮。但是，用户应该清楚上下文，单击图标按钮后将触发什么操作。按钮上显示的图标还应清楚地表示函数的含义。使用鼠标悬停工具提示可显示按钮的文本版本。

使用按钮时，请避免以下设计选择：
+ 不要在句子中包含按钮。
+ 不要在 UI 上包含多个原色按钮。
+ 当没有足够的上下文让用户了解按钮的用途时，不要使用图标按钮。
+ 如果按钮被禁用，则从不隐藏按钮。按钮应始终对用户可见，无论其状态如何。

## 应用样式

您可以在 Qt Designer 中应用四种按钮样式中的一种，它保存在`.ui`文件中，也可以在您的代码中应用。有关详细信息，请参阅以下示例中的注释。

### 示例

```cpp
#include <AzQtComponents/Components/Widgets/PushButton.h>
#include <QPushButton>
#include <QToolButton>
#include <QMenu>

// Secondary button is the default style for QPushButton.
QPushButton* pushButtonSecondary = new QPushButton(parent);

// To define a QPushButton as a primary button,
// set "default" property to true in the .ui, or apply the primary style.
QPushButton* pushButtonPrimary = new QPushButton(parent);
AzQtComponents::PushButton::applyPrimaryStyle(ui->dropdown);

// Apply the icon style to a QToolButton by adding an icon.
QIcon icon(":/stylesheet/img/logging/add-filter.svg");
QToolButton* toolButton = new QToolButton(parent);
toolButton->setIcon(icon);

// You can also apply the small icon style to an icon button.
AzQtComponents::PushButton::applySmallIconStyle(toolButton);

// Add the dropdown style to a push button or tool button using setMenu().
QMenu* menu = new QMenu(parent);
auto menuOption = menu->addAction("Option");
menuOption->setCheckable(true);
pushButtonSecondary->setMenu(menu);
toolButton->setMenu(menu);
```

## C++ API参考

有关按钮样式的详细信息，请参阅 [O3DE UI 扩展 C++ API 参考](/docs/api/frameworks/azqtcomponents/namespace_az_qt_components.html) 中的以下主题：
+  [AzQtComponents::PushButton](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_push_button.html)

相关的 Qt 文档包括以下主题：
+  [QPushButton Class](https://doc.qt.io/qt-5/qpushbutton.html)
+  [QToolButton Class](https://doc.qt.io/qt-5/qtoolbutton.html)
