---
title: 材质编辑器
description: 学习如何使用材质编辑器在 Atom 中创建材质。
toc: true
---

**材质编辑器**是一个独立的应用程序，允许艺术家查看、编辑和创建新材质。本节将介绍材质编辑器，并介绍编辑器布局及其各个面板。

有关 Atom 工具的常用功能、菜单、可停靠面板和文档处理的更多信息，请参阅 [Atom 工具常用功能](/docs/atom-guide/look-dev/tools/atom-tools-common-features/)。

## 快速入门
要开始使用，请启动 “材质编辑器 ”并创建或编辑一个材质。

### 启动材质编辑器
启动材质编辑器有多种方法。
- 从 **Open 3D Engine (O3DE)** 编辑器选择 **主菜单 > Tools > Material Editor**
- 从 Material Component 上下文菜单，选择 **Open Material Editor...**
- 从 Asset Browser，双击一个`.material` 或 `.materialtype` 文件
- 从 Asset Browser，右击一个`.material` 或 `.materialtype`，并选择 **Open in Material Editor...** 
- Material Editor 还是一个独立的可执行文件，可直接从文件浏览器或命令控制台启动。
  - 启动可执行程序 `<build>\bin\profile\MaterialEditor.exe`
  - 这需要在命令行参数中传递 -project-path（项目路径），然后是项目路径。

### 创建材质
开始编辑材质时，您有多种选择。
- 根据材料类型创建新材料。
  1. 选择 **Main Menu > File > New...**。这将打开 **Create Material Document** 对话框。
  2. 选择一种材料类型作为新材料的基础。
  3. 为新材料选择路径和文件名。
- 打开现有材料，
  1. 选择 **Main Menu > File > Open...**.
- 从 “资产浏览器 ”打开或创建材料。

### 编辑材质
要编辑材质，请在材质编辑器检查器中更改材质的属性。系统会记录您所做的任何更改，因此您可以随时撤销或重做更改。

###子材料
材料可以引用父材料来继承和覆盖其属性。除非子材质覆盖了父材质或另一个祖先材质的属性，否则对父材质或另一个祖先材质的更改会自动反映到子材质中。

要创建子材质，请打开现有材质并选择 **主菜单 > File > Save As Child...**。 

### 将更改镜像到 O3DE 编辑器
- 在 “材质编辑器 ”中保存的材质将由 “资产处理器 ”处理。
- 如果 O3DE 编辑器已打开，且保存的材质已分配给材质组件或地形，则它不会自动重新加载以反映更改。
- 在 “材质编辑器 ”设置对话框中启用自动保存功能将使 O3DE 编辑器中不断映射更新。

## 浏览材质编辑器
启动 “材质编辑器 ”后，您将看到以下主窗口。

![Material Editor](/images/atom-guide/tools/material-editor.jpg)

有关 Atom 工具常用功能、菜单、可停靠面板和处理文档的更多信息，请参阅 [Atom 工具常用功能](/docs/atom-guide/look-dev/tools/atom-tools-common-features/)。 

### 文档类型和视图
材料编辑器支持材料文档， `.material` 和 `.materialtype`。 

#### 材料文件
您可以创建、打开和编辑 `.material files`。 您也可以打开`.materialtype` 文件查看其属性，但不能在 “材质编辑器 ”中编辑它们。

当前正在编辑的材质会显示在视口中的模型上。当您通过检查器或脚本编辑材质时，视口中显示的材质会自动更新以反映更改。

### 主菜单
主菜单包含所有 Atom 工具通用的子菜单和操作。

有关 Atom 工具常用功能、菜单、可停靠面板和处理文档的更多信息，请参阅 [Atom 工具常用功能](/docs/atom-guide/look-dev/tools/atom-tools-common-features/)。

### 视口
使用**视口**面板预览在不同光照条件下应用于模型的材质。

{{< note >}}
视口不能完全关闭。如果关闭了所有选项卡，视口将显示一个包含空白模型、地面和自然光的默认场景。
{{< /note >}}

For more information about the viewport and how to interact with it, see [Atom Tools Viewport](/docs/atom-guide/look-dev/tools/atom-tools-viewport/).  

### Viewport Settings
视口设置 面板显示用于配置视口属性的选项。这些属性可控制视口的显示方式以及哪些功能处于活动状态。您还可以创建或编辑视口模型和照明环境的配置预置。

有关视口设置的更多信息，请参阅 [Atom 工具视口](/docs/atom-guide/look-dev/tools/atom-tools-viewport/)。

## 故障排除
### 材料编辑器无法启动
材质编辑器会初始化游戏项目启用的所有 O3DE Gems，以访问相同的渲染功能和资产。为减少启动时间和系统资源利用率，Material Editor 和其他 Atom 工具包含注册表设置文件，可强制禁用工具内不需要的多个标准 O3DE Gems。

如果 “材质编辑器 ”无法启动，那么可能是因为活动项目中宝石的依赖性问题。检查 `MaterialEditor.log` 是否有任何系统实体或模块初始化错误。如有必要，更改或删除 Material Editor 项目注册表文件夹中的自定义注册表设置。
