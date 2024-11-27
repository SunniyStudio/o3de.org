---
description: 使用 Open 3D Engine 的 PhysX 系统创建逼真的物理效果，例如碰撞检测和刚体动力学模拟。 
linktitle: NVIDIA PhysX
title: 使用 PhysX 系统模拟物理行为
weight: 100
---


O3DE 的 PhysX 系统作用于实体以创建逼真的物理效果，例如碰撞检测和刚体动力学模拟。

**主题**
+ [PhysX Gems](#physx-gems)
+ [PhysX 版本支持](#physx-version-support)
+ [PhysX 组件](#physx-components)
+ [PhysX 配置](#physx-configuration)
+ [物理材质](#physics-materials)
+ [PhysX 调试](#physx-debugging)
+ [配置PhysX系统](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/))
+ [PhysX场景模拟](/docs/user-guide/interactivity/physics/nvidia-physx/scene-queries/)
+ [PhysX 模拟体](/docs/user-guide/interactivity/physics/nvidia-physx/simulated-bodies/)
+ [使用 PhysX 的动态关节](/docs/user-guide/interactivity/physics/nvidia-physx/joint-intro/)
+ [调试 PhysX](/docs/user-guide/interactivity/physics/debugging/)
+ [PhysX 最佳实践](/docs/user-guide/interactivity/physics/nvidia-physx/best-practices/)
+ [使用 NVIDIA Cloth 模拟布料](/docs/user-guide/interactivity/physics/nvidia-cloth/)
+ [确定性](#determinism)
<!-- + [使用 NVIDIA Blast 进行模拟破坏](/docs/user-guide/interactivity/physics/nvidia-blast/) -->


## PhysX Gems 

PhysX 系统使用以下 Gem，您可以在 Project Manager 中启用这些 Gem。

+ **[PhysX](/docs/user-guide/gems/reference/physics/nvidia/physx/)** - 提供 [NVIDIA PhysX 4 SDK](https://developer.nvidia.com/physx-sdk) 与 O3DE 的集成。提供的集成包括一套组件、通过 **O3DE 编辑器**进行配置、Script Canvas 集成、**PhysX Visual Debugger** 集成以及一个简化的游戏 API 抽象层。

  有关更多信息，请参阅 [PhysX](/docs/user-guide/gems/reference/physics/nvidia/physx/).
+ **[PhysX 调试](/docs/user-guide/gems/reference/physics/nvidia/physx-debug/)** - 提供 PhysX 场景几何体的调试可视化效果，您可以使用控制台命令和其他工具启用这些几何体。

  有关更多信息，请参阅 [PhysX 调试](/docs/user-guide/gems/reference/physics/nvidia/physx-debug/).

## PhysX版本支持

O3DE 默认使用 PhysX 4.1。在配置和构建项目或引擎时，您可以通过在配置步骤中将`-DAZ_USE_PHYSX5=ON`指定为命令行选项来启用 PhysX 5.1。以下是启用 PhysX 5.1 的示例配置命令。

```cmd
cmake -B build/windows -S . -G "Visual Studio 16" -DLY_3RDPARTY_PATH=C:\o3de-packages -DAZ_USE_PHYSX5=ON
```

有关配置和构建项目的更多信息，请参阅 [配置和构建](/docs/user-guide/build/configure-and-build/) 主题。

## PhysX 组件 

**PhysX** gem 具有以下组件，您可以[添加](/docs/user-guide/components/reference/#adding-components-to-an-entity) 到实体，使用[**Entity Inspector**](/docs/user-guide/editor/entity-inspector/):
+ **[PhysX Primitive Collider](/docs/user-guide/components/reference/physx/collider/)** - 使物理对象能够使用基元形状与其他物理对象碰撞。
+ **[PhysX Mesh Collider](/docs/user-guide/components/reference/physx/mesh-collider/)** - 使物理对象能够使用 PhysX 资产定义的形状与其他物理对象发生碰撞。
+ **[PhysX Shape Collider](/docs/user-guide/components/reference/physx/shape-collider/)** - 允许物理对象使用由 **[Shape 组件](/docs/user-guide/components/reference/shape/)** 定义的几何体与其他物理对象发生碰撞.
+ **[PhysX Force Region](/docs/user-guide/components/reference/physx/force-region/)** - 使实体能够指定对实体施加物理力的区域。对于每个物理模拟帧，该组件将力应用于区域边界内的实体。
+ **[PhysX Static Rigid Body](/docs/user-guide/components/reference/physx/static-rigid-body/)** - 使不可移动的实体成为物理模拟的一部分。静态刚体可以与其他模拟刚体发生碰撞。
+ **[PhysX Dynamic Rigid Body](/docs/user-guide/components/reference/physx/rigid-body/)** - 使可移动实体成为物理模拟的一部分。Dynamic Rigid body 类型可以是 **kinematic** 或 **simulated**。模拟刚体响应与其他刚体的碰撞事件。运动学刚体不受外力和重力的影响;他们的运动是由脚本驱动的。
+ **[PhysX Character Controller](/docs/user-guide/components/reference/physx/character-controller/)** - 实现与物理世界的基本角色交互。例如，它可以控制与斜坡和台阶的交互，管理与其他角色的交互，并防止角色穿过墙壁或穿过地形。
+ **[PhysX Character Gameplay](/docs/user-guide/components/reference/physx/character-gameplay/)** - 为可能需要特定于游戏进行调整的角色控制器行为提供示例实现，例如检测角色是否在地面上、与重力交互以及与运动学体和其他控制器交互的行为。
+ **[PhysX Ragdoll](/docs/user-guide/components/reference/physx/ragdoll/)** - 启用某些角色行为的动画。物理表示通常是刚体的分层集合，这些刚体具有由关节连接的简单形状。
+ **[PhysX Ball Joint](/docs/user-guide/components/reference/physx/ball-joint/)** - 创建一个动态球形关节，该关节将实体约束到关节中，并可以自由地围绕关节的 y 轴和 z 轴旋转。
+ **[PhysX Fixed Joint](/docs/user-guide/components/reference/physx/fixed-joint/)** - 创建一个动态固定关节，该关节将实体约束到关节，在任何轴上都没有自由度。
+ **[PhysX Hinge Joint](/docs/user-guide/components/reference/physx/hinge-joint/)** - 创建动态铰链关节，将实体约束到关节，并可以自由地围绕关节的 x 轴旋转。
+ **[PhysX Prismatic Joint](/docs/user-guide/components/reference/physx/prismatic-joint/)** - 创建一个动态棱柱形关节，将实体约束到关节，保持相同的旋转，但允许它沿一个轴自由移动。

## PhysX Configuration 

使用 O3DE Editor 中的 **PhysX Configuration** 窗口配置全局设置、碰撞层、碰撞组和 PhysX Visual Debugger 设置。

有关更多信息，请参阅 [Configuring the PhysX System](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/).

## 物理材质

物理材质允许按实体配置模拟属性。材质可自定义对象在撞击表面时的反应方式，并控制摩擦力和弹性等质量。使用 **Asset Editor** 创建物理材质，然后将材质分配给碰撞器。

有关更多信息，请参阅 [Physics materials](/docs/user-guide/interactivity/physics/nvidia-physx/materials/).

## PhysX 调试

要验证模拟世界中交互的实现，可以使用以下工具。
+ **PhysX Debug gem** - 如果您是开发人员或技术美工人员，建议使用 PhysX Debug Gem。您可以使用此工具在 O3DE Editor 的编辑器模式或游戏模式下实时查看物理世界。要激活该工具，请使用控制台命令或即时模式图形用户界面 （ImGui）。该工具在 Editor 和 Game 模式中显示 PhysX 调试行。

  有关更多信息，请参阅 [PhysX Debug](/docs/user-guide/gems/reference/physics/nvidia/physx-debug/).
+ **PhysX Visual Debugger** - [PhysX Visual Debugger (PVD)](https://developer.nvidia.com/physx-visual-debugger) 是 NVIDIA 提供的第三方工具，可用于深入检查 PhysX 世界。O3DE 可以将 PhysX 世界和场景连接到正在运行的 PVD 应用程序实例。您可以使用 PVD 逐步完成仿真，并按照自己的节奏详细检查各种属性。

  有关配置 O3DE 与 PVD 的连接的信息，请参阅 [调试器配置](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-debugger/).

有关更多信息，请参阅 [Debugging PhysX](/docs/user-guide/interactivity/physics/debugging/).

## 确定性
尽管 PhysX 确实支持 [**确定性行为**](https://docs.nvidia.com/gameworks/content/gameworkslibrary/physx/guide/Manual/BestPractices.html#determinism)，在构建和步进物理场景时需要特定的条件，而 O3DE 中没有满足这些条件。此外，O3DE 中的物理系统与许多其他非确定性系统交互，例如动画、脚本和异步资产加载。因此，O3DE 中的 PhysX 模拟预计不是确定性的。
