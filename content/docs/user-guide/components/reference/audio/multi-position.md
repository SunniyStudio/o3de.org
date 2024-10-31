---
linkTitle: Multi-Position Audio
title: Multi-Position Audio 组件
description: 使用Multi-Position Audio组件在 Open 3D Engine中的多个位置播放声音。
toc: true
---

Multi-Position Audio 组件有助于创建从多个位置发出的声音。例如，覆盖一个区域（如河流）的声音就应使用该组件。任何有多个实例的声音也应使用该组件，因为它有助于减少资源。

![The Multi-Position Audio component pane in the O3DE Editor.](/images/user-guide/component/audio/multi-position-component.png)

## Multi-Position Audio 属性

| 名称 | 说明 | 默认值 |
|------|-------------|---------|
| Entity References | 实体列表，其世界位置将被用作声音的多重位置。 | `<Empty>` |
| Behavior Type | 下拉选择，选项为 “Separate ”或 “Blended”。Separate表示声音位置将被视为单独的点声源声音。Blended 是指声音位置将被视为覆盖一个区域的一个声音。. | Separate |

## EBus请求事件接口

使用 EBus 接口的下列请求功能可与游戏的其他组件进行通信。

有关使用事件总线（EBus）接口的更多信息，请参阅 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus/)。

| 名称 | 说明 | 参数 | 返回值 | 可脚本化 | Lua 枚举绑定 |
|------|-------------|------------|--------|------------|-------------------|
| AddEntity | 向列表中添加一个实体，其位置将用于多位置音频。 | `entityId` - EntityId | None | Yes | `N/A` |
| RemoveEntity | 从列表中删除一个实体，其位置也将从多位置音频中删除。| `entityId` - EntityId | None | Yes | `N/A` |
| SetBehaviorType | 将多位置音频的行为类型设置为 `Blended` 或 `Separate`。 | `Audio::MultipositionBehaviorType` - enum | None | Yes | `MultiPositionBehaviorType_Separate`, `MultiPositionBehaviorType_Blended` |
