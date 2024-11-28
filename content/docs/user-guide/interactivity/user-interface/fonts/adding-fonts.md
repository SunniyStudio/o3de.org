---
linkTitle: 添加新字体
description: 将新的字体资源添加到您的 Open 3D Engine 游戏 UI。
title: 添加新字体
weight: 100
---

对于要添加的每种新字体，您需要以下文件：
+ 字体资产 - True Type Font \(`.ttf`\) 或 Open Type Font \(`.otf`\) 文件
+ 字体 `.font` 文件 描述资产 {#create-font-xml-file}

**向 UI 添加新字体**

1. 保存字体资产 \(`.ttf` 或 `.otf` file\) 添加到您的游戏项目目录中，例如 `dev\SamplesProject\Font`.

1. 复制现有的资产 `.font` 文件到游戏项目 `Font` 目录。以下目录（包含在 O3DE 项目中）包含字体 `.font` 文件以供参考：
   + `dev\Engine\Fonts\`
   + `dev\SamplesProject\Fonts\`

1. 更改 `.font` 文件名 \(保留 `.font` 扩展名\)。使用任何描述性且适合您用途的文件名。

1. 打开 `.font` 文件并编辑以下行以指向您的字体资源文件名。

   ```
   <font path="yourFont.ttf" w="512" h="256"/>
   ```

在 Asset Processor 处理完您的字体资源后，您可以通过 [加载字体 '.font' 文件](../components/visual/components-text)的 UI 编辑器。
