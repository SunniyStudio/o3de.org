---
linkTitle: 风力
title: 创建风力
description: 在 Open 3D 引擎 （O3DE） 中使用 PhysX 创建全局或局部风力。
weight: 200
toc: true
---

使用 **PhysX Force Region** （PhysX 力区域） 组件，您可以创建碰撞器体积中包含的全局风力或局部风力。风力作用于具有受风影响的组件的实体，例如 [Cloth 组件](/docs/user-guide/components/reference/physx/cloth/)。在本教程中，请确保您在项目中启用了 **NVIDIA Cloth** Gem，以便可以轻松测试 wind provider 实体的结果。

{{< note >}}
风力仅影响支撑它的组件，例如 **Cloth** 组件。风力不会影响 **PhysX Dynamic Rigid Body** （PhysX 动态刚体） 组件。
{{< /note >}}

## 创建一个风供应商实体

在本节中，您将为 wind 提供商设置一个实体。

1. 为 wind 提供商创建一个实体。

1. 向实体添加 **Tag** 组件。Tag （标记） 组件指定风力是全局的还是局部的。

1. 添加 tag 值以指定风的类型。要确定要用于 Tag （标记） 组件的值，请从 **Tools** 菜单中选择 **PhysX Configuration**。在 **Global Configuration** 选项卡的底部，有一个标有 **Wind Configuration** 的部分。

    ![PhysX Wind Configuration tags.](/images/learning-guide/tutorials/physx/wind-configuration.png)

    **Wind Configuration** 包括 **Global wind tag** 和 **Local wind tag** 属性。您可以使用默认值，也可以根据需要进行设置。PhysX 系统使用这些标记来识别提供风力的实体。

    对于此示例，请使用 **Global wind tag** 属性值。在实体的 Tag （标记） 组件中，选择 {{< icon "add.svg" >}} **Add** 按钮以添加标记元素。为其指定值`global_wind`。

    ![PhysX Wind Configuration tag component setup.](/images/learning-guide/tutorials/physx/wind-tag-setup.png)

    {{< note >}}
    如果您选择使用 **Local wind tag** 属性，则风力仅影响您在下一步中添加的 **PhysX Primitive Collider** （PhysX 基元碰撞器） 组件体积内的实体。
    {{< /note >}}

## 使用 PhysX Primitive Collider （PhysX 基元碰撞器） 定义力区域

在本节中，您将向风提供程序实体添加一个盒形 PhysX Primitive Collider （PhysX 基元碰撞器） 组件，并将其放置在关卡中。

1. 将 PhysX Primitive Collider （PhysX 基元碰撞器） 组件添加到风提供程序实体。如果您使用的是 **Local wind tag** 属性，则此碰撞器将定义包含风力的体积。使用 **Global wind tag** 属性时，此碰撞器的大小和位置并不重要，因为风力是全局的。但是，碰撞器为全局风力提供了有用的可视化效果。

1. 将 PhysX Primitive Collider （PhysX 基元碰撞器） 组件的 **Shape** 属性设置为`Box`。

1. 缩放 PhysX Primitive Collider （PhysX 基元碰撞器） 组件并定位实体。根据需要在 PhysX Primitive Collider （PhysX 基元碰撞器） 组件中设置 **Box Dimensions** （盒体尺寸） 属性。如果要创建局部风力，请将碰撞器尺寸扩大到足够大的大小，以包含接收风力的实体。使用 **Move** 工具将实体放置在关卡中。在以下示例中，碰撞器在每个维度上为 5 米，并且风提供程序实体的位置使框的底部位于地平面上。

    {{< image-width src="/images/learning-guide/tutorials/physx/wind-collider-setup.png" width="900" alt="Setting up a PhysX Primitive collider for a force region" >}}

## 创建一个 PhysX 力区域

在本节中，您将设置一个生成风力的 PhysX Force Region （PhysX 力区域） 组件。

1. 将 PhysX Force Region （PhysX 力区域） 组件添加到实体。此零部件产生风力。

1. 在 **Forces**旁边，选择 {{< icon "add.svg" >}} **Add**（添加）按钮以添加新的力。

1. 在新力的 **Direction** 属性中，将 **Y** 分量设置为`-1.0`，并将 **Z** 分量设置为`0.0`，以创建风力的方向。

1. 在 **Magnitude** 属性中，设置值 `10.0`以创建风力的大小。

    PhysX Primitive collider （PhysX 基元碰撞器） 框显示表示风力方向的锥体。

    {{< image-width src="/images/learning-guide/tutorials/physx/wind-force-region-setup.png" width="900" alt="Setting up a PhysX Force Region component" >}}

## 添加 Cloth 预制件

在本节中，您将添加用于测试的 Cloth 预制件。

1. 要测试 wind 提供程序，请添加具有 NVIDIA Cloth 网格的预制件。在 **Asset Browser**中，导航到`Gems\NvCloth\Assets\prefabs\Cloth`，找到`cloth_locked_edge.prefab`，然后将其拖动到视区中。

1. 使用 Move 工具放置 Cloth 预制件。如果您使用的是 **Local wind tag** 属性，则必须将布料资产放置在风提供程序实体的 PhysX Primitive Collider （PhysX 基元碰撞器） 体积内。

    ![Positioning the cloth prefab in the wind provider entity.](/images/learning-guide/tutorials/physx/wind-provider-cloth-prefab.png)

1. 预制件就位后，您可以隐藏风提供程序实体。在 **Entity Outliner** 中，在 wind provider 实体右侧的列中，选择 **Show/Hide Entity** 切换。

1. 布料预制件在其 **Cloth** 组件上启用了 local wind 属性。这将从 Cloth 组件生成局部风力，该风力将覆盖您创建的风提供程序实体中的风力。要停用预制件的局部风力，以便您可以查看您创建的风提供程序的结果，请执行以下操作：
        
    * 在 Entity Outliner 中，双击 {{< icon "prefab.svg" >}} **cloth_locked_edge** 预制件以在焦点模式下对其进行编辑。

    * 选择 {{< icon "entity.svg" >}} **cloth_locked_edge** 子实体以将其选中。

        ![Open the cloth prefab for editing in Focus Mode.](/images/learning-guide/tutorials/physx/edit-cloth-locked-edge-prefab.png)

    * 选择 {{< icon "entity.svg" >}} **cloth_locked_edge** 实体后，在 Entity Inspector 的 Cloth 组件中，展开 **Wind** 属性组并关闭 **Enable local wind velocity**。

        {{< image-width src="/images/learning-guide/tutorials/physx/disable-local-wind-velocity.png" width="450" alt="Turning off local wind velocity in a Cloth component." >}}

## 测试风模拟

最后，您可以测试模拟。在 Focus Mode 中打开 **cloth_locked_edge** 预制件以进行编辑后，执行以下操作：

1. 在 Cloth 组件的顶部，打开 **Simulate in editor**。模拟开始时，布料对象可能会剧烈翻转和拉伸，但它很快就会进入微风模拟。

1. 在编辑器模式下播放风模拟。您可以通过修改 Cloth 组件的各种属性来调整模拟。尝试在 Cloth 组件中调整以下属性：

    * **Air drag coefficient**
    * **Air lift coefficient**
    * **Air Density**

    {{< image-width src="/images/learning-guide/tutorials/physx/cloth-simulate-in-editor.png" width="450" alt="Simulating cloth in editor." >}}

以下视频显示了使用上图所示的 Cloth 组件设置的结果。

{{< video src="/images/learning-guide/tutorials/physx/wind-simulation-result.mp4" autoplay="true" loop="true" info="Wind simulation in editor." >}}
