---
linkTitle: 编辑器导览
title: O3DE 编辑器简介
description: O3DE 编辑器概述，Open 3D Engine 的主要创建工具，以及导航的快速介绍。
weight: 300
toc: true
---

**O3DE 编辑器** 是您的主要工作区。在这里，您可以访问所有工具来设计、创建、测试和部署您的项目。如果你用过其他专业的引擎或 3D 动画包，你会发现用户体验很熟悉，可以快速适应 O3DE 编辑器。

您可以从下面的快速视频教程中学习导航和自定义 O3DE Editor 的基础知识，并通过阅读本主题了解一些其他提示。

{{< youtube-width id="tFfiXhckd7g" title="O3DE Editor Tour" >}}

## 启动 O3DE 编辑器

O3DE 编辑器可以通过在安装过程中打开放置在桌面上的快捷方式来启动，也可以通过从 O3DE 构建或安装目录启动`Editor.exe` 应用程序来启动(例如: `o3de/<version>/bin/Windows/profile/Default`)。**项目管理器**迎接您，它允许您选择或创建项目。当您选择项目时，O3DE Editor 将启动，您可以选择创建新关卡或加载现有关卡。关卡不仅仅是环境。关卡是复杂的资源，其中包含许多文件，这些文件代表项目的某个部分。通常，关卡存储在工程的 `Levels` 目录的命名子目录中。

## O3DE 编辑器默认布局

O3DE Editor 的默认布局包含配置中最常用的工具，类似于其他内容创建应用程序。O3DE 的核心工作流程是在关卡中创建和放置实体，因此默认布局包含菜单栏、工具栏、窗格和工具选项卡，这些选项卡专注于实体的创建和放置。

您可以通过拖放来自定义布局，并通过主菜单栏的 **View** 菜单中的 **Layouts** 选项保存到自定义布局。在窗格之间拖动分隔条以调整窗格的大小。拖动窗格的标题栏以撕下窗格。该窗格可以拖放到布局中的任何位置，也可以作为自己的窗口放在 O3DE 编辑器之外。要恢复默认布局，请在主菜单栏中选择 **View > Layouts > Default Layout**。

![The default O3DE Editor layout.](/images/welcome-guide/ui-editor-labeled.png)

* **A:** O3DE 编辑器顶部附近是 **菜单栏** 和 **工具栏**。

   菜单栏包含几个熟悉的菜单：

   * **File** - File （文件） 菜单项包括用于打开和保存关卡、管理编辑器和项目设置以及创建和打开项目的操作。

   * **Edit** - “编辑”菜单项包括用于处理选区 （如复制、删除、隐藏和显示选区） 以及使用选区变换的操作。

   * **Game** - Game 菜单项包括用于运行项目、启用编辑器内模拟、启用和刷新音频以及调试的操作。

   * **Tools** - Tools 菜单项提供对所有 O3DE 工具和编辑器的访问。

   * **View** - View （视图） 菜单项包括用于配置 Perspective （透视图） 视区和 O3DE Editor 布局的操作。

   * **AWS** - AWS 菜单项包括用于在 O3DE 项目中使用 AWS 的工具和文档链接。

   * **Help** - 帮助菜单项提供指向 O3DE 社区和文档资源的链接。

   工具栏提供对各种编辑器工具和功能的轻松访问。左侧是用于打开各种 O3DE 工具和编辑器的按钮，右侧是用于运行项目或激活编辑器内模拟的控件。默认情况下，Tool Bar 停靠在编辑器的顶部，但您也可以将其垂直停靠在编辑器的边缘。要自定义工具栏，请右键单击工具栏上的任意位置，然后从上下文菜单中选择 **Customize**。您可以选择要包含的工具栏，并向工具栏添加命令。

* **B:** 在 O3DE 编辑器的左侧，**Entity Outliner** 显示当前关卡中的实体和Prefab列表。在 Entity Outliner 中右键单击以打开上下文菜单以创建实体和实例化Prefab。在 Entity Outliner 中选择实体或Prefab时，上下文菜单还包含用于复制或删除实体、查找所选实体和Prefab、组织列表以及打开所选实体或Prefab的属性的选项。

* **C:** Entity Outliner 下面是 **Asset Browser**，您可以使用它来浏览项目的磁盘资源。网格、动画和纹理等资产是在第三方应用程序中创建的。材质、脚本和Prefab等资源是在 O3DE Editor 中创建的，或者在 **Script Canvas** 等编辑器工具中创建的。您创建的资源存储在您的项目目录中。您还可以浏览 O3DE 附带的默认资源，以及已添加到项目中的 Gem 附带的资源。

   资源浏览器的左窗格显示一个目录结构，您可以浏览该目录结构以查找可用资源。选择资产后，右侧的预览窗格将显示缩略图预览和有关该资产的信息（如果可用）。

   在 资源浏览器中选择资源后，右键单击上下文菜单包含用于打开 **Scene Settings** 的选项，您可以在其中为资源设置 **Asset Processor** 选项，以及在关联的应用程序（如建模程序）中打开资源，或在系统文件浏览器中打开文件位置。

