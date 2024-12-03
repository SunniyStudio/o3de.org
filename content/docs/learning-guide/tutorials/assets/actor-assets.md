---
linkTitle: Actor Processing
title: Customize Actor Asset Processing
description: Learn to customize how actor assets are processed for Open 3D Engine (O3DE) with Scene Settings.
weight: 300
toc: true
---

本教程演示如何使用 [**Scene Settings**](/docs/user-guide/assets/scene-settings/scene-settings) 来自定义 **Open 3D Engine （O3DE）** 的角色资产处理。角色包含一个或多个绑定到骨架的网格。在本教程中，您可以使用任何角色资源。O3DE 支持的主要 3D 场景格式是`.fbx`，因此建议使用保存到`.fbx`文件的演员资产。 如果您没有自己的角色源资源，则可以使用 [Mixamo](https://www.mixamo.com/#/?page=1&type=Character).

{{< note >}}
了解用于创建角色源资源的 [最佳实践](/docs/user-guide/assets/scene-settings/source-asset-best-practices#actors)可以缓解您在处理 O3DE 角色时可能遇到的问题。有关 Actor 支持的数据的技术细节，请参考 [支持的 3D 场景数据](/docs/user-guide/assets/scene-settings/scene-format-support#supported-3d-scene-data) 表。
{{< /note >}}

| O3DE 体验 |完成时间 |功能聚焦 |最后更新|
| - | - | - | - |
| 新手 | 10 分钟 | 使用 Scene Settings 对 `.fbx` 文件中的 actor 资源进行自定义处理。 | January 4, 2023 |

## 制备

将角色`.fbx`源资源放在项目的 [扫描目录](/docs/user-guide/assets/pipeline/scan-directories)中，例如`Assets`子目录，以便[**Asset Processor**](/docs/user-guide/assets/asset-processor)检测到它。

## 处理角色资产

当您将角色`.fbx`源资源放在项目的`Assets`目录中时，Asset Processor 会检测到该资源，检查其内容，并使用一组默认规则对其进行处理。这些默认规则可能足以处理简单的角色资源，但是，角色通常需要对自定义法线和切线、布料网格、根运动提取或坐标空间更改进行额外处理。要自定义处理角色资产的规则，请执行以下操作：

1. 在 **O3DE 编辑器 **中，在 **Asset Browser** 中找到您的资源。您可以在 Asset Browser （资源浏览器） 顶部的搜索字段中键入资源的名称，以筛选列表并找到您的角色资源。

    ![ Search for a specific mesh asset in Asset Browser. ](/images/learning-guide/tutorials/assets/actor-search-asset-browser.png)

    如果您的资产已处理完毕，您可能会看到该资产的预览图像，以及“.fbx”源资产下方的产品资产列表。

1. **双击**  `.fbx` 源资源，然后从上下文菜单中选择 **Edit Settings...** 以打开场景设置。

    ![ Open Scene Settings from Asset Browser. ](/images/learning-guide/tutorials/assets/actor-edit-settings.png)

1. 选择 [**Meshes**](/docs/user-guide/assets/scene-settings/meshes-tab) 标签页。

    ![ Scene Settings meshes tab. ](/images/learning-guide/tutorials/assets/actor-mesh-scene-settings.png)

    在上图中，**Meshes** 选项卡中有一个 **Mesh group**。网格组生成运行时优化的网格资产。默认情况下，源资产中的所有网格都被选中用于网格组，并作为单个网格进行处理。

    可以通过选择 **Add another mesh** （添加其他网格） 来创建其他网格组。但是，一个角色实体只能有一个网格组件。角色的所有网格 - 也就是说，绑定到角色骨架的所有网格 - 必须包含在单个网格组中。此示例中的角色有一个网格，但角色通常具有许多唯一的网格。

    下表说明了角色的默认网格组的重要元素：

    | 元素 | 说明 |
    | --- | --- |
    | **Name mesh** | 此属性是网格组的名称。此网格组的所有产品资产都使用此字符串作为其名称的前缀。产品资源显示在 资源浏览器 中源资源的下方。默认情况下，使用源场景文件的名称。 |
    | **Select meshes** | 选择作为网格组的一部分进行处理的网格。选择 {{< icon browse-edit-select-files.svg >}} **node select** 按钮允许您从`.fbx`源资源中选择哪些网格以包含在网格组中。默认情况下，`.fbx`源资产中的所有网格都处于选中状态。 |
    | **Skin** | [Skin](/docs/user-guide/assets/scene-settings/meshes-tab/#skin) 修改器会自动添加到绑定到骨架的网格。Skin （蒙皮） 修改器设置网格的每个顶点的最大权重，以及骨骼影响的权重阈值。 **Max weights per vertex** property 的默认值为 '`8`'，因此最多 8 个骨骼可以影响网格中的每个顶点。**Weight threshold** 的默认值是 `0.001`. 任何低于阈值的顶点权重都将被忽略。 |
    | **Material** | [Material](/docs/user-guide/assets/scene-settings/meshes-tab/#material) 修改器会自动添加到网格组中。使用 Material （材质） 修改器，您可以选择更新已处理角色的材质，并删除之前可能已为角色处理过的未使用的材质。 |

    {{< note >}}
如果您的角色具有使用法线贴图的自定义法线和材质，则可能需要添加[**Mesh (Advanced)**](/docs/user-guide/assets/scene-settings/meshes-tab/#mesh-advanced) 和 [**Tangents**](/docs/user-guide/assets/scene-settings/meshes-tab/#tangents)  修饰符。要了解有关网格修改器的更多信息，请参阅用户指南中的 [Meshes Tab](/docs/user-guide/assets/scene-settings/meshes-tab) 主题。
    {{< /note >}}

1. 点击 {{< icon browse-edit-select-files.svg >}} 文件选择按钮查看和选择要包含在角色的网格组中的网格。

    ![ Select meshes for actor. ](/images/learning-guide/tutorials/assets/select-actor-mesh.png)

1. 在 **Select nodes** 对话框中，仅选择角色所需的网格节点。Mesh 节点由网格图标表示。选择 **Select** 以完成选择。

1. 选择 [**Actors**](/docs/user-guide/assets/scene-settings/actors-tab) 选项卡以指定 Actor 骨架的根关节。

    ![ Select the Actors tab. ](/images/learning-guide/tutorials/assets/actors-tab.png)

    在上图中，有一个 **Actor group**。

    **Name actor** 属性是 actor 组的名称。此参与者组的所有产品资产都使用此字符串作为其名称的前缀。此角色组的产品资源显示在 Asset Browser 中源资源的下方。默认情况下，使用源场景文件的名称。

    **Select root bone** 属性指定要为 actor 处理的骨架的根骨骼。

1. 单击 **Select root bone** 属性以显示骨骼列表。从列表中选择骨架的根骨骼。

    ![ Specify the root bone for the actor. ](/images/learning-guide/tutorials/assets/select-actor-root.png)

    {{< note >}}
列表中的第一个节点是场景根节点，而不是骨架的根。确保您选择的骨骼是角色骨架的根骨骼。骨架根骨骼对于动画和根运动至关重要。有关更多信息，请参阅 [数据驱动的根运动](/docs/learning-guide/tutorials/animation/data-driven-root-motion) 教程。
    {{< /note >}}

1. 选择 **Update** 以保存角色的自定义设置。资产会立即使用新设置重新处理。

1. 在 资源浏览器 中找到角色产品资源，并将该角色拖动到视区中，为该角色创建实体。

    ![ Drag the actor from Asset Browser to create an actor entity. ](/images/learning-guide/tutorials/assets/actor-entity-instance.png)

    在 **Entity Inspector** 中，请注意，该实体包含一个 **Actor** 组件，该组件引用您拖动到视区中的角色产品资产。

    ![ Actor entity in Entity Inspector. ](/images/learning-guide/tutorials/assets/actor-entity-components.png)
