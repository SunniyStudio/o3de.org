---
linktitle: Gradient Transform Modifier
title: Gradient Transform Modifier 组件
description: 使用Gradient Transform Modifier组件在Open 3D Engine (O3DE)中变换梯度坐标。
---

**Gradient Transform Modifier** 组件将渐变坐标转换为相对于形状的空间，并提供覆盖形状变换、旋转和缩放的选项。

## 提供方

[Gradient Signal Gem](/docs/user-guide/gems/reference/utility/gradient-signal)

## 依赖

对实体应用Gradient Transform Modifier时，实体必须具有以下组件之一：

- [Axis Aligned Box Shape](../shape/axis-aligned-box-shape)
- [Box Shape](../shape/box-shape)
- [Capsule Shape](../shape/capsule-shape)
- [Compound Shape](../shape/compound-shape)
- [Cylinder Shape](../shape/cylinder-shape)
- [Disk Shape](../shape/disk-shape)
- [Polygon Prism Shape](../shape/polygon-prism-shape)
- [Quad Shape](../shape/quad-shape)
- [Shape Reference](../shape/shape-reference)
- [Sphere Shape](../shape/sphere-shape)
- [Tube Shape](../shape/tube-shape)

## Gradient Transform Modifier 属性

![Gradient Transform Modifier component properties](/images/user-guide/components/reference/gradient-modifiers/gradient-transform-modifier-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Transform Type** | 设置如何解释形状变换的空间坐标。 | `Origin`, `World Transform`, `Relative to Parent`, `World Transform (of Reference)`, 或 `Relative to Reference` | `World Transform` |
| **Wrapping Type** | 设置如何评估梯度边界以外的值。 | `None (unbounded)`, `Clamp To Edge`, `Clamp To Zero`, `Mirror`, or `Repeat`. | `None (unbounded)` |
| **Frequency Zoom** | 用乘法因子重调梯度坐标。 | Float: 0.0001 to Infinity  | `1.0` |
| **Advanced Mode** | 启用变换修改器的高级配置选项。 | Boolean | `Disabled` |

### Advanced 属性

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Sample in 3D** | 如果`Enabled`，UVW 贴图将基于三维世界空间。 | Boolean | `Disabled` |
| **Allow Reference** | 如果`Enabled`，附加的 **Shape** 组件提供的边界和变换将被 **Shape Reference** 中选择的实体覆盖。 | Boolean | `Disabled` |
| **Shape Reference** | 设置具有有效形状组件的实体，以覆盖附加的形状组件。 | EntityId | None |
| **Override Bounds** | 如果`Enabled`，梯度的边界将在 **Bounds** 中手动设置。 | Boolean | `Disabled` |
| **Bounds** | 设置用于重映射、包围、夹紧和缩放梯度坐标的局部（未变换）框的边界。 | Vector3: -Infinity to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Override Translate** |  如果`Enabled`，梯度的平移将在**Translate**中手动设置。  | Boolean | `Disabled` |
| **Translate** | 设置用于重映射、包裹、夹紧和缩放渐变坐标的形状的平移值。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Override Rotate** |  如果`Enabled`，梯度的旋转将在 **Rotate** 中手动设置。  | Boolean | `Disabled` |
| **Rotate** | 设置用于重映射、包裹、夹紧和缩放渐变坐标的形状的旋转角度。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Override Scale** |  如果`Enabled`，则在 **Scale** 中手动设置渐变的比例。  | Boolean | `Disabled` |
| **Scale** | 设置用于重映射、包裹、夹紧和缩放渐变坐标的形状的比例。 | Vector3: 0.0001 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |

## GradientTransformModifierRequestBus

使用以下带有 `GradientTransformModifierRequestBus` EBus 接口的请求函数，可与游戏中的渐变变形修改器组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetAllowReference` | 返回 **Allow Reference** 的值。 | None | Boolean | Yes |
| `GetBounds` | 返回 **Bounds** 的值。 | None | Bounds: Vector3 | Yes |
| `GetFrequencyZoom` | 返回 **Frequency Zoom** 的值。 | None | Multiplication Factor: Float | Yes |
| `GetIs3D` | 返回 **Sample in 3D** 的值。 | None | Boolean | Yes |
| `GetOverrideBounds` | 返回 **Override Bounds** 的值。 | None | Boolean | Yes |
| `GetOverrideRotate` | 返回 **Override Rotate**. | None | Boolean | Yes |
| `GetOverrideScale` | 返回 **Override Scale** 的值。 | None | Boolean | Yes |
| `GetOverrideTranslate` | 返回 **Override Translate** 的值。 | None | Boolean | Yes |
| `GetRotate` | 返回 **Rotate** 的值。 | None | Rotation: Vector3 | Yes |
| `GetScale` | 返回 **Scale** 的值。 | None | Scale: Vector3 | Yes |
| `GetShapeReference` | 返回 **Shape Reference** 的值。 | None | Shape Entity: EntityId | Yes |
| `GetTransformType` | 返回 **Transform Type** 的值。 | None | Transform Type Index: Integer | Yes |
| `GetTranslate` | 返回 **Translate** 的值。 | None | Translation: Vector3 | Yes |
| `GetWrappingType` | 返回 **Wrapping Type** 的值。 | None | Wrapping Type Index: Integer | Yes |
| `SetAllowReference` | 设置 **Allow Reference**的值。 | Boolean | None | Yes |
| `SetBounds` | 设置 **Bounds**的值。 | Bounds: Vector3 | None | Yes |
| `SetFrequencyZoom` | 设置 **Frequency Zoom**的值。 | Multiplication Factor: Float | None | Yes |
| `SetIs3D` | 设置 **Sample in 3D**. | Boolean | None | Yes |
| `SetOverrideBounds` | 设置 **Override Bounds**的值。 | Boolean | None | Yes |
| `SetOverrideRotate` | 设置 **Override Rotate**的值。 | Boolean | None | Yes |
| `SetOverrideScale` | 设置 **Override Scale**的值。 | Boolean | None | Yes |
| `SetOverrideTranslate` | 设置 **Override Translate**的值。 | Boolean | None | Yes |
| `SetRotate` | 设置 **Rotate**的值。 | Rotation: Vector3 | None | Yes |
| `SetScale` | 设置 **Scale**的值。 | Scale: Vector3 | None | Yes |
| `SetShapeReference` | 设置 **Shape Reference**. | Shape Entity: EntityId | None | Yes |
| `SetTransformType` | 设置 **Transform Type**的值。 | Transform Type Index: Integer | None | Yes |
| `SetTranslate` | 设置 **Translate**的值。 | Translation: Vector3 | None | Yes |
| `SetWrappingType` | 设置 **Wrapping Type**的值。 | Wrapping Type Index: Integer | None | Yes |
