---
linkTitle: 使用多个蒙皮附件
description: ' 在 Open 3D Engine 中为一个Actor组件使用多个皮肤附件，为Actor创建可互换的部件，如帽子和腰带。
'
title: 为Actor使用多个蒙皮附件
---

您可以在Actor或角色身上使用多个皮肤附件来添加帽子或腰带等可互换部件。这些附加部件的网格可以是单个 FBX 文件的一部分，也可以分成多个 FBX 文件。在决定使用单个 FBX 文件还是多个 FBX 文件时，请考虑以下几点：

**嵌入所有蒙皮附件（网格）的单一 FBX 文件**
+ 对添加或激活附件数量有限的Actor效果良好。
+ 使用资产处理器时会导致初始性能降低，尤其是高保真网格。但最终的性能与使用多个 FBX 文件时相同。
+ 减少了需要管理的 FBX 文件数量，但限制了修改资产的灵活性。

**多个 FBX 文件，每个文件都嵌入了可应用于Actor的皮肤附件（网格）。**
+ 在一个项目中可管理多个源文件。
+ 为多个内容创建者提供灵活性，可根据需要处理网格。
+ 无论您是否打算共享材质，资产处理器都会为每个 FBX 生成一个`.mtl`（材质）文件。

无论是使用单个 FBX 文件还是多个 FBX 文件，都要为每个蒙皮网格附件创建一个Actor。使用单个 FBX 文件的方法需要在**FBX 设置**工具中进行一些额外配置。

更多信息，请参阅 [使用 FBX 设置自定义 FBX 资产导出](/docs/user-guide/assets/scene-settings/)。

## 使用单个 FBX 文件

使用嵌入多个网格的单个 FBX 文件时，必须为每个附件创建一个Actor。例如，牛仔角色是一个Actor，而他的可互换帽子和腰带必须分别添加为其他Actor。

**在 FBX 设置工具中从单个 FBX 文件添加多个Actor**

1. 在 O3DE 编辑器的**资产浏览器**中，右键单击包含网格的`.fbx`文件，然后选择**Edit Settings**。

1. 在**Actors**项卡上，点击**Add another actor**。

   您的角色应该已经有了一个Actor。您添加的其他Actor是附件，例如衣服。

1. 在**Actor**组中，输入Actor名称，如**cowboy\_hat**，然后单击网格图标，选择要作为附件的网格。

    ![Click the mesh icon to select a mesh in FBX Settings.](/images/user-guide/visualization/animation/component-actor-single-fbx-2.png)

1. 在**Select nodes**对话框中，选择网格，然后点击**Select**。

    ![Select the appropriate nodes in FBX Settings.](/images/user-guide/visualization/animation/component-actor-single-fbx-3.png)

    你的**Actor**组看起来如下所示。
    **例如**

    ![Create multiple actor groups from a single FBX file in the FBX Settings tool.](/images/user-guide/visualization/animation/component-actor-single-fbx-1.png)

1. 点击**Update**保存你的设置，并关闭**FBX Settings** 工具。

1. 在**Asset Browser**中，导航到包含 FBX 文件的目录，查看Actor文件。您可以看到主要Actor和添加的每个附件。
**示例**

    在**Asset Browser**中，单个FBX文件包含主Actor和附件，

    ![View actor files in the Asset Browser.](/images/user-guide/visualization/animation/component-actor-component-entity-setup-1.png)

## 使用多个FBX文件

当您为每个皮肤附件使用一个 FBX 文件时，Asset Processor 会自动为每个 FBX 文件生成一个**Actor**文件。只有当你想改变默认的 Asset Processor 行为时，才可以修改 **FBX Settings**工具。

**示例**
在 **Asset Browser**中，多个FBX文件，每个都包含一个Actor文件。

![View actor files in the Asset Browser.](/images/user-guide/visualization/animation/component-actor-multiple-fbx-files.png)
