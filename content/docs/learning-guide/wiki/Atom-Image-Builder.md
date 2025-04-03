---
title: "Image Builder"
description: ""
toc: false
---

# Image Builder
O3DE 图像生成器是一种资产生成器，用于将不同类型的图像文件转换为用于 O3DE 运行时的纹理。生成器由 AssetProcessor 调用。
映像生成器位于 Atom 的 ImageProcessingAtom Gem 中，该 Gem 位于 O3DE 引擎的 \Gems\Atom\Asset\ImageProcessingAtom\ 文件夹下。

# 支持的图像文件类型
以下是支持的图像文件类型列表
* "*.tif"
* "*.tiff"
* "*.png"
* "*.bmp"
* "*.jpg"
* "*.jpeg"
* "*.tga"
* "*.gif"
* "*.dds"
* "*.exr"

# 支持的图像像素格式
以下是支持的图像像素格式列表：
(源: o3de/Gems/Atom/Asset/ImageProcessingAtom/Code/Source/Processing/PixelFormatInfo.cpp)

Unsigned
* "R8G8B8A8"
* "R8G8B8X8"
* "R8G8"
* "R8"
* "A8"
* "R16G16B16A16"
* "R16G16"
* "R16"

ASTC
* "ASTC_4x4"
* "ASTC_5x4"
* "ASTC_5x5"
* "ASTC_6x5"
* "ASTC_6x6"
* "ASTC_8x5"
* "ASTC_8x6"
* "ASTC_8x8"
* "ASTC_10x5"
* "ASTC_10x6"
* "ASTC_10x8"
* "ASTC_10x10"
* "ASTC_12x10"
* "ASTC_12x12"

BCn
* "BC1"
* "BC3"
* "BC4"
* "BC5"
* "BC6UH"
* "BC7"

Float formats
* "R9G9B9E5"
* "R32G32B32A32F"
* "R32G32F"
* "R32F"
* "R16G16B16A16F"
* "R16G16F"
* "R16F"


# Image Builder 设置
映像生成器使用的默认设置位于 O3DE 的 \Gems\Atom\Asset\ImageProcessingAtom\Assets\Config\ 文件夹中，我们称之为默认文件夹。

有两种类型的设置文件：一个 ImageBuilder.settings 文件和 .preset 文件。这两个设置文件都是 json 格式。

ImageBuilder.settings 定义生成器的全局设置。您可以在 BuilderSettingManager::Reflect（AZ::ReflectContext* context） 函数中找到可用设置的列表。 

预设文件定义图像转换过程中可能发生的所有转换。可用设置列在 void PresetSettings::Reflect（AZ::ReflectContext* context） 函数中。

每个 O3DE 项目都可以覆盖或修改 ImageBuilder.settings 文件或任何预设文件。它还可以添加新的预设。这些文件应放在 %project%\Config\AtomImageBuilder\ 文件夹中，我们称之为 project folder。

要修改 ImageBuilder.settings，用户可以使用相同的名称 ImageBuilder.settings 在所述文件夹中创建一个 json 合并补丁。图像生成器将合并此文件和 \Gems\Atom\Asset\ImageProcessingAtom\Assets\Config\ImageBuilder.settings 以获取最终设置。
例如，ImageBuilder.settings 可能如下所示：

```
{
    "ClassData": {
        "PresetsByFileMask": {
            // remove _diff file mask
            "_diff": null,
            // add new mapping to map _alpha to Alpha preset
            "_alpha": [ "Alpha"]
        }
    }
}
```

对于预设文件，当前项目文件夹中的相同预设将覆盖默认文件夹中的预设。（我们可能会考虑将来将其切换为使用 merge）。用户还可以在项目文件夹中添加新的预设文件，以添加新的自定义预设。最好在 ImageBuilder.settings 中为新预设添加文件蒙版映射，以便轻松地将新预设应用于文件。每个预设中的 fileMasks 仍受支持，但将来可能会弃用。

