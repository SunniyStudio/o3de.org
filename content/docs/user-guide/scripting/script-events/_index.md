---
title: Script Events
description: 使用脚本事件使实体能够在 Open 3D Engine （O3DE） 中相互通信。
weight: 200
---

O3DE 中的脚本是围绕事件驱动的范例设计的。您可以使用事件在解耦的环境中发送和接收信息并执行操作，而不是直接从给定实体或其组件之一访问信息。

与实体和组件一样，脚本可以使用事件相互通信。这些事件称为脚本事件。

要编写脚本事件，请使用 Asset Editor 或 Lua。Script Canvas 和 Lua 可以发送或接收您创建的事件。在 Script Canvas 中，您可以添加发送或接收脚本事件的节点。从 Script Canvas 发送的事件可以在 Lua 中处理。同样，从 Lua 发送的事件也可以在 Script Canvas 中处理。

## 先决条件

要使用脚本事件，您必须在项目中启用[Script Events Gem](/docs/user-guide/gems/reference/script/script-events) 。
