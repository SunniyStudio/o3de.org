---
linktitle: 浏览编辑
title: O3DE UI浏览编辑组件
description: 使用 O3DE UI 浏览编辑组件，使用户能够从列表或目录中选择一个或多个值。
toc: true
---

**浏览编辑** 组件是 Qt 和 O3DE UI 库提供的几种类型的输入框之一。使用 browse edit 组件，使用户能够从选择窗口中选择一个或多个值。典型用途包括从目录中选择文件，或从列表中选择一个或多个项目 - 例如：从动画资产集合中选择运动。使用右边缘的按钮或双击输入框中的按钮打开选择窗口。如果已启用此按钮，则可以使用 clear 按钮清除所选值。

{{< note >}}
如果有预定义的项目列表可供选择，并且用户只需从列表中选择一个值，请改用下拉列表 [组合框](/docs/tools-ui/component-library/uidev-combobox-component)。
{{< /note >}}

## 浏览编辑 Widget 剖析

**浏览编辑** 小部件具有多个自定义选项。标准功能包括以下元素：

![component browse edit anatomy](/images/tools-ui/component-browse-edit-anatomy.png)

1.  **Label**

    虽然从技术上讲不是 Widget 的一部分，但您应该在 UI 布局中为输入框指定一个标签。

1.  **Placeholder text**

    （可选）当小组件文本为空时，您使用`setPlaceholderText()`设置的提示文本会出现在此处。

1.  **Input box**

    您使用`setText()`设置的文本显示在此处。用户可以双击此处打开您已连接到`BrowseEdit::attachedButtonTriggered`信号的选择窗口。单击操作在此处不执行任何操作，除非 read-only 属性未设置为`true`。

    {{< note >}}
通常，在使用 **浏览编辑** 组件时，将输入框设置为只读。如果要允许用户直接从输入框编辑值，请改用 [行编辑](/docs/tools-ui/component-library/uidev-line-edit-component) 组件，旁边有一个分离的按钮。
{{< /note >}}

1.  **Attached button**

    （可选）**浏览编辑**输入框有一个附加的按钮。默认按钮使用文件夹图标，但您可以指定其他图标。要指定用户按下按钮时发生的情况，请连接到`BrowseEdit::attachedButtonTriggered`信号。
    
    {{< note >}}
当使用非 O3DE 提供的自定义图标时，图标应为 16 x 16 的倍数，并且只能采用 SVG 格式。
{{< /note >}}

1.  **Tooltip**

    （可选）如果您为小组件设置工具提示文本，它将出现在用户悬停位置附近。

    ![component browse edit anatomy clear](/images/tools-ui/component-browse-edit-anatomy-clear.png)

1.  **Clear button**

    （可选）如果启用小组件的清除按钮，它将在输入框不为空时显示。当用户选择清除按钮时，输入框将返回空值。

    ![component browse edit anatomy error state](/images/tools-ui/component-browse-edit-anatomy-error-state.png)

1.  **Error state indicator**

    （可选）如果为输入框设置了验证器，并且验证失败，则错误状态指示器图标会显示在输入框末尾的清除按钮之前。

1.  **Error tooltip**

    （可选）当存在错误状态时，如果已为小组件设置了错误工具提示，它将出现在用户悬停位置附近。当存在错误状态时，此工具提示将代替正常的工具提示文本显示。如果您为输入框设置了验证器，强烈建议您同时设置错误工具提示。

## 基本浏览编辑

![component browse edit basic](/images/tools-ui/component-browse-edit-basic.png)

此组件的一个简单示例包括 browse edit 小部件和附加的按钮处理程序。在此示例中，我们允许用户从文件浏览器中选取文件，并选择启用清除按钮。

### 示例

```cpp
#include <AzQtComponents/Components/Widgets/BrowseEdit.h>
#include <QFileDialog>

// Create a new browse edit widget.
AzQtComponents::BrowseEdit* browseEdit = new AzQtComponents::BrowseEdit(parent);

// Enable the clear button.
browseEdit->setClearButtonEnabled(true);

// Require users to use the attached button or double-click the input box to open the selection window.
browseEdit->setLineEditReadOnly(true);

// Connect the attached button to the QFileDialog function.
connect(browseEdit, &AzQtComponents::BrowseEdit::attachedButtonTriggered, this, [this]() {
    QString file = QFileDialog::getOpenFileName(this, "Select a file asset");
    if (!file.isEmpty())
    {
        QFileInfo fileName(file);
        browseEdit->setText(fileName.fileName());
    }
});

// Then add browseEdit to a UI layout as needed.
```

