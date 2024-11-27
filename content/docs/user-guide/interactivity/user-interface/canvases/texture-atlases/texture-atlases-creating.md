---
linkTitle: 创建纹理图集
description: 创建纹理图集
title: 创建纹理图集
weight: 100
---

要创建纹理集，请创建一个纹理集 \(`.texatlas`\) 文件。纹理图集文件是一个文本文件，用于指定要添加到纹理图集的图像文件。Asset Processor 会自动构建具有`.texatlas`扩展名的文件。

## 纹理图集文件格式

`.texatlas`文件的每一行都表示一个按顺序执行的命令。空行将被忽略。

### 评论
任何以 `//`开头的行都表示注释。

**示例**

```
// This is a comment that is ignored.
```

### 属性
具有等于  \(`=`\) 符号的行被视为属性值赋值。未指定的属性使用其默认值。

下表显示了可配置属性的列表。

| 属性 | 默认值 | 目的 |
| --- | --- | --- |
| maxdimension | 4096 | 输出纹理图集的最大宽度和高度。 |
| padding | 1 | 纹理图集中每个纹理周围最小额外像素数。出于压缩目的，将复制每个纹理的边缘像素。重复量由计算 `image_size + padding` 四舍五入到最接近的压缩单位 4 来确定。 |
| poweroftwo | false |  输出纹理图集的宽度和高度是否为 2 的幂。如果 iOS 使用 [PVRTC](https://en.wikipedia.org/wiki/PVRTC 压缩，则无论此设置如何，输出纹理都是 2 的幂。 |
| square | false |  输出纹理图集的宽度和高度是否相同。如果 PVRTC 压缩用于 iOS，则无论此设置如何，输出纹理都是正方形。  |
| unusedcolor | \#3CB371FF | 输出纹理图集中未使用空间的颜色。 |
| whitetexture | true | 是否在输出纹理图集中包含路径名为 WhiteTexture 的白色纹理。 |
| presetname | TextureAtlas | 用于图像处理的预设。如果将 TextureAtlas 用作 **presetname**，无论是显式的还是默认的，图像都将被压缩。这种压缩可能会导致图像质量下降。  |

在分配属性值时，请注意以下事项：
+ 允许使用空格。
+ 属性和值不区分大小写。
+ 如果属性值分配了两次，则仅接受最后一次分配。
+ 以下条目向 Asset Processor 报告错误，并使资产处理作业失败：
  + 无法识别的属性
  + 属性的值不正确
  + 具有多个等号 （=） 的行

### 文件路径
如果有一行指定了图像文件的路径，则该图像将包含在纹理图集中。图像文件路径可以相对于 Asset Processor 监控资产的任何监视文件夹。如果一行引用无法加载的文件，则会向 Asset Processor 报告错误，并且资产处理作业将失败。既不是注释也不是属性的行被假定为图像文件路径。

**示例**

```
UI/Textures/LyShineExamples/button.tif
```

## 更新纹理图集

如果图集中的纹理发生更改或现有源`.texatlas`文件发生更改，Asset Processor 会自动重新构建纹理图集。

## 纹理图集输出文件

Asset Processor 输出两个表示纹理图集的文件：`.dds`文件和`.texatlasidx`文件。`.dds` 文件是包含`.texatlas`文件中指定的所有图像的纹理。`.texatlasidx`文件存储坐标和其他图像信息。
