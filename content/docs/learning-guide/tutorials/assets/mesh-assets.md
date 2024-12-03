---
linkTitle: 网格处理
title: 自定义网格资产处理
description: 了解如何使用 Scene Settings 自定义 Open 3D Engine （O3DE） 的网格资产处理方式。
weight: 200
toc: true
---

本教程演示如何使用 [**Scene Settings**](/docs/user-guide/assets/scene-settings/scene-settings) 来自定义 **Open 3D Engine （O3DE）** 的网格资产处理。在本教程中，您可以使用任何网格。O3DE 支持的主要 3D 场景格式是“.fbx”，因此在本教程中，建议使用保存为`.fbx`文件的简单网格资源。如果您没有自己的源资产，则可以使用 O3DE 提供的资产之一。

{{< note >}}
了解用于创建网格源资产的 [最佳实践](/docs/user-guide/assets/scene-settings/source-asset-best-practices#meshes) 可以缓解您在处理 O3DE 网格时可能遇到的问题。有关网格支持的数据的技术细节，请参阅 [支持的 3D 场景数据](/docs/user-guide/assets/scene-settings/scene-format-support#supported-3d-scene-data) 表。

{{< /note >}}

| O3DE 体验 |完成时间 |功能聚焦 |最后更新 |
| - | - | - | - |
| 初级 |10 分钟 |使用 Scene Settings 对 `.fbx` 文件中的网格资产进行自定义处理。 | January 4, 2023 |

## 准备

将网格`.fbx`源资源放在项目的 [扫描目录](/docs/user-guide/assets/pipeline/scan-directories)中，例如 `Assets` 子目录，以便[**Asset Processor**](/docs/user-guide/assets/asset-processor)检测到它。

## 处理网格资产

当您将网格`.fbx`源资源放在项目的`Assets`目录中时，Asset Processor 会检测到该资源，检查其内容，并使用一组默认规则对其进行处理。要自定义处理网格资产的规则，请执行以下操作：

1. 在 **O3DE 编辑器** 中，在 **Asset Browser** 中找到您的资源。如果您没有自己的资产，则可以在资产浏览器顶部的搜索字段中键入`.fbx`，然后使用 O3DE 附带的`.fbx`文件之一。

    ![ Search for a specific mesh asset in Asset Browser. ](/images/learning-guide/tutorials/assets/meshes-search-asset-browser.png)

    如果您的资产已处理完毕，您可能会看到该资产的预览图像，以及`.fbx`源资产下方的产品资产列表。

1. **双击** `.fbx`源资源，然后选择 **Edit Settings...** 以打开 Scene Settings （场景设置）。

    ![ Open Scene Settings from Asset Browser. ](/images/learning-guide/tutorials/assets/meshes-edit-settings.png)

1. 选择 [**Meshes**](/docs/user-guide/assets/scene-settings/meshes-tab) 选项卡。

    ![ Scene Settings meshes tab. ](/images/learning-guide/tutorials/assets/meshes-scene-settings.png)

    在上图中，有一个 **Mesh group**。默认情况下，源资产中的所有网格都作为单个网格组处理。每个网格组都会生成一组产品资产。您可以通过选择 **Add another mesh** （添加其他网格） 来创建其他网格组。

    **Name mesh** 属性包含源资源的名称。此网格组的所有产品资产都使用此字符串作为其名称的前缀。当有多个网格组时，产品资产将显示在源资产下方的 Asset Browser 中，并按网格组名称进行组织。

    **Select meshes** （选择网格） 属性显示为 **All meshes selected（所有网格选定）**。您可以选择 {{< icon browse-edit-select-files.svg >}} **节点选择** 按钮选择要包含在网格组中的网格。

1. 添加修饰符以自定义资源的处理方式。选择 **Add Modifier** 按钮查看网格修改器列表，然后选择[**Coordinate system change**](/docs/user-guide/assets/scene-settings/meshes-tab#coordinate-system-change).

    ![ Scene Settings meshes tab, adding mesh a modifier. ](/images/learning-guide/tutorials/assets/meshes-coordinate-system-change.png)

1. 坐标系更改网格修饰符用于缩放或变换资产，适用于资产在 O3DE 中可能太小、太大或方向不正确的情况。默认情况下，修改器提供将网格旋转 180 度的单个选项。激活 **Use advanced settings** 切换以显示高级修饰符设置。

    ![ Scene Settings meshes Coordinate system change modifier, Use advanced settings. ](/images/learning-guide/tutorials/assets/meshes-use-advanced-settings.png)

1. 自定义资产的比例。将 **Scale** 属性设置为 `5.0` 以将资源缩放到其大小的五倍。

1. 选择 Scene Settings （场景设置） 右下角的 **Update** （更新） 按钮。这将创建或更新 `.assetinfo` sidecar 文件，并触发 Asset Processor 重新处理资产。

    当 Asset Processor 检测到`.assetinfo`文件时，它会使用文件中的设置来处理相关的源资源。此 sidecar 文件被视为资产的源依赖项。这意味着，如果 `.assetinfo`文件发生更改，即使源资产未更改，也会重新处理源资产。

1. 将`.azmodel`产品资源从 Asset Browser 拖动到视区中。

    {{< image-width "/images/learning-guide/tutorials/assets/meshes-finished.png" "800" "Drag the mesh product asset into the viewport">}}

    当您将资产拖动到视区中时，O3DE 会自动创建一个实体，该实体具有引用网格产品资产的 **Mesh** 组件。如果源资源包含已处理的材质，则这些材质会自动应用于网格。