## 使用自定义图标和占位符文本浏览编辑

![component browse edit icon and placeholder](/images/tools-ui/component-browse-edit-icon-and-placeholder.png)

以下示例演示如何将默认按钮图标替换为您自己的图标，以及如何添加在值为空时显示的占位符文本。

{{< note >}}
当使用非 O3DE 提供的自定义图标时，图标应为 16 x 16 的倍数，并且只能采用 SVG 格式。
{{< /note >}}

### 示例

```cpp
#include <AzQtComponents/Components/Widgets/BrowseEdit.h>
#include <QInputDialog>

// Set a custom attached button icon.
QIcon icon(":/stylesheet/img/help.svg");
browseEdit->setAttachedButtonIcon(icon);

// Require users to use the attached button or double-click the input box to open the selection window.
browseEdit->setLineEditReadOnly(true);

// Set the placeholder text.
browseEdit->setPlaceholderText("Enter an offset value");

// Connect the attached button to the QInputDialog function.
connect(browseEdit, &AzQtComponents::BrowseEdit::attachedButtonTriggered, this, [this]() {
    QString text = QInputDialog::getText(this, "Offset", "Enter an offset value");
    if (!text.isEmpty())
    {
        browseEdit->setText(text);
    }
});
```

## 使用工具提示和验证器浏览编辑

![component browse edit error state](/images/tools-ui/component-browse-edit-error-state.png)

在以下示例中，已定义标准工具提示和错误工具提示。当鼠标悬停在 Widget 上时，将显示标准工具提示。当鼠标悬停在 Widget 上时，如果存在错误状态，则会显示错误工具提示。

当设置验证器但其验证失败时，会出现错误状态。

### 示例

```cpp
#include <AzQtComponents/Components/Widgets/BrowseEdit.h>
#include <QInputDialog>
#include <QRegExpValidator>

// Set a custom attached button icon.
QIcon icon(":/stylesheet/img/help.svg");
browseEdit->setAttachedButtonIcon(icon);

// Enable the clear button.
browseEdit->setClearButtonEnabled(true);

// Require users to use the attached button or double-click the input box to open the selection window.
browseEdit->setLineEditReadOnly(true);

// Set the placeholder text.
browseEdit->setPlaceholderText("Enter an integer between 0 and 9999");

// Define and set the validator.
QRegExp intRangeExp("[0-9]\\d{0,3}$");
browseEdit->setValidator(new QRegExpValidator(intRangeExp, 0));

// Set the tooltip text.
browseEdit->setToolTip("Enter an offset value using the button.");
browseEdit->setErrorToolTip("Acceptable values are integers between 0 and 9999.");

// Connect the attached button to the QInputDialog function.
connect(browseEdit, &AzQtComponents::BrowseEdit::attachedButtonTriggered, this, [this]() {
    QString text = QInputDialog::getText(this, "Offset", "Enter an integer between 0 and 9999");
    if (!text.isEmpty())
    {
        browseEdit->setText(text);
    }
});
```

## 已禁用浏览编辑

![component browse edit disabled](/images/tools-ui/component-browse-edit-disabled.png)

在以下示例中，小组件及其功能已在代码中禁用。

### 示例

```cpp
#include <AzQtComponents/Components/Widgets/BrowseEdit.h>

// Disable the widget.
browseEdit->setEnabled(false);
```

## C++ API 参考

有关 **浏览编辑** API 的详细信息，请参阅 [O3DE UI 扩展 C++ API 参考](/docs/api/frameworks/azqtcomponents/namespace_az_qt_components.html) 中的以下主题：
+  [AzQtComponents::BrowseEdit](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_browse_edit.html)

## 相关链接

有关与 **browse edit** 组件相关的组件，请参阅以下主题：
+  [Line edit](./uidev-line-edit-component)
+  [Spinbox](./uidev-spinbox-component)
