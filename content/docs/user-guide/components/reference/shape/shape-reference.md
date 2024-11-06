---
linktitle: Shape Reference
title: Shape Reference 组件
description: 使用 Shape Reference （形状参考） 组件在 Open 3D Engine （O3DE） 关卡中引用和重复使用形状组件。
weight: 100
---

添加 **Shape Reference** 组件以引用和重用 **Open 3D Engine （O3DE）** 关卡中的形状组件。

## 提供者

[O3DE Core (LmbrCentral) Gem](/docs/user-guide/gems/reference/o3de-core)

## Sphere Shape 属性

![Shape Reference component properties](/images/user-guide/components/reference/shape/shape-reference-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Shape Entity Id** | 选择具有有效 Shape 组件的实体。 | Entity Id | None |


## ReferenceShapeRequestBus

将以下请求函数与 '`ReferenceShapeRequestBus`' 事件总线接口结合使用，以便与游戏中的 Shape Reference 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetShapeEntityId` | 返回 **Shape Entity ID**的值。 | None | Entity Id | Yes |
| `SetShapeEntityId` | 设置 **Shape Entity ID**的值。 | Entity Id | None | Yes |
