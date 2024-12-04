---
linktitle: 行编辑
title: O3DE UI 行编辑组件
description: 使用 O3DE UI 行编辑组件从用户那里捕获自由格式的文本。
toc: true
---

**行编辑** 组件是 Qt 和 O3DE UI 库提供的几种类型的输入框之一。使用行编辑组件使用户能够输入自由格式的文本。请注意字段的文本长度，并始终与有用、清晰的标签配对。

## 行编辑小部件剖析

**线条编辑** 小部件有几个自定义选项。标准功能包括以下元素：

![component line edit anatomy](/images/tools-ui/component-line-edit-anatomy.png)

1.  **Label**

    虽然从技术上讲不是 Widget 的一部分，但您应该在 UI 布局中为输入框指定一个标签。

1.  **Placeholder text**

    （可选）当小组件文本为空时，在 UI 中设置的或使用`setPlaceholderText()`设置的提示文本会显示在此处。

1.  **Input box**

    用户输入的文本或您使用`setText()`设置的文本将在此处显示。

1.  **Tooltip**

    （可选）如果您为小组件设置工具提示文本，它将出现在用户悬停位置附近。

    ![component line edit anatomy clear](/images/tools-ui/component-line-edit-anatomy-clear.png)

1.  **Clear button**

    （可选）如果启用小组件的清除按钮，它将在输入框不为空时显示。当用户选择清除按钮时，输入框将返回空值。

    ![component line edit anatomy error state](/images/tools-ui/component-line-edit-anatomy-error-state.png)

1.  **Error state indicator**

    （可选）如果为输入框设置了验证器，并且验证失败，则错误状态指示器图标会显示在输入框末尾的清除按钮之前。

1.  **Error tooltip**

    （可选）当存在错误状态时，如果已为小组件设置了错误消息，则它将出现在用户悬停位置附近。当存在错误状态时，此工具提示将代替正常的工具提示文本显示。如果未设置错误消息，则默认错误工具提示文本为“Invalid input”。

## 基本行编辑

![component line edit basic](/images/tools-ui/component-line-edit-basic.png)

一个简单的 **line edit** 组件以 `QLineEdit` Qt 小部件开始。您可以通过在 Qt Designer 或代码中更改窗口小部件设置来设置其他选项。

### 示例

```cpp
#include <QLineEdit>

// Create a new line edit widget.
QLineEdit* lineEdit = new QLineEdit(parent);

// Set the placeholder text.
lineEdit->setPlaceholderText("Hint text");

// Enable the clear button.
lineEdit->setClearButtonEnabled(true);

// Then add lineEdit to a UI layout as needed.
```

## Line edit with search

![component line edit search style](/images/tools-ui/component-line-edit-search-style.png)

`AzQtComponents::LineEdit`类提供了多个静态样式函数，用于将样式应用于`QLineEdit`小部件。一个示例是搜索样式，它添加了一个搜索图标。其他样式记录在[AzQtComponents::LineEdit](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_line_edit.html)的 API 参考中。

### 示例

```cpp
#include <AzQtComponents/Components/Widgets/LineEdit.h>
#include <QLineEdit>

// Add a search icon.
AzQtComponents::LineEdit::applySearchStyle(lineEdit);

// Listen for changes to the text by the user.
connect(lineEdit, &QLineEdit::textEdited, this, [](const QString& newText) {
    // Perform the search as the user is typing.
});
```

## 监听行编辑更改

以下示例演示了如何在 `QLineEdit` 小部件中侦听各种类型的文本更改。

### 示例

```cpp
#include <QLineEdit>

// Listen for changes to the text, either by the user or when setText() is called.
connect(lineEdit, &QLineEdit::textChanged, this, [](const QString& newText) {
    // Handle text change.
});

// Listen for changes to the text by the user.
connect(lineEdit, &QLineEdit::textEdited, this, [](const QString& newText) {
    // Handle when user changes the text.
});

// Listen for when the Return or Enter key has been pressed, or when the input box loses focus.
connect(lineEdit, &QLineEdit::editingFinished, this, [lineEdit]() {
    // New text can be retrieved from lineEdit->text().
});
```

## 将行编辑作为放置目标

![component line edit drop target style](/images/tools-ui/component-line-edit-drop-target-style.png)

以下示例演示了如何将放置目标样式应用于或删除 `QLineEdit` 构件。

### 示例

```cpp
#include <AzQtComponents/Components/Widgets/LineEdit.h>
#include <QLineEdit>

// To show the QLineEdit as a valid drop target:
AzQtComponents::LineEdit::applyDropTargetStyle(lineEdit, true);

// To show the QLineEdit as an invalid drop target:
AzQtComponents::LineEdit::applyDropTargetStyle(lineEdit, false);

// To clear the drop target style:
AzQtComponents::LineEdit::removeDropTargetStyle(lineEdit);
```

## Line edit with validator

![component line edit error state](/images/tools-ui/component-line-edit-error-state.png)

在以下示例中，已定义错误工具提示的验证程序和错误消息。当鼠标悬停在 Widget 上时，将显示标准工具提示。当鼠标悬停在 Widget 上时，如果存在错误状态，则会显示错误工具提示。

当设置验证器但其验证失败时，会出现错误状态。

### 示例

```cpp
#include <AzQtComponents/Components/Widgets/LineEdit.h>
#include <QLineEdit>
#include <QDoubleValidator>

// Define and set the validator.
auto validator = new QDoubleValidator(lineEdit);
validator->setNotation(QDoubleValidator::StandardNotation);
validator->setTop(4.0);
validator->setBottom(3.0);
lineEdit->setValidator(validator);

// Set the error tooltip text.
AzQtComponents::LineEdit::setErrorMessage(lineEdit, QStringLiteral("Value must be between 3.0 and 4.0"));
```

## 禁用的行编辑

![component line edit disabled](/images/tools-ui/component-line-edit-disabled.png)

在以下示例中，小组件及其功能已在代码中禁用。

### 示例

```cpp
#include <QLineEdit>

// Disable the widget.
lineEdit->setEnabled(false);
```

## C++ API 参考

有关 **行编辑** API 的详细信息，请参阅 [O3DE UI 扩展 C++ API 参考](/docs/api/frameworks/azqtcomponents/namespace_az_qt_components.html) 中的以下主题：
+  [AzQtComponents::LineEdit](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_line_edit.html)

相关的 Qt 文档包括以下主题：
+  [QLineEdit Class](https://doc.qt.io/qt-5/qlineedit.html)

## 相关链接

有关与 **line edit** 组件相关的组件，请参阅以下主题：
+  [Browse edit](./uidev-browse-edit-component)
+  [Spinbox](./uidev-spinbox-component)
