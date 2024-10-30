---
title: Light 组件
linktitle: Light
description: ' Open 3D Engine (O3DE) Light组件参考 '
toc: true
---

**Light** 组件通过各种类型的点光源和区域光源来模拟柔和的摄影棚光线。灯光组件支持点光源 Point（简单点光源）和聚光源 Spot（简单点光源），它们的体积非常小。它们的性能略高于对应的区域光，但产生的光效更简单。光线组件还支持Point（球形）、Spot（圆盘形）、Capsule胶囊光源、Quad四边形光源和Polygon多边形光源。这些光源能最精确地模拟现实世界中的光源。


## Light types

Point (sphere)
: 从球体表面向各个方向发光，类似于标准灯泡。点（球形）光源类型支持阴影效果。

Point (simple punctual)   
: 从空间中的一个点发射光线。点（简单准时）光线类型的逼真度不如点（球形）光线类型，但点（简单准时）光线类型的性能更高。

Spot (disk)   
: 从三维空间中的一个圆圈发出光线，类似于聚光灯或嵌入式照明灯。您可以添加快门，将光线限制在锥形范围内。聚光灯（圆盘）类型支持阴影效果。

Spot (simple punctual)   
: 从空间中的单点发射光线，受限于锥形。光点（简单准时）光类型的逼真度低于光点（圆盘）光类型，但光点（简单准时）光类型的性能更高。

Quad   
: 从三维空间中的矩形表面发射光线。四边形灯光类型最适用于用漫射光照亮较大的区域。默认情况下，Quad 灯光类型使用线性变换余弦来计算精确的照明。它还支持快速近似计算，这种计算方法性能更高，但产生的光线质量较低。四边形灯光类型可以从一个或两个方向发光。

Polygon   
: 从三维空间中任意形状的多边形发射光线，该多边形通过线性变换余弦近似。这种多边形最多可以有 64 个点，但多边形光源类型的成本会随着多边形点数的增加而提高。多边形光源类型是最昂贵的光源类型，但它能产生非常逼真的区域照明效果。多边形灯光类型可以从一个或两个方向发光。

## 提供方

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## 依赖

根据灯光类型的不同，灯光组件有以下依赖关系：

- **Point (sphere)**: [Sphere Shape](/docs/user-guide/components/reference/shape/sphere-shape/)
- **Point (simple punctual)**: None
- **Spot (disk)**: [Disk Shape](/docs/user-guide/components/reference/shape/disk-shape/)
- **Spot (simple punctual)**: None
- **Capsule**: [Capsule Shape](/docs/user-guide/components/reference/shape/capsule-shape/)
- **Quad**: [Quad Shape](/docs/user-guide/components/reference/shape/quad-shape/)
- **Polygon**: [Polygon Prism Shape](/docs/user-guide/components/reference/shape/polygon-prism-shape/)



## 属性

{{< tabs name="light-component-ui" >}}
{{% tab name="Default" %}}

![light-component-default-light-type](/images/user-guide/components/reference/atom/light-component-ui/default.png)

{{% /tab %}}
{{% tab name="Point (sphere)" %}}

![light-component-point-sphere-light-type](/images/user-guide/components/reference/atom/light-component-ui/point-sphere.png)

{{% /tab %}}
{{% tab name="Point (simple punctual)" %}}

![light-component-point-simple-punctua-light-type](/images/user-guide/components/reference/atom/light-component-ui/point-simple-punctual.png)

{{% /tab %}}
{{% tab name="Spot (disk)" %}}

![light-component-spot-disk-light-type](/images/user-guide/components/reference/atom/light-component-ui/spot-disk.png)

{{% /tab %}}
{{% tab name="Spot (simple punctual)" %}}

![light-component-spot-simple-punctual-light-type](/images/user-guide/components/reference/atom/light-component-ui/spot-simple-punctual.png)

{{% /tab %}}
{{% tab name="Capsule" %}}

![light-component-capsule-light-type](/images/user-guide/components/reference/atom/light-component-ui/capsule.png)

{{% /tab %}}
{{% tab name="Quad" %}}

![light-component-quad-light-type](/images/user-guide/components/reference/atom/light-component-ui/quad.png)

{{% /tab %}}
{{% tab name="Polygon" %}}

![light-component-polygon-light-type](/images/user-guide/components/reference/atom/light-component-ui/polygon.png)

{{% /tab %}}
{{< /tabs >}}


