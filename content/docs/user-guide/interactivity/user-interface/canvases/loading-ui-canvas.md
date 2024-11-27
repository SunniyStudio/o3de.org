---
linkTitle: 加载 UI 画布
description: ' 了解如何在运行时使用 Open 3D Engine （O3DE） 加载 UI 画布。 '
title: 加载 UI 画布
weight: 800
---

使用 **UI 编辑器** 创作 UI 画布后，可以使用以下方法之一在运行时加载它。

- 使用 **UI Canvas Asset Ref** 组件.
    [UI Canvas Asset Ref component](/docs/user-guide/components/reference/ui/canvas-asset-ref/) 引用 UI Canvas 资产，并有一个选项，用于在组件的实体激活时自动加载 UI Canvas。

- 使用 **Load Canvas** Script Canvas 节点.
    使用加载画布 [Script Canvas 节点](/docs/user-guide/scripting/script-canvas/editor-reference/nodes/)按路径名加载 UI 画布。将节点添加到分配给关卡中实体上的 **Script Canvas** 组件的 Script Canvas 图形中。

- 使用 **UI Canvas Manager** EBus.
    UI 画布管理器 [EBus](/docs/user-guide/programming/messaging/ebus) 包含按路径名加载画布和按实体 ID 卸载画布的方法。在分配给关卡中实体上的 **Lua 脚本** 组件的 [Lua 脚本](/docs/user-guide/scripting/lua/) 中使用此事件总线。

以下示例显示了如何在 Lua 脚本中使用 UI Canvas Manager 总线。

```lua
-- Load UI canvas
local canvasEntityId = UiCanvasManagerBus.Broadcast.LoadCanvas("UI/Canvases/MyCanvas.uicanvas")

-- Unload UI canvas
UiCanvasManagerBus.Broadcast.UnloadCanvas(canvasEntityId)
```

以下示例显示了如何在 C++ 中使用 UI Canvas Manager 总线。

```cpp
// Load UI canvas
AZ::EntityId canvasEntityId;
UiCanvasManagerBus::BroadcastResult(canvasEntityId, &UiCanvasManagerInterface::LoadCanvas, "UI/Canvases/MyCanvas.uicanvas");

// Unload UI canvas
UiCanvasManagerBus::Broadcast(&UiCanvasManagerInterface::UnloadCanvas, canvasEntityId);
```
