---
linktitle: Gradient Baker
title: Gradient Baker 组件
description: 在Open 3D Engine (O3DE)中，使用Gradient Baker组件将复杂的渐变节点图烘焙成单个静态图像。
---

使用 **Gradient Baker** 组件可将复杂的渐变节点图烘焙成一张静态图像。这样，你就可以在运行时使用单个[Image Gradient](/docs/user-guide/components/reference/gradients/image-gradient/)来优化性能，而不用每次都计算整个节点图。

例如，假设您有一个复杂的梯度链，其中包含一个生成器和多个梯度修改器。

你可以创建一个**Gradient Baker**，并将其输入梯度连接到你想要烘焙的梯度链输出。**Gradient Baker**还需要一个输入边界，用于对数据进行采样。将这两部分连接起来后，图形将如下所示。

![Complex chain with gradient baker](/images/user-guide/components/reference/gradients/gradient-baker-chain-with-baker.png)

点击**Bake image**组件上的**Gradient Baker**按钮，就能烘焙出一张静态图像，该图像可用于单个[Image Gradient](/docs/user-guide/components/reference/gradients/image-gradient/)，从而将整个复杂图形简化为单个组件。

![Complex chain with gradient baker](/images/user-guide/components/reference/gradients/gradient-baker-single-image-gradient.png)

最后一个优化步骤是选中梯度链中的所有梯度，并将其标记为 **Editor only**。这将防止在运行时加载/计算它们。您也可以选择将**Gradient Baker**标记为**Editor only**，但这不会产生任何影响，因为它没有运行时组件。

![Mark gradient/gradient generators Editor only to disable at runtime](/images/user-guide/components/reference/gradients/gradient-baker-editor-only.png)

## 提供方

[Gradient Signal Gem](/docs/user-guide/gems/reference/utility/gradient-signal)

## Gradient Baker 属性

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Preview** | 显示在应用所有属性后将从该组件烘焙出的渐变图像。 | | |
| **Input Bounds** | 带有形状组件的实体，用于确定数据采样的位置。 | EntityId | None |
| **Resolution** | 输出烘焙图像的分辨率。| Vector2: 1 to Infinity | X:`512`, Y:`512` |
| **Output Format** | 输出烘焙图像的输出格式。 | `R8`, `R16`, `R32` | `R32` |
| **Output Path** | 输出图像的文件路径。<br><br>**注意：** 默认情况下，初始输出路径的后缀为 `_gsi`。这是因为将消耗输出图像的**Image Gradient**只支持可用像素格式的子集。后缀 `_gsi` 将确保资产处理器使用受支持的格式。如果输出图像已配置为使用受支持的格式，则可以省略后缀 `_gsi`。有关此限制的更多信息，请参阅[Image Gradient文档](/docs/user-guide/components/reference/gradients/image-gradient/#image-gradient-properties) | AZ::IO::Path | None |

### Gradient 属性

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Gradient Entity Id** | 设置具有活动 **Gradient** 组件的实体。| EntityId | None |
| **Opacity** | 设置输入渐变的不透明度。 | Float: 0.0 - 1.0 | `1.0` |
| **Invert Input** | 反转输入梯度的值。 | Boolean | `Disabled` |
| **Preview (Input)** | 显示由 **Gradient Entity Id** 中设置的实体提供的渐变。 |  |  |
| **Enable Transform** | 如果`Enabled`，则可以修改输入梯度的平移、缩放和旋转。 | Boolean | `Disabled` |
| **Translate** | 设置输入梯度的平移。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Scale** | 设置输入梯度的比例。 | Vector3: 0.0001 to Infinity | X:`1.0`, Y:`1.0`, Z:`1.0` |
| **Rotate** | 设置输入梯度的旋转角度。 | Vector3: -Infinity to Infinity | X:`0.0`, Y:`0.0`, Z:`0.0` |
| **Enable Levels** | 如果`Enabled`，则可以修改输入梯度的输入值和输出值。 | Boolean | `Disabled` |
| **Input Mid** | 设置输入梯度的中值。| Float: 0.0 - 1.0 | `1.0` |
| **Input Min** | 设置输入梯度的最小值。 | Float: 0.0 - 1.0 | `0.0` |
| **Input Max** | 设置输入梯度的最大值。 | Float: 0.0 - 1.0 | `1.0` |
| **Output Min** | 设置输出梯度的最小值。 | Float: 0.0 - 1.0 | `0.0` |
| **Output Max** | 设置输出梯度的最大值。 | Float: 0.0 - 1.0 | `1.0` |

## GradientBakerRequestBus

使用以下带有 `GradientBakerRequestBus` EBus 接口的请求函数与 **Gradient Baker** 组件进行通信。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `BakeImage` | 将图像烘焙到输出路径。 | None | None | Yes |
| `GetInputBounds` | 返回要采样的输入边界的 AZ::EntityId。 | None | AZ::EntityId | Yes |
| `SetInputBounds` | 设置要采样的输入边界的 AZ::EntityId。 | AZ::EntityId | None | Yes |
| `GetOutputResolution` | 返回烘焙图像的输出分辨率。 | None | AZ::Vector2 | Yes |
| `SetOutputResolution` | 设置烘焙图像的输出分辨率。 | AZ::Vector2 | None | Yes |
| `GetOutputFormat` | 返回烘焙图像的输出格式。 | None | GradientSignal::OutputFormat | Yes |
| `SetOutputFormat` | 设置烘焙图像的输出格式。 | GradientSignal::OutputFormat | None | Yes |
| `GetOutputImagePath` | 返回烘焙输出图像的路径。 | None | AZ::IO::Path | Yes |
  | `SetOutputImagePath` | 设置输出图像的烘焙路径。 | AZ::IO::Path | None | Yes |
