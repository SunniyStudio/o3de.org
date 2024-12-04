---
linkTitle: 图层
title: 创建植被图层
description: 在 Open 3D Engine中创建一个基本植被层。
weight: 100
toc: true
---

创建植被层是创建动态植被的最基本步骤，但要成功生成植被，必须满足一些要求。


**先决条件**

要使用**Vegetation Layer Spawner**组件生成植被资产，您必须有一个可以种植的表面。您可以通过以下方式建立一个表面：

* 在 *Ground* 实体中添加一个 **Mesh Surface Tag Emitter** 组件，该组件作为 *Atom Default Environment* 实体层次结构的一部分存在，每个新层次都包含该组件。(在下面的示例中使用了这种技术）。
* 在任何其他有网格组件的实体上添加一个**Mesh Surface Tag Emitter**组件。
* 在有形状组件的实体上添加一个**Mesh Surface Tag Emitter**组件。
* 在关卡中添加 [Terrain](/docs/user-guide/components/reference/terrain/layer_spawner)。

您还必须拥有**Vegetation Layer Spawner**可以生成的植被资产。在下面的示例中，我们使用以下步骤创建了一个名为`DryGrassLarge.prefab`的Prefab：

1. 创建一个新实体。
2. 添加一个**Mesh**组件。
3. 为**Mesh**组件分配一个植被`.fbx`文件。
4. 将实体保存为Prefab。

现在，生成植被的基本要求已经满足，我们可以创建植被层了。


**创建植被层**

1. 创建一个实体并命名。

   在本例中，实体名为 *BasicCoverage*。

    ![Create an entity and name it BasicCoverage.](/images/user-guide/vegetation/dynamic/create-vegetation-layer-basic-coverage.png)

2. 添加**Vegetation Layer Spawner** 组件到你的实体。

    ![Add the Vegetation Layer Spawner component to your entity.](/images/user-guide/vegetation/dynamic/create-vegetation-layer-layer-spawner.png)

    **Vegetation Layer Spawner** 组件是初始化生成植被的引擎的核心组件。

3. 点击 **Add Required Component** 并选择 **Shape Reference**。

    ![Choose the Shape Reference for your Vegetation Layer Spawner.](/images/user-guide/vegetation/dynamic/create-vegetation-layer-add-shape.png)

    **Shape Reference** 自身没有形状。接下来，您必须创建一个子实体，并添加一个**Shape组件**，您将在**Shape Reference**中引用该组件。 

4. 右击**BasicCoverage**，选择**Create Entity**，并命名为 *TestBox*。

5. 选择 *TestBox*, 点击 **Add Component**, 并选择 **Box Shape** 组件。

6. 调整形状的大小和位置，使其足够大，并与地面相交。

    ![Adjust your shape to cover a sufficient area and intersect with the ground.](/images/user-guide/vegetation/dynamic/create-vegetation-layer-adjust-shape.png)

7. 选择 **BasicCoverage** 并在 **Shape Reference** 组件中，单击 {{< icon "picker.svg" >}} **实体选择器** 图标，然后选择 **TestBox** 实体。

8. 点击 **Add Required Component** 并选择 **Vegetation Asset List**。

    **Vegetation Asset List** 组件定义要种植的植物。在这里您可以指定植被资产。

    ![On the Vegetation Layer Spawner, click Add Component and select Vegetation Asset List.](/images/user-guide/vegetation/dynamic/create-vegetation-layer-asset-list.png)

9. 在 **Vegetation Asset List** 组件中，选择 **Instance Spawner** 类型为 Prefab，然后在 **Prefab Asset** 旁，点击 {{< icon "file-folder.svg" >}} **文件夹** 图标浏览找到你的资产。

    ![Click the Folder icon to select a Prefab Asset.](/images/user-guide/vegetation/dynamic/create-vegetation-layer-browse.png)

10. 在**Pick a Prefab**窗口中，在搜索框中输入 **grass**，筛选可用的Prefab。在结果中选择 `DryGrassLarge.prefab` 资产。

     ![Use search to find a prefab asset.](/images/user-guide/vegetation/dynamic/create-vegetation-layer-asset-grass.png)

    你应该有一块均匀的草地，草地呈网格状。

     ![After selecting your grass asset, you have a grassy field with a grid-like appearance.](/images/user-guide/vegetation/dynamic/create-vegetation-layer-grass-grid.png)
