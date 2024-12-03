---
linkTitle: 创建地形形状
title: 创建地形形状
description: 在关卡中创建地形的整体形状。
weight: 300
toc: true
---

在本教程小节中，您将在关卡中创建一个具有所需形状和大小的地形。

## 创建地形

生成地形需要多个相互引用的实体。您可以手动设置实体和引用，也可以使用 [Landscape Canvas](/docs/user-guide/gems/reference/environment/landscape-canvas)在生成地形的可视化图形中放置和连接节点。使用 Landscape Canvas 的优势在于，它提供了一个对艺术家友好的节点图形界面，用于在关卡中创建和配置地形实体，以及提供这些实体和组件之间关系的可视化表示。

了解各种 gradient （渐变） 和 terrain （地形） 组件如何协同工作会很有帮助，因此建议您在下面的选项卡中完成本教程的两个版本。

{{< tabs name="terrain-creation-tutorials" >}}

{{% tab name="Terrain creation with entities" %}}

在本节中，您将创建一个地形生成器实体和一个渐变实体。terrain spawner 实体定义地形并引用提供高程数据的梯度。gradient 实体提供包含海拔数据的梯度（在本例中为高度贴图图像）。

### 创建地形生成器实体

1. 创建新实体 (快捷键 **Ctrl + Alt + N**)。命名实体为`Terrain Spawner`。

2. 选中实体的[**Transform**](/docs/user-guide/components/reference/transform)组件的 **Translate** 值的每个数值为`0.0 m`，以便实体存在于世界的起点。

3. 添加 [**Terrain Layer Spawner**](/docs/user-guide/components/reference/terrain/layer_spawner)组件。此组件使实体能够生成地形。

4. **Terrain Layer Spawner** 组件显示有关缺少必需组件的警告。选中 **Add Required Component** 并从列表中选择 [**Axis Aligned Box Shape**](/docs/user-guide/components/reference/shape/axis-aligned-box-shape)。此框形状定义生成的地形的范围。

5. 要生成与高度贴图具有相同分辨率的地形，在 **Axis Aligned Box Shape** 组件中，设置 **Dimensions:** **X** 和 **Y** 属性的值为高度图的分辨率（单位：像素）。**Dimensions: Z** 属性可用于缩放高程。为 **Dimensions: Z** 属性输入一个值，该值大致表示所需的最大标高（以米为单位）。我们的教程高度图图像为 256 x 256 像素，表示 50 米的海拔变化，因此将尺寸设置为 `(256.0, 256.0, 50.0)`。如果您希望使用不同的高度贴图，请根据该高度贴图的分辨率设置尺寸。

    {{< tip >}}
为了获得最佳效果，应将 **Axis Aligned Box Shape** 的 **X** 和 **Y** 尺寸设置为高度图像素大小乘以 **Height Query Resolution**。这可确保地形系统使用的每个世界单位只有一个高度贴图像素。当 **Height Query Resolution** 设置为 1 米（默认值）时，以米为单位的框大小应与高度贴图像素分辨率完全相同。如果框尺寸太小，则不会使用某些高度图像素，这可能会导致意外的平滑区域或过度急剧的海拔变化区域。
    {{< /tip >}}

6. **Axis Aligned Box Shape**以实体的位置为中心，因此当实体位于`(0.0, 0.0, 0.0)`时，框在 Z 轴上从`-25.0` 到 `25.0`。但是，默认情况下，最小地形高程设置为`0.0`。长方体形状的底部至少需要位于世界 Z 轴上的最小地形高程值，因为任何低于最小高程的地形都将被裁剪。在 Terrain Spawner 实体的 **Transform** 组件中，将 **Translate: Z** 属性设置为一个值，该值是您在上一步中用于 **Dimensions: Z** 属性的值的一半。例如，如果您在上一步中输入了`50.0`，则对 **Translate: Z** 属性使用`25.0` 来向上移动实体，使框的底部在世界 Z 轴上位于 `0.0`，而框的顶部延伸到`50.0`。

    {{< tip >}}
