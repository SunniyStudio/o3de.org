---
linkTitle: Actor LODs
title: 使用Actor LODs优化性能
description: 在Open 3D Engine (O3DE)中为Actor创建细节级别 (LOD)，以优化游戏性能。
toc: true
## weight: TBD on reorg of animation topics 
---

您可以使用细节级别 ([LODs](/docs/user-guide/appendix/glossary#lod)) 来优化**Open 3D Engine (O3DE)**的性能。当Actor远离摄像机时，Actor LOD 会逐步简化网格、骨骼、材质和纹理。

O3DE 中的Actor最多可以有六个 LOD（包括基本网格）。LOD0 是Actor最靠近摄像机时显示的基本网格。它具有最高保真度的网格和纹理，以及最复杂的骨架和材质。LOD5 的保真度最低，在Actor离摄像机最远时显示。每个连续 LOD 的顶点数量通常比前一级少 50%，骨架可能会通过移除叶骨进行优化，材质和纹理可能会合并和简化，动画采样率可能会降低。

## 特性

O3DE 为 Actor LOD 提供以下支持：

* [Mesh LODs](/docs/user-guide/assets/scene-settings/meshes-tab/#level-of-detail-lod) 可以在[Scene Settings](/docs/user-guide/assets/scene-settings/scene-settings/)中手动分配，也可以通过命名约定自动分配，或根据源资产中的 LOD 组自动处理。
* [Skeleton LODs](/docs/user-guide/assets/scene-settings/actors-tab/#skeleton-lod) 可在 “场景设置 ”中手动分配，或根据源资产中的 LOD 组自动处理，或根据皮肤重量数据自动生成。
    {{< note >}}
  骨架 LOD 不能按命名规则自动分配。
    {{< /note >}}
* 材质和纹理 LOD 是通过将简化的材质和纹理分配给相应的网格 LOD 来创建的。
* [Simple LOD Distance](/docs/user-guide/components/reference/animation/simple-lod-distance) 组件支持设置每个 LOD 的最大视距和最大动画采样率。

## 指导原则

在 O3DE 中，可以在几种不同的情况下使用 LOD。以下指导原则对于Actor LOD 尤为重要。有关创建Actor资产的其他信息，请参阅[场景源资产最佳实践](/docs/user-guide/assets/scene-settings/source-asset-best-practices/#actors)主题中的Actor资产。

### LOD 网格

* 每个 LOD 网格必须绑定到一个骨架上。
* 不要求 LOD 的网格数量相同。每个连续的 LOD 都可以合并网格。一个Actor在 LOD 0 时可能有十几个网格，通过合并和简化，在 LOD 5 时可能只有一个网格。
* 对网格的命名没有要求，但建议使用易于确定网格预期 LOD 的名称。
* 要将网格自动分配给 LOD，请使用适当的后缀（`_lod1`、`_lod2` 等），或在源场景文件中使用 LodGroups。这两种方法将在本主题的以下章节中进行说明。

### 骨架

* LOD 可以使用相同的骨架，也可以为每个 LOD 优化骨架。
* 骨架 LOD 唯一允许的优化是移除 *leaf* 骨骼，即位于骨架链末端的骨骼。骨骼层次结构中的骨骼不能被移除。例如，一个Actor的指骨可能会减少，手指也会减少，因为骨架会根据每个连续的 LOD 进行优化。要移除叶状骨骼进行优化，应从最外层的叶状骨骼开始，然后沿着骨架层次向上移除。
* 每个骨骼 LOD 必须具有相同的骨骼名称和层次结构，不包括为优化而移除的叶状骨骼。
* 如果选择在数字内容创建 (DCC) 应用程序中创建骨架 LOD，则必须通过源文件中的 LodGroups 将其分配给 LOD，或在 “场景设置 ”中手动分配 LOD。

### 材质和纹理

* LOD 可以共享材质和纹理，也可以拥有独特的材质和纹理。
* 材质和纹理分配给每个 LOD 的各个网格。

## 使用 “场景设置 ”手动设置 LOD

网格和骨骼 LOD 可以在 “场景设置 ”中手动配置。不过，手动设置对于复杂的Actor来说可能比较困难，因此建议使用 LodGroups 自动生成Actor LOD。要手动设置Actor LOD，请执行以下操作：

### 手动设置网格 LOD

1. 在 “场景设置 ”中，选择 **Meshes** 选项卡。

2. 在 **Mesh Group** 中，点击 {{< icon "browse-edit-select-files.svg" >}} 节点选择按钮，打开场景节点列表。

3. 在节点列表中，只选择应包含在最高级网格 LOD 中的网格节点。选择 **Select** 完成选择并关闭列表。

4. 点击 **Add Modifier** 并从修改器列表选择 **Level of Detail** 。

5. 在Level of Detail修改器中，点击 {{< icon "add.svg" >}} 来添加网格 LOD。根据需要最多可添加五个网格 LOD。

6. 对于每个网格LOD，点击 {{< icon "browse-edit-select-files.svg" >}} 节点选择按钮打开场景节点列表，选择要包含在 LOD 中的网格节点。

### 手动设置骨架 LOD


7. 在 “场景设置 ”中，选择 **Actors** 选项卡。

8. 点击 **Add Modifier** 并从修改器列表中选择 **Skeleton LOD** 。

9. 在 Skeleton LOD 修改器中，点击 {{< icon "add.svg" >}} 来添加骨架 LOD。根据需要添加任意数量的骨架 LOD。

10. 对于每个骨架 LOD，选择{{< icon "browse-edit-select-files.svg" >}} 节点选择按钮打开场景节点列表，选择要包含在 LOD 中的骨骼节点。

{{< note >}}
使用Actor LOD 时，不需要构建或手动设置骨骼 LOD。在大多数情况下，Actor可以在每个 LOD 中使用相同的基础骨骼，也可以根据皮肤重量信息为每个 LOD 自动生成优化骨骼。只有在 LodGroups 或自动骨骼 LOD 生成效果不理想的情况下，才建议手动分配骨骼 LOD。
{{< /note >}}

将所有网格和骨骼分配到相应的 LOD 后，在 “场景设置 ”窗口底部选择 **Save** 保存偏好设置，并自动处理Actor的 LOD。

## 使用 LodGroups 自动设置 LOD

LodGroup 是在数字内容创建 (DCC) 应用程序（如 Maya 或 Blender）中创建的，方法是将每个 LOD 的网格和骨架按组的层次结构进行排列。LodGroups 可自动处理，无需在 “场景设置 ”中进行额外配置。

LodGroup 节点指定其子群组各包含一个 LOD。Maya 有一个特殊的 LOD 组工具，可以从编辑菜单中访问该工具来创建 LodGroup。在其他应用程序中，例如[Blender](https://www.blender.org/)，您可以通过以下操作创建 LodGroup：

1. 在 Blender 中创建一个空（变换）节点。将节点命名为 `LodGroup`。

2. LodGroup 节点使用一个特殊的字符串属性来通知资产处理器其子节点包含 LOD。为 LodGroup 节点添加一个自定义属性，属性如下：
    * Type - `String`
    * Name - `fbx_type`
    * Value - `LodGroup`

3. 作为 LodGroup 节点的子节点，最多再创建六个空节点（每个所需 LOD 一个）。

4. 相应地命名节点，例如 `LOD0`、`LOD1` 等。

5. 在每个 LOD 节点下放置适当的 LOD 网格和骨架。例如，分辨率最高的网格和骨架放在 LOD0 下。保真度次高的网格和骨骼放在 LOD1 下，依此类推。

6. 以 FBX 或 GLTF 格式导出源场景资产。

7. 将源场景资产放在项目目录结构中的某个位置，如`Assets`目录。资产处理器会自动检测并处理Actor及其 LOD。

下图演示了在 Blender 中设置 LodGroup 的示例。请注意应用于 LodGroup 节点的自定义字符串属性。有三个子 LOD 节点，每个节点都包含一个 ARM 网格的版本。骨架 (ArmatureHigh) 用于所有 LOD，因此骨架不包含在任何特定的 LOD 中。要为 LodGroup 创建骨架 LOD，请参考之前的[创建 LOD 骨架](#skeletons)指南。骨架 LOD 会与相应的网格一起添加到每个 LodGroup 中。

![An example LodGroup setup in Blender.](/images/user-guide/visualization/animation/lodgroup-setup-blender.png)

## 自动分配网格 LOD

网格 LOD 可通过命名规则自动分配。

1. 创建一个具有多个 LOD 网格的Actor。每个 LOD 都可以根据需要拥有尽可能多的网格。例如，LOD0 可能有几十个高分辨率网格。通过一系列合并和优化，LOD5 可能只有一个网格。

2. 为 LOD 中的每个网格添加相应的 LOD 后缀（`_lod0`、`_lod1`、`_lod2` 等）。

3. 以 FBX 或 GLTF 格式导出源场景资产。

4. 将源场景资产放在项目目录结构中的某个位置，如Assets目录。

5. 打开资产场景设置。

6. 确认已添加适当数量的网格 LOD，并为每个 LOD 分配了正确的网格。

7. 选择**Save**，保存您的选项并自动处理Actor。

有关详细信息，请参阅场景设置功能文档中的[Level or Detail (LOD)](/docs/user-guide/assets/scene-settings/meshes-tab#level-of-detail-lod) 修改器主题。

## 自动优化骨架 LOD

骨架 LOD 可使用[骨架优化](/docs/user-guide/assets/scene-settings/actors-tab#skeleton-optimization) 修改器自动创建，该修改器应用于 “场景设置 ”中的**Actor group**。骨架优化修改器通过移除网格上没有蒙皮的叶骨来创建骨架 LOD。这个过程需要特别注意每个网格 LOD 的皮肤权重。

1. 在 DCC 应用程序中，为Actor创建一系列网格 LOD。

2. 在为每个网格 LOD 绘制皮肤时，请密切注意哪些骨骼会影响网格顶点。您必须确保在较低级别的 LOD 中，影响网格的叶状骨骼越来越少。举例如下

   1. 在 LOD0 时，Actor网格有一整套独立的手指，每个手指有 3 根骨头。

   2. 在 LOD1 时，中指、无名指和小指的网格被融合在一起。它们的皮肤与中指骨骼相连。无名指和小指的骨骼不会影响任何网格顶点，可以通过骨骼优化自动移除。

   3. 在 LOD2 时，所有的手指网格都融合在一起，并与食指骨骼相连。中指、无名指和小指的骨骼不会影响任何网格顶点，可以通过骨骼优化自动移除。

   4. 在 LOD3 时，手部模型是松散闭合的拳头，并与手骨相连。手指和拇指骨骼不影响网格顶点，可通过骨架优化自动移除。

3. 以 FBX 或 GLTF 格式导出源场景资产。

4. 将源场景资产放在项目目录结构中的某个位置，如`Assets`目录。

5. 打开资产场景设置。

6. 选择 “**Actors**”选项卡。

7. 在 **Actor group** 中选择 **Add Modifier**，并从修改器列表中选择 **Skeleton optimization** 。

8. 确保已启用 **Auto Skeleton LOD** 。

9. 选择 **Save** 保存选项并自动处理Actor。

## 设置Actor LOD 距离

Actor的 LOD 距离是通过 [Simple LOD Distance](/docs/user-guide/components/reference/animation/simple-lod-distance) 组件设置的。要设置Actor LOD 距离，请执行以下操作：

1. 确保您的Actor资产有通过前面章节的流程创建的网格 LOD（如果需要，还有骨架 LOD）。

2. 将 “资产浏览器 ”中的 “Actor资产 ”拖到视口中，在关卡中实例化Actor资产。

3. 向Actor实体添加Simple LOD distance组件。

4. Simple LOD distance组件会自动填充**LOD distance (Max)**列表，为Actor中的每个 LOD 添加一个条目。每个条目代表以世界单位（米）表示的与摄像机的距离。根据需要设置每个 LOD 的距离。

    ![Setting up the Simple LOD distance component.](/images/user-guide/visualization/animation/simple-lod-distance-setup.png)

    {{< note >}}
    您还可以通过启用**Enable LOD anim graph sampling**属性并为每个Actor LOD 设置动画采样率（每秒采样）来创建动画 LOD。较低的数值会减少采样次数，从而提高较低Actor LOD 的性能。
    {{< /note >}}

5. 在视口中，将焦点对准Actor，使用鼠标滚轮放大或缩小。显示的Actor LOD 会根据Actor与摄像机的距离发生变化。
