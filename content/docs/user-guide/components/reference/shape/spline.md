---
title: Spline 组件
linktitle: Spline
description: ' Open 3D Engine (O3DE) Spline 组件。 '
weight: 100
---



**Spline** 组件创建一条 8 米长的线，其中包含 4 个点。点位置在实体的局部空间中定义。样条的长度、分段和形状可以使用组件的 **Spline Type** 属性和 **Edit** 功能来定义。样条曲线是连接两个或多个点的曲线，可以用作动画实体的路径或组件的主干，例如 [Tube Shape](/docs/user-guide/components/reference/shape/tube-shape/) 组件。

## 提供者

[O3DE Core (LmbrCentral) Gem](/docs/user-guide/gems/reference/o3de-core)

## Spline 属性

{{< tabs name="spline-component-ui" >}}
{{% tab name="Linear Spline" %}}

![Linear Spline](/images/user-guide/components/reference/shape/linear-spline-component-ui-01.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Visible** | 启用此选项可始终在视区中显示样条曲线，即使未选择实体也是如此。禁用可在未选择实体时隐藏样条曲线。 | Boolean | `Enabled` |
| **Configuration - Spline Type** | 定义样条线段的插值类型。线性样条曲线具有直线段。Bezier 样条曲线在样条曲线的分段中插入具有均匀步长的曲线。Catmull-Rom 样条由控制点定义。Catmull-Rom 样条需要四个控制点来定义每个线段，因此具有四个点的默认样条线将仅生成一个线段。 | [Linear,](#linear-spline-type-properties) [Bezier,](#bezier-spline-type-properties) [Catmull-Rom](#catmull-rom-spline-type-properties) | `Linear` |
| **Spline** | 所选样条类型的 Spline （样条） 属性组选项。 | | |
| **Closed** | 启用此选项可闭合样条曲线并创建循环。 | Boolean | `Disabled` |
| **Edit** | 选择 **Edit** 按钮进入 Edit 模式。在编辑模式下，您可以使用下面的 [编辑模式操作](#edit-mode-actions) 中概述的方法在视口中修改样条的长度、分段和形状。在 Edit 模式下，菜单栏中的 Edit （编辑） 菜单显示可用的操作和热键。要退出 Edit 模式，请在组件界面中选择 **Done**。 |  |  |

{{% /tab %}}
{{% tab name="Bezier Spline" %}}

![Bezier Spline](/images/user-guide/components/reference/shape/bezier-spline-component-ui-01.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Visible** | 启用此选项可始终在视区中显示样条曲线，即使未选择实体也是如此。禁用可在未选择实体时隐藏样条曲线。 | Boolean | `Enabled` |
| **Configuration - Spline Type** | 定义样条线段的插值类型。线性样条曲线具有直线段。Bezier 样条曲线在样条曲线的分段中插入具有均匀步长的曲线。Catmull-Rom 样条由控制点定义。Catmull-Rom 样条需要四个控制点来定义每个线段，因此具有四个点的默认样条线将仅生成一个线段。  | [Linear,](#linear-spline-type-properties) [Bezier,](#bezier-spline-type-properties) [Catmull-Rom](#catmull-rom-spline-type-properties) | `Linear` |
| **Spline** | 所选样条类型的 Spline （样条） 属性组选项。 | | |
| **Closed** | 启用此选项可闭合样条曲线并创建循环。 | Boolean | `Disabled` |
| **Granularity** | 每个样条线段中的插值步数。粒度值越高，曲线段越平滑。 | 2 - 64 | `8` |
| **Edit** | 选择 **Edit** 按钮进入 Edit 模式。在编辑模式下，您可以使用下面的 [编辑模式操作](#edit-mode-actions) 中概述的方法在视口中修改样条的长度、分段和形状。在 Edit 模式下，菜单栏中的 Edit （编辑） 菜单显示可用的操作和热键。要退出 Edit 模式，请在组件界面中选择 **Done**。 |  |  |

{{% /tab %}}
{{% tab name="Catmull-Rom Spline" %}}

![Catmull-Rom Spline](/images/user-guide/components/reference/shape/catmull-rom-spline-component-ui-01.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Visible** | 启用此选项可始终在视区中显示样条曲线，即使未选择实体也是如此。禁用可在未选择实体时隐藏样条曲线。 | Boolean | `Enabled` |
| **Configuration - Spline Type** | 定义样条线段的插值类型。线性样条曲线具有直线段。Bezier 样条曲线在样条曲线的分段中插入具有均匀步长的曲线。Catmull-Rom 样条由控制点定义。Catmull-Rom 样条需要四个控制点来定义每个线段，因此具有四个点的默认样条线将仅生成一个线段。  | [Linear,](#linear-spline-type-properties) [Bezier,](#bezier-spline-type-properties) [Catmull-Rom](#catmull-rom-spline-type-properties) | `Linear` |
| **Spline** | 所选样条类型的 Spline （样条） 属性组选项。 | | |
| **Closed** | 启用此选项可闭合样条曲线并创建循环。 | Boolean | `Disabled` |
| **Knot Parameterization** | 指定样条曲线在控制点之间的插值方式。较小的值会锐化控制点周围的插值，而较高的值会简化控制点周围的插值。 | 0 - 1 | `0.0` |
| **Granularity** | 每个样条线段中的插值步数。Granularity值越高，曲线段越平滑。 | 2 - 64 | `8` |
| **Edit** | 选择 **Edit** 按钮进入 Edit 模式。在编辑模式下，您可以使用下面的 [编辑模式操作](#edit-mode-actions) 中概述的方法在视口中修改样条的长度、分段和形状。在 Edit 模式下，菜单栏中的 Edit （编辑） 菜单显示可用的操作和热键。要退出 Edit 模式，请在组件界面中选择 **Done**。 |  |  |

{{% /tab %}}
{{< /tabs >}}

## 编辑模式操作

* **选择一个点** - **左键单击** 任何点。
* **添加到所选内容** - 按住 **Ctrl** 并 **左键单击** 未选中的点。
* **从选择中删除** - 按住 **Ctrl** 并 **左键单击** 选定点。
* **选择多个** - **左键单击** 并拖动多个点。
* **移动点** - 选中点后，**左键单击** 并拖动变换操纵器。
* **添加点** - 按住 **Ctrl** 并 **左键单击** 现有点之间的线段。
* **删除点** - 按住 **Alt** 并 **左键单击** 一个点。
* **删除所选点** - 按 **删除** 删除所有所选点。
* **将点对齐到位置** - 在视区中按住 **Ctrl + Shift** 和 **左键单击** 以将所选点对齐到该位置。
* **对齐点到网格** - 如果在视口选项中将 **启用网格对齐** 设置为 true，则点将对齐到工作平面上的位置。

## SplineComponentRequestBus

将以下请求函数与 '`SplineComponentRequestBus`' 事件总线接口结合使用，以便与游戏中的 Spline 组件进行通信。Spline 组件还使用 '`VertexContainer`' 函数。有关更多信息，请参阅 [顶点容器](/docs/user-guide/components/reference/shape/vertex-container/)。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetSpline` | 返回指向基础样条类型的常量指针。您可以使用此函数根据光线投射和位置查询样条。您还可以请求信息，例如样条曲线的长度、位置、法线和沿样条曲线各个点的切线。 | None | Spline: `AZ::ConstSplinePtr` | No |
| `ChangeSplineType` | 将样条曲线的类型更改为 Linear、Bezier 或 Catmull-Rom。 | Spline Type: '`AZ:u64`' 包含 **Spline Type** 的 RTTI 哈希。| None | No |
| `SetClosed` | 指定 '`True`' 以连接样条曲线的端点并创建一个闭合循环。指定 '`False`' 以断开样条曲线的端点并创建开放曲线。  | SetClosed: Boolean | None | No |

## SplineComponentNotificationBus

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `OnSplineChanged` | 通知侦听器样条已更新。 | None | None | Yes |
