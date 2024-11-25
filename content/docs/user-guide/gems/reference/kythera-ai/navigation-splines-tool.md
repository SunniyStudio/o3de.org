---
linkTitle: Navigation Splines
title: Kythera AI 3D Navigation Splines Tool
description: Open 3D Engine （O3DE） 中 Kythera AI Gem 中的 3D 导航样条工具指南
weight: 600
toc: true
---

Kythera Navigation Splines 允许设计师精确控制飞行中的船舶运动。除了指定所需的路径外，样条曲线还可以指定路径上每个点的速度和方向，并提供有关飞船外观的反馈。

飞船自身的运动和物理特性仍然被使用，保持一致、逼真的外观，而不是强制动画。它还允许其 AI 的其他方面覆盖样条或与样条交互，因此飞船可以根据情况选择加入或断开样条，或保留对其移动的部分控制。样条曲线形成从设计人员到代理行为的复杂 “提示”，而不是硬脚本序列。

除了设计器工具之外，还有 Kythera 的底层功能，用于跟踪和连接样条，以及使用它们的各种方法。

Kythera Navigation Splines 具有与基于引擎的原生样条相似的外观和感觉，但具有额外的数据（例如速度和方向）和额外的功能来管理它，并形成快速自然的工作流程以实现精确的船舶控制。

## 应用

当前支持的典型 Navigation Splines 用途包括：

* 着陆、起飞、飞越序列

* 在战斗前提供有趣的闲置行为

* 意图明确地进入玩家区域

* 通过复杂几何图形进行标准导航的替代方案

* 利用系统物理和图形效果快速创建游戏内过场动画或视频创建

虽然样条曲线可以用作纯粹的动画工具，但代理行为有许多选项可供选择、忽略或解释样条曲线，从而允许样条曲线与系统行为相结合。例如，飞船可能会因为检测到敌对物体而离开样条，在跟随样条的同时旋转攻击目标，或者暂时离开样条以绕过另一艘移动的飞船。

请注意，默认情况下，飞船仍然只会在有有效八叉树体积的地方移动，并且仍然会尝试避开路径上的动态障碍物。如果飞船在跟随样条时停下来，这可能是原因。

O3DE 尚不支持一些高级功能，包括：

*   机动 - 描述例如桶滚或旋转的短样条，可以随时随地调用

*   Stunt splines （特技样条） - 根据行为系统地选择的样条，例如躲避或逃跑

*   目标/轰炸运行 - 允许在样条曲线上直接指定目标和其他操作

*   移动样条线 - 跟随附加到另一个缓慢移动对象的样条线

## 基础设置

通过添加新实体并从 **Add Component** 下拉菜单中选择 **Kythera / Navigation Spline** 来创建新的 NavSpline：

![spline tool create nav spline](/images/user-guide/gems/kythera-ai/spline-tool-create-navspline.png)

这将创建一个具有（默认）4 个控制点的新样条。请注意，样条曲线始终包含至少两个控制点。

![spline tool new spline](/images/user-guide/gems/kythera-ai/spline-tool-new-spline.png)

**确保启用“显示辅助对象”以便能够看到样条曲线：**

![spline tool display helpers](/images/user-guide/gems/kythera-ai/spline-tool-display-helpers.png)

## 编辑路径

样条的编辑是通过 O3DE 的 Component Mode 完成的。

### 移动或旋转整个样条线

您可以通过选择样条曲线（不是单个控制点）并使用标准的 O3DE Move/Rotate 控件来移动或旋转整个样条曲线。 
请注意，当前不支持缩放样条曲线（这在编辑器中似乎有效，但底层 Kythera 表示不支持）。

### 移动或旋转特定控制点

在视区中，双击样条线，或者在**Entity Inspector**中，点击**Edit**。

在视区中，左键单击要操作的控制点。

使用**Edit > Toggle Current Mode**或按下**M键**更改编辑模式。

您可以在 **Translate**、**Rotate** 和 **Rotate Vehicle** 模式之间切换。Translate 和 Rotate 模式是不言自明的。请参阅 Vehicle Orientation 部分，了解有关 Rotate Vehicle 模式的更多详细信息。

![spline tool selected control point](/images/user-guide/gems/kythera-ai/spline-tool-selected-control-point.png)

### 添加控制点

要向样条添加新的控制点，请 **Ctrl + 单击** 样条上的任意位置：

![spline tool new point](/images/user-guide/gems/kythera-ai/spline-tool-new-point.png)

![spline tool new point 2](/images/user-guide/gems/kythera-ai/spline-tool-new-point-2.png)

请注意，样条曲线下方的现有点将重新编号。

### 移除控制点

同样，在控制点上 **Alt + 单击** 将从样条曲线中删除该点。

## 控制点编辑

选择点后，可以使用 Navigation Spline （导航样条） 面板的 **Selected Control Point** （所选控制点） 部分。我们可以指定船舶此时的行驶速度。速度在控制点之间线性插值。

![spline tool spline dialog](/images/user-guide/gems/kythera-ai/spline-tool-spline-dialogue.png)

