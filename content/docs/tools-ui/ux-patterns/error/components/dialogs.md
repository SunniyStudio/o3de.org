---
linktitle: 对话框
title: 对话框中的错误消息
description: 了解如何使用 Open 3D Engine （O3DE） 中的 Blue Jay Design System （BJDS） 在对话框中设计错误/警告/成功/信息消息。
weight: 200
toc: true
---

*对话框* 通知用户特定任务，并要求用户立即响应。它还可以包含其他相关信息或请求用户输入。对话框对用户具有高度破坏性，必须谨慎使用。对话框可能由用户的操作或系统提示，以请求用户输入或向用户提供有关其工作流程的关键信息。


## 对话框的结构

1. **Header**: 包含标题和关闭按钮。

2. **Close Button**: 关闭对话框而不解决问题。

3. **Icon**: 标准图标，大小为 48 x 48 像素。请参阅 [标准图标](../#standard-icons). 

4. **Body**: 包含有关用户任务以及如何解决该任务的信息。

5. **Actions**: 解决或退出对话框的主要和可选的辅助行动号召按钮。

![Dialog Structure](/images/tools-ui/dialogs/dialog-structure.png)


## 对话框的类型

|类型 |用法 |
|--- |--- |
|模态 |显示关键信息或请求用户提供必要的输入。|
|非模态 |显示相关信息或请求用户的非关键输入。完成用户工作流可能不需要此任务。|

![Modal and non-modal dialogs](/images/tools-ui/dialogs/modal-dialog-and-non-modal-dialog.png)


## 规格

创建对话框时，请查看以下规范：

* 请勿在对话框的标题中包含 O3DE 徽标标志。

* 在对话框标题的标题中简要描述对话框的任务或目的。

* 在对话框的标题和正文中编写相关信息以帮助用户完成对话框的任务。请参阅 [编写错误消息的准则](../guidelines).


* 仅包含一个图标（如果有）。使用 [标准图标](../#standard-icons) 列表中的以下图标之一： 错误/失败、警告、成功或信息图标。

* 包括用于对话框操作的主按钮和/或可选的辅助按钮。
  
  * 使用 primary 按钮执行主要操作。主按钮具有蓝色背景，使其对用户更加明显。在主按钮上编写可操作的文本，例如“保存”、“重新启动”或“打开”。

* 使用辅助按钮进行替代选项或被动操作，例如 “取消” 或 “确定”。



## 最佳实践

* 请谨慎使用对话框。对话框具有破坏性，如果使用不当或重复使用，可能会使用户感到烦恼。

* 使用模式对话框停止用户的工作流，直到他们解决对话框。

* 使用非模式对话框作为工作流的积极强化的一部分。

* 仅当消息要求用户执行操作时，才使用对话框。


## 示例

#### 没有图标的对话框

对于非模式对话框，建议使用不带图标的对话框。

![O3DE dialog without icon](/images/tools-ui/dialogs/dialog-without-icon.png)


### 具有一个操作的对话框

具有一个操作的对话框可以使用主按钮或辅助按钮。如果用户接受或同意某项内容，或者对话框纯粹是信息性的，则建议这样做。

![O3DE dialogs with one action, primary](/images/tools-ui/dialogs/dialog-with-one-action-primary.png)

![O3DE dialog with one action, secondary](/images/tools-ui/dialogs/dialog-with-one-action-secondary.png)


### 包含两个操作的对话框

当使用包含两个操作的对话框时，请将主按钮放在左侧，将辅助按钮放在右侧。

![O3DE dialog with two actions](/images/tools-ui/dialogs/dialog-with-two-actions.png)


### 包含多条消息的对话框

您可以使用表将多个错误消息合并到一个对话框中。当用户完成一组操作并可以评估可能同时发生的错误时，请考虑使用此方法。由于对话框会破坏用户的工作流程，因此显示的对话框越少越好。

![O3DE dialog with a log of errors](/images/tools-ui/dialogs/dialog-with-multiple-messages-log.png)

您还可以在可折叠的表中隐藏错误消息列表，以便用户可以根据需要选择查看详细信息。

![O3DE dialog with a collapsible table of log of errors, closed state](/images/tools-ui/dialogs/dialog-with-multiple-messages-closed.png)

链接到与问题相关的日志文件（如果适用）。这允许用户解决他们的问题并加快他们的工作流程。

![O3DE dialog with a log of errors with links to their log files.](/images/tools-ui/dialogs/dialog-with-multiple-messages-open.png)

### 具有长消息字符串的对话框

错误消息可能需要描述许多详细信息才能对用户有效。在这种情况下，请考虑使用带有滚动条的对话框。

![O3DE dialog with a long message](/images/tools-ui/dialogs/dialog-with-long-message.png)
