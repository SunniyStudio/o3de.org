---
linktitle: 日志表
title: Log Table 中的错误消息
description: 了解如何在 Open 3D Engine （O3DE） 中使用 Blue Jay Design System （BJDS） 在日志表中设计错误/警告/成功/信息消息。
weight: 400
toc: true
---

*日志表* 用于传达连续发生的长流通知更新。日志表中的每个项目都附有一个状态指示器，该指示器会通知用户需要用户注意的项目。这些经常用于发出错误/故障、警告或信息的信号。

例如，下图显示了一个日志表，其中 Asset Processor 中的每个项目旁边都有状态指示器。

![Event Log](/images/tools-ui/log-table/event-log.png)

日志表可能包含两个部分。在第一部分中，显示错误的表将显示一个辅助框，该框允许用户扩展特定错误以获取其完整详细信息。本节的重要性在于它为用户提供了足够的信息来理解错误。第二部分包含一个表，该表显示用户如何解决错误或指向帮助文档的链接。
