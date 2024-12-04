---
linktitle: 进度指示器
title: O3DE UI 进度指示器
description: 了解 O3DE UI 进度指示器，包括微调器和进度条组件。
toc: true
---

使用进度和状态指示器向用户传达 O3DE 应用程序正在处理某个进程，以及该进程完成时的结果。当用户可能想知道某个进程是否正常工作或挂起时，应使用指示器。

![component progress indicators style](/images/tools-ui/component-progress-indicators-style.png)

## 使用指南

在设计带有进度指示器的 UI 时，请遵循以下准则：

1. 当预期延迟为 3 毫秒或更长时间时显示进度。

1. 在确定何时何地展示进展时，考虑上下文和流程。

1. 进度指示器以动画形式显示，以强化活动正在发生。

{{< note >}}
请参阅以下部分中的其他使用指南，这些指南适用于特定类型的进度指示器。
{{< /note >}}

使用进度指示器时，请避免以下设计选择：
+ 不要一次使用多个进度指示器。

## 基本进度条

![component progress bar determinate](/images/tools-ui/component-progress-bar-determinate.png)

将确定进度显示为线性进度条，以显示具有明确开始和结束的流程或任务。在此方案中，系统知道采取了多少步骤，以及完成时间的可能性。

如果系统出现故障或没有这些数据点中的任何一个，请考虑使用微调器。

进度条的其他使用准则包括：

1. 如果可能，请使用文本向用户清楚地显示系统正在工作。对于特别长的流程，请考虑使用文本组合来指示它们在流程中的位置，如上图所示。

1. 如果可用，请在进度条下显示数字计数。

1. 对于对话框中的进度条，请提供一个按钮来取消该过程。

下面的示例演示了简单进度条的初始化。请参阅 [QProgressBar](https://doc.qt.io/qt-5/qprogressbar.html)上的 Qt 文档以了解其他功能。

### 示例

```cpp
#include <QProgressBar>

// Create the progress bar.
QProgressBar* progressBar = new QProgressBar(parent);

// Set the numeric range of the bar.
progressBar->setRange(0, 36);

// Set the current progress value.
progressBar->setValue(12);

// Hide the progress text, as specified by the UX spec. It defaults to on, so it should be turned off.
progressBar->setTextVisible(false);
// Note that it can also be set from the .ui file, or from Qt Designer or Creator.
```

## 基本进度微调器

![component progress spinner basic](/images/tools-ui/component-progress-spinner-basic.gif)

当不清楚该过程何时完成时，请使用微调器。

微调器的其他使用准则包括：

1. 在窗口、面板、列表的上下文中显示微调器，或与其他元素内联。

### 示例

```cpp
#include <AzQtComponents/Components/StyledBusyLabel.h>

// Create the spinner.
AzQtComponents::StyledBusyLabel* spinner = new AzQtComponents::StyledBusyLabel(parent);

// Set the spinner icon size.
spinner->SetBusyIconSize(18);

// Start the spinner.
spinner->SetIsBusy(true);
```

## C++ API参考

有关 **进度指示器** API 的详细信息，请参阅 [O3DE UI 扩展 C++ API 参考](/docs/api/frameworks/azqtcomponents/namespace_az_qt_components.html) 中的以下主题：
+  [AzQtComponents::ProgressBar](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_progress_bar.html)
+  [AzQtComponents::StyledBusyLabel](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_styled_busy_label.html) (spinner)

相关的 Qt 文档包括以下主题：
+  [QProgressBar](https://doc.qt.io/qt-5/qprogressbar.html)
