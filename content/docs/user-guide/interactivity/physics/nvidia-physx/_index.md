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
+ [决定论](#determinism)
<!-- + [使用 NVIDIA Blast 进行模拟破坏](/docs/user-guide/interactivity/physics/nvidia-blast/) -->


## PhysX Gems 

The PhysX system uses the following Gems, which you can enable in **Project Manager**.

+ **[PhysX](/docs/user-guide/gems/reference/physics/nvidia/physx/)** - Provides integration for the [NVIDIA PhysX 4 SDK](https://developer.nvidia.com/physx-sdk) into O3DE. The integration provided includes a suite of components, configuration via the **O3DE Editor**, Script Canvas integration, **PhysX Visual Debugger** integration, and a simplified API abstraction layer for games.

  有关更多信息，请参阅 [PhysX](/docs/user-guide/gems/reference/physics/nvidia/physx/).
+ **[PhysX Debug](/docs/user-guide/gems/reference/physics/nvidia/physx-debug/)** - Provides debug visualizations of PhysX scene geometry that you can enable with console commands and other tools.

  有关更多信息，请参阅 [PhysX Debug](/docs/user-guide/gems/reference/physics/nvidia/physx-debug/).

## PhysX version support

O3DE uses PhysX 4.1 by default. You can enable PhysX 5.1 by specifying `-DAZ_USE_PHYSX5=ON` as a command-line option in the configuration step when you configure and build your project or the engine. The following is an an example configuration command that enables PhysX 5.1.

```cmd
cmake -B build/windows -S . -G "Visual Studio 16" -DLY_3RDPARTY_PATH=C:\o3de-packages -DAZ_USE_PHYSX5=ON
```

For more information on configuring and building projects see the [Configure and Build](/docs/user-guide/build/configure-and-build/) topic.

## PhysX Components 

The **PhysX** gem has the following components, which you can [add](/docs/user-guide/components/reference/#adding-components-to-an-entity) to entities by using the [**Entity Inspector**](/docs/user-guide/editor/entity-inspector/):
+ **[PhysX Primitive Collider](/docs/user-guide/components/reference/physx/collider/)** - Enables physics objects to collide with other physics objects, using primitive shapes.
+ **[PhysX Mesh Collider](/docs/user-guide/components/reference/physx/mesh-collider/)** - Enables physics objects to collide with other physics objects, using shapes defined by a PhysX asset.
+ **[PhysX Shape Collider](/docs/user-guide/components/reference/physx/shape-collider/)** - Enables physics objects to collide with other physics objects, using geometry defined by a **[Shape component](/docs/user-guide/components/reference/shape/)**.
+ **[PhysX Force Region](/docs/user-guide/components/reference/physx/force-region/)** - Enables an entity to specify a region that applies physical force to entities. For each physics simulation frame, the component applies force to entities that are in the bounds of the region.
+ **[PhysX Static Rigid Body](/docs/user-guide/components/reference/physx/static-rigid-body/)** - Enables a non-movable entity to be part of the physics simulation. Static rigid bodies can collide other simulated rigid bodies.
+ **[PhysX Dynamic Rigid Body](/docs/user-guide/components/reference/physx/rigid-body/)** - Enables a movable entity to be part of the physics simulation. Dynamic Rigid body type can be **kinematic** or **simulated**. Simulated rigid bodies respond to collision events with other rigid bodies. Kinematic rigid bodies are not affected by outside forces and gravity; their motion is driven by scripting.
+ **[PhysX Character Controller](/docs/user-guide/components/reference/physx/character-controller/)** - Implements basic character interactions with the physical world. For example, it can control interactions with slopes and steps, manage interactions with other characters, and prevent characters from walking through walls or passing through terrain.
+ **[PhysX Character Gameplay](/docs/user-guide/components/reference/physx/character-gameplay/)** - Provides example implementations for character controller behaviors which are likely to require game-specific tweaking, such as detecting whether the character is on the ground, interacting with gravity, and behavior for interacting with kinematic bodies and other controllers. 
+ **[PhysX Ragdoll](/docs/user-guide/components/reference/physx/ragdoll/)** - Enables animation of certain character behaviors. The physical representation is usually a hierarchical collection of rigid bodies with simple shapes connected by joints.
+ **[PhysX Ball Joint](/docs/user-guide/components/reference/physx/ball-joint/)** - Creates a dynamic ball joint that constrains an entity to the joint with freedom to rotate around the y- and z-axes of the joint.
+ **[PhysX Fixed Joint](/docs/user-guide/components/reference/physx/fixed-joint/)** - Creates a dynamic fixed joint that constrains an entity to the joint with no degree of freedom in any axis.
+ **[PhysX Hinge Joint](/docs/user-guide/components/reference/physx/hinge-joint/)** - Creates a dynamic hinge joint that constrains an entity to the joint with freedom to rotate around the x-axis of the joint.
+ **[PhysX Prismatic Joint](/docs/user-guide/components/reference/physx/prismatic-joint/)** - Creates a dynamic prismatic joint that constrains an entity to the joint, keeping the same rotation but allowing it to move freely along one axis.

## PhysX Configuration 

Use the **PhysX Configuration** window in O3DE Editor to configure global settings, collision layers, collision groups, and PhysX Visual Debugger settings.

有关更多信息，请参阅 [Configuring the PhysX System](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/).

## 物理材质

物理材质允许按实体配置模拟属性。材质可自定义对象在撞击表面时的反应方式，并控制摩擦力和弹性等质量。使用 **Asset Editor** 创建物理材质，然后将材质分配给碰撞器。

有关更多信息，请参阅 [Physics materials](/docs/user-guide/interactivity/physics/nvidia-physx/materials/).

## PhysX 调试

要验证模拟世界中交互的实现，可以使用以下工具。
+ **PhysX Debug gem** - 如果您是开发人员或技术美工人员，建议使用 PhysX Debug Gem。您可以使用此工具在 O3DE Editor 的编辑器模式或游戏模式下实时查看物理世界。要激活该工具，请使用控制台命令或即时模式图形用户界面 （ImGui）。该工具在 Editor 和 Game 模式中显示 PhysX 调试行。

  有关更多信息，请参阅 [PhysX Debug](/docs/user-guide/gems/reference/physics/nvidia/physx-debug/).
+ **PhysX Visual Debugger** - The [PhysX Visual Debugger (PVD)](https://developer.nvidia.com/physx-visual-debugger) is a third party tool provided by NVIDIA that is useful for deep inspection of the PhysX world. O3DE can connect PhysX worlds and scenes to a running PVD application instance. You can use the PVD to step through your simulation and examine various properties at your own pace in detail.

  For information on configuring O3DE's connection to PVD, see [Debugger Configuration](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-debugger/).

有关更多信息，请参阅 [Debugging PhysX](/docs/user-guide/interactivity/physics/debugging/).

## Determinism
Although PhysX does have support for [**deterministic behavior**](https://docs.nvidia.com/gameworks/content/gameworkslibrary/physx/guide/Manual/BestPractices.html#determinism), it requires specific conditions when constructing and stepping physics scenes, which are not met in O3DE. Furthermore, the physics system in O3DE interacts with many other systems which are not deterministic, such as animation, scripting and asynchronous asset loading. Therefore, the PhysX simulation in O3DE is not expected to be deterministic.
