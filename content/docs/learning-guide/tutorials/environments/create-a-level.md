---
linkTitle: 创建关卡
title: 在 Open 3D Engine 中创建关卡
description: 了解如何在 Open 3D Engine （O3DE） 中创建关卡。
weight: 200
toc: true
---

在 **Open 3D Engine （O3DE）** 中，*关卡* 是项目的可玩部分。在关卡中，您可以创建实体、实例化Prefab、排列光照和摄像机，以及实现脚本化行为和交互性。关卡可以解释为游戏的关卡;但是，关卡是所有 O3DE 项目的基础。非游戏应用程序（如模拟）至少需要一个级别。一个项目可以具有单个级别或多个级别。



在 O3DE 中，关卡存储在项目的 `Levels` 目录的子目录中的`.prefab` 文件中。关卡`.prefab`文件为 JSON 格式。它包含放置在关卡中的实体列表，包括定义实体的组件、值和资产引用。所有资源（包括网格、脚本、材质、音频文件和组成关卡的其他Prefab）都在关卡`.prefab`文件中引用。


{{< note >}}
默认情况下，所有关卡都必须放置在项目的 `Levels` 子目录中。
{{< /note >}}

## 创建关卡

您可以在启动 **O3DE 编辑器** 时出现的 **欢迎使用 O3DE** 对话框中创建关卡，也可以从 O3DE 编辑器中的 **文件** 菜单创建关卡。


1. 在 Welcome to O3DE 对话框中，选择 **Create new...** 按钮以打开 **New Level ** 对话框。或者，从 O3DE 编辑器的 **文件** 菜单中，选择 **New Level**（热键 **Ctrl + N**）以打开**New Level**对话框。


    ![Welcome to O3DE dialog](/images/learning-guide/tutorials/environments/create-a-level-A.png)

1. 在 **New Level** 对话框中，输入级别的名称。


    ![New Level dialog](/images/learning-guide/tutorials/environments/create-a-level-B.png)

1. 选择 **OK** 创建关卡。


## Atom Default Environment

新关卡中填充了一些基本实体。在 **Entity Outliner** 中，有一个名为 **Atom Default Environment** 的根实体。在 **Entity Outliner** 中选择默认实体左侧的箭头以展开子实体列表。


{{< note >}}
Atom 默认环境是一个 *prefab*。Prefab是预配置实体的集合，作为可重用的`.prefab`资产文件存储在磁盘上。Prefab可以在关卡中实例化。您可以通过将自己的Prefab保存到 `/o3de/Assets/Editor/Prefabs/Default_Level.prefab`来修改默认关卡环境的内容。
{{< /note >}}

![Default level prefab](/images/learning-guide/tutorials/environments/create-a-level-C.png)

| 实体名称 | 说明 |
| - | - |
| Atom Default Environment | 这是根实体。它包含一个 **Transform** 组件，并且是默认环境实体的父级。 |
| Global Sky | 包含一个 **Global Skylight (IBL)** 组件和一个 **HDRI Skybox** 组件。此实体使用高动态范围图像提供基于图像的照明，并将图像显示为天空盒。 |
| Ground | 包含一个 **Mesh** 组件和一个 **Material** 组件，用于显示带有棋盘格材质的简单地平面。 |
| Grid | 包含一个与 Ground 实体对齐的 **Grid** 组件，该组件可用作放置和对齐实体和Prefab的构造平面。 |
| Shader Ball | 包含一个 **Mesh** 组件和一个 `shaderball_default_1m` 网格资源。此网格资源为开发材质提供了良好的基础。 |
| Sun | 包含一个 **Directional Light** 组件。平行光将光线均匀地投射到单个方向上，并模拟远处的光源。 |
| Camera | 包含一个 **Camera** 组件，该组件提供用于查看关卡的摄像机视图视锥体，以及一个 **Fly Camera Input** 组件，该组件接受用户输入并在 **Game** 模式下移动摄像机。 |
