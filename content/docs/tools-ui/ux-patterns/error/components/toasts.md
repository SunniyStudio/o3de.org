---
linktitle: Toasts
title: Toast 中的错误消息
description: 了解如何使用 Open 3D Engine （O3DE） 中的 Blue Jay Design System （BJDS） 在 toast 中设计错误/警告/成功/信息消息。
weight: 600
toc: true
---

在 O3DE 中，*toast* 是无中断的通知，显示在窗口右下角页面的大多数元素上。它们是非干扰性的，因为它们可以显示在用户工作流的中间，而不会妨碍用户的操作。

以下示例演示了显示在 **O3DE Animation Editor** 窗口右下角的 toast。

![Example of a floating toast in the O3DE Animation Editor](/images/tools-ui/toasts/floating-toast-message-in-animation-editor.png)

## 使用标准图标

Toast 传达消息的意图，并且必须与相应的图标对应，以便为用户提供一致的体验。下图显示了不同消息意图的 toast：error/failure、warning、success 和 information。

请参阅下表，根据消息的用例确定要在 toast 中使用的图标。

![Floating Toasts - Decision Table](/images/tools-ui/toasts/floating-toasts-decision-table.png)

**示例**

![Floating Toasts - Messages](/images/tools-ui/toasts/floating-toasts-messages.png)


## 规格

在创建 Toast 时，请查看以下规范：

* Toast 是被动且无中断的消息。它们不应妨碍用户的工作流程。

* 编写清晰简洁的信息。Toast 最多只能显示两行。

* 吐司必须在 3 秒（或最多 5 秒）后消失。例外情况是，如果消息包含要求用户采取措施以使 Toast 消失的链接。

* 请勿堆叠多个 Toast，使它们在垂直或水平方向上并排放置，因为这可能会阻止用户的工作流程。相反，请按重要性顺序顺序显示多个 Toast。

* 使用 [标准图标](../#standard-icons) 列表中的以下图标之一：错误/失败、警告、成功或信息图标。

* Toast 必须具有固定宽度，并且不应扩展以适应内容区域。

* Toast 不能包含行动号召按钮。

* Toast 必须显示在页面内容上方和适合叠加层的屏幕上。

* 不得在模态中使用 Toast。考虑在较大的系统中使用它们，如 **O3DE Editor**, **Material Editor**, **Viewport**等。


![Floating Toast Messages - Best Practices](/images/tools-ui/toasts/floating-toast-messages-best-practices.png)


## UI 尺寸

| 属性            |规格 |
|---------------|--|
| Icon Size     | 24 x 24px |
| Padding       | 12px |
| Border Radius | 6px |
| Font          | Open Sans |
| Weight        | 400 |
| Line Height   | 16px |

![Floating Toast, marked up](/images/tools-ui/toasts/floating-toasts-message-markup.png)


## 带链接的 Toast

在字幕文本中包含链接的 Toast 必须执行以下操作：

* 与标准 Toast 不同，它会持续显示并且不会自动消失。

* 显示手动关闭按钮。

* 指向系统外部，而不是应用程序的其他部分，因为这可能会中断用户的工作流程。例如，您可以链接到外部 Web 地址：“在此处查看有关如何创建材质的教程， \<link\>."

![Floating Toasts - Messages with Links](/images/tools-ui/toasts/floating-toasts-messages-with-links.png)
