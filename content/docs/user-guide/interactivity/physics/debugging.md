---
description: ' 在 Open 3D Engine 中调试游戏的 PhysX 问题。 '
title: 调试 PhysX
weight: 1000
---

PhysX 系统具有以下功能，可用于调试问题。

{{< note >}}
您必须首先启用[PhysX Debug](/docs/user-guide/gems/reference/physics/nvidia/physx-debug/) gem.
{{< /note >}}

**Topics**
- [PhysX调试控制台变量](#physx-debug-console-variables)
- [使用 ImGui 工具进行调试](#debugging-with-the-imgui-tool)
- [PhysX 配置中的 Debug Options （调试选项）](#debug-options-in-the-physx-configuration)
- [在 PhysX SDK 中启用其他检查和错误报告](#enable-additional-checks-and-error-reporting-in-physx-sdk)

## PhysX 调试控制台变量

输入以下控制台变量以调试您的 PhysX 问题。

设置调试首选项。作为推荐的最佳实践，请输入此 console variable 命令作为调试的第一步。

**示例**

```
physx_Debug 1
```

您可以指定以下值：
+ `1` - 启用调试可视化效果。默认情况下，此值将启用 PhysX 实体的碰撞形状和边缘。
+ `2` - 启用所有配置选项。这将启用所有可用的可视化选项。
+ `3` - 切换基于接近度的碰撞器可视化。此值仅适用于网格碰撞器。请参阅 [物理资产碰撞器](/docs/user-guide/components/reference/physx/collider/#physics-asset-colliders).
+ `0` - 禁用调试可视化效果。

**示例**

切换视觉剔除框帧。

```
physx_CullingBox 
```

将剔除框大小调整为 **100**。输入 **0** 以禁用剔除。

```
physx_CullingBoxSize 100
```

连接到 PhysX Visual Debugger。您必须打开 PhysX Visual Debugger 才能运行此命令。请参阅 [调试器配置](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-debugger/).

```
physx_PvdConnect
```

断开与 PhysX Visual Debugger 的连接。您必须打开 PhysX Visual Debugger 才能运行此命令。请参阅 [调试器配置](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-debugger/).

```
physx_PvdDisconnect
```

有关详细信息，请参阅 [使用控制台窗口](/docs/user-guide/editor/console/).

## 使用 ImGui 工具进行调试

在游戏模式下，您可以使用即时模式图形用户界面 （**ImGui**） 工具配置 PhysX 调试设置。

{{< note >}}
您必须启用 [ImGui Gem](/docs/user-guide/gems/reference/debug/imgui) 才能访问此工具。
{{< /note >}}

**使用 ImGui 工具进行调试**

1. 按 **Ctrl+G** 进入游戏模式。

1. 按 **Home** 键打开 **ImGui** 工具。**PhysX Debug** 菜单显示在 **Perspective** 视区下。

1. 单击 **PhysX Debug**。

![PhysX Debug menu in gameplay mode.](/images/user-guide/physx/physx-debugger-imgui-tool.png)

1. 您可以进行以下更改。
****

| **默认** | **说明** |
|-------|--------|
| **Debug visualizations** | 启用调试可视化效果模式。这与 physx_Debug 1 控制台变量相同。 |
| **Visualize colliders** | 允许显示碰撞器。这与 physx_Debug 3 控制台变量相同。 |
| **Culling** | 您可以指定以下选项：<ul><li>**Wireframe** – 在视区中显示线框。</li><li>**Size** – 单击并拖动滑块以指定线框的大小。作为最佳实践，请将此值保持较小，以防止出现性能问题。</li></ul> |
| **Collisions** | 启用碰撞类型的调试。您可以指定以下选项：<ul><li>**Shapes**</li><li>**Edges**</li><li>**F Normals**</li><li>**Aabbs**</li><li>**Axis**</li><li>**Compounds**</li><li>**Static**</li><li>**Dynamic**</li></ul> |
| **Body** | 启用主体类型的调试。您可以指定以下选项：<ul><li>**Axes**</li><li>**Mass Axes**</li><li>**Linear Velocity**</li><li>**Angular Velocity**</li></ul> |

## PhysX 配置中的 Debug Options （调试选项）

您还可以在 **PhysX Configuration** 工具中指定调试设置。请参阅 [调试器配置](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-debugger/). 

## 在 PhysX SDK 中启用其他检查和错误报告

您可以使 O3DE 的配置文件配置使用 PhysX SDK 库的 **checked** 版本。PhysX 将执行其他检查，以检测无效参数、API 竞争条件和 API 的其他错误使用，否则可能会导致模拟中出现神秘的崩溃或故障。这样做的好处是，无需在调试中运行 O3DE 即可启用来自调试配置的相同安全检查，因为低帧速率可能会影响仿真。

使用 PhysX 的选中版本会影响性能。使用它来检测模拟中的错误或确保场景设置正确，但在执行分析或尝试识别性能瓶颈时禁用它。

您可以通过在 CMake 配置期间将 **LY_PHYSX_PROFILE_USE_CHECKED_LIBS** 设置为 **TRUE** 来启用 PhysX 的选中版本：

```
-DLY_PHYSX_PROFILE_USE_CHECKED_LIBS=TRUE
```