* **D:** 默认 O3DE 编辑器布局的中心是 **Perspective**。此 3D 视窗是关卡的实时视图。在 Perspective 中，您可以创建和放置实体，以及查看和播放您的项目。

   右键单击 Perspective 的标题栏以打开 perspective 菜单。从 perspective （透视） 菜单中，您可以切换各种辅助对象 （如构造平面、图标、边界和参考线） 的可见性。您还可以选择纵横比，通过关卡中放置的各种摄像机进行查看，从当前视图创建新摄像机，以及将 Perspective 拆分为多个视图。

   Perspective （透视） 标题栏的右侧有几个图标，用于选择摄像机、设置摄像机移动速度、设置信息显示、启用视图图标、设置纵横比和设置网格对齐选项。

   在 Perspective 的视区中右键单击以打开上下文菜单以创建实体和Prefab。Perspective 中的许多上下文菜单功能与 Entity Outliner 的上下文菜单功能共享。

   Perspective 的左上角和右上角是用于操作实体的图标托盘。在左侧 （**D1**），您可以使用图标选择转换操作。从上到下，图标代表 {{< icon "move.svg" >}} 平移, {{< icon "rotate.svg" >}} 旋转, 和 {{< icon "scale.svg" >}} 缩放操作。在右侧 （**D2**），您可以使用图标为转换操作选择空间。从上到下，图标代表 {{< icon "world.svg" >}} 世界, {{< icon "parent.svg" >}} 父级, 和 {{< icon "local.svg" >}} 本地空间。

* **E:**  在 O3DE 编辑器的右侧，**Entity Inspector** 显示当前所选实体的组件。Entity Inspector 的顶部是实体 **Name** 的字段和一个 **Add Component**按钮。Add Component （添加组件） 按钮将打开一个可用组件列表，这些组件按类型排序，可以添加到实体中。每个组件都有自己的一组属性，这些属性显示在 Entity Inspector 中。所有实体都包含一个 transform 组件，用于设置实体在关卡中的位置、旋转和缩放。

   {{< note >}}
   组件由 Gem 提供。如果您在 available components 列表中找不到特定组件，请确保您在项目中启用了提供该组件的 Gem。
   {{< /note >}}

* **F:**  默认 O3DE 编辑器布局的底部是 **编辑器控制台**，它显示 O3DE 编辑器和您的项目的命令和进程输出。例如，在加载关卡时，控制台会在加载资产和配置文件时显示有关它们的消息，如果遇到问题，则可能会显示警告和错误。

   您可以在控制台底部的输入字段中输入控制台命令，例如设置控制台变量。选择 Editor Console 左下角的 **{x}** 按钮以打开 **Console Variables Editor**，它提供了一个用于设置控制台变量的简单界面。

   O3DE 编辑器的最底部是状态栏，其中显示有关编辑器进程、Asset Processor 作业和版本控制信息的信息。

## Navigating the O3DE Perspective viewport

O3DE 具有基于第一人称 PC 游戏和流行建模应用程序的熟悉的视互模型，并进行了一些细微的调整和添加。移动由键盘输入处理，视图由指针设备输入处理。

![WSAD and Mouse graphic.](/images/welcome-guide/wg-WASD.png)

* **W** - 向前移动。
* **S** - 向后移动。
* **A** - 向左移动。
* **D** - 向右移动。
* **Q** - 向下移动。
* **E** - 向上移动。
* **Z** - 聚焦在选定对象上。
* **RMB+拖动** - 旋转视图，在大多数游戏中称为 *mouselook*。
* **MouseWheel Up/Down** - 缩放视图。
* **MMB+拖动** - 平移视图。
* **LMB** - 选择实体。
* **LMB+Drag** - 区域选择实体。

上面的相机控件以游戏为中心。如果您更喜欢使用更接近 DCC 应用程序（如 Maya）中的摄像机控件，请使用这些热键。

* **Alt+LMB+Drag** - 动态观察视图。
* **Alt+RMB+拖动** - 推拉视图。
* **Alt+MMB+拖动** - 轨迹视图。


## 移动首选项

您可能希望反转编辑器摄像机控件的任一轴，或者您可能希望加快或减慢编辑器摄像机的默认移动或旋转。您可以通过在 **Global Preferences** 编辑器中设置 **Camera Movement Settings** 属性来调整默认编辑器摄像机控制行为。

![O3DE movement preferences.](/images/welcome-guide/ui-preferences-movement.png)

在 **Edit** 菜单中，从 **Editor Settings** 组中，选择 **Global Preferences**。从左侧列表中选择 **Camera**。在这里，您可以反转鼠标的任一轴并调整编辑器摄像机的移动速度。

当您根据自己的喜好设置移动首选项时，您可能会发现在某些情况下，编辑器摄像机的移动有时太快或太慢。您可以通过选择 **Perspective** 窗格顶部的 **Perspective Toolbar** 中的 **Camera** 图标来调整移动速度。

![O3DE Perspective movement speed.](/images/welcome-guide/ui-camera-speed.png)

在 **Speed** 属性中输入浮点值以设置移动速度。您还可以单击 **Speed** 属性右侧的箭头，将移动速度设置为 **0.1**、**1.0** 或 **10.0**。

## 保存 Perspective 位置<a name="save-perspective-locations"></a>

在构建关卡时，您可能会发现保存预设的 **Perspective** 视图以供以后使用非常有用。您可以保存当前编辑器摄像机视图，并将其分配给 **[Function]** 键：

* 要保存 Perspective *location*，请按 **[Control + Function(1-12)]**.
* 要将 Perspective *视图* 设置为保存的位置，请按 **[Shift + Function(1-12)]**.
