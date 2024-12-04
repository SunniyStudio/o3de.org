---
linktitle: Inline Notifications
title: Error Messages in Inline Notifications
description: Learn how to design Error/Warning/Success/Information messages in inline notifications using the Blue Jay Design System (BJDS) in Open 3D Engine (O3DE).
weight: 300
toc: true
---

内嵌通知是仅限于界面内特定区域的非中断消息。内联通知用于向用户显示即时反馈。它们经常用作 toast 的替代方法，与日志表结合使用，以及在组件卡中。

![Inline Toast Messages](/images/tools-ui/inline-notifications/inline-toast-messages.png)

## 规格

在创建内嵌通知时，请查看以下规范：

* 持续显示内嵌通知，直到用户将其关闭或问题得到解决。

* 编写清晰简洁的信息。建议使用一行。

* 启动内联通知以响应用户在工作流中的操作，或由系统响应。

* 包含与消息相关的可选行动号召按钮。

- 使用 [标准图标](../#standard-icons)：错误/失败或警告图标。

* 仅在具有多个操作目的的屏幕上显示内嵌通知。例如，它们可以出现在非模态对话框中，但不能出现在模态对话框中。

* 不要使用内嵌通知消息代替日志文件。


## UI 尺寸

| 属性          |规格|
|-------------|---|
| Icon Size   | 24 x 24px |
| Padding     | 10px |
| Fill        | Specific color opacity: 10% |
| Border      | Specific Color |
| Font        | Open Sans |
| Weight      | 400 |
| Size        | 12px |
| Line Height | 20px |

![Inline Toast - Markup](/images/tools-ui/inline-notifications/inline-toast-markup.png)


## 最佳实践

* 将内嵌通知放在内容顶部，以免它们覆盖任何内容。

* 不要堆叠多个内嵌通知。相反，应按顺序按优先级顺序一次显示一个，而不会阻止任何用户操作。

* 消息必须具有描述性，并指导用户明确的后续步骤。

* 使用内嵌通知将用户的焦点从当前界面重定向到消息上。

* 消息可以包含指向系统外部的帮助程序链接，而不是指向应用程序的其他部分。

![Inline Error with CTA](/images/tools-ui/inline-notifications/inline-error-with-cta.png)


## 多行消息

建议将内嵌通知消息放在一行中。但是，如果它们适合多行，请确保满足以下条件：

* 使用项目符号列表来显示问题列表。列表有助于将用户注意重要信息。
* 在非模式对话框中，指导用户执行后续步骤以及如何解决问题。

![Inline Toast - Multi-line](/images/tools-ui/inline-notifications/inline-toast-multiline.png)


## 组件卡片中的内联通知

组件卡中的内联通知显示以指示错误/失败或警告状态。内嵌消息会告知用户如何恢复或解决问题。

![Inline Error in Component Cards](/images/tools-ui/inline-notifications/inline-notifications-in-component-cards.png)

### 规格

* 在问题得到解决之前，在组件卡中持续显示内嵌通知。

* 避免水平或垂直堆叠多个内联消息。相反，在用户解决每个问题时一个接一个地显示这些消息。

* 编写清晰简洁的信息。建议最多使用两行。

* 使用标准图标列表中的错误/失败或警告图标，具体取决于以下使用案例。请参阅 [标准图标](../#standard-icons). 

    ![Inline Error in Component Cards](/images/tools-ui/inline-notifications/inline-notifications-in-component-cards-decision-table.png)

* 仅在必要时添加行动号召按钮。

* 在组件卡的内容上方显示内嵌通知。
  
* 组件卡的标题具有灰色的警告图案背景，并显示图标。这指示组件卡的状态，即使它已折叠。

### UI 尺寸

| 属性          |规格 |
|-------------|---|
| Icon size   | 24 x 24px |
| Padding     | 8px |
| Fill        | Warning color opacity: 10% |
| Border      | Warning color code |
| Font        | Open Sans |
| Weight      | 400 or 600 |
| Size        | 12px |
| Line weight | 16px |

![Inline Messages in Component Cards - Markup](/images/tools-ui/inline-notifications/inline-messages-in-component-cards-markup.png)
