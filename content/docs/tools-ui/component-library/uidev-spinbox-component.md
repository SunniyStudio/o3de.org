---
linktitle: Spinbox
title: O3DE UI Spinbox 组件
description: 了解旋转框的 O3DE UI 样式，包括旋转框和双旋转框组件。
toc: true
---

使用旋转框作为数字编辑组件，使用户能够使用各种控件在输入框中向上或向下“旋转”数值。该值将按步长值中指定的量进行更改。

使用 `SpinBox` 类来保存有符号整数值，使用  `DoubleSpinBox` 类来保存双精度值。

{{< note >}}
在您将使用旋转框的情况下，还可以考虑使用 [滑块组合](/docs/tools-ui/component-library/uidev-sliders-component)  小部件，它将滑块的额外视觉提示与旋转框的轻松调整相结合。
{{< /note >}}

## 旋转框小部件剖析

Spinbox 为用户提供了各种用于输入或更改其数值的控件。

![component spinbox anatomy](/images/tools-ui/component-spinbox-anatomy.png)

1.  **输入框值**

    输入框区域中的当前值是可编辑的。

1.  **递增和递减按钮**

    用户单击递增或递减按钮以按步长量调整数值。

1.  **旋转控制**

    当指针靠近输入框的边缘时，将显示旋转控件。这是用户按步长调整当前值的更快方法。用户在按下鼠标按钮的同时沿其中一个箭头的方向移动时，会不断更改该值。

1.  **当前值指示器**

    当用户将鼠标悬停在组件上 1 秒钟时，当前值将显示在此对话框中。与在输入框编辑区域中显示的值不同，此处显示的值不会被截断。

## 基本 spinbox

![component spinbox basic](/images/tools-ui/component-spinbox-basic.png)

下面的示例演示了如何创建一个简单的双旋转框。

### 示例

```cpp
#include <AzQtComponents/Components/Widgets/SpinBox.h>

// Create a new double spinbox widget.
AzQtComponents::DoubleSpinBox* doubleSpinBox = new AzQtComponents::DoubleSpinBox(parent);

// Set its range from 0.0 to 20.0 and its initial value to 15.0.
doubleSpinBox->setRange(0.0, 20.0);
doubleSpinBox->setValue(15.0);

// Set the step value to 0.1.
doubleSpinBox->setSingleStep(0.1);
```

## C++ API参考

有关 **spinbox** API 的详细信息，请参阅 [O3DE UI 扩展 C++ API 参考](/docs/api/frameworks/azqtcomponents/namespace_az_qt_components.html) 中的以下主题:
+  [AzQtComponents::SpinBox](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_spin_box.html)
+  [AzQtComponents::DoubleSpinBox](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_double_spin_box.html)

相关的 Qt 文档包括以下主题：
+  [QSpinBox Class](https://doc.qt.io/qt-5/qspinbox.html)
+  [QDoubleSpinBox Class](https://doc.qt.io/qt-5/qdoublespinbox.html)