# 预设设置
预设文件 （*.preset） 中的设置在`PresetSettings `类中定义，该类位于`o3de\Gems\Atom\Asset\ImageProcessingAtom\Code\Source\BuilderSettings\PresetSettings.h`文件中。设置的名称映射到 `void PresetSettings::Reflect(AZ::ReflectContext* context)` 函数中。以下是预设文件中一些常用的设置。（这里没有列出一些不经常使用的设置。您可以在头文件中找到它们）
|设置名称 |说明|
|-------------|-------------------------|
|Name         |预设的唯一名称|
|Description  |有关预设用途的简短描述|
|GenerateIBLOnly|控制此预设是否仅调用 IBL 预设，而不生成自己的输出产品|
|SourceColor  |源图像的色彩空间。它可以是 sRGB 或 Linear|
|DestColor    |输出图像的色彩空间。它可以是 sRGB、线性或自动|
|FileMasks    |文件后缀列表，首选选择带有这些后缀的文件|
|PixelFormat  |输出图像的像素格式。如果 OutputTypeHandling 设置为 UseInputFormat，则将忽略该格式|
|DiscardAlpha |设置为 true 可在转换过程中丢弃 Alpha 通道数据|
|MaxTextureSize|输出图像的最大纹理大小 |
|MinTextureSize|输出图像的最小纹理大小|
|SizeReduceLevel|使用指定级别 （mip） 减小纹理大小|
|GlossFromNormal|设置为 true 可从法线贴图生成玻璃数据并将其保存到 Alpha 通道。法线贴图将被规格化。|
|MipRenormalize|设置为 true 可重新规范化 mipmap|
|NumberResidentMips|定义在创建图像时有多少个低级 mipmap 是常驻 mipmap。如果设置为 0，系统将决定多少|
|Swizzle      |重排颜色通道|
|CubemapSettings|立方体贴图的设置|
|MipMapSetting|用于生成 mipmap 的设置|
|OutputTypeHandling|确定输出像素格式是使用 PixelFormat 还是使用源图像像素格式|

以下是用于处理用于天空盒的纹理的示例预设文件
```
{
    "Type": "JsonSerialization",
    "Version": 1,
    "ClassName": "MultiplatformPresetSettings",
    "ClassData": {
        "DefaultPreset": {
            "UUID": "{F359CD3B-37E6-4627-B4F6-2DFC2C0E3C1C}",
            "Name": "Skybox",
            "SourceColor": "Linear",
            "DestColor": "Linear",
            "SuppressEngineReduce": true,
            "PixelFormat": "R9G9B9E5",
            "DiscardAlpha": true,
            "MinTextureSize": 256,
            "IsPowerOf2": true,
            "CubemapSettings": {
                "RequiresConvolve": false
            }
        },
        "PlatformsPresets": {
            "android": {
                "UUID": "{F359CD3B-37E6-4627-B4F6-2DFC2C0E3C1C}",
                "Name": "Skybox",
                "SourceColor": "Linear",
                "DestColor": "Linear",
                "SuppressEngineReduce": true,
                "PixelFormat": "R9G9B9E5",
                "DiscardAlpha": true,
                "MinTextureSize": 256,
                "IsPowerOf2": true,
                "CubemapSettings": {
                    "RequiresConvolve": false
                }
            },
            "ios": {
                "UUID": "{F359CD3B-37E6-4627-B4F6-2DFC2C0E3C1C}",
                "Name": "Skybox",
                "SourceColor": "Linear",
                "DestColor": "Linear",
                "SuppressEngineReduce": true,
                "PixelFormat": "R9G9B9E5",
                "DiscardAlpha": true,
                "MinTextureSize": 256,
                "IsPowerOf2": true,
                "CubemapSettings": {
                    "RequiresConvolve": false
                }
            },
            "mac": {
                "UUID": "{F359CD3B-37E6-4627-B4F6-2DFC2C0E3C1C}",
                "Name": "Skybox",
                "SourceColor": "Linear",
                "DestColor": "Linear",
                "SuppressEngineReduce": true,
                "PixelFormat": "R9G9B9E5",
                "DiscardAlpha": true,
                "MinTextureSize": 256,
                "IsPowerOf2": true,
                "CubemapSettings": {
                    "RequiresConvolve": false
                }
            },
            "provo": {
                "UUID": "{F359CD3B-37E6-4627-B4F6-2DFC2C0E3C1C}",
                "Name": "Skybox",
                "SourceColor": "Linear",
                "DestColor": "Linear",
                "SuppressEngineReduce": true,
                "PixelFormat": "R9G9B9E5",
                "DiscardAlpha": true,
                "MinTextureSize": 256,
                "IsPowerOf2": true,
                "CubemapSettings": {
                    "RequiresConvolve": false
                }
            }
        }
    }
}

```

# 文件掩码映射
我们使用文件掩码（后缀）作为确定在处理图像文件时如何为图像文件选择正确预设的主要方法。这是为了避免用户必须为每个纹理创建设置。

如果用户不想对特定图像文件使用系统建议的预设，则可以使用 Texture Settings Editor 为纹理选择所需的预设。Per image （按图像） 设置保存在与图像位于同一文件夹中的 .assetinfo 文件中。

纹理设置编辑器可以通过编辑器->资源浏览器->（右键单击图像文件）纹理设置编辑器来访问。

# 其他信息
* AssetProcessor 窗口中图像资产的资产作业日志可能会显示图像的一些有用信息。它包括用于处理图像的预设、图像的输出分辨率、像素格式、文件大小、压缩器、压缩时间等。
* 用户还可以使用 Asset Browser 中的右键单击菜单将处理后的图像文件 （.streamingimage） 保存为 DDS 文件，部分 dds 查看者可以查看该文件。
