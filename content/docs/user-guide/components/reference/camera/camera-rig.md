---
linkTitle: Camera Rig
title: Camera Rig 组件
description: ' 使用 Camera Rig组件添加和删除行为，以驱动Open 3D Engine (O3DE)中的摄像机实体。 '
---

使用**Camera Rig**组件添加和删除行为，以驱动摄像机实体。

## 提供方 ##

[Camera Framework Gem](/docs/user-guide/gems/reference/rendering/camera-framework/)

## 依赖 ##

使用 [Camera 组件](./camera) 的实体。

## Camera Rig 属性 

![Camera Rig properties](/images/user-guide/components/reference/camera/camera-rig-component.png)

Target acquirers
: 定义摄像机如何选择目标的行为阵列。设备会按照所列顺序尝试每个采集器，直到成功找到目标为止。

Look-at behaviors
: 修改目标变换的行为数组。钻机运行每个行为以生成最终的目标变换。

Transform behaviors
: 根据观察目标变换修改摄像机变换的行为数组。在设置摄像机变换之前，钻机会依次运行每个行为。

## Target acquirers 属性 

{{< tabs name="target-acquirers-ui" >}}
{{% tab name="Acquire By EntityId" %}}

![Acquire by entityId properties](/images/user-guide/components/reference/camera/camera-rig-acquire-by-entityid.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Entity target** | 为摄像机装备选择一个目标实体。 | EntityId | None |
| **Use target rotation** | 如果启用，摄像机装备在确定行为时会使用目标实体的旋转。 | Boolean | `Enabled` |
| **Use target position** | 如果启用，摄像机装备在确定行为时会使用目标实体的位置。| Boolean | `Enabled` |

{{% /tab %}}
{{% tab name="Acquire By Tag" %}}

![Acquire by tag properties](/images/user-guide/components/reference/camera/camera-rig-acquire-by-tag.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Target tag** | 通过标签查找目标。如果找到多个实体，摄像头目标将是第一个做出反应的实体。 | Crc32 | None |
| **Use target rotation** | 如果启用，摄像机装备在确定行为时会使用目标实体的旋转角度. | Boolean | `Enabled` |
| **Use target position** | 如果启用，摄像机装备在确定行为时会使用目标实体的位置。| Boolean | `Enabled` |

{{% /tab %}}
{{< /tabs >}}

## Look-at behaviors 属性 

{{< tabs name="look-at-behaviours-ui" >}}
{{% tab name="Offset Position" %}}

使用 **OffsetPosition** 来更改目标变换的位置。例如，您可以将摄像机设置为距离角色基座 1.8 米的目标位置。

![Offset position properties](/images/user-guide/components/reference/camera/camera-rig-offset-position.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Positional Offset** | 目标变换位置的向量位移。 | Vector3 |  X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Offset Is Relative** | 如果启用，**Positional Offset** 是在本地空间。如果禁用，**Positional Offset** 是在世界空间。 | Boolean | `Disabled` |

{{% /tab %}}
{{% tab name="Rotate Camera Target" %}}

使用**Rotate Camera Target**可将摄像机目标与其源目标分开旋转。例如，您可以设置摄像机在 X 轴上俯仰，以模拟角色向上或向下看。

![Rotate camera target properties](/images/user-guide/components/reference/camera/camera-rig-rotate-camera-target.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Axis Of Rotation** | 摄像机围绕目标旋转的轴线。 | **X**, **Y**, or **Z** axis. | `Camera Target's X Axis` |
| **Event Name** | 提供旋转值的输入事件名称。 | String | None |
| **Invert Axis** | 如果启用，反转 **Axis Of Rotation**。 | Boolean | `Disabled` |
| **Rotation Speed Scale** | 输入事件值的乘数，用于缩放旋转速度。 | 0.001 to Infinity | `1.0` |

有关输入事件的更多信息，请参阅 [使用Input组件](/docs/user-guide/interactivity/input/using-player-input)。

{{% /tab %}}
{{% tab name="根据角度沿轴线滑动" %}}

使用 **SlideAlongAxisBasedOnAngle**（基于角度的滑动轴）可根据角度修改观察目标的位置。例如，您可以设置当角色向下看时，摄像机移动到角色前方。

![Slide along axis based on angle properties](/images/user-guide/components/reference/camera/camera-rig-slide-along-axis-based-on-angle.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Axis to slide along** | 相机的滑动轴。 | `Forwards and Backwards`, `Right and Left`, `Up and Down` | `Forwards and Backwards` |
| **Angle Type** | 以滑动为基础的旋转类型。 | `Pitch`, `Roll`, `Yaw` | `Pitch` |
| **Vector Component To Ignore** | 选择一个要忽略的向量分量，将摄像机的移动限制在一个平面内。  | `None`, `X`, `Y`, `Z` | `None` |
| **Max Positive Slide Distance** | 旋转角度为 90 度时，摄像机的最大滑动距离（米）。 | -Infinity to Infinity | `0.0` |
| **Max Negative Slide Distance** | 旋转角度为 -90 度时，摄像机的最大滑动距离（米）。 | -Infinity to Infinity | `0.0` |

{{% /tab %}}
{{< /tabs >}}

## Transform behaviors 属性 

{{< tabs name="transform-behaviours-ui" >}}
{{% tab name="Offset Position" %}}

**Offset Position** 设置摄像机相对于目标位置的位置。

![Offset position properties](/images/user-guide/components/reference/camera/camera-rig-transform-offset-position.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Offset** | 目标变换位置的向量位移。 | Vector3 |  X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Is Offset Relative** | 如果启用，**Offset** 将位于本地空间。如果禁用，**Offset** 将位于世界空间。 | Boolean | `Disabled` |

{{% /tab %}}
{{% tab name="Follow Target From Distance" %}}

**FollowTargetFromDistance** 会使摄像机从指定距离跟踪目标。您还可以设置输入事件来触发摄像机放大或缩小目标。

![Follow target from distance properties](/images/user-guide/components/reference/camera/camera-rig-follow-target-from-distance.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Follow Distance** | 摄像机跟踪目标的距离（以米为单位）。必须大于或等于 **Minimum Follow Distance**，小于或等于 **Maximum Follow Distance**。 | 0 to Infinity | `0.0` |
| **Minimum Follow Distance** | 摄像机跟踪目标的最小距离（米）。必须小于或等于 **Maximum Follow Distance**。 | 0 to Infinity | `0.0` |
| **Maximum Follow Distance** | 摄像机跟踪目标的最大距离（米）。 | 0 to Infinity | `0.0` |
| **Zoom In Event Name** | 输入事件名称，缩小当前跟踪距离，实际上就是放大。 | String | None |
| **Zoom Out Event Name** | 输入增加当前跟随距离的事件名称，实际上就是放大。 | String | None |
  | **Zoom Speed Scale** | 用于缩放速度的输入事件值乘数。 | -Infinity to Infinity | `1.0` |

For more information about Input Events, refer to [Working with the Input component](/docs/user-guide/interactivity/input/using-player-input).

{{% /tab %}}
{{% tab name="Rotate" %}}

使用 **Rotate** 使摄像机绕其中一个轴旋转 (**X**, **Y**, 或 **Z**)。

![Rotate properties](/images/user-guide/components/reference/camera/camera-rig-rotate.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Angle** | 旋转摄像机的角度（单位：度）。| -Infinity to Infinity | `0.0` |
| **Axis** | 摄像机的旋转轴。 | `X`, `Y`, `Z` | `X` |

{{% /tab %}}
{{% tab name="Follow Target From Angle" %}}

**FollowTargetFromAngle** 会使摄像机从指定角度跟踪目标。该功能对俯视、等距和侧滚动摄像机非常有效。

![Follow target from angle properties](/images/user-guide/components/reference/camera/camera-rig-follow-target-from-angle.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Angle** | 跟踪目标的角度（单位：度）。| -Infinity to Infinity | `0.0` |
| **Rotation Type** | 旋转类型为 **Angle**。 | `Pitch`, `Roll`, `Yaw` | `Pitch` |
| **Distance From Target** | 摄像机跟踪目标的距离（以米为单位）。 | -Infinity to Infinity | `1.0` |

{{% /tab %}}
{{% tab name="Face Target" %}}

**FaceTarget** 会使摄像机改变其变换的旋转角度，以观察目标。要使用此功能，只需添加即可。无需配置其他属性。

![Face target properties](/images/user-guide/components/reference/camera/camera-rig-face-target.png)

{{% /tab %}}
{{< /tabs >}}
