---
linktitle: Image Gradient
title: Image Gradient 组件
description: 使用Image Gradient组件从Open 3D Engine (O3DE)中的图像生成渐变。
---

添加**Image Gradient**组件，从图像中生成渐变。该组件提供平铺、缩放和自定义采样类型等高级配置选项。还支持图像创建和编辑。

## 提供方

[Gradient Signal Gem](/docs/user-guide/gems/reference/utility/gradient-signal)

## 依赖

[Gradient Transform Modifier](/docs/user-guide/components/reference/gradient-modifiers/gradient-transform-modifier)

## Image Gradient 属性

![Image Gradient component properties](/images/user-guide/components/reference/gradients/image-gradient-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示该组件应用所有属性后的输出渐变效果。 | | |
| **Pin Preview to Shape** | 使用给定实体中兼容的形状组件的边框来确定预览的世界尺寸。如果**Constrain to Shape**为`Enabled`，预览将约束到实际形状，而不仅仅是形状边界。 | EntityId | Current Entity |
| **Preview Position** | 设置预览的世界位置。<br> <br>只有在**Pin Preview to Shape**中没有选择实体时，此字段才可用。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Preview Size** | 设置预览的尺寸。<br> <br>只有在**Pin Preview to Shape**中没有选择实体时，此字段才可用。 | Vector3: 0.0 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Constrain to Shape** | 如果`Enabled`，渐变预览将使用在**Pin Preview to Shape**中选择的实体的边界。<br> <br>此字段仅在**Pin Preview to Shape**中选择了实体时可用。 | Boolean | `Disabled` |
| **Image Asset** | 设置作为渐变值的源图像。<br><br>**注意：**  **Image Gradient** 目前只支持所有可用像素格式的一个子集。支持大部分未压缩的格式，以及 `BC1` 压缩格式。有关支持格式的完整列表，请参阅 [`AZ::RPI::IsImageDataPixelAPISupported`](https://github.com/o3de/o3de/blob/development/Gems/Atom/RPI/Code/Include/Atom/RPI.Public/RPIUtils.h)。 | `.streamingimage` | None |
| **Sampling Type** | 图像数据的采样类型。 | `Point`, `Bilinear`, `Bicubic` | `Point` |
| **Tiling** | 设置水平（X）和垂直（Y）平铺图像的次数。 | Vector2: 0.01 to Infinity | X: `1.0`, Y: `1.0` |
| **Channel To Use** | 要采样的图像的通道分量。<br><br>`Terrarium` 选项用于 Mapzen 定义的基于图像的地形文件格式[此处](https://www.mapzen.com/blog/terrain-tile-service/). | `Red`, `Green`, `Blue`, `Alpha`, `Terrarium` | `Red` |
| **Mip Index** | 指定从哪个 mip 级索引采样。<br><br>如果您指定的 mip 级别高于图像中可用的 mip 级别数，那么将使用现有的最高 mip 索引（请参阅 [`MipCountMax`](https://github.com/o3de/o3de/blob/development/Gems/Atom/RHI/Code/Include/Atom/RHI.Reflect/Limits.h)）。 | Int: `0` to `15` | `0` |
| **Custom Scale** | 选择应用于所有图像数据的数值缩放操作。<br><br>`Auto`选项将根据图像数据中的最小/最大值，自动将数值缩放到`0`-`1`范围内。 | `None`, `Auto`, `Manual` | `None` |
| **Range Minimum** | 用于将图像数据缩放到梯度的 `0`-`1` 范围内的最小值。所有位于或低于最小值的图像值都将缩放到 `0`。<br> <br>只有当 **Custom Scale** 字段设置为`Manual`时，该字段才可用。 | Float: `0.0` to `1.0`  | `0.0` |
| **Range Maximum** | 用于将图像数据缩放到梯度的 `0`-`1` 范围内的最大值。所有位于或高于最大值的图像值都将缩放到 `1`。<br> <br>只有当 **Custom Scale** 字段设置为`Manual`时，该字段才可用。 | Float: `0.0` to `1.0`  | `1.0` |
| **Save Mode** | 指定图像编辑后如何选择保存图像的路径。 | `Save As...`, `Auto Save`, `Auto Save With Incrementing Names` | `Auto Save` |

### Sampling types

有多种取样类型可供选择。选择最适合特定需求的采样类型通常需要在性能和质量之间取得平衡。

| Sampling Type | 说明 | 性能开销 | 质量 | 示例 |
| - | - | - | - | - |
| `Point` | `Point`采样是默认设置，仅对源图像的每个像素的一个点进行采样。<br><br>如果图像的采样频率高于像素数，则会产生块状伪影。当像素大小与采样频率完全匹配时，`Point`采样会产生最佳效果。 | 1x | Poor | ![Image Gradient using point sampling](/images/user-guide/components/reference/gradients/image-gradient-component-point.png) |
|  `Bilinear` | `Bilinear` 滤波器通过在请求像素周围的网格中请求四个点，然后在点之间执行插值来平滑图像。<br><br>由于双线性滤波器在 4 个点之间使用线性插值，因此会在平滑数据中产生明显的加形伪影。 `Bilinear`采样是最佳的通用选择，因为它兼顾了性能和质量。 | 4x | Good | ![Image Gradient using bilinear interpolation](/images/user-guide/components/reference/gradients/image-gradient-component-bilinear.png) |
|  `Bicubic` | `Bicubic`滤波器通过在所请求的像素周围的网格中请求 16 个点，然后在这些点之间执行 Catmull-Rom 插值来平滑图像。当质量比性能更重要时，`Bicubic`采样是最佳选择。 | 16x | Great | ![Image Gradient using bicubic interpolation](/images/user-guide/components/reference/gradients/image-gradient-component-bicubic.png) |

### 创建图像

要创建新图像，请按 **Create New Image...创建新图像...** 按钮。系统将提示您以像素为单位的图像宽度和高度，然后提示您图像的保存位置。如果图像保存在项目使用的[源资产目录](/docs/user-guide/assets/pipeline/scan-directories/)中，图像渐变器将自动用保存的图像填充**Image Asset**字段。

{{< important >}}
图像名称应以 `_gsi`结尾（例如，`image_gsi.tif`）。这将确保资产处理器把图像作为梯度信号图像（gsi）资产处理，图像数据不会被压缩。
{{< /important >}}

## 编辑图像

要编辑现有图像，请按下Image Gradient组件上的**Edit**按钮，或在视口中的**Component Switcher**中选择 “图像渐变 ”组件的图标。这将进入 “画笔 ”模式。有关如何使用画笔的详细信息，请参阅[Paint Brush](/docs/user-guide/components/reference/paintbrush/paintbrush) 文档。

编辑完成后，按**Esc**、按图像渐变组件上的**Done**按钮或在视口中的**Component Switcher**中选择其他组件图标，结束画笔模式。此时，图像更改将被保存，由资产处理器重新处理，并在处理完成后重新加载。

**Save Mode**决定图像的保存位置。

| Save Mode | 说明 |
| - | - |
| `Save As...` | 每次保存图像时都会提示保存位置。 |
| `Auto Save` | 加载关卡后首次保存图像时会提示保存位置，但以后每次保存时都会自动覆盖该位置的图像。 |
| `Auto Save With Incrementing Names` | 自动保存名称末尾带有递增数字的图像，只有在已有名称为该名称的图像时才会提示保存位置。<br><br>例如，如果最初选择的图像是 `image_gsi.tif`，这将把它保存为 `image_gsi.0000.tif`，然后保存为 `image_gsi.0001.tif`，然后保存为 `image_gsi.0002.tif`，等等。|

## ImageGradientRequestBus

使用以下带有`ImageGradientRequestBus` EBus 接口的请求函数与游戏中的Image Gradient 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetImageAssetPath` | 返回`AZ::RPI::StreamingImageAsset` 属性 的路径。 | None | String | Yes |
| `GetTilingX` | 返回 **Tiling X** 属性的值。property. | None | Float | Yes |
| `GetTilingY` | 返回 **Tiling Y** 属性的值。 | None | Float | Yes |
| `SetImageAssetPath` | 使用资产的绝对路径或相对路径设置`AZ::RPI::StreamingImageAsset` 属性的路径。 | String | None | Yes |
| `SetTilingX` | 设置 **Tiling X** 属性的值。 | Float | None | Yes |
| `SetTilingY` | 设置 **Tiling Y** 属性的值。 | Float | None | Yes |
