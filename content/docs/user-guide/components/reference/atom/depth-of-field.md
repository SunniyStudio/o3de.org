---
title: Depth of Field 组件
linktitle: Depth of Field
description: "Open 3D Engine (O3DE) Depth of Field 组件参考。"
toc: true
---

**Depth of Field** 组件模拟现实世界中相机的镜头效果，聚焦于特定区域，并模糊焦外区域。


## 提供方

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)

## 依赖

[Camera 组件](/docs/user-guide/components/reference/camera/camera/)

[PostFX Layer 组件](/docs/user-guide/components/reference/atom/postfx-layer/)


## Base 属性

![Depth of Field 组件属性](/images/user-guide/components/reference/atom/depth-of-field.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Camera Entity** | 对包含**Camera**组件的实体的引用。启用景深和配置以下属性时需要用到它。 | 实体引用 | Empty |
| **Overrides** | 属性集合，您可以覆盖这些属性来计算景深效果。 |  |  |
| **Enable Depth of Field** | 如果启用，景深效果生效。 | Boolean | Enabled |
| **Quality Level** | 控制焦外区域的模糊质量。数值越大，虚化质量越高，但性能负荷也越大。 | `0` 到 `1` | `1` |
| **Aperture F** | 控制景深效果的浅度，相当于对焦场景的深度范围。光圈 F 用 1/F 数值来衡量。当此属性为`0`时，F 值为`256`。当为`1`时，F 值为 `0.12`。 | `0.00` 到 `1.00` | `0.50` |
| **F Number** | (只读）与光圈 F 相对应的值。 | `256.00` 到 `0.12` |  |
| **Focus Distance** | 摄像机到对焦对象的距离（以米为单位）。这需要禁用启用自动对焦属性。 | -Infinite to Infinite  | `100.0` |
| **Auto Focus** | 参阅 [Auto Focus 属性](#auto-focus-properties)。 |  |  |
| **Debugging** | 允许着色以调试场景中的景深效果。<br><ul><li>**Red**: Back, large blur</li><li>**Orange**: Back, middle blur</li><li>**Yellow**: Back, small blur</li><li>**Green**: Focused area</li><li>**Blue green**: Front, small blur</li><li>**Blue**: Front, middle blur</li><li>**Purple**: Front, large blur</li></ul> | Boolean | Disabled |

## Auto Focus 属性

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Enable Auto Focus** | 使摄像机自动对指定区域对焦。 | Boolean | Enabled |
| **Focus Screen Position** | 以 XY 坐标指定屏幕空间的焦点位置。(`0`,`0`)对应屏幕左上角；(`1`,`1`)对应屏幕右下角。 | XY: `0.00` to `1.00` | (`0.5`,`0.5`) |
| **Auto Focus Sensitivity** | 模拟对焦滞后，即对焦改变时，景深改变前会有一个滞后。数值越大，响应速度越快。数值越小，需要更大的景深差异才能重新对焦。当值为`1.0`时，没有滞后。 | `0.00` to `1.00` | `1.00` |
| **Auto Focus Speed** | 指定焦点每秒移动的距离。它将近景到远景的距离归一化为 1.0。 | `0.00` to `2.00` | `2.00` |
| **Auto Focus Delay** | 指定焦点开始从一个目标转移到另一个目标时的延迟时间（以秒为单位）。 | `0.00` to `1.00` | `1.00` |


## 用法

1. 创建一个实体，并添加一个**Camera**组件。在下面的步骤中，这将被称为 **摄像头实体**。如果您已经有一个摄像机实体，请使用该实体。

2. 为摄像机实体添加Depth of Field组件。Depth of Field取决于摄像机组件的属性。这也会提示您添加PostFX Layer组件，因为Depth of Field 组件依赖于它。 

3. 在Depth of Field组件中:

   - 设置Camera Entity属性为此摄像机实体。
  
   - 激活 Enable Depth of Field 属性。

4. 配置景深效果 模糊效果取决于两个属性：Depth of Field组件的Aperture F 属性和Camera组件的视野属性。Aperture F 越大，视野越小，模糊效果就越强。

5. 您可以通过启用启用调试颜色属性来查看模糊级别。有关颜色级别，请参阅 [Base 属性](#base-properties)部分中的调试属性。