设置框形状的尺寸后，可以通过禁用 **Axis Aligned Box Shape** 组件中的 **Filled** 属性来隐藏它。
    {{< /tip >}}

7. 在 Entity Inspector中，添加一个 [**Terrain Height Gradient List**](/docs/user-guide/components/reference/terrain/height_gradient_list) 组件到 **Terrain Spawner** 实体。此组件将在后续步骤中引用 gradient 实体。

下图是完整的 **Terrain Spawner** 实体：

{{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/terrain-spawner-entity.png" width="450" alt="The completed Terrain Spawner entity." >}}

### 创建地形高度梯度实体

1. 选择 **Terrain Spawner** 实体后，创建一个新的子实体（热键 **Ctrl + Alt + N**）。为新实体命名`Terrain Height`.

2. 添加 [**Image Gradient**](/docs/user-guide/components/reference/gradients/image-gradient) 组件到 **Terrain Height** 实体。 您将使用 **Image Gradient** 组件来添加和配置地形的高度贴图。

3. **Image Gradient** 组件显示有关缺少必需组件的警告。点击 **Add Required Component** 并从列表选择 [**Gradient Transform Modifier**](/docs/user-guide/components/reference/gradient-modifiers/gradient-transform-modifier)。**Gradient Transform Modifier** 组件可用于变换世界空间中的渐变。

4. **Gradient Transform Modifier** 组件显示有关缺少必需组件的警告。 点击 **Add Required Component** 并从列表中选择 **Shape Reference** 。在后续步骤中，您将在 **Terrain Spawner** 实体中添加对 **Axis Aligned Box Shape** 组件的引用，以确保地形边界和渐变边界相同。

5. 添加高度图到**Terrain Height**实体。在**Image Gradient**组件中，在 **Image Asset** 属性右侧，点击{{< icon "file-folder.svg" >}} **File** 按钮并选择`tutorial_terrain_heightmap_gsi` 保存在 Tutorial Level 文件夹中的高度图资源，或者您想要使用的任何其他高度贴图图像（可选）。

    {{< tip >}}
将 **Image Gradient** 组件中的 **Sampling Type** 属性从 `Point` 到 `Bilinear` 或 `Bicubic`，以使高度变化更平滑，以防高度贴图分辨率和 **Axis Aligned Box Shape** 组件的尺寸不同。
    {{< /tip >}}

6. **Shape Reference** 组件需要引用 **Terrain Spawner**实体 中的 **Axis Aligned Box Shape** 组件。在 **Shape Reference** 组件中，点击 {{< icon "picker.svg" >}} **Picker** 按钮，然后点击**Entity Outliner**中的**Terrain Spawner**实体创建引用。下图展示了最终的 **Terrain Height** 实体：

    {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/terrain-gradient-entity.png" width="450" alt="The completed Terrain Gradient entity." >}}

7. 选择**Terrain Spawner**实体。在**Terrain Height Gradient List** 组件中，点击 {{< icon "add.svg" >}} **Add** 按钮以添加新的渐变槽。单击 {{< icon "picker.svg" >}} **Picker** 按钮，然后在**Entity Outliner**中点击**Terrain Height**实体创建引用。

8. 选择**Terrain Height**实体。在**Gradient Transform Modifier**组件中，设置**Wrapping Type** 属性为 `Clamp To Edge`。将此属性设置为`Unbounded` 可能会在地形的边缘产生伪影。

9. **Save**关卡 (快捷键 **Ctrl + S**)。

{{< note >}}
在继续下一部分之前，请务必通过选择本节顶部的选项卡来了解 Landscape Canvas 的地形设置。
{{< /note >}}

{{% /tab %}}

{{% tab name="Terrain creation with Landscape Canvas" %}}

如果您在左侧选项卡中使用实体创建地形，则可以看到该方法可能非常耗时且难以排除故障。Landscape Canvas 可以大大简化该过程，并为生成地形的各种实体之间的关系提供易于理解的可视化图表。当您创建复杂的渐变和渐变修饰符网络以定义地形高程和表面类型时，Landscape Canvas 特别有用。

在本小节中，您将使用 Landscape Canvas 基于高度贴图创建地形。

1. 如果您的关卡中存在现有地形，请将其删除。

2. 在 O3DE 编辑器中，从 **Tools** 菜单，选择 **Landscape Canvas**。

3. 在 Landscape Canvas 中，从 **File** 菜单，选择 **New Asset** (快捷键 **Ctrl + N**)。请注意，名为 **Entity1** 的新图形将显示在 Landscape Canvas 视图中。在 O3DE Editor 中，将创建包含 **Landscape Canvas** 组件的相应实体。当您在 Landscape Canvas 中工作时，将创建其他实体并将其添加为 **Entity1** 的子实体。您可以重命名 Landscape Canvas 创建的实体。

4. 在 Landscape Canvas 中，在 **Node Palette** 中，展开 **Terrain** 节点，拖拽一个 **Terrain Layer Spawner** 节点到图表中。请注意，在 O3DE 编辑器中，将创建一个名为 **Entity2** 的新子实体，其中包含地形生成器所需的组件。另请注意，这些组件显示在 Landscape Canvas 的 **Node Inspector**中。

5. 在 Landscape Canvas 中，在 **Terrain Layer Spawner** 节点上，选择 **Add Extenders** 并从列表中选择 **Terrain Height Gradient List**。这会将 **Terrain Height Gradient List** 组件添加到地形生成器实体中，您将在本教程后面使用该组件来引用高度贴图。

6. 在 Node Inspector 中，在 **Axis Aligned Box Shape** 组件中，设置 **Dimensions:** **X** and **Y** 属性的值为高度图的分辨率（单位：像素）。为**Dimensions: Z**属性输入一个值，该值表示所需的最大高程变化（以米为单位）。我们的教程高度图图像为 256 x 256 像素，表示 50 米的海拔变化，因此将尺寸设置为 `(256.0, 256.0, 50.0)`。如果您希望使用不同的高度贴图，请根据该高度贴图的分辨率设置尺寸。

7. 在 Node Inspector 中，在 **Transform** 组件中，设置 **Translate: Z** 属性设置为一个值，该值是您在上一步中用于 **Dimensions: Z** 属性的值的一半。例如，如果您在上一步中输入了`50.0`，则对 **Translate: Z** 属性使用 `25.0`以向上移动实体，使框的底部在世界 Z 轴上为 0。下图显示了已完成的 **Terrain Layer Spawner** 节点：

  {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/landscape-canvas-terrain-layer-spawner.png" width="650" alt="The configured terrain layer spawner node in Landscape Canvas." >}}

8. 在 Landscape Canvas 的 Node Palette 中，展开 **Gradients** 节点列表，然后将 **Image** 节点拖动到图表中。请注意，在 O3DE 编辑器中，将创建一个名为 **Entity3** 的新子实体，其中包含图像渐变所需的组件。另请注意，组件显示在 Landscape Canvas 的 Node Inspector 中。

9. 在 **Image** 节点的 **Image Gradient** 组件中，在 **Image Asset** 属性的右侧，单击  {{< icon "file-folder.svg" >}}  **File** 按钮，然后选择您保存在教程关卡文件夹中的`tutorial_terrain_heightmap_gsi` 高度贴图资产，或者您要使用的任何其他高度贴图图像（可选）。

10. 单击 **Axis Aligned Box Shape** 节点的 **Bounds** 引脚，然后拖动到 **Shape Reference** 节点的 **Inbound Shape** 以连接节点并指向 **Image Gradient** 以使用与地形相同的框。

11. 单击 **Image** 节点的 **Outbound Gradient** 引脚，然后拖动到 **Terrain Layer Spawner** 节点的 **Inbound Gradient** 以连接节点并生成地形。最终图形和 **Image** 节点配置如下图所示：

    {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/landscape-canvas-terrain-spawner-image-gradient.png" width="938" alt="The configured image gradient node and terrain network in Landscape Canvas." >}}

12. **Save** 关卡 (快捷键 **Ctrl + S**).

{{% /tab %}}

{{< /tabs >}}

## （可选）调整高度设置

实体完成后，**Terrain Spawner** 会生成具有不同高度的无纹理地形，类似于以下示例：

{{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/level-with-untextured-terrain.png" width="810" alt="Illustration of the level with an untextured terrain added to it." >}}

但是，您可能会遇到地形中的山谷和山峰由于裁剪而显得平坦的问题。下图显示了一个地形，该地形的放置使其框形状的底部低于 **Terrain World** 关卡组件中指定的 **Min Height**：

{{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/terrain-clipping-example.png" width="550" alt="An example terrain demonstrating clipping due to entity placement." >}}

以下属性密切相关，如果您的地形存在问题（例如裁剪），则可能需要特别注意：

* **Terrain World** 关卡组件的 **Min Height** 和 **Max Height** 属性定义了关卡中用于生成地形的垂直范围。此范围应足够大，以涵盖 Terrain Spawner 实体的盒形的垂直范围。如果框形状延伸到 **Min Height** 值以下或高于 **Max Height**值，则地形山谷或峰值将被剪切。

* **Axis Aligned Box Shape** 组件的 **Dimensions** 属性指定地形区域。特别是 **Dimensions: Z** 属性指定地形的高度。其值不应超出 **Terrain World** 关卡组件中由 **Min Height** 和 **Max Height** 属性定义的范围。

* Terrain Spawner 实体的 **Transform** 组件的 **Translate** 属性将地形实体放置在关卡中。Terrain Spawner 实体的平移位置位于定义地形区域的框形状的中心。这意味着，如果长方体形状是一个 512 个单位的立方体，并且 **Translate** 属性在每个轴上都是`0.0`，那么地形将从关卡的原点向每个方向延伸 256 个单位。请注意，确保 **Translate: Z** 值将框形状置于 **Terrain World** 关卡组件的 **Min Height** 和 **Max Height** 属性定义的范围内。

## （可选）调整地形网格渲染设置

如果您使用的是教程资源，则默认的地形网格渲染设置就足够了。但是，如果您选择使用与本教程中列出的不同的高度贴图和地形生成器框大小，并且框大小大于默认地形渲染距离 4096 米或默认摄像机视图距离 1024 米，则需要调整渲染距离以使整个地形可见。

1. 在 **Terrain World Renderer** 关卡组件t中，将 **Mesh render distance**（网格渲染距离）设置为大于地形生成器框大小的值，以便整个地形都可见。

2. 如果盒体尺寸大于 256 x 256，请执行以下操作：

    i. 设置 **First LOD distance** 属性为 `256`。这会将地形的最高细节级别 （LOD） 显示设置为距摄像机 256 米的距离。

    ii. 设置 **CLOD Distance** 属性为 `32`。这会将 LOD 之间的混合距离设置为 32 米。较大的值使 LOD 过渡更平滑，但该值应小于 **First LOD distance** 值的 1/4，以确保有足够的距离来混合每个 LOD。

3. 在 **Camera** 实体的 **Camera** 组件中，将 **Far clip distance** 属性设置为大于框大小的值。

4. 在 **Camera** 实体的 **Camera** 组件中，选择 **Be this camera** 以通过相机实体查看关卡。

5. 在 **Edit** 菜单、**Editor Settings**、**Global Preferences...** 中，在 **Camera** 首选项中，将 **Camera Movement Speed** 设置为适合地形大小的值。`100.0` 是大型地形的良好参考点。

这些建议旨在帮助您在 O3DE Editor 中构建地形时查看整个地形。在 Launcher 应用程序中，应调整这些设置，以便在性能和质量之间取得适当的平衡。
