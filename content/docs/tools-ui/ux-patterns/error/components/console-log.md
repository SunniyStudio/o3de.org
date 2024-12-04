---
linktitle: 控制台日志
title: 控制台日志中的错误消息
description: 了解如何使用 Open 3D Engine （O3DE） 中的 Blue Jay Design System （BJDS） 在控制台日志中设计错误/警告/成功/信息消息。
weight: 100
toc: true
---

*控制台日志* 显示一组 *控制台消息*，这些消息通知用户事件期间发生的错误或故障、警告和其他信息。控制台消息采用颜色编码，并带有其状态前缀，以帮助用户区分消息。

#### 例

例如，当您在 Open 3D Engine （O3DE） Editor（打开 3D 引擎 （O3DE） 编辑器）中打开项目时，控制台日志会告知您项目中不同系统和对象的状态。警告消息的颜色为橙色的变体，前缀为"\[Warning\]"，错误消息的颜色为红色的变体，前缀为"\[Error\]"。


![Example of console log with console messages](/images/tools-ui/console-log/console-log-messages.png)

## 规格

在创建控制台日志时，请查看以下规范：

- 控制台消息必须遵循以下格式：
  
  ```
  [<status>] (<system affected>) - [<number of problems>] - <problem>. <ways to resolve the problem>.
  ```
  - `<status>`: "Error", "Failure", or "Warning".
  - `<system affected>`: 发生错误、故障或警告的系统的名称。
  - `<number of problems>`: 量化单封邮件中出现的问题数（如果适用）。
  - `<problem>`: 描述问题。
  - `<ways to resolve the problem>`: 描述用户可以采取的解决或解决问题的可行步骤。

有关如何描述问题和解决问题的方法的帮助，请参阅 [编写错误消息的准则](../guidelines)。

- 对控制台消息进行颜色编码和前缀处理，具体取决于以下使用案例。

  ![Console Log decision table](/images/tools-ui/console-log/console-log-decision-table.png)

对文本使用以下颜色，具体取决于控制台消息的状态和控制台日志的主题：
  - 深色主题：
    - **错误/失败**: `#FA2727`
    - **警告**: `#FFAA22`
  - 浅色主题：
    - **错误/失败**: `#C80000`
    - **警告**: `#807000`

  ![Console Log - Error and Warning examples](/images/tools-ui/console-log/console-log-state-colors.png)

## 最佳实践

* 在控制台消息中仅使用纯文本。请勿使用富文本，例如图标和格式化文本。

* 使用控制台消息向用户显示被动通知。用户可能无法看到控制台消息。
  
* 连续显示多条相同状态的消息。例如，列出所有警告，然后列出所有错误/失败。


## 自定义主题

您可以自定义控制台日志的主题以符合您的偏好。有两个主题，**浅色**和默认的**深色**。
- 浅色主题背景色： `#FFFFFF`
- 深色主题背景色：`#111111`

![Console Log - Global Preferences](/images/tools-ui/console-log/global-preferences.png)

要自定义背景：

1. 在 O3DE 编辑器中，打开工具菜单中的 **Edit** 下拉菜单。

2. 选择 **Editor Settings** > **Global Preferences...** 打开 **Preferences** 对话框。

3. 在**General Settings** 组旁边的 **Console Background**，选择一个主题选项。

4. 点击 **OK** 保存设置。
