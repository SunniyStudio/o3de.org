---
description: ' 在 Open 3D Engine中通过渲染输出、捕捉轨迹或控制台变量捕捉图像帧。 '
title: 采集图像帧
---

你可以使用渲染输出、捕捉轨迹或控制台变量捕捉图像帧。

## 使用渲染输出捕捉图像帧

你可以使用轨迹视图中的**Render Output**工具来捕捉图像帧。

**使用渲染输出捕捉图像帧**

1. 在 O3DE 编辑器中，选择 **Tools**, **Track View**。

1. 在 Track View 中，点击 **Tools**, **Render Output**。

1. 在**Render Output**中，设置输入和输出属性，然后单击**Add**。您将看到捕捉添加到**Batch**下。

1. 单击 **Start** 开始捕捉。

    {{< note >}}
   您可能需要调整采集图像帧的宽高比。
{{< /note >}}

**更改图像帧捕获的宽高比**

1. 在 O3DE 编辑器中，选择 **Edit**、**Editor Settings**、**Global Preferences**。

1. 在**Preferences**中，单击**Viewports**。

1. 在**General Viewport Settings**下，更改**Perspective View Aspect Ratio**的值。默认值为 `1.3333`。

## 使用捕捉轨道捕捉图像帧

在游戏模式下播放序列时，您可以捕捉图像帧。

**使用捕捉轨道捕捉图像帧**

1. 在 O3DE 编辑器中，选择 **Tools**，**Track View**。

1. 在轨迹视图中，右键单击**Director**节点，然后选择**Add Track**，**Capture**。

1. 1. 双击创建的轨道，添加捕捉关键帧。您可以设置以下关键帧属性：
****


1. 设置一个 Script Canvas 图形，在游戏开始时播放序列。

## 使用控制台变量捕捉图像帧

使用以下控制台变量捕捉图像帧。更多信息，请参阅 [使用控制台窗口](/docs/user-guide/editor/console/)。


**捕捉图像帧控制台变量**

| 控制台变量 | 说明 |
| --- | --- |
| fixed\_time\_step |  降低游戏速度，以在整个序列中达到恒定的帧速率。例如， `0.04`的时间步长值表示 25 帧/秒的游戏速度。默认值： `0.0`  |
| capture\_frames |  启用帧捕捉，如果值设置为 `1`。  |
| capture\_file\_format |  设置图像的输出格式。有效值： `.jpg`, `.tga`, `.tif`  |
| capture\_file\_prefix |  为捕获的帧设置文件名前缀。默认值： `Frame`  |
| capture\_buffer |  设置要捕获的缓冲区类型。有效值：  `0` = Color (RGB pixels) `1` = Color with Alpha (存在几何体的 RGBA 像素，其中 alpha 通道设置为 255)  |