### Base 属性

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Light type** | 光的类型。每种灯的特性都不尽相同。 | `Point (sphere)`, `Point (simple punctual)`, `Spot (disk)`, `Spot (simple punctual)`, `Capsule`, `Quad`, `Polygon` | - |
| **Color** | 光的颜色。**Color***对纯白光起到凝胶作用，并减少任何其他颜色光的总能量输出。例如，一个 100 `Lumen`的区域灯，如果颜色设置为中灰（18%），则只能输出 18 流明的光。 | Eight bits per channel color: `0` to `255` | `255,255,255` |
| **Intensity mode** | 光的光度单位。 `Candela` 和 `Lumen`指定从形状整个表面区域发出的光能总量。如果将此模式设置为`Candela` 或 `Lumen`并将**强度**指定为`100.0`，那么所提供的形状分量越大，光线就越暗。这是因为总光能分布在更大的表面区域。`Nit`表示每平方米的光能（烛光）。Ev100 "表示光能在一定面积上的曝光值。`Ev100` 值是指数值，因此 5.0 `Ev100` 值的亮度是 4.0 `Ev100` 值的两倍。对于 `Nit` 和 `Ev100`，形状越大，光就越亮。这是因为总光能随着形状表面积的增加而增加。 | `Candela`, `Lumen`, `Nit`, `Ev100` | `Lumen` |
| **Intensity** | 在**Intensity mode**下所选择的光度单位的光能输出或亮度。 | Candela, Lumen, Nit: `0` to infinity<br><br>Ev100: -infinity to +infinity | `100` |
| **Attenuation radius** | 请参阅下面的 [Attenuation radius 属性](#attenuation-radius-properties)。 | Boolean | `Disabled` |
| **Shadows** | 请参阅下面的 [Shadows 属性](#shadows-properties)。 | Boolean | `Disabled` |
| **Shutters** | 请参阅下面的 [Shutters 属性](#shutters-properties)。 | Boolean | `Disabled` |
| **Both directions** | 启用此属性可从形状的两侧发光。<br> <br>*此字段仅适用于Quad 四边形和Polygon 多边形灯光类型。* | Boolean | `Disabled` |
| **Fast approximation** | 启用此属性可使用速度更快、质量更低的近似光照计算方法，而不是默认的高质量线性余弦变换技术。<br> <br>*此字段仅适用于 Quad 灯光类型。* | Boolean | `Disabled` |


### Attenuation radius 属性

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Mode** | 指定衰减是自动计算还是明确设置。**Attenuation**是光能在传播过程中强度的降低，有时也称为 “衰减”。  | `Automatic`, `Explicit` | `Automatic` |
| **Radius** | 以米为单位定义光的衰减距离。从区域光表面上的给定点出发，半径是光在其范围之外不起作用的距离。如果为强光设置的**Radius**值过小，可能会导致非逼真的衰减。<br> <br>*该字段仅在 **Mode**设置为`Explicit`时可用。* | `0` to infinity | `0.5` |


### Shadows 属性
*Shadow 属性仅适用于Spot (disk) 和 Point (sphere)类型。*

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Enable shadow** | 启用阴影效果。 | Boolean | `Disabled` |
| **Shadowmap size** | 设置阴影贴图的宽度和高度。阴影贴图纹理包含场景中某个区域的光线和物体信息。它用于**Shadow Mapping**，这是一种渲染场景中阴影的技术。尺寸越大，阴影效果越细致。 | `256`, `512`, `1024`, `2048` | `256` |
| **Shadow filter method** | 设置阴影过滤方法，以减少阴影贴图中的混叠。支持的方法有*percentage-closer filtering (PCF)* 或 *exponential shadow maps (ESM)*。`ESM+PCF`使用 ESM，但在 ESM 可能失效的区域会退回到 PCF。 | `None`, `PCF`, `ESM`, `ESM+PCF` | `None` |
| **Softening boundary width** | 要调整阴影边缘的柔和度，请设置阴影区域和光照区域边界的宽度（以米为单位）。如果宽度为 0，则禁用柔和边缘。<br> <br>此字段仅在**Pcf method**设置为`BoundarySearch`时可用。 | `0` to `1` meter | `0.25`|
| **Prediction sample count** | 用于预测像素是否在边界上的采样计数。<br> <br>只有当 **Pcf method** 设置为`BoundarySearch`时，该字段才可用。 | `0` to `16` | `4` |
| **Filtering sample count** | 用于过滤阴影边界的样本数。<br> <br>只有当**Shadow filter method**设置为 `PCF` 或 `ESM+PCF`时，该字段才可用。 | `0` to `64` | `12` |
| **Pcf method** | 用于阴影的 PCF 方法。*边界搜索*方法会执行不同次数的点触。第一次点触会确定你是否处于阴影边界上，其余的点触则会找出遮挡量。 *bicubic*方法使用固定大小的 PCF 内核，内核权重设定为近似二次方滤波。<br> <br>只有当**Shadow filter method**设置为 `PCF` 或 `ESM+PCF`时，该字段才可用。 | `BoundarySearch`, `Bicubic` | `Bicubic` |

#### 提示

- 对于阴影效果，不同的属性和滤波方法对性能和质量的影响各不相同。使用 PCF 和双三次滤波法可以在性能和质量之间取得最佳平衡。

-  **Shadowmap size**为 `512`时，可产生兼顾质量和性能的效果。`256`尺寸产生的阴影质量较低。`1024`和 `2048` 尺寸的阴影质量极佳，但价格较高。

### Shutters 属性
快门属性仅适用于聚光灯（圆盘）和聚光灯（简单准时）类型。

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Enable shutters** | 启用快门，将光线限制为锥形。<br> <br>*该字段仅适用于 Spot（简单准时）灯光类型.* | Boolean | `Disabled` |
| **Inner angle** | 以度为单位设置内锥角。 | `0.5` to `90.0` degrees | `35.0` |
| **Outer angle** | 以度为单位设置外锥角。 | `0.5` to `90.0` degrees | `45.0` |


## z注意
  
- 编辑Light组件时，要查看灯光的形状和方向，请在**Viewport中启用**Debug Helpers**。
