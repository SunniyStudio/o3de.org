---
linkTitle: PhysX 资产
title: 处理 PhysX 碰撞器资产
description: 了解如何使用 Scene Settings（场景设置）在 Open 3D Engine （O3DE） 中自定义 PhysX 碰撞器资产处理。
weight: 400
toc: true
---

**Open 3D Engine （O3DE）** 具有一组用于生成 PhysX 碰撞器资产的强大选项。在 [场景设置](/docs/user-guide/assets/scene-settings/scene-settings)中，您可以自定义 PhysX 碰撞器资产生成。 生成的 PhysX 碰撞器存储在`.pxmesh`产品资产中。您可以将 PhysX 碰撞器资产添加到 **PhysX 网格碰撞器** 组件。

生成的 PhysX 碰撞器存储在`.pxmesh`产品资产中。您可以将 PhysX 碰撞器资产添加到 **PhysX 网格碰撞器** 组件。


{{< note >}}
了解用于创建 PhysX 碰撞器源资源的 [最佳实践](/docs/user-guide/assets/scene-settings/source-asset-best-practices#physx)可以缓解您在处理 O3DE 碰撞器时可能遇到的问题。碰撞体支持的数据技术详情请参考[支持的 3D 场景数据](/docs/user-guide/assets/scene-settings/scene-format-support#supported-3d-scene-data) 表。
{{< /note >}}

| O3DE 体验 |完成时间 |功能聚焦 |最后更新|
| - | - | - | - |
| 初级 |25 分钟 |使用 Scene Settings（场景设置）对`.fbx`文件中的 PhysX 碰撞器资产进行自定义处理。 | January 4, 2023 |

## 实体行为

在了解碰撞器类型之前，您应该了解在给定场景中可能影响碰撞器资产类型的三种实体行为类型：

* **Static** -- 静态实体具有 **PhysX Static Rigid Body** （PhysX 静态刚体），可以与之碰撞，但它们不会移动，并且 PhysX 碰撞和力不会影响它们。静态实体可以使用任何碰撞器类型。
* **Kinematic** -- 运动学实体具有 **PhysX Dynamic Rigid Body** 组件和由代码或脚本驱动的运动。运动实体可以碰撞，但 PhysX 碰撞和力不会影响它们。Kinematic 实体可以使用任何碰撞器类型。
* **Simulated** -- 模拟实体具有 **PhysX Dynamic Rigid Body** 组件和由 PhysX 碰撞和力产生的模拟运动。模拟实体只能使用基元碰撞器和凸面碰撞器。

## PhysX 碰撞器资产

碰撞器资源基于您在 Scene Settings （场景设置） 中指定的输入网格。您可以使用单个输入网格，使用多个输入网格，甚至可以指定在生成碰撞器资产之前，将多个输入网格合并并焊接成单个网格。生成碰撞器资产时，源资产不会更改。

由于物理模拟的计算成本可能很高，因此了解各种碰撞器类型、它们的限制以及可以使用它们的场景非常重要。

### 三角形网格

顾名思义，三角形网格碰撞器由三角形网格组成。您可以在数字内容创建 （DCC） 应用程序中专门为碰撞器创建简化的网格，也可以基于资产的渲染网格生成三角形网格碰撞器。三角形网格碰撞器可以非常接近复杂的渲染网格，但您只能将它们用于静态和运动实体。由于三角形网格碰撞体可以包含许多顶点，并且碰撞体资产不需要是凸面的，因此这些碰撞体通常比原始碰撞体产生更高的性能成本。

三角形网格碰撞体可以具有多个物理材质分配。由于碰撞器派生自输入网格，因此您可以为每个材质分配将物理材质应用于碰撞器。这对于在材质（如混凝土和泥土、木板和地毯）之间过渡的地面和地板碰撞器特别有用。

碰撞体占用与输入网格相同的物理空间。在下图中，三角形网格碰撞器资产偏移到生成碰撞器的输入网格资产的右侧，以便您可以清楚地查看碰撞器。请注意，碰撞器资产（右）准确表示渲染网格（左），并且碰撞器支持网格的正面和背面（洋红色线框）和侧面（青色线框）的单独物理材质。

{{< image-width "/images/learning-guide/tutorials/assets/triangle-mesh-collider-example.png" "700" "An example triangle mesh collider asset." >}}

三角形网格碰撞器通常用于具有复杂形状的固定环境对象，例如树干、地板、大型石头和建筑特征以及雕像。

### 基元

基元碰撞器由球体、长方体或胶囊体基元形状组成。由于 **Radius**、**Height** 和 **Width** 等简单属性定义了基元形状，因此基元碰撞器通常提供最佳模拟性能。基元碰撞器会自动拟合到输入网格。但是，根据输入网格的复杂程度，碰撞器的区域可能位于输入网格表面之内或远处。这可能会导致视觉上不准确的碰撞。**Decompose Meshes** 属性将复杂的输入网格分解为更小的部分，并使原始碰撞体适合每个部分。您可以将基元碰撞器与静态、运动和模拟实体一起使用。这些碰撞体只能有一个物理材质分配。

碰撞体占用与输入网格相同的物理空间。在下图中，基元碰撞器资产偏移到生成碰撞器的输入网格资产的右侧，以便您可以清楚地查看碰撞器。碰撞器资产（右）是一个简单的长方体，它已自动定向和缩放以包含渲染网格（左）。请注意，基元碰撞体的一个角从输入网格延伸得很远，并穿透地平面。这演示了在复杂输入网格上使用基元碰撞器的缺点之一。在这是模拟实体的情况下，地平面碰撞会将资产向上推，并且由于基元碰撞体底部的尖角，资产不会直立静止。

{{< image-width "/images/learning-guide/tutorials/assets/primitive-collider-example.png" "700" "An example primitive collider asset." >}}

当输入网格与简单的基元形状之一非常相似时，以及在快速动态碰撞分辨率比视觉准确性更重要的场景中，通常使用基元碰撞器。

### 凸面

凸面碰撞器是自动生成的凸面外壳。凸包没有凹面或空心表面积。对于 PhysX，凸包是在有限数量的顶点内构建的，并适合输入网格。与基元碰撞器相比，凸面碰撞体可以更好地近似复杂的输入网格，但凸面碰撞体会产生更大的性能成本。与原始碰撞体相比，凸面碰撞体还可能具有碰撞体表面落在输入网格内部或外部的区域，这可能会导致碰撞在视觉上不准确。Decompose Meshes （分解网格） 属性将复杂的输入网格分解为较小的部分，并使凸面碰撞体适合每个部分。您可以将凸面碰撞体与静态、运动或模拟实体一起使用。这些碰撞体只能有一个物理材质分配。

碰撞体占用与输入网格相同的物理空间。在下图中，凸面碰撞器资产偏移到生成碰撞器的输入网格资产的右侧，以便您可以清楚地查看碰撞器。请注意，碰撞器资产（右）已自动生成，以包含并大致近似渲染网格的轮廓形状（左）。但是，凸面碰撞器不包括输入网格中间的孔等细节。

{{< image-width "/images/learning-guide/tutorials/assets/convex-collider-example.png" "700" "An example convex collider asset." >}}

凸面碰撞体通常用于具有必须动态模拟的复杂渲染网格的实体，例如交互式道具，以及必须分解为较小部分以提供具有更高视觉准确性的碰撞的实体。

### 总结

下表总结了有关可用碰撞器类型的大多数导入信息：

| 类型 |实体行为 |描述 |局限性|
| - | - | - | - |
| **Triangle mesh** | Static, Kinematic | 由三角形组成的碰撞体。可以非常接近复杂的输入网格，并具有多个物理材质分配。 | 只能用于静态和运动实体。模拟的计算成本可能比其他碰撞器类型更高。 |
| **Primitive** | Static, Kinematic, Simulated | 由自动拟合到输入网格的简单基本球体、长方体或胶囊体形状定义的碰撞器。通常提供最佳性能。| 可能无法非常接近复杂的输入网格。可以拟合到输入网格，但在某些情况下，可能位于输入网格的范围之内或远远超出输入网格的范围，从而产生视觉上不准确的碰撞。只能有一个物理材质分配。 |
| **Convex** | Static, Kinematic, Simulated | 自动生成的碰撞器，它是一个凸包，由有限数量的顶点组成。凸包没有凹面或空心表面积，并且会自动拟合到输入网格。与原始碰撞器相比，此碰撞器可以更好地近似输入网格，但模拟的计算成本更高。 | 可能无法近似输入网格和三角形网格碰撞器，或提供原始碰撞器的性能。可能略微落在输入网格的范围之内或之外。只能有一个物理材质分配。 |

## Generate PhysX collider （生成 PhysX 碰撞器） 资产

您可以从包含至少一个网格的任何源资产生成 PhysX 碰撞器资产。您可以在 Scene Settings（场景设置）的 **PhysX** 选项卡中自定义 PhysX 碰撞器生成的设置。如果不熟悉 Scene Settings，请参考 [网格处理教程](../mesh-assets) 并检查 [场景设置 PhysX 选项卡](/docs/user-guide/assets/scene-settings/physx-tab)主题，了解有关生成 PhysX 碰撞器资产的选项的详细信息。

您可以使用包含至少一个网格的任何源资源来遵循本教程。

1. 在 **O3DE 编辑器**的 **资源浏览器** 中，找到您的源资源。如果您没有自己的资源，则可以在 Asset Browser 顶部的搜索字段中键入`fbx`并使用提供的`.fbx`文件之一，例如 `sphere.fbx`.

    ![ Search for a specific mesh asset in Asset Browser. ](/images/learning-guide/tutorials/assets/meshes-search-asset-browser.png)

    如果您的资产已处理完毕，您可能会在“.fbx”源资产下方看到该资产的预览图像和产品资产列表。

1. 要打开 Scene Settings，请右键单击`.fbx`源资源，然后从上下文菜单中选择 **Edit settings...**。

    ![ Open Scene Settings from Asset Browser. ](/images/learning-guide/tutorials/assets/meshes-edit-settings.png)

1. Scene Settings （场景设置） 窗口根据源资源文件的内容显示不同的选项卡。选择 **PhysX** 选项卡。如果选项卡为空，要创建 PhysX 网格组，请选择 **Add another physxmesh**。

    ![ Scene Settings PhysX tab. ](/images/learning-guide/tutorials/assets/physx-scene-settings.png)

    在此图像中，有一个 **PhysX mesh group**。每个 PhysX 网格组都会生成一个`.pxmesh`产品资产。您可以通过选择 **Add another physxmesh** （添加另一个 physxmesh） 为源资产创建其他 PhysX 网格组。

    **Name PhysX Mesh** 属性包含源资产的名称。此 PhysX 网格组的`.pxmesh`产品资产使用此字符串作为其名称。

1. 要选择要包含在 PhysX 网格组中的网格，请在 **Select meshes** （选择网格） 属性旁边，选择文件选择{{< icon browse-edit-select-files.svg >}} 按钮。列表中的网格由紫色网格图标表示，如下图所示。如果资产中有多个网格可用，您可以在此处选择多个网格。如果选择多个网格，则可以额外启用 **Merge Meshes** 和 **Weld Vertices** 属性，以确保输入网格得到优化。

    ![ Selecting a PhysX mesh. ](/images/learning-guide/tutorials/assets/select-physx-mesh.png)

    {{< note >}}
要将源资产中的网格自动分配给 PhysX 网格组，请在 DCC 应用程序中将后缀`_phys`添加到网格节点名称中。在源资源中的节点名称后加上`_phys`的任何网格都将从默认渲染网格组中排除，并自动添加到 Scene Settings （场景设置） 中的单个 PhysX 网格组中。
    {{< /note >}}

1. 通过将 **Export as** 属性设置为`Convex`来自定义 PhysX 网格碰撞器类型，以便您可以创建任何类型的实体（静态、运动或模拟）。

    您在 Scene Settings 中进行的自定义存储在扩展名为 `.assetinfo` 的 *sidecar 文件*中。当 Asset Processor 检测到`.assetinfo`文件时，它会使用文件中的设置来处理相关的源资产。此 sidecar 文件被视为资产的源依赖项。这意味着，如果 `.assetinfo` 文件发生更改，则会重新处理源资产，即使源资产未发生更改。

1. 在 Scene Settings （场景设置） 的右下角，选择 **Update**（更新）。这将创建或更新 `.assetinfo` sidecar 文件，并触发 Asset Processor 重新处理资产。

1. 将 `.azmodel` 产品资源从 Asset Browser 拖动到视区中。

    {{< image-width "/images/learning-guide/tutorials/assets/physx-entity.png" "900" "Drag the mesh asset into the viewport.">}}

    当您将资产拖动到视区中时，O3DE 会自动创建一个实体，该实体具有引用网格产品资产的 **Mesh** 组件。如果源资源包含已处理的材质，则这些材质将自动应用于网格。请注意，在 Asset Browser 中，`.pxmesh`产品资产已生成，并显示在源资产下方。

1. 将 PhysX Mesh Collider （PhysX 网格碰撞器） 组件添加到实体。在视区中选择实体后，在 **Entity Inspector** 中选择 **Add Component**，然后从组件列表中选择 **PhysX Mesh Collider**。该组件会自动检测`.pxmesh`资产，并将其分配给 **PhysX Mesh** 属性。

1. 根据所需的实体类型，执行以下操作之一：

    * 对于静态实体，请添加 **PhysX Static Rigid Body** （PhysX 静态刚体） 组件。在视区中选择实体后，在 **Entity Inspector** 中选择 **Add Component**，然后从组件列表中选择 **PhysX Static Rigid Body**。要优化当前的静态实体，请在 **Transform** 组件中启用 **Static** 属性。此属性可确保静态实体的最佳运行时性能。

    * 对于模拟实体，请添加 **PhysX Dynamic Rigid Body** （PhysX 动态刚体） 组件。在视区中选择实体后，在 **Entity Inspector **中，选择 **添加组件**，然后从组件列表中选择 **PhysX Dynamic Rigid Body**。如果您现在选择 {{< icon simulate-physics.svg >}} 模拟按钮，实体会随着重力而下降。

    * 对于运动实体，请添加 **PhysX Dynamic Rigid Body** 组件，就像对模拟实体执行的操作一样。然后，在 **PhysX Dynamic Rigid Body** 组件中，选择 Type（类型）属性中的 **Kinematic**（运动学）。

    
    {{< caution >}}
对于运动学或模拟实体，在 **Transform** 组件中，确保 **Static** 属性*未*启用。
    {{< /caution >}}

## 分解输入网格

由多个网格组成的资产或具有复杂网格的资产可能需要类似的复杂 PhysX 碰撞器资产。对于运动实体和模拟实体尤其如此，它们不能使用三角形网格碰撞器。在这些情况下，您可以将输入网格分解为凸面部分。您可以自动生成原始碰撞体或凸面碰撞体，将它们拟合到每个部分，并将它们作为碰撞体`.pxmesh`产品资产进行处理。

网格分解是生成和拟合碰撞器资产过程的一部分，它不会改变输入网格。您只能对基元碰撞器或凸面碰撞器使用网格分解。

{{< note >}}
在 O3DE 中，网格分解使用 V-HACD 库。有关更多信息，包括演示复杂网格分解的示例图像，请参阅GitHub上的[V-HACD 库](https://github.com/kmammou/v-hacd)。
{{< /note >}}

要使用网格分解，请执行以下操作：

1. 在 Scene Settings 中，在**PhysX Mesh group**下，对于**Export As**，选择`Primitive` 或 `Convex`。
1. 启用 **Decompose Meshes**.

启用 Decompose Meshes （分解网格） 会显示许多可用于微调网格分解的选项。有关网格分解选项的指导，请参阅 **Scene Settings PhysX Tab** 主题的 [分解网格](/docs/user-guide/assets/scene-settings/physx-tab/#decompose-meshes)部分。

### 原始分解

当您为原始碰撞器启用 Decompose meshes （分解网格） 时，输入网格将分解为凸面部分。系统会自动选择并转换最适合的基元形状以包含每个部分，并将基元作为碰撞器`.pxmesh`产品资产进行处理。在下图中，输入网格（左）分解为四个部分，并且每个部分（右）都自动拟合了一个原始碰撞器。这些零件装有长方体基元碰撞器。

{{< image-width "/images/learning-guide/tutorials/assets/primitive-decompose.png" "700" "An example of mesh decomposition with primitive collider assets." >}}

### 凸分解

当您为凸面碰撞器启用 Decompose meshes （分解凸面碰撞器的网格） 时，输入网格将分解为凸面部分。为每个零件生成一个凸包，并将凸包处理为碰撞器`.pxmesh`产品资产。在下图中，输入网格（左）被分解为四个部分，并为每个部分（右）生成凸包。凸面碰撞体提供了相当准确的渲染网格表示，这在许多情况下可能就足够了。


{{< image-width "/images/learning-guide/tutorials/assets/convex-decompose.png" "700" "An example of mesh decomposition with convex collider assets." >}}

通常，使用分解网格生成的碰撞器资产比单个基元或凸面碰撞器提供更准确的渲染网格表示。例如，请注意，在上述两个结果中，碰撞器表面不会挡住徽标中间的孔，这与凸面碰撞器资产类型的第一个示例图像不同。
