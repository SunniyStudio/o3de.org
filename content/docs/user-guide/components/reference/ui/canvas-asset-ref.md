---
linkTitle: UI Canvas Asset Ref
title: UI Canvas Asset Ref 组件
description: 使用 Open 3D Engine （O3DE） 中的 UI Canvas Asset Ref 组件将 UI 画布与关卡中的组件实体相关联。
---

使用 **UI Canvas Asset Ref** 组件，您可以将 UI 画布与实体相关联。

## 用法

在 **Canvas pathname** 属性字段中，输入项目中的`.uicanvas`资源的名称。然后，从自动填充的下拉列表中选择要与此组件关联的画布。 或者，单击 {{< icon browse-edit-select-files.svg >}} 按钮，然后从文件浏览器中选择`.uicanvas`资源。

选择 UI 画布后，您可以设置要自动加载的其他属性，并选择是以启用还是禁用状态加载它。

使用 UI Canvas Asset Ref 节点在 Script Canvas 中引用画布。

如果要将 UI 画布放置在用户可以与之交互的 3D 网格上，请将此组件与 [**UI Canvas on Mesh**](/docs/user-guide/components/reference/ui/canvas-on-mesh) 组件结合使用。有关更多信息，请参阅 [在 3D 世界中放置 UI 画布](/docs/user-guide/interactivity/user-interface/canvases/placing-canvases-3d)。

## 提供者

[LyShine Gem](/docs/user-guide/gems/reference/ui/lyshine)

## UI Canvas Asset Ref 属性 

![UI Canvas Asset Ref properties](/images/user-guide/components/reference/ui/ui-canvas-asset-ref-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Canvas pathname** | 选择要加载的画布资源。 | `.uicanvas` | None |
| **Load automatically** | 如果启用，则当此组件激活时，画布将自动以启用状态加载。  | Boolean | `Disabled` |
| **Load in disabled state** | 如果启用，画布将自动加载，但处于禁用状态。画布必须先启用，然后才能显示。<br> <br>仅当 **Load automatically** 设置为 '`Enabled`' 时，此属性才可用。 | Boolean | `Disabled` |

## UiCanvasRefBus

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetCanvas` | 返回在 **Canvas pathname** 属性中设置的 UI Canvas 资产的名称。 | None | Name: String | Yes |

有关更多信息，请参阅 [使用事件总线 （EBus） 系统](/docs/user-guide/programming/messaging/ebus/)。
