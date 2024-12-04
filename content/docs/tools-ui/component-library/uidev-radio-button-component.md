---
linktitle: 单选按钮
title: O3DE UI单选按钮样式
description: 了解如何在 O3DE Gem 和工具中将 O3DE UI 样式应用于单选按钮。
toc: true
---

当用户必须只选择一个选项时，使用单选按钮使用户能够从两个或多个选项中进行选择。选择单选按钮将取消选择之前选择的按钮。

![component radio button style](/images/tools-ui/component-radio-button-style.png)

## 使用指南

在设计带有单选按钮的 UI 时，请遵循以下准则：

1. 当您有两个或多个互斥的选项时，请使用单选按钮。

1. 确保单击或点按标签以选中单选按钮。

1. 默认情况下，应始终选择一个单选按钮。

1. 将同一单选按钮组中的单选按钮放在附近。

使用单选按钮时，请避免以下设计选择：
+ 不要水平排序单选按钮。
+ 选择单选按钮时不触发事件，例如生成弹出窗口、弹出窗口、新页面或新窗口。

## 基本单选按钮

![component radio button basic](/images/tools-ui/component-radio-button-basic.png)

在 Qt Designer 或代码中设置和控制单选按钮。

属于同一父级的单选按钮将自动成为独占组的一部分。您也可以使用`QButtonGroup`创建自己的组。

### 示例

```cpp
#include <QRadioButton>
#include <QButtonGroup>

QRadioButton* radioButton1 = new QRadioButton(parent);
QRadioButton* radioButton2 = new QRadioButton(parent);

// Create a mutually exclusive group.
QButtonGroup* buttonGroup = new QButtonGroup(this);
buttonGroup->addButton(radioButton1);
buttonGroup->addButton(radioButton2);

// To set a radio button to the selected state:
radioButton1->setChecked(true);

// To disable a radio button:
radioButton1->setEnabled(false);
```

## C++ API 参考

有关 **单选按钮** API 的详细信息，请参阅 [O3DE UI 扩展 C++ API 参考](/docs/api/frameworks/azqtcomponents/namespace_az_qt_components.html) 中的以下主题:
+  [AzQtComponents::RadioButton](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_radio_button.html)

相关的 Qt 文档包括以下主题：
+  [QRadioButton Class](https://doc.qt.io/qt-5/qradiobutton.html)
