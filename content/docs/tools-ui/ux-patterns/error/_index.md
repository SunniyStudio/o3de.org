---
linktitle: 错误处理
title: Blue Jay Design System 中的错误处理
description: 了解使用 Open 3D Engine （O3DE） 中的 Blue Jay 设计系统进行错误处理的模式，例如如何设计 UI 消息和通知，以及如何为 toast、内联通知、对话框、控制台日志、事件日志、事件表、组件卡和文本输入验证实施错误处理。
weight: 200
toc: true
---

通过查看本节中的原则和最佳实践，了解如何制作用户界面 （UI） 消息来帮助您的用户，而不是让他们感到沮丧。* UI 消息 * 指示组件的状态，例如错误、失败、警告、信息和成功消息。

本节中的页面介绍如何为各种类型的 UI 消息设置错误消息的样式，包括 toast、内联通知、对话框、控制台日志、事件日志、事件表、组件卡和文本输入验证。O3DE 的 UI 消息中的一致样式至关重要，因为它可以帮助用户更好地识别错误以及如何解决错误。

查看以下相关页面，了解如何设置特定于每种类型的 UI 消息的错误消息的样式：

|类型 |描述 | 
| - | - |
| [Console Log](./components/console-log) |  |
| [Dialogs](./components/dialogs) |  |
| [内联通知 ](./components/inline-notifications) |  |
| [Log Table](./components/log-table) |  |
| [Text Input Validation](./components/text-input-validation) |  |
| [Toasts](./components/toasts) |  |


## 先决条件

为了帮助你学习，建议你启动**Qt Control Gallery**，这是一个包含Qt小部件库中的UI元素的应用程序，允许你编程和预览其他元素。QT Control Gallery 是在构建 O3DE 时构建的，位于引擎的`build/<platform>/bin/<configuration>` 目录中。

有关更多信息，请参阅 [O3DE Qt Control Gallery 工具](/docs/tools-ui/uidev-control-gallery)。


## 标准图标

O3DE 附带的标准图标是 **错误**、**警告**、**成功**和 **信息**图标。它们向用户传达不同严重性级别的信息。

这些图标显示在各种 O3DE UI 系统上，例如 toast 消息、对话框和消息框。它们通常也显示在事件日志、控制台日志和组件卡中。您将在此页面上了解有关这些系统的更多信息。

### 错误 / 失败

![O3DE Standard Error or Failure Icon](/images/tools-ui/overview/standard-icons/error-or-failure.png)

指示进程失败，或者存在需要用户立即操作的错误---也就是说，用户可能受到严重影响，或处于潜在的不可逆状态。错误或故障可能是引擎的异常、功能故障或用户要求确认破坏性操作。

*用于：错误、失败、失败的进程、紧急情况、紧急警报*

{{< note >}}
在 O3DE 中，我们定义了两个不同的术语来描述问题：*error* 和 *failure*。误差是与实际输出和预期输出的偏差。故障是指系统无法执行所需的功能。
{{< /note >}}

### Warning

![O3DE Standard Warning Icon](/images/tools-ui/overview/standard-icons/warning.png)

表示需要用户采取纠正措施来防止严重故障的情况，或发生非严重错误的情况。警告并不表示对用户的直接影响，但它们建议用户执行预防措施以简化工作流程。

*用于：警告、不可用、注意、预防、不稳定*


### Success

![O3DE Standard Success Icon](/images/tools-ui/overview/standard-icons/success.png)

表示已成功完成流程和任务，或表示系统中不存在问题。当用户成功完成操作时，必须提供用户反馈，或者指示不需要立即执行用户操作。

*用于：成功、完成、稳定、活跃状态、验证、进度指示器*


### Information

![O3DE Standard Information Icon](/images/tools-ui/overview/standard-icons/information.png)

指示不需要用户操作的其他 （非关键） 信息。这可用于通知用户某些内容已准备好查看，例如用于系统反馈或表示自上次交互以来的更改。

*用于：信息、指导、例外*


## Semantic colors

在消息中，语义颜色表示消息的用途，用户可以通过颜色快速识别。例如，绿色具有积极的含义，因此它被用来传达成功或确认。

- Error: `#FA2727`
- Warning: `#FFAA22`
- Success: `#58BC61`
- Information: `#1E70EB`
- Information icon: `#FFFFFF`

![O3DE Message Colors](/images/tools-ui/overview/colors.png)
