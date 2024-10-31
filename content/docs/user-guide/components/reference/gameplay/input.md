---
linkTitle: Input
title: Input 组件
description: 使用Input组件将原始输入绑定到Open 3D Engine (O3DE)中的事件。
---

使用 **Input** 组件将原始输入与游戏中的事件绑定。Input组件引用一个`.inputbindings`文件，该文件将一组输入（如来自鼠标、键盘、游戏控制器的输入）绑定到一个事件。

## 提供方

[Starting Point Input Gem](/docs/user-guide/gems/reference/input/starting-point-input)

## Input 属性

![Input component properties](/images/user-guide/interactivity/input/input-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Input to event bindings** | 引用`.inputbindings`文件，该文件定义了原始输入与事件的绑定。 | Inputbinding 资产 | None |
| **Local player index** | 设置接收输入事件的播放器索引。如果设置为 `-1`，此Input组件将接收来自所有控制器的输入事件。如果设置为`0` 到 `3`，此Input组件将接收来自单个控制器的输入。 | -1 到 3 | `-1` |

{{< note >}}
**Local player index**属性仅在**Local player index***与 `Local user Id`相对应的平台（如 PC）上有效。 对于其他平台，必须在运行时使用已登录用户的 ID 调用 `SetLocalUserId`。
{{< /note >}}
