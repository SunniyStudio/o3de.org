---
linktitle: Toggle Switch
title: O3DE UI Toggle Switch 组件
description: 了解如何将 O3DE UI 切换开关样式应用于 O3DE Gem 和工具中的复选框组件。
toc: true
---

使用切换开关使用户能够以最少的工作量在两种状态之间快速切换。状态将清楚地反映给他们，并在会话中持续存在。

![component toggle switch style](/images/tools-ui/component-toggle-switch-style.png)

## 使用指南

在设计带有切换开关的 UI 时，请遵循以下准则：

1. 当您的小部件有两种二进制状态时，请使用切换开关，例如：“开/关”或“是/否”。对于潜在的状态，不应该有歧义。

1. 始终将拨动开关的 active 状态用于 disfirmative state。肯定状态是 “on”、“yes” 或 “active” 等状态。此处的一致性将帮助客户更好地了解设置的状态。

1. 即使活动状态始终表示“开”，也要清楚地说明开关的后果。

使用拨动开关时，请避免以下设计选择：
+ 当呈现给用户的选项不是二进制时，不要使用。例如，如果邮件设置将 IMAP 和 POP 作为两个邮件检索选项，请改用单选按钮组。在此示例中，实际上有四种状态 - IMAP 打开、IMAP 关闭、POP 打开和 POP 关闭。
+ 如果状态不持久或在会话之间序列化，则不使用。例如，不要用于搜索筛选条件，因为每次用户执行新搜索时，筛选条件都会重置。

## 基本拨动开关

![component toggle switch basic](/images/tools-ui/component-toggle-switch-basic.png)

拨动开关是使用 **QCheckBox** Qt 类实现的。然后使用静态函数`AzQtComponents::CheckBox::applyToggleSwitchStyle()`应用切换开关样式。

### 示例

```cpp
#include <AzQtComponents/Components/Widgets/CheckBox.h>
#include <QCheckBox>

// Apply the toggle switch style to a QCheckBox.
QCheckBox* toggleSwitch = new QCheckBox(parent);
AzQtComponents::CheckBox::applyToggleSwitchStyle(toggleSwitch);

// To set the switch to the "on" state:
toggleSwitch->setChecked(true);

// To set the switch to the "off" state:
toggleSwitch->setChecked(false);

// To disable the toggle switch:
toggleSwitch->setEnabled(false);
```

## C++ API参考

有关用于设置切换开关样式的 **复选框** API 的详细信息，请参阅 [O3DE UI 扩展 C++ API 参考](/docs/api/frameworks/azqtcomponents/namespace_az_qt_components.html) 中的以下主题:
+  [AzQtComponents::CheckBox](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_check_box.html)

相关的 Qt 文档包括以下主题：
+  [QCheckBox Class](https://doc.qt.io/qt-5/qcheckbox.html)

## 相关链接

有关 **toggle switch** 组件的其他信息，请参阅以下主题：
+  [Checkbox](./uidev-checkbox-component)