## 详细的路径调整

“Tangents” 部分允许对控制点之间的样条形状进行更多控制。以这一点为例...

![spline tool tension](/images/user-guide/gems/kythera-ai/spline-tool-tension-1.png)

…例如，我们可以更改该点的 **Tension In** 和 **Tension Out**，从而影响线在通过该点时与切线的距离。

高张力：

![spline tool tension high display](/images/user-guide/gems/kythera-ai/spline-tool-tension-high-display.png)

![spline tool tension high line](/images/user-guide/gems/kythera-ai/spline-tool-tension-high-line.png)

低张力：

![spline tool tension low display](/images/user-guide/gems/kythera-ai/spline-tool-tension-low-display.png)

![spline tool tension low line](/images/user-guide/gems/kythera-ai/spline-tool-tension-low-spline.png)

默认情况下，Tension In（接近 Control Point）和 Tension Out（远离控制点）与相同的值相关联。可以通过取消选中 **Link Tangent Tension** 复选框来单独更改它们：

![spline tool tension different display](/images/user-guide/gems/kythera-ai/spline-tool-tension-different-display.png)

![spline tool tension different line](/images/user-guide/gems/kythera-ai/spline-tool-tension-different-line.png)

重新选中 **Link Tangent Tension** 会将 Tension In 和 Tension Out 设置为相同的值，即前两个值的平均值。

我们还可以通过 Control Point 编辑切线的方向。通常，此项设置为 **Auto Tangent** 以选择最适合曲线的自然切线。通过取消选中 **Auto Tangent**，我们可以手动设置切线。在此之后，即使我们移动点，它也将保持固定。

![spline tool manual tangent display](/images/user-guide/gems/kythera-ai/spline-tool-manual-tangent-display.png)

![spline tool rotate spline](/images/user-guide/gems/kythera-ai/spline-tool-rotate-spline.png)

## 车辆方向

在 Vehicle Rotation （车辆旋转） 模式下，您编辑的不是样条曲线本身的形状，而是飞行器在飞越样条曲线时的朝向。

## 3D 导航样条工具

默认情况下，飞船在飞行时将指向样条曲线。您可以通过从 Control Point Edit Mode 下拉菜单中选择并使用 Vehicle Rotation 模式来覆盖此设置。这允许独立于路径设置飞船的方向，例如，它可以在起飞和降落时侧向移动或旋转。当您进入此模式时，将在选定的控制点处绘制一艘虚拟飞船，以便您查看其方向。然后，您可以使用 Gizmo 旋转它们。飞船的方向在设置此信息的控制点之间进行插值。

![spline tool rotate vehicle](/images/user-guide/gems/kythera-ai/spline-tool-rotate-vehicle.png)

有两个复选框，用于控制如何应用任何方向。设置 **Vehicle Up** 框后，飞船的正常向上移动向量将被覆盖，导致其侧飞或倒挂飞行。设置 **Vehicle Forward** 框时，其法向前向向量将被覆盖，导致其向后或侧向飞行。

![spline tool vehicle up fwd](/images/user-guide/gems/kythera-ai/spline-tool-veh-up-fwd.png)

通常，船舶最好同时遵循 Up 和 Forward 矢量。但是，如果样条曲线可能会移动或旋转，并且您希望使用当时最自然的 Up 向量，或者您可能希望为飞船留出一些自由度，以便在转弯时倾斜，则可以不设置 Up 向量。同样，您可以设置 Up 向量，但让飞船在转弯时最好地定位自己。

在编辑某个点的 Vehicle Rotation 之前，它们都设置为 off，此时它们都将默认为 on。如果两者都处于关闭状态，则将删除该点的方向信息。

## Ghost Vehicle

要查看船舶如何沿样条向下移动的表示形式，请在 Entity Inspector 中 Navigation Spline 组件的 **Visualisation** 部分中选择 **Ghost Vehicle （Time）**。

![spline tool ghost](/images/user-guide/gems/kythera-ai/spline-tool-ghost-chx.png)

这将显示沿样条曲线的虚拟飞船，这些飞船以飞船移动的一秒为间隔定位。这使您可以可视化飞船的速度和方向：

![spline tool flat spline](/images/user-guide/gems/kythera-ai/spline-tool-flat-spline.png)

当使用 Navigation Splines 进行方向更改的复杂移动时，这可能特别有用：

![spline tool complex spline](/images/user-guide/gems/kythera-ai/spline-tool-complex-spline.png)

## Inspector 调试选项

为此功能添加了一个新的 Inspector 选项。和以前一样，Inspector 可以在 [http://localhost:8081](http://localhost:8081/)找到。

通过在 Kythera Inspector 的 Blackboards 选项卡中将 **Global > ConsoleVariables > DrawMaster** 设置为`1`（或通过编辑器中的 Kythera 工具栏中的复选框）来确保启用调试绘制。

使用 Global > DebugOptions 中的 Splines 复选框启用 Spline 调试。这将以橙色绘制关卡中的所有样条曲线、控制点处的速度值以及当前跟随样条曲线的任何飞船的计划路径。
