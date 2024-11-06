---
linkTitle: White Box Collider
title: 'White Box Collider 组件'
description: ' 使用 White Box Collider （白盒碰撞器） 组件将 PhysX 碰撞添加到 Open 3D Engine （O3DE） 中的白盒网格。 '
---




您可以通过将 **White Box Collider** 组件添加到具有 **White Box** 组件网格的实体，在 **Open 3D Engine （O3DE）** 中启用白盒网格上的碰撞。White Box Collider （白盒碰撞器） 组件支持碰撞层和物理材质。它可以与静态和运动白色盒体网格一起使用。White Box Collider （白盒碰撞器） 组件使用白盒网格作为碰撞表面。与 **PhysX Primitive Collider** 或 **PhysX Mesh Collider** 组件不同，无需指定碰撞形状或提供 PhysX 网格资产。

![White Box static collider.](/images/user-guide/components/reference/shape/white-box-collider-A.gif)

在上图中，White Box Collider （白盒碰撞器） 组件被添加到具有静态 White Box （白盒） 组件的实体中。您可以在编辑白盒网格后立即测试碰撞的变化。

## 提供者

[White Box Gem](/docs/user-guide/gems/reference/design/white-box)

## 依赖

[White Box component](./white-box)

## White Box Collider 属性 

![White Box Collider component interface.](/images/user-guide/components/reference/shape/white-box-collider-component-ui-01.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Collision Layer** | 分配给碰撞器的碰撞层。有关详细信息，请参阅 [碰撞图层](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-collision-layers/). || `Default` |
| **Collides With** | 包含此碰撞器与之碰撞的图层的碰撞组。有关详细信息，请参阅 [碰撞组](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-collision-groups/). || `All` |
| **Physics Materials** | 为此白盒碰撞器选择物理材质。 | A `.physxmaterial` asset assigned. | `(default)` |
| **Tag** | 为此碰撞器设置标签。标签可用于快速识别脚本或代码中的组件。 | Crc32 | None |
| **Rest offset** | 实体将按其 **Rest offset** 值的总和分隔。必须小于 **Contact offset**。 | -Infinity to 50.0 | `0.0` |
| **Contact offset** | 当实体位于其 **Contact offset** 值之和内时，将开始生成触点。 必须大于 **Rest offset** | 0.0 - 50.0 | `0.02` |
  | **Body Type** | 为非移动实体选择 '`Static`'。为动画实体选择 '`Kinematic`'。必须将 White Box collider （白盒碰撞器） 设置为 '`Static`' （静态） ） 才能与 **PhysX Character Controller （PhysX 角色控制器）** 交互。 | Static, Kinematic | `Static` |
