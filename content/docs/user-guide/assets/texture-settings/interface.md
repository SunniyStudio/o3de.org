---
linkTitle: 用户界面
title: Texture Settings用户界面
description: 学习如何浏览纹理设置界面，自定义处理图像源资产的方式。
weight: 300
toc: true
---

通过**纹理设置**，您可以指定如何为**Open 3D Engine (O3DE)** 处理纹理源资产。纹理设置 一次对一个纹理源资产进行操作。您在 “纹理设置 ”中选择的选项会保存在一个 JSON 格式的`.assetinfo` sidecar 文件中。**资产处理器**会读取`.assetinfo`文件，并在处理纹理源资产时应用这些选项。

## 使用 Texture Settings

要打开Texture Settings，执行以下操作：
1. 在**O3DE Editor**中，在**Asset Browser**中，选择要修改的纹理源资产。您可以使用 “资产浏览器 ”顶部的搜索栏来过滤资产列表。

1. **右击** 纹理源资产，然后点击 **Edit Texture Settings...**。

![ Right click an asset to open Texture Settings. ]( /images/user-guide/assets/texture-settings/open-texture-settings.png )

## 界面部分

Texture Settings 界面包含三个部分：

* [Texture preview](#texture-preview) - 在 “纹理设置 ”窗口左侧有一个大纹理预览、纹理分辨率和文件大小信息以及各种预览工具。
* [Texture settings](#texture-settings) - 各种平台的纹理处理预设和分辨率设置位于 “纹理设置 ”窗口的右上方。
* [Mipmap settings](#mipmap-settings) - 在 “纹理设置 ”窗口的右下方，可以切换启用 mipmap 生成和 mipmap 处理设置。

{{< image-width "/images/user-guide/assets/texture-settings/texture-settings-ui-01.png" "800" "The Texture Settings user interface texture preview." >}}

### Texture preview

纹理预览部分显示处理后的纹理产品资产。本部分还提供了信息和工具，您可以用来检查源资产的处理结果。

| 部分 | 说明 |
| - | - |
| **Channel Selector** | 通过左上角的下拉列表，您可以选择要在纹理预览中显示的通道。您可以选择显示 RGB、带 Alpha 的 RGB 或单独的红、绿、蓝或 Alpha 通道。 |
| **Tiled Preview** | 启用后，将显示一个两两平铺的纹理预览，以便检查重复纹理是否有可见接缝。 |
| **Preview Update** | 预览窗口上方右侧的下拉列表 {{< icon "caret-open.svg" >}} 可选择在纹理设置更改时自动更新预览，还是手动更新预览。默认情况下，纹理预览会自动更新。当预览更新设置为手动时，**点击** {{< icon "refresh-active.svg" >}} 刷新按钮即可更新纹理预览。 |
| **Preview Window** | 预览窗口显示处理后的纹理。按住下列热键之一可切换纹理预览显示。<br><br>**Shift** - 显示 RGBA 通道。<br>**Alt** - 显示 Alpha 通道。<br>**空格** - 显示全分辨率纹理。 |
| **Mipmap Level** | 预览窗口下方的中心位置是 Mimap 级别。显示的是当前显示的 mipmap 的编号。您可以**点击** {{< icon "arrow_left-default.svg" >}} 上一个按钮和 {{< icon "arrow_right-default.svg" >}} 下一个按钮来循环浏览可用的 mipmap。 |
| **Resolution** | 显示所选 mipmap 级别的分辨率。  |
| **Size** | 显示所选 mipmap 级别的文件大小。 |


### Texture settings

纹理设置部分包含不同纹理类型的预设以及各种目标平台的选项和信息。

![Texture Settings preset and resolution options.](/images/user-guide/assets/texture-settings/texture-settings-ui-02.png)

| 部分 | 说明 |
| - | - |
| **Preset** | 为各种纹理使用情况提供预设列表。所选预置指定了纹理源资产的处理方式。某些预设需要专门的纹理源资产。预设在 JSON 格式的`.preset`文件中定义，这些文件位于`/o3de/Gems/Atom/Asset/ImageProcessingAtom/Assets/config/`中。你可以根据现有预设的规格创建自己的预设。<br><br>更多信息请参阅 [纹理预设](texture-presets) 表格。 |
| **Information** | 在**预设**列表右侧的 {{< icon "info.svg" >}}信息图标，会显示所选预设的相关信息，包括文件掩码列表，你可以将文件掩码附加到纹理文件名中，从而自动选择预设。每个预设的文件掩码在`/o3de/Gems/Atom/Asset/ImageProcessingAtom/Assets/config/ImageBuilder.settings`中指定。你可以编辑该文件，修改现有的文件掩码或为自己的预设添加字符串。<br><br>可以通过选择不同的预设并将选择保存到`.assetinfo` sidecar 文件来覆盖自动预设选择。 |
| **Reset** | {{< icon "refresh-active.svg" >}} 重置图标可将 “纹理设置 ”中的更改重置为默认预设值。 |
| **Use Max Res** | 启用后，即使在性能规格较低的平台上，也会使用纹理的最佳质量版本。对于包含文字或其他细节的纹理，建议启用**Use Max Res**，因为当纹理靠近摄像头时，文字或其他细节必须清晰可辨。|
| **Platform** | 在**Platform**部分，您可以为不同目标平台设置纹理的最大分辨率。各列显示了不同目标平台上纹理产品资产的最大分辨率和像素格式。<br><br>每个目标平台的产品资产最大分辨率显示在**Max Res**列中。对于某些预设，可在`.preset`文件中指定特定平台的默认最大分辨率。如果没有为某个平台指定最大分辨率，默认的最大分辨率就是纹理源资产的分辨率。<br><br>您可以通过编辑 **Res Limit** 列中的值来降低平台的最大分辨率。**Res Limit** 值范围为 0 - 5，结果如下：<br><br>**0** - 使用默认的最大纹理分辨率。默认分辨率是纹理源资产的分辨率，或在`.preset`文件中指定的平台最大分辨率。<br>**1** - 纹理分辨率降低 1/2。<br>**2** - 纹理分辨率降低 1/4。内存消耗降至全分辨率的 1/16。<br>**3** - 纹理分辨率降低 1/8。内存消耗降至全分辨率的 1/64。<br>**4** - 纹理分辨率降低 1/16。内存消耗降至全分辨率的 1/256。<br>**5** - 纹理分辨率降低 1/32。内存消耗降至全分辨率的 1/1024。<br><br>The **Format** 列显示各目标平台纹理产品资产的像素格式。|

### Mipmap settings

![Texture Settings mipmap options.](/images/user-guide/assets/texture-settings/texture-settings-ui-03.png)

贴图设置部分提供了生成纹理*贴图*的选项。贴图是纹理的缩小版本，在不需要全分辨率纹理时使用，例如物体距离摄像机较远或在性能规格较低的目标平台上。

第 **[0]** 层是全分辨率纹理。层**[1]**至**[5]**是纹理的连续缩小版本。**[1]**级是最大的 mipmap，**[5]**级是最小的。在每一级 mipmap 中，纹理的分辨率都会减半，因此该级 mipmap 的大小是前一级的四分之一。

| 部分 | 说明 |
| - | - |
| **Create Mipmaps** | 启用后，可使用 mipmap 设置，并为纹理生成 `.imagemipchain` 产品资产。 |
| **Pixel Sampler** | 设置生成 mipmap 的像素采样类型。<br>**Min** - 使用最小像素值。<br>**Max** - 使用最大像素值。<br>**Sum** - 求像素值的总和并使用结果。|
| **Filter Type** | 选择用于生成 mipmap 的滤波类型。当每个 mipmap 的分辨率降低时，滤波类型会使用不同的样本大小和算法来计算每个像素的值。<br><br>过滤器类型从列表顶部的基本型（**Point**, **Average**, **Linear**, 和 **Bilinear**）到底部的高级型（**Gaussian**, **BlackmanHarris**, 和 **KaiserSinc**）依次排列。各种滤镜都有各自的优势，使用最先进的滤镜不一定能获得最佳效果。高级过滤器类型可能需要更多的处理时间，如果您有大量的纹理源资产或许多纹理源资产需要处理，这可能是一个考虑因素。 |
| **Adjust Alpha** | 启用后，可使用 **Alpha Test Bias** 在每个 mipmap 级别手动调整 mipmap alpha 通道，以确保每个 mipmap 级别都有适当的 alpha 覆盖范围。  |
| **Alpha Test Bias** | 将 mipmap 的 alpha 通道值乘以基于 alpha 覆盖范围的比例值。可为每个 mipmap 级别指定一个从**0** - **100**的值。 |
| **Apply** | **Apply**按钮会将纹理设置保存到`.assetinfo`边卡文件中。创建或更新`.assetinfo`文件时，会自动处理纹理源资产。 |
| **Close** | **Close**按钮可关闭 “纹理设置 ”窗口。 |

## `.assetinfo` sidecar 文件

当您在 “纹理设置 ”界面右下方选择**Apply**时，您在 “纹理设置 ”中设置的选项将保存到 `.assetinfo`副卡文件中。Asset Processor 会将侧卡文件识别为纹理源资产的源依赖，并在创建或更新`.assetinfo`文件时自动处理纹理源资产。

`.assetinfo`边卡文件采用 JSON 格式，可由人工读取，并可通过 Python 脚本等自动化流程轻松生成和修改。
