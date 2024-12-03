---
linkTitle: 了解频率分离
title: 了解频率分离
description: 了解各种频率分离技术以及它们如何应用于细节材质。
weight: 300
toc: true
---

本教程帮助我们了解频率分离 （FS） 的重要性，FS 是图像编辑中使用的一种技术，用于将高频细节（如纹理和瑕疵）与低频信息（如颜色和色调）分开。这允许编辑器分别对这些不同的元素进行调整，从而实现更精确和有针对性的编辑。例如，图像作者可以使用频率分离来平滑肖像的肤色而不会丢失皮肤的纹理，或者在不影响整体肤色的情况下去除瑕疵。该技术涉及在图像中创建两个图层，一个用于高频，一个用于低频，然后使用模糊和其他技术将这两个图层分开。频率分离在各种编辑情况下都是一个有用的工具，包括人像修饰、产品摄影和横向编辑。

## 生成低通和高通

地形细节贴图可以通过对图像运行高通过滤器来获得。从具有大量颜色和细节的图像开始，我们将分离出低频和高频数据。
从图像中分离低频/高频数据是图像编辑器（如 Photoshop）中的手动过程。

您可以通过多种方式生成基本的高通纹理：
1. 在图像编辑器中使用高通滤波器：
    * Adobe® Photoshop® 软件: 菜单栏 > filter > other > High Pass
    * Gimp: 使用 Filters->Enhance->High Pass, with Std Dev=10, Contrast=1.0.
    * Krita: 您可以使用本文档中概述的方法
1. 或者您的材质创作应用程序中的类似高通滤波器，例如“Adobe® Substance3D Designer® 软件：
    * 在 Google 上搜索“substance high-pass”或“substance luminance highpass”会让你进入这些帮助页面

本文档介绍了一种具有更大灵活性的非滤镜方法，该方法可用于将图像分为两个纹理：
1. 适合在 Terrain Detail 材质中使用的高频变化纹理。
1. 可在 Terrain Macro Color 工作流中使用的低频颜色贴图。

以下部分介绍如何在 Photoshop 中生成包含基本图像的低通和高通滤镜的图层：

| <div style="width:250px">原始 (Layer 1)</div> | <div style="width:250px">低通滤波器(Layer 2)</div> | <div style="width:250px">高通滤波器(Layer 3)</div> |
| :-- | :-- | :-- |
|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/original_image.png" width="250" alt="Original (Layer 1)" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/original_low_pass.png" width="250" alt="Low pass filter (Layer 2)" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/original_high_pass.png" width="250" alt="High pass filter (Layer 3)" >}}|
|<ul><li>Base layer only</li></ul>|<ul><li>将原始图像复制到第 2 层</li><li>高斯模糊（我用了 16）</li><li><b>注意:</b>模糊程度越高，<br>低通中的信息越少，而高通中的信息就越多。</li><li><b>Alternatively:</b> 您可以找到<br>整个图像的平均单色值，然后将此纯<br>色用作低通。这将最大化<br>高<br>通道细节中保留的细节量和颜色变化。</li></ul>|<ul><li>将原始图像再次复制到图层 3 中</li><li>现在我们将生成高通</li><li>In photoshop, with Layer 3 selected</li><li>使用菜单选项： Image > Apply Image</li><li>在对话框中，使用以下设置<ul><li>Layer: (Use Layer 2)</li><li>Blending: (Use subtract)</li><li>Scale: (Use 2)</li><li>Offset: (Use 128)</li></ul></li></ul>这将为您提供如上所示的高通图像 ^<br>{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/ps_blur_dialog.png" alt="Photoshop gaussian blur for 8-bit image" >}}<br>注： 以上设置是 8 位图像的设置。<br>16 位图像的缩放和偏移设置如下：<ul><li>Check invert</li><li>Blending: (Use add)</li><li>Scale: (Use 2)</li><li>Offset: (Use 0)</li></ul></li></ul>{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/ps_blur_dialog_16.png" alt="Photoshop gaussian blur for 16-bit image" >}}|

### 平铺图像

对于平铺图像，请在分离低频和高频之前使用以下步骤，以确保低通和高通都能正确平铺：

