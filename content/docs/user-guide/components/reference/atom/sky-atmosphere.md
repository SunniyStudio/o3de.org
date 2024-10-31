---
title: Sky Atmosphere 组件
linktitle: Sky Atmosphere
description: 'Open 3D Engine (O3DE) Sky Atmosphere组件参考。'
toc: true
---

**Sky Atmosphere** 组件提供基于物理的天空和大气渲染功能。

天空是通过对从摄像机开始并与大气层相交的光线上的点进行采样来渲染的。 由于每帧对许多光线进行采样是一项昂贵的操作，因此使用亮度和体积查找表来执行较少的计算。  

![Sky atmosphere in desert mountain scene](/images/user-guide/components/reference/atom/sky-atmosphere/desert-day.jpg)

## 提供方

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)

## 属性

![sky-atmosphere-component-properties](/images/user-guide/components/reference/atom/sky-atmosphere/sky-atmosphere-component-properties.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Origin** | 大气层使用的原点。 如果选择`Ground at world origin`（默认），那么大气层的底部（即地面）将位于世界原点，或 `0,0,0`处。 如果使用`Ground at local origin`，那么大气层的底部将位于天空大气层组件所在实体的变换组件的世界位置。 使用`Ground at local origin`可以方便地控制大气层底部的位置。 地面在本地原点 "的一个实际用途是向下移动地面位置，以便从地面到天空的渐变过渡更加平缓。 尝试使用`Ground at local origin`，并通过将`Z`值设置为`-100`来向下移动实体变换，以获得更平滑的过渡。`Planet center at local origin`设置会使大气层的行星中心位于天空大气层组件所在实体的变换组件的世界位置。 `Planet center at local origin`在需要为行星设置大气层时非常有用。 | `Ground at world origin`,`Ground at local origin`, `Planet center at local origin`| `Ground at world origin` |
| **Ground radius** | 行星的地面半径，单位千米。 | `0.0 km` - `100000.0 km`  | `6360.0 km`  |
| **Ground albedo** | 来自地表的额外光色。 |  | `0.0 0.0 0.0` |
| **Atmosphere height** | 大气层高度（千米）。 | `0.0 km` - `10000.0 km` | `100.0 km` |
| **Illuminance factor** | 使整体氛围明亮或暗淡的额外因素 | | `0.0 0.0 0.0` |
| **Rayleigh scattering scale** | 瑞利散射尺度，用于将瑞利散射值调整到一个易于用颜色选择器控制的范围内。 | `0.0` - `1.0` | `0.0331` |
| **Rayleigh scattering** | 地球表面空气分子的雷利散射系数。 | | `0.175287 0.409607 1.0` |
| **Rayleigh exponential distribution** | 瑞利散射降低到大约 40% 的高度（以千米为单位）。 | | `8 km` |
| **Mie scattering scale** | 米氏散射刻度，用于将米氏散射值调整到一个范围内，以便于使用颜色选择器进行控制。 | | `0.003996` |
| **Mie scattering** | 地球表面气孔分子的米氏散射系数。 | | `1.0 1.0 1.0` |
| **Mie absorption scale** | 米氏吸收率刻度，用于将米氏吸收率值控制在一个范围内，以便于使用颜色选择器进行控制。 | | `0.004440` |
| **Mie absorption** | 地球表面气孔分子的米氏吸收系数。 | | `1.0 1.0 1.0` |
| **Mie exponential distribution** | 米氏散射降低到大约 40% 的高度（以千米为单位）。 | | `1.2 km` |
| **Ozone absorption scale** | 臭氧分子吸收率刻度，用于将臭氧吸收率值控制在一个范围内，以便于使用颜色选择器进行控制。 | | `0.001881` |
| **Ozone absorption** | 臭氧分子在大约大气层中间高度密度最大的一层中的吸收系数。 | | `0.345561 1.0 0.045188` |
| **Aerial depth factor** | 应用于场景深度的系数，用于增加或减少空中透视。 | | `1.0` |
| **Show sun** | 是否显示太阳 | | `True` |
| **Sun orientation** | 用于确定太阳方位的可选实体。 设置后，太阳将使用该实体的旋转。 如果没有指定实体，太阳旋转将使用该组件所在实体的变换组件中的旋转。 | | |
| **Sun color** | 太阳颜色。 | | `1.0 1.0 1.0 1.0` |
| **Sun luminance factor** | 太阳亮度系数，用于增加太阳亮度。 | `0.0` - `1000000.0` | `0.05` |
| **Sun limb color** | 太阳边缘颜色 - 太阳的外缘颜色。 | | `1.0 1.0 1.0 1.0` |
| **Sun radius factor** | 太阳半径系数，用于设置太阳盘的大小。 | `0.0001` - `100.0` | `1.0` |
| **Sun falloff factor** | 太阳落差系数，用于控制小于 `1.0`时的内部落差和大于 `1.0`时的外部落差。 | `0.0` - `200.0`| `1.0` |
| **Fast sky** | 启用后可使用精度较低但性能更快的天空算法，该算法使用一个小型查找表来近似显示可见天空的颜色。未激活时，每个像素都会有一条射线投射到天空中，而不是使用查找表。 | | `True` |
| **Fast aerial perspective** | 启用Volume查找纹理，以获得更快的性能，但需要更多内存。 | | `True` |
| **Aerial perspective** | 用于绘制场景中物体的大气散射效果。 | | `True` |
| **Enable shadows** | 启用大气阴影采样功能。 | | `False` |
| **Near clip** | 开始绘制大气层的距离。 这对于防止建筑物内大气散射的影响非常有用。 | | `0.0` |
| **Near fade distance** | 在大气中渐变的距离，以米为单位。如果设置为 `0.0`，则不执行渐变。 当使用近剪辑设置来淡入大气层空中透视时，这将非常有用。 | | `0.0` |
| **Min samples** | 跟踪时最少的样本数。 样本数越多，精度越高，但性能成本也越高。 | `1` - `64`| `4` |
| **Max samples** | 跟踪时的最大样本数。样本数越多，精度越高，但性能代价也越高。 | `1` - `64`| `14` |


## 示例

### 沙漠太阳

![Sky atmosphere in desert mountain scene](/images/user-guide/components/reference/atom/sky-atmosphere/desert-day.jpg)

#### 步骤 1. 关闭现有天空盒
如果您的场景中有**HDRI天空盒**组件，您可能需要移除或禁用该组件，除非其中有您希望透过天空看到的内容（如云彩）。

#### 步骤 2. 添加**Sky atmosphere**组件
将**Sky atmosphere**组件添加到场景中带有太阳光组件的实体中。

#### 步骤 3。根据需要旋转太阳
通过设置**Transform组件**中的**Rotate**字段来旋转太阳实体： `-151.0` y: `0.0` z: `16.0`。

#### 步骤 4. 调整大气高度
将**Sky atmosphere**原点设置为**Ground at local origin**，并将太阳实体的**Transform组件**中`Z`分量设置为`-500.0`，这样大气层中的透视高度会更高，天空和地面之间的过渡也会更平滑。

#### 步骤 5. 添加 **Bloom** 
为太阳实体添加**Bloom**和**Post FX Layer**组件，并打开**Enable Bloom**设置。将**Intensity**设置为 `0.1`。

#### 步骤 6. 使太阳呈黄色
调整 **Sky Atmosphere** 太阳设置，使其更加橙色，落差更柔和：
* Sun color: `255,235,176`
* Sun luminance factor: `10.0`
* Sun limb color: `215,87,12`
* Sun radius factor: `7.0`
* Sun falloff factor: `102.0`

调整 **Directional light Sky Atmosphere** 的颜色为 `251,247,220`。

(可选) 如果要在大型场景中投射阴影，还需要增加**Shadow far clip**，并调整分割比例，使阴影能充分覆盖场景中的物体。



### 夸张空中透视的夕阳天空

![Sky atmosphere in sunset mountain scene](/images/user-guide/components/reference/atom/sky-atmosphere/sunset-aerial-perspective-shadows.jpg)

#### 步骤 1. 关闭现有天空盒
如果您的场景中有**HDRI skybox**组件，您可能需要移除或禁用该组件，除非其中有您希望透过天空看到的内容（如云彩）。

#### 步骤 2. 添加**天空氛围**组件
将 **Sky atmosphere** 组件添加到场景中带有太阳光组件的实体中。

#### 步骤 3. 将太阳旋转到地平线
将**Rotate**字段设置为 X，旋转太阳实体，使太阳位于地平线正上方： 180.0` y: `0.0` z: `0.0
#### 步骤 4. 调整大气高度
将**Sky atmosphere**原点设置为**Ground at local origin**，并将太阳实体的**Transform组件**的`Z`分量设置为`-500.0`，这样透视图在大气层中就会更高，天空和地面之间的过渡也会更平滑。

#### 步骤 5. 添加**Bloom**
为太阳实体添加**Bloom**和**Post FX Layer**组件，并打开**Enable Bloom**设置。将**Intensity**设置为`1.0`。

#### 步骤 6. 使太阳呈黄色
调整**Sky Atmosphere**的太阳设置，使日落看起来更像日落：
* Sun color: `255,235,176`
* Sun luminance factor: `1.0`
* Sun limb color: `234,86,0`
* Sun radius factor: `6.0`
* Sun falloff factor: `3.0`

调整 **Directional light Sky Atmosphere** 颜色为 `216,69,20`。

(可选) 如果要在大型场景中投射阴影，还需要增加**Shadow far clip**，并调整分割比例，使阴影能充分覆盖场景中的物体。

#### 步骤 8. 增加空中透视

将**Aerial depth factor**设置为`5.0`，以真正增加远处物体前的大气厚度。请注意，此时使用此设置增加大气厚度将不会考虑阴影区域，因此如果太阳从山的后面走过，您可能会在本应没有光亮的地方看到不必要的光亮。

### 微型行星上的多重天空大气层

![miniature planet atomspheres](/images/user-guide/components/reference/atom/sky-atmosphere/planet-atmosphere.png)

如果将**Sky atmosphere**原点设置为`Planet center at local origin`，并缩放**Ground radius**以适应行星，同时将**Atmosphere height**设置为适当的值，则可以在如上场景中存在多个大气层。 为了获得所需的厚大气层向太空的过渡，您需要调整**Mie exponential distribution**和**Rayleigh exponential distribution**，使其值位于新的大气层高度内。 在类似地球的大气层中，**Rayleigh exponential distribution**的高度大约是整个大气层高度的 8%，而**Mie exponential distribution**的高度是大气层高度的 1.2%。

在大气层外时，您可能需要关闭**Fast sky**和**Fast aerial perspective**，以避免出现图形假象，因为查找表尚未针对大气层外时的使用进行优化。
