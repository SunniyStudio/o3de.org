---
linkTitle: 场景设置
title: 场景设置
description: 学习浏览场景设置界面，自定义处理源资产的方式。
weight: 200
toc: true
---

通过**场景设置**，您可以指定从三维场景源资产中处理哪些数据，并添加修改器以自定义如何为**Open 3D Engine (O3DE)**处理数据。场景设置选项卡提供了**Meshes**, **Actors**, **Motions**, 和 **PhysX** 碰撞体的选项。场景设置界面根据所选源资产文件中检测到的数据类型显示选项卡。

{{< image-width "/images/user-guide/assets/scene-settings/scene-settings-interface.png" "650" "The Scene Settings user interface." >}}

## 源资产和产品资产

在 “场景设置 ”中，您可以指定任意数量的网格、Actor、PhysX 碰撞体和动作，以便从单个源资产中进行处理。源资产通常会生成多个产品资产。例如，一个网格组可以生成一个`.azmodel`产品资产、多个`.azlod`产品资产以及用于`.azmodel`和`.azlod`产品资产的多个 `.azbuffer`产品资产。根据所选的修改器和选项，一个静态网格组可以生成几十个产品资产。

您可以通过在**资产浏览器**中展开源资产的资产列表来查看产品资产。在下图中，`Lucy_High.fbx`是源资产，高亮框内的资产是`Lucy_High.fbx`的所有产品资产

{{< image-width "/images/user-guide/assets/scene-settings/asset-browser-product-assets.png" "650" "An expanded list of product assets in Asset Browser." >}}

## 使用场景设置

场景设置一次只能对一个源资产进行操作。要打开场景设置，请按以下步骤操作：

1. 在**O3DE 编辑器**的 “资产浏览器 ”中，选择要修改的 3D 场景源资产。您可以使用资产浏览器顶部的搜索栏过滤列表。

1. **右键单击**三维场景源资产，然后选择 **Edit Settings...**。

{{< image-width "/images/user-guide/assets/scene-settings/asset-browser-edit.png" "650" "Open Scene Settings from a selected 3D scene source asset in Asset Browser." >}}
<!-- Don't remove the newline below. -->
\
{{< note >}}
O3DE 集成了  [Open Asset Import Library](https://github.com/assimp/assimp) 来解析三维场景源资产，默认支持 `.fbx`、`.stl` 和 `.gltf`。如果想尝试 Open Asset Import Library 支持的其他[场景格式](https://github.com/assimp/assimp/blob/master/doc/Fileformats.md) ，可以编辑`o3de/Registry/sceneassetimporter.setreg` 设置文件，并在`"SupportedFileTypeExtensions":`列表中添加格式扩展名。添加新格式扩展名时，**编辑设置...**选项会出现在该源资产格式的上下文菜单中。
{{< /note >}}

## `.assetinfo` sidecar 副卡文件

在 “场景设置 ”界面右下方选择**Update**时，您创建的资产组、添加的修改器以及在 “场景设置 ”中设置的选项都会保存到`.assetinfo`侧卡文件中。资产处理器会将侧卡文件识别为源资产的源依赖，并在创建或更新`.assetinfo`文件时自动处理源资产。

例如，您可以创建一个 3D 场景源资产，其中包含不同类型和大小的植物，并带有蒙皮网格和 LOD，然后使用 “场景设置 ”为源资产中的每种植物指定网格组、角色、动作和 PhysX 碰撞体。处理所有植物所需的信息都包含在一个`.assetinfo`文件中。需要注意的是，如果您选择从单一源资产中处理多个产品资产，对源资产或其依赖关系的任何方面进行更改，都将自动重新处理源资产中包含的所有内容。

`.assetinfo` sidecar 文件采用 JSON 格式，人类可读，可通过 Python 脚本等自动化流程轻松生成和修改。

## Scene Settings 标签页

场景设置中有四个选项卡： **Meshes**, **Actors**, **Motions**, 和 **PhysX**。每个选项卡都提供了专门用于处理该类型数据的修改器和选项。特定源资产显示的选项卡取决于源资产的内容。下表提供了每个选项卡的主题链接，以及在 “场景设置 ”界面中显示该选项卡所需的源资产数据。

| 标签页 | 说明 | 要求 |
| - | - | - |
| [Meshes](meshes-tab) | 创建网格组并修改网格的处理任务设置。网格处理会生成 `.azmodel`、`.azlod` 和 `.azbuffer` 产品资产。 | 源资产必须至少包含一个网格。 |
| [Actors](actors-tab) | 创建Actor组并修改演员的流程作业设置。Actor是任何源资产（不一定是Actor）。Actor处理会生成“.actor ”和“.skinmeta ”产品资产。 | 源资产必须至少包含一个骨骼和一个蒙皮网格。 |
| [Motions](motions-tab) | 为动作（动画）创建和修改处理任务设置。动作处理会生成 `.motion` 产品资产。 | 源资产必须包含带有关键帧动画的骨架。 |
| [PhysX](physx-tab) | 创建 PhysX 网格组并修改 PhysX 碰撞体的处理任务设置。PhysX 处理会生成 `.pxmesh` 产品资产。 | 源资产必须至少包含一个网格。项目必须启用 **PhysX Gem** 才能处理和使用 PhysX 碰撞体。 |