1. 选择基础图像
1. 选择  **Edit** > **Define Pattern...**
1. 将选定的基础图像存储为图案
1. 选择  **Image** > **Canvas Size...**
1. 在Canvas Size对话框中，设置**Width** and  **Height** 为 300%
1. 创建 3x3 平铺图像。选择**Edit** > **Fill...**。在 Fill （填充） 对话框中，执行以下操作：
    * 设置 **Contents** 为 Patterns
    * 选择 **Custom Pattern** ，然后选择您在步骤 3 中从基础图像创建的图案。
1. 执行前面的步骤 [生成低通和高通](#generating-low-and-high-pass) 图层
1. 选择 **Image** > **Canvas Size...**
1. 在Canvas Size对话框中，设置**Width** 和 **Height** 为 33.33%，或原始图像大小（以像素为单位）以将图像裁剪到中心图块。

前面的步骤允许高斯模糊考虑沿相邻边界的包裹平铺。计算低通（模糊）时，像素信息将换行。当它被减去和裁剪时，低通道和高通道都会正确平铺。

## Blending

接下来，您将组合（混合）低通（第 2 层）和高通（第 3 层）。

在 Photoshop 中，将高通的图层混合模式设置为线性光。

![Photoshop: Set blend mode of high pass to Linear light](/images/learning-guide/tutorials/environments/detail_macro_materials/ps_linear_light.png)

在前面的屏幕截图中，通过将低通频率和高通频率正确混合在一起，可以恢复原始图像保真度。

## Downsampled low pass

通过低通和高通的混合，您可以对低通图像进行下采样，并且仍然可以获得最终的混合图像，而质量损失很小。以下示例将低通下采样到更小的图像，然后从两个不同分辨率的映射中重建图像。一些信息会丢失，这可能会降低图像的保真度、质量和整体数据完整性等方面，但您可以尝试这些级别以找到可接受的结果。

| <div style="width:160px">Original Low Pass<br>(1024 x 1024 pixels)</div> | <div style="width:160px">Downsampled<br>(64 x 64 pixels)</div> | <div style="width:160px">Interpolated<br>(Bilinear)</div> | <div style="width:160px">Reconstructed<br>(Interpolated + Original High Pass)</div> |<div style="width:160px">Difference<br>(Original - Reconstructed)</div> |
|-|-|-|-|-|
|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/original_low_pass.png" width="150" alt="Original Low Pass (1024 x 1024 pixels)" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/downsampled_64.png" width="150" alt="Downsampled (64 x 64 pixels)" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/interpolated_64.png" width="150" alt="Interpolated (Bilinear)" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/blended_64.png" width="150" alt="Interpolated + Original High Pass (Reconstructed blend)" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/difference_64.png" width="150" alt="Difference (Original minus Reconstructed blend)" >}}|

正如您在最终重建图像中看到的那样，质量几乎没有明显的损失。您需要放大差异图像以观察差异的微小程度，从而进一步突出了这一点。

让我们进一步尝试另一个 downsampled。

| <div style="width:160px">Original Low Pass<br>(1024 x 1024 pixels)</div> | <div style="width:160px">Downsampled<br>(16 x 16 pixels)</div> | <div style="width:160px">Interpolated<br>(Bilinear)</div> | <div style="width:160px">Reconstructed<br>(Interpolated + Original High Pass)</div> |<div style="width:160px">Difference<br>(Original - Reconstructed)</div> |
|-|-|-|-|-|
|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/original_low_pass.png" width="150" alt="Original Low Pass (1024 x 1024 pixels)" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/downsampled_16.png" width="150" alt="Downsampled (16 x 16 pixels)" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/interpolated_16.png" width="150" alt="Interpolated (Bilinear)" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/blended_16.png" width="150" alt="Interpolated + Original High Pass (Reconstructed blend)" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/difference_16.png" width="150" alt="Difference (Original minus Reconstructed blend)" >}}|

在这个级别的下采样中，完整性存在明显的差异，质量可以说是降低了。但是，保真度仍然足够好，可用于地形细节映射用例。

## 结果

以下是所有三个重建再次并排，每个是 1024 像素的最终重建分辨率，只有低频低通发生了变化。很难在视觉上分辨出差异，但如果你仔细观察处理最多的极右版本，则在某些区域丢失了一些降低的对比度。

| Low Pass: 1024<br>High Pass: 1024</div> | Low Pass: 64 (Bilinear upsample)<br>High Pass: 1024</div> | Low Pass: 16 (Bilinear upsample)<br>High Pass: 1024</div> |
|-|-|-|
|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/results_0.png" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/results_1.png" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/results_2.png" >}}|

## Color Alteration

这是一种非常灵活的技术，因为高通频率可以应用于低通基色的大范围变化，并且仍然可以获得不错的结果。以下是一些极端的例子：

| <div style="width:160px">Original Low Pass<br>(1024 x 1024 pixels)</div> | <div style="width:160px">Downsampled<br>(32 x 32 pixels)</div> | <div style="width:160px">Interpolated<br>(Bilinear)</div> | <div style="width:160px">Reconstructed<br>(Interpolated + Original High Pass)</div> |
|-|-|-|-|
|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/color_shift_0.png" width="150" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/color_shift_1.png" width="150" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/color_shift_2.png" width="150" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/color_shift_3.png" width="150" >}}|

