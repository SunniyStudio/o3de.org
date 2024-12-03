---
linkTitle: 对地形进行纹理处理
title: 对地形进行纹理处理
description: 向地形添加颜色和表面类型。
weight: 400
toc: true
---

在本教程部分中，您将向地形添加颜色和表面类型。

## 应用地形材质

向地形添加颜色和表面类型需要多个相互引用的实体。您可以手动设置实体和引用，也可以使用[Landscape Canvas](/docs/user-guide/gems/reference/environment/landscape-canvas)在生成地形的可视化图形中放置和连接节点。

了解各种 gradient （渐变） 和 terrain （地形） 组件如何协同工作会很有帮助，因此建议您在下面的选项卡中完成本教程的两个版本。

{{< tabs name="terrain-texturing-tutorials" >}}

{{% tab name="Terrain texturing with entities" %}}

在本节中，您将创建定义表面类型和向地形添加材料所需的实体和组件。

1. 在开始此部分之前 **Save** 关卡（热键 **Ctrl + S**），以便如果您同时选择使用 Landscape Canvas 版本的地形纹理，则可以重新加载和还原使用实体进行纹理处理的步骤。

### 应用宏材质

1. 选择 Terrain Spawner 实体后，添加 **Terrain Macro Material** 组件。

2. 在 **Terrain Macro Material** 组件中，选择保存在教程关卡文件夹中的 `tutorial_terrain_basecolor`宏颜色纹理，或者选择您选择的任何其他颜色纹理。本教程不使用宏法线纹理，但是如果您的地形创作软件支持生成宏法线纹理，您也可以在此处选择它。

{{< important >}}
如果使用宏法线纹理，则地形区域大小必须与地形编写软件中的地形大小完全匹配，否则法线将不会指向正确的方向。
{{< /important >}}

此时，地形是彩色的，但近距离观察它缺少任何表面细节：

