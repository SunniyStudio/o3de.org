---
linktitle: 复选框
title: O3DE UI 复选框组件
description: 了解如何在 O3DE Gem 和工具中使用 O3DE UI 复选框。
toc: true
---

当用户可以选择任意数量的选项（包括零个、一个或多个）时，使用复选框使用户能够从选项列表中进行选择。每个复选框都独立于列表中的所有其他复选框，因此选中一个复选框不会取消选中其他复选框。

![component checkbox style](/images/tools-ui/component-checkbox-style.png)

## 使用指南

在设计带有复选框的 UI 时，请遵循以下准则：

1. 每个复选框都应该有一个明确的 yes/no 状态供其选择。

1. 仅当有明确理由相信用户会期望这样做，或者当它反映了用户的当前状态时，才将复选框默认为“选中”。

1. 确保单击或点按标签以选中复选框。

1. 在树状图中使用复选框时，应包括部分选定的状态。

使用复选框时，请避免以下设计选择：
+ 不要水平排序复选框。
+ 选择单选按钮时不触发事件，例如生成弹出窗口、弹出窗口、新页面或新窗口。

## 基本复选框

![component checkbox basic](/images/tools-ui/component-checkbox-basic.png)

在 Qt Designer 或代码中设置和控制复选框。

请注意，要在三态复选框中设置 “partially checked” 状态，您必须使用 code。

### 示例

```cpp
#include <QCheckBox>

QCheckBox* checkBox = new QCheckBox(parent);

// To set a checkBox to checked, do one of the following:
checkBox->setCheckState(Qt::Checked);
checkBox->setChecked(true);

// To set a checkbox to unchecked, do one of the following:
checkBox->setCheckState(Qt::Unchecked);
checkBox->setChecked(false);

// To turn a checkBox into a tri-state checkbox, so it can be on, off, or partial:
checkBox->setTristate(true);

// To set a checkBox to partially on:
checkBox->setCheckState(Qt::PartiallyChecked);

// To disable the checkBox:
checkBox->setEnabled(false);
```

## C++ API参考

有关 **复选框** API 的详细信息，请参阅 [O3DE UI 扩展 C++ API 参考](/docs/api/frameworks/azqtcomponents/namespace_az_qt_components.html) 中的以下主题：
+  [AzQtComponents::CheckBox](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_check_box.html)

相关的 Qt 文档包括以下主题：
+  [QCheckBox Class](https://doc.qt.io/qt-5/qcheckbox.html)

## 相关链接

有关 **checkbox** 组件的其他信息，请参阅以下主题：
+  [Toggle switch](./uidev-toggle-switch-component)