在下一个版本中，您只需简单地改变我们原来的低通颜色的色相。

| <div style="width:160px">Original Low Pass<br>(1024 x 1024 pixels)</div> | <div style="width:160px">Downsampled<br>(32 x 32 pixels)</div> | <div style="width:160px">Interpolated<br>(Bilinear)</div> | <div style="width:160px">Reconstructed<br>(Interpolated + Original High Pass)</div> |
|-|-|-|-|
|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/hue_shift_0.png" width="150" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/hue_shift_1.png" width="150" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/hue_shift_2.png" width="150" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/hue_shift_3.png" width="150" >}}|

如您所见，您可以对基色进行非常突然和狂野的更改，但仍然可以获得视觉上有趣的结果！

## 宏观素材低通

您可以在我们的 Macro 材质的纹理中使用小的缩减采样低通图像。此用例有以下几个选项：
* 使用下采样的低通图像作为纹理输入进行纹理处理
* 使用下采样的低通生成色带（和匹配的高度贴图），可以用作 World Machine 等程序的输入，用于着色
* 使用缩减采样的低通道作为绘制地形的颜色样本

然后，通过适当的混合和同步重复，您可以使用高通细节纹理来增强这一点，并在特写时产生类似于原始图像的效果。

## 高通道细节图

您可以使用 Photoshop 的内置高通滤镜生成高通细节贴图，然后将其应用回原始图像，以通过以下过程生成匹配的低通宏材质纹理：

| <div style="width:160px">Generate the High Pass</div> | <div style="width:160px">Linear Burn</div> | <div style="width:160px">Linear Add</div> | <div style="width:160px">Low Pass</div> | <div style="width:160px">Low Pass<br>(Swapped ordering)</div> |
|-|-|-|-|-|
|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/generated_high_pass.png" width="150" alt="Generate the High Pass" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/linear_burn.png" width="150" alt="Linear Burn" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/linear_add.png" width="150" alt="Linear Add" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/low_pass_errors.png" width="150" alt="Low Pass" >}}|{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/low_pass_order_swap.png" width="150" alt="Low Pass (Swapped ordering)" >}}|
|Leave the original unaltered image in the default Photoshop layer "background".<br><br>Duplicate the original image into "Layer 1", we will use this layer to generate the high-pass.<br><br>Use "Layer 1" to generate the high-pass using the filter method.<br><br>Filter > Other > High Pass<ul><li>Use a radius of 16</li></ul>|<ul><li>Duplicate the High Pass (Layer 2)</li><li>Level the Image, Output Levels: 0 ... 128</li><li>Invert the Image</li><li>Set the Layer to "Linear Burn"</li></ul>{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/ps_linear_burn.png" alt="Photoshop linear burn" >}}|<ul><li>Duplicate the High Pass (Layer 3)</li><li>Level the Image, Output Levels: 128 ... 255</li><li>Invert the Image</li><li>Set the Layer to "Linear Dodge (Add)"</li></ul>{{< image-width src="/images/learning-guide/tutorials/environments/detail_macro_materials/ps_linear_add.png" alt="Photoshop linear add" >}}|As you can see, we are pretty close to the simple Gaussian Blurred Low Pass Method.<br>Close enough that after downsampling and interpolation the errors might be removed.<br><br>But as you can see in the image to the right, the errors are a result of the order of operation.|If we swap the ordering of<br>Layer 2 / 3, the errors show up<br>in the upper ranges instead.<br><br>The other method is preferred<br>since it gives you full control<br>over the low pass, separates<br>the high pass into less steps, and can be done in a single operation without errors.|
