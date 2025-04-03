---
linkTitle: UI Canvas Proxy Ref
title: UI Canvas Proxy Ref 组件
description: 使用 UI Canvas Proxy Ref 组件在 Open 3D Engine （O3DE） 中的实体之间共享 UI 画布引用。
---

使用 UI Canvas Proxy Ref 组件，您可以引用由其他实体管理的 UI 画布。如果要将 UI 画布放置在 [**网格**](/docs/user-guide/components/reference/atom/mesh) 或 [**Actor**](/docs/user-guide/components/reference/animation/actor) 组件上，以便玩家可以在关卡中的多个位置与之交互，请将此组件与 [**UI Canvas on Mesh**](/docs/user-guide/components/reference/ui/canvas-on-mesh/) 组件结合使用。

## 用法

使用此组件通常是一种特殊情况，因为它支持在 3D 世界中的多个实体上显示相同的 UI 画布。**UI Canvas Proxy Ref** 组件允许它所在的实体像具有 [**UI Canvas Asset Ref**](/docs/user-guide/components/reference/ui/canvas-asset-ref/) 组件一样运行，但不必加载 UI 画布的另一个副本。这意味着，当用户与 3D 对象上的一个 UI 画布交互时，另一个 3D 对象会显示相同的更改。

下图显示了共享同一加载画布的三个实体。曲面平面实体具有 **UI Canvas Asset Ref** 组件，蛋和球体都具有 **UI Canvas Proxy Ref** 组件：

![Three entities with shared canvas](/images/user-guide/component/ui_canvas/component-ui-canvas-proxy-ref-screenshot.png)

有关更多信息，请参阅 [在 3D 世界中放置 UI 画布](/docs/user-guide/interactivity/user-interface/canvases/placing-canvases-3d)。

## 提供者 ##

[LyShine Gem](/docs/user-guide/gems/reference/ui/lyshine/)

## UI Canvas Proxy Ref 属性 

![UI Canvas Proxy Ref properties](/images/user-guide/components/reference/ui/ui-proxy-ref-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Canvas Asset Ref entity** | 选择具有 **UI Canvas Asset Ref** 组件的实体。 | EntityId | None |

## UiCanvasProxyRefBus

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `SetCanvasRefEntity` | 使用 UI 画布设置目标实体，以便与当前实体关联。 | Target Entity: EntityId, Current Entity: EntityId | None | Yes |

## UiCanvasRefNotificationBus

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `OnCanvasRefChanged` | Notifies listeners that the canvas reference has changed.通知侦听器画布引用已更改。 | None | Old Reference: EntityId, New Reference: EntityId | Yes |

有关更多信息，请参阅 [使用事件总线 （EBus） 系统](/docs/user-guide/programming/messaging/ebus/)。
