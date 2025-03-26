---
title: "Image Builder"
description: ""
toc: false
---

# Image Builder
The O3DE image builder is the asset builder to convert different type of images files to textures used for O3DE runtime. The builder is called by AssetProcessor. 
The image builder is located in Atom's ImageProcessingAtom gem which is under O3DE engine's \Gems\Atom\Asset\ImageProcessingAtom\ folder.

# Supported Image File Types
Here are a list of supported image file types
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

# Supported Image Pixel Format
Here are a list of supported image pixel formats:
(origin: o3de/Gems/Atom/Asset/ImageProcessingAtom/Code/Source/Processing/PixelFormatInfo.cpp)

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


# Image Builder Settings
The default settings used by the image builder is located in O3DE's \Gems\Atom\Asset\ImageProcessingAtom\Assets\Config\ folder which we call it default folder. 

There are two types of settings files: one ImageBuilder.settings file and .preset files. Both of these setting files are in json format. 

The ImageBuilder.settings defines global settings for the builder. You can find the list of available settings in BuilderSettingManager::Reflect(AZ::ReflectContext* context) function. 

A preset file defines all the conversion which could happen in an image conversion process. The available settings are listed in void PresetSettings::Reflect(AZ::ReflectContext* context) function. 

Each O3DE project can override or modify both the ImageBuilder.settings file or any preset files. It can also add new presets. These files should be placed in %project%\Config\AtomImageBuilder\ folder which we call it project folder. 

For modifying ImageBuilder.settings, user may create a json merge patch in the said folder using the same name ImageBuilder.settings. The image builder will merge this file and \Gems\Atom\Asset\ImageProcessingAtom\Assets\Config\ImageBuilder.settings to get the final settings. 
For example, the ImageBuilder.settings may look like this:

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

For preset files, currently the same preset in project folder is overriding the preset in default folder. (We may consider to switch it to use merge in the future). Users may also adding new preset files in project folder to add new customized presets. It's preferred to add file mask mapping for the new presets in ImageBuilder.settings for easy apply new presets to files. The fileMasks in each preset is still supported but it might be deprecated in the future.

# Preset settings
The settings in a preset file (*.preset) are defined in `PresetSettings `class which is located in `o3de\Gems\Atom\Asset\ImageProcessingAtom\Code\Source\BuilderSettings\PresetSettings.h` file. The names of the settings are mapped in `void PresetSettings::Reflect(AZ::ReflectContext* context)` function. Here some common used settings in a preset file. (There are few settings which are not often used are not listed here. You may find them in the header file)
|Setting name |Description|
|-------------|-------------------------|
|Name         |A unique name of the preset|
|Description  |A short description about what the preset is used for|
|GenerateIBLOnly|Controls whether this preset only invokes IBL presets and does not generate its own output product|
|SourceColor  |The color space of the source image. It can be sRGB or Linear|
|DestColor    |The color space of the output image. It can be sRGB, Linear or Auto|
|FileMasks    |A list of file suffixes which files with these suffixed are preferred to choose this preset|
|PixelFormat  |The pixel format of output image. The format will be ignored if the OutputTypeHandling is set to UseInputFormat|
|DiscardAlpha |Set to true to discard alpha channel data during converting|
|MaxTextureSize|The maximum texture size for the output image |
|MinTextureSize|The minimum texture size for the output image|
|SizeReduceLevel|Reduce the texture size with specified levels (mip)|
|GlossFromNormal|Set to true to generate glass data from the normal map and save it to alpha channel. The normal map will be normalized.|
|MipRenormalize|Set to true to re-normalize mipmaps|
|NumberResidentMips|Defines how many low level mipmaps are resident mipmaps when create the image. The system will decide how many if it's set to 0|
|Swizzle      |Swizzle color channels|
|CubemapSettings|Settings for cubemap|
|MipMapSetting|Settings for generating mipmaps|
|OutputTypeHandling|Decides if the output pixel format uses the PixelFormat or use the source image pixel format|

Here is an example preset file for processing textures used for skybox
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

# File Mask Mapping
We are using file masks (suffix) as the primary way to determinate how to choose the right preset for an image file when processing it. This is to avoid user has to create settings for each texture.

If users don't want to use the system suggested preset for specific image file, they could use Texture Settings Editor to select the preset they want for the texture. The per image setting is saved in an .assetinfo file in the same folder as the image.

The Texture Settings Editor can be accessed via Editor->Asset Browser->(right click an image file) Texture Settings Editor. 

# Other Information 
* The asset job log for image assets in AssetProcessor window may show some useful information for the image. It includes the preset used for processing the image, the image's output resolution, pixel format, file size, compressor, compression time and etc. 
* User can also use right click menu in Asset Browser to save the processed image file (.streamingimage) to a DDS file which can be viewed by some dds viewers. 