{{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/level-with-macro-color-far-and-near.png" width=1000 alt="Illustrations of the macro material applied to a terrain." >}}

### 应用详图材质

要将材质作为细节材质应用于地形，请执行以下操作：

#### 添加默认曲面材质

首先添加默认表面材质，以验证您之前创建的地形细节材质是否正常工作并提供您期望的外观。

1. 选择 Terrain Spawner 后，添加 **Terrain Surface Materials List** 组件。

2. 在 **Default Material** 属性右侧，点击 {{< icon "file-folder.svg" >}} **File**按钮并选择`rock_boulder_dry` 材质资源。将使用您创建的岩石材质重新绘制整个地形。

3. 现在，将 **Default Material（默认材质）** 属性设置为 `forrest_ground_01`材质资源。这会将整个地形表面更改为草地。

此时，地形具有 Height Variation（高度变化）、Color Variation（颜色变化）和 grass texture（草地纹理）：

{{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/terrain-default-surface.png" width=1000 alt="Terrain covered with a single default surface material." >}}

#### 为草地表面权重创建渐变实体

此过程与为高度贴图创建渐变实体相同。每个渐变实体都有三个组件：

* 引用渐变图像的 **Image Gradient** 组件。可以使用其他渐变类型，但图像渐变提供最多的控制，并且在创建地形时用于许多场景。

* 一个 **Gradient Transform Modifier** 组件，用于将渐变放置在关卡中。

* 形状组件。大多数情况下，这是一个 **Shape Reference** 组件，它引用 Terrain Spawner 实体中的 **Axis Aligned Box Shape** 组件。

1. 创建名为 `Terrain Grass` 的 Terrain Spawner 实体的子实体，然后添加**Image Gradient**, **Gradient Transform Modifier** 和 **Shape Reference** 组件。

2. 在 Shape Reference （形状引用） 组件上，单击{{< icon "picker.svg" >}} **Picker** 按钮，然后单击 Entity Outliner 中的 Terrain Spawner 实体以创建引用。

3. 在 Image Gradient 上，将 Image Asset 设置为您下载到教程级别文件夹中的 `tutorial_grass_splatmap_gsi` 图像。

#### 为岩石表面权重创建渐变实体

在具有许多不同表面类型的关卡中，您通常会为每个表面权重使用单独的图像。但是，本教程仅使用两种表面类型，因此可以将岩石梯度设置为草地梯度的倒数。

1. 创建 Terrain Spawner 实体的子实体，名为`Terrain Rock`，并向其添加[**Invert Gradient Modifier**](/docs/user-guide/components/reference/gradient-modifiers/invert-gradient-modifier)组件和一个 **Shape Reference** 组件。

2. 在 Shape Reference （形状引用） 组件上，单击 {{< icon "picker.svg" >}}  **Picker** 按钮，然后单击 Entity Outliner 中的 Terrain Spawner 实体以创建引用。

3. 在 Invert Gradient Modifier 组件上，单击 {{< icon "picker.svg" >}} **Picker** 按钮，然后单击 Entity Outliner 中的 Terrain Grass 实体以创建引用。

#### 添加地形表面渐变列表

这些步骤将渐变图元与您创建的曲面标记名称相关联。

1. 选中 Terrain Spawner 后，添加一个 **Terrain Surface Gradient List** 组件。

2. 在 **Terrain Surface Gradient List** 组件中，单击 {{< icon "add.svg" >}} **Add** 按钮以添加新的渐变槽。

3. 在 **Gradient Entity** 属性的右侧，单击 {{< icon "picker.svg" >}} **Picker** 按钮，然后单击 Entity Outliner 中的 Terrain Grass 实体以引用它。

4. 对于 **Surface Tag** 属性，从列表中选择 `tutorial_grass` Surface Tag 名称。请注意，该列表包含您在 Asset Editor 中创建的曲面标记名称。

5. 重复步骤 2 到 4 以添加 Terrain Rock 实体并将其与`tutorial_rock`表面标签关联。

#### 将表面权重添加到 Terrain Surface Materials List

这将根据表面标签名称和关联的渐变图元将细节材质分配给地形。

1. 在 Entity Outliner 中选择 Terrain Spawner 实体。

2. 在 **Terrain Surface Materials List** 组件中，清除默认材质的条目。我们的表面贴图将覆盖整个地形，因此不再需要默认材质。

3. 在 **Material Mappings** 属性组的右侧，单击{{< icon "add.svg" >}} **Add** 按钮以添加新的材质槽。

4. 在 **Surface Tag** 属性中，选择 `tutorial_grass`。

5. 在 **Material asset** 属性右侧，点击 {{< icon "file-folder.svg" >}} **File** 按钮，然后选择要与 Surface 标记关联的`forest_ground_01`材质资源。

6. 重复步骤 3 到 5 以添加`tutorial_rock`并将其与`rock_boulder_dry`材质资源关联。

当您将细节材质和表面标签名称对添加到 **Terrain Surface Materials List** 组件时，材质将显示在地形表面上。

{{% /tab %}}

{{% tab name="Terrain texturing with Landscape Canvas" %}}

在本节中，您将使用 Landscape Canvas 对地形进行着色和曲面处理。

1. 如果您已经完成了“Terrain texturing with entities（使用实体进行地形纹理处理）”选项卡，您将需要重新加载您的关卡而不保存，以恢复您所做的更改。

2. 在 Entity Outliner 中，选择 **Terrain Spawner** 实体。

3. 如果此实体上还没有 **Landscape Canvas** 组件，请选择 **Add Component** 并将 **Landscape Canvas** 组件添加到实体中。

4. 按 **Landscape Canvas** 组件上的 **Edit** 按钮，以启动加载正确地形图的 Landscape Canvas 工具。

### 在 Landscape Canvas 中应用宏材质

1. 选择 **Terrain Layer Spawner** 节点，按 **Add Extender**，然后选择 **Terrain Macro Material**。

2. 在 Node Inspector 的 **Terrain Macro Material** 组件上，选择您保存在教程关卡文件夹中的`tutorial_terrain_basecolor`宏颜色纹理，或者选择您选择的任何其他颜色纹理。本教程不使用宏法线纹理，但是如果您的地形创作软件支持生成宏法线纹理，您也可以在此处选择它。

{{< important >}}
如果使用宏法线纹理，则地形区域大小必须与地形编写软件中的地形大小完全匹配，否则法线将不会指向正确的方向。
{{< /important >}}

此时，地形是彩色的，但近距离观察它缺少任何表面细节：

{{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/level-with-macro-color-far-and-near.png" width=1000 alt="Illustrations of the macro material applied to a terrain." >}}

### 在 Landscape Canvas 中应用细节材质

要将材质作为细节材质应用于地形，请执行以下操作：

#### 在 Landscape Canvas 中添加默认表面材质

首先添加默认表面材质，以验证您之前创建的地形细节材质是否正常工作并提供您期望的外观。

1. 选择 **Terrain Layer Spawner** 节点, 点击 **Add Extenders**, 并选择 **Terrain Surface Materials List**.

2. 在Node Inspector中，在**Terrain Surface Materials List**组件上，在**Default Material** 属性右侧, 点击 {{< icon "file-folder.svg" >}} **File** 按钮并选择`rock_boulder_dry`材质资源。将使用您创建的岩石材质重新绘制整个地形。

3. 现在，将 **Default Material（默认材质）** 属性设置为 `forrest_ground_01` 材质资源。这会将整个地形表面更改为草地。

此时，地形具有 Height Variation（高度变化）、Color Variation（颜色变化）和 grass texture（草地纹理）：

{{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/terrain-default-surface.png" width=1000 alt="Terrain covered with a single default surface material." >}}

#### 在 Landscape Canvas 中创建草地表面权重

1. 在 Landscape Canvas 的 Node Palette 中，展开 **Gradients** 节点列表，然后将一个 **Image** 节点拖到图表中。将实体命名为`Terrain Grass`。

2. 单击 **Axis Aligned Box Shape** 节点的 **Bounds** 引脚，然后拖动到新创建的 **Shape Reference** 节点的 **Inbound Shape** 以连接节点，这样 **图像渐变** 就可以使用与地形相同的边界框。

3. 在 Image Gradient（图像渐变）上，将 Image Asset（图像资源）设置为您下载到教程关卡文件夹中的`tutorial_grass_splatmap_gsi`图像。

#### 在 Landscape Canvas 中为岩石表面权重创建渐变实体

在具有许多不同表面类型的关卡中，您通常会为每个表面权重使用单独的图像。但是，本教程只有两种表面类型，因此可以将岩石渐变设置为草地渐变的倒数。

1. 在 Landscape Canvas 的 Node Palette 中，展开 **Gradient Modifiers** 节点列表，然后将 **Invert** 节点拖动到图表中。将实体命名为  `Terrain Rock`。

2. 单击 **Terrain Grass** 节点的 **Outbound Gradient** 引脚，然后拖动到 **Invert** 节点的 **Inbound Gradient** 引脚。

3. 单击 **Axis Aligned Box Shape** 节点的 **Bounds** 引脚，然后拖动到 **Invert** 节点的 **Preview Bounds** 引脚。

#### 在 Landscape Canvas 中添加地形表面渐变列表

这些步骤将曲面权重与您创建的曲面标记名称相关联。

1. 选择 **Terrain Layer Spawner** 节点，按 **Add Extenders**，然后选择 **Terrain Surface Gradient List**。

2. 单击 **Terrain Grass** 节点的 **Outbound Gradient** 引脚，然后拖动到 **Terrain Surface Gradient List** 的 **入站梯度** 引脚。

3. 单击 **Terrain Rock** 节点的 **Outbound Gradient** 引脚，然后拖动到 **Terrain Surface Gradient List** 的 **Inbound Gradient** 引脚，这将变成一个 **Inbound Gradient** 引脚。

4. 在 Node Inspector 的 **Terrain Surface Gradient List** 中，为第一个 **Surface Tag** 选择`tutorial_grass`，为第二个 **Surface Tag** 选择`tutorial_rock`。

#### 将表面权重添加到 Landscape Canvas 中的 Terrain Surface Materials List

这将根据表面标签名称和关联的渐变图元将细节材质分配给地形。

1. 在 Node Inspector 的 **Terrain Surface Materials List** 组件中，清除默认材质的条目。我们的表面贴图将覆盖整个地形，因此不再需要默认材质。

2. 在 **Material Mappings** 属性组右侧，点击 {{< icon "add.svg" >}} **Add**按钮添加新的材质插槽。

3. 在**Surface Tag**属性中，选择`tutorial_grass`。

4. 在**Material asset** 属性右侧，点击 {{< icon "file-folder.svg" >}} **File** 按钮，然后选择要与 Surface 标记关联的`forest_ground_01`材质资源。

5. 重复步骤 2 到 4 以添加`tutorial_rock`并将其与`rock_boulder_dry`材质资产关联。

当您将细节材质和表面标签名称对添加到 **Terrain Surface Materials List** 组件时，材质将显示在地形表面上。

The final Landscape Canvas graph looks like this:

{{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/landscape-canvas-graph.png" width=1000 alt="Landscape Canvas graph for the terrain." >}}

{{% /tab %}}

{{< /tabs >}}

## 调整材质渲染设置

现在，地形已完全纹理化，可以调整 **Terrain World Renderer** 组件上的设置以提供更好的结果。

1. 在 Entity Outliner 中选择 **Level** 实体。

2. 在 **Terrain World Renderer** 组件中，在 **Detail material configuration** 下启用 **Height based texture blending**。这会将地形渲染器从对细节材质使用纯 Alpha 混合更改为在材质上使用置换贴图以根据材质高度进行混合。基于高度的混合在细节材质之间提供更清晰的细节和更逼真的过渡。

    | Alpha blending | Height-based blending |
    | - | - |
    | {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/detail-alpha-blending.png" alt="Detail materials blended with alpha blending." >}} | {{< image-width src="/images/learning-guide/tutorials/environments/terrain-from-images/detail-height-based-blending.png" alt="Detail materials blended with height-based blending." >}} |

    如果性能成为问题，可以调整细节材质的渲染和淡化距离，以减少细节材质的绘制距离。

3. **Save** 关卡 (快捷键 **Ctrl + S**)。
