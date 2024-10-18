---
linktitle: 材质系统概述
title: "材质系统概述"
description: "材质包含的数据可控制模型表面在 3D 环境中的显示效果。"
weight: 100
---

**材质**包含控制模型表面在 3D 环境中显示方式的数据。所有材质都有一个**材质类型**，它将具有相同属性和着色器代码的材质进行分类，例如硬质表面、布料或皮肤。材质可直接从材质类型或其他*父*材质继承属性。每个`.material`文件只需存储与其父材质不同的属性值。

<!-- SVG file edited using https://app.diagrams.net/ -->
![Material Files Diagram](/images/atom-guide/materials/material-file-diagram.svg)

材质和材质类型作为数据项存储在 JSON 文件中。Atom 材质生成器会将数据文件转换为材质资产。然后，应用程序会使用这些材质资产，并将其应用到网格表面。

## 材质
材质**是引用材质类型并定义材质属性值的数据项。材质必须引用材质类型，该类型定义了材质的工作方式和可用属性。另外，材质也可以引用另一个**父**材质，并继承其属性值。材质继承树的深度可根据需要而定。

材质文件（`*.material`）采用 JSON 格式。它们可以使用**材质编辑器**进行编辑，也可以直接编写。文件采用简单的 JSON 格式，可通过材质编辑器之外的脚本自动生成或机器生成。

更多信息，请参阅 [材质文件规范](/docs/atom-guide/look-dev/materials/material-file-spec/)。

### 材质文件示例

在下面的示例中，材质使用了 **StandardPBR** 材质类型，并包含`baseColor`、`roughness`和`normal`属性。

```json
{
    "materialType": "Materials/Types/StandardPBR.materialtype",
    "materialTypeVersion": 6,
    "propertyValues": {
        "baseColor.textureMap": "Textures/Default/default_basecolor.tif",
        "roughness.textureMap": "Textures/Default/default_roughness.tif",
        "normal.factor": 0.8,
        "normal.textureMap": "Textures/Default/default_normal.tif"
    }
}
```

{{< image-width "/images/atom-guide/materials/simple-example.jpg" "720" "Example Material Screenshot" >}}

{{< note >}}
上面示例中的纹理可以在以下文件中找到`Gems/Atom/Feature/Common/Assets/Textures/Default`。
{{< /note >}}

## 材质类型
**材质类型**是一个数据项，其中包含描述如何渲染网格所需的所有内容：
- 一套**材质属性**定义
- 着色器链接
- 描述如何使用材质属性的脚本
- 材质编辑器在显示材质属性时使用的元数据

材质类型文件 (`*.materialtype`)采用 JSON 格式，可直接编写。该文件将其他几个文件链接在一起，形成完整的材质类型定义，如`*.shader`、`*.azsl`和`*.lua`文件。

更多信息，请参阅 [材质类型文件规范](./material-type-file-spec)。  

{{< note >}}
基于节点图的材质类型创建工具正在开发中。在此之前，材质类型可直接编写。
{{< /note >}}

## Atom PBR 材质类型
**基于物理的渲染（Physically based rendering，PBR）** 是一种渲染技术，可模拟材质与光线之间逼真的相互作用。Atom 基于使用金属和粗糙度属性的行业标准工作流程，提供多种核心材质类型。Atom 的材质系统还能通过替代材质类型渲染非照片逼真技术和特效。用户可以根据自己的需要创建自己的材质类型。

有关可用 PBR 材质类型的更多信息，请参阅 [基于物理的渲染（PBR)](/docs/atom-guide/look-dev/materials/pbr/)<!-- and [Working with StandardPBR materials](./material-build-pipeline)DRAFT TOPIC-->。 

## 材质资产处理

资产处理器会加载材质和材质类型源文件，并在缓存中保存相应的产品文件。运行时会加载这些文件并用于渲染模型。有关此主题的背景信息，请参阅 [资产处理](/docs/user-guide/assets/pipeline/asset-processing/)。

一个材质类型源文件（`*.materialtype`）会在缓存中生成一个材质类型资产（`*.azmaterialtype`）。它包含属性布局信息、要使用的着色器列表以及可能用于特殊处理的函数。

材质源文件（`*.material`）会在缓存中生成一个材质资产（`*.azmaterial`）。每个 “材质资产 ”都会引用一个 “材质类型资产”。它包含材质属性值的继承树，并压缩成一个列表。材质资产可被模型资产引用，和/或使用**Open 3D Engine (O3DE)编辑器**中的材质组件分配给模型。

其他源文件也可以生成材质资产。例如，*.fbx*文件通常用于存储使用三维建模软件创建的模型，也可能包含材质数据。O3DE 的**场景生成器**将处理这些文件，生成模型资产（`.azmodel`）和材质资产（`.azmaterial`）。请注意，这些 “材质资产 ”在源文件夹中没有相应的.材质文件，但仍可被 “Mesh组件 ”和/或 “Material组件 ”用于渲染。

Material组件可在运行时创建任意数量的材质实例，供渲染器使用。使用Material组件，可以更改材质实例的属性值，而不会影响同一材质的其他实例。与材质资产不同，材质实例只存在于内存中，而不在磁盘上。

<!-- SVG file edited using https://app.diagrams.net/ -->

![Material Assets Diagram](/images/atom-guide/materials/material-asset-diagram.svg)
