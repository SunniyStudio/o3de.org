---
linkTitle: 组件参考文档
title: 编写 O3DE 组件参考文档
description: 使用组件参考文档模板为 Open 3D Engine （O3DE） 编写组件参考文档的准则。
weight: 200
toc: true
---

本主题将指导您完成 **Open 3D Engine （O3DE）** 组件参考文档的编写过程。

工作流程：

1. [确定您的文档是否属于 O3DE Component Reference。](#does-my-document-belong-in-the-o3de-component-reference) 

2. [设置存储库并下载组件参考文档模板。](#setting-up-repository-and-downloading-component-template)

3. [遵循编写指南来帮助您填写模板。](#writing-a-component-reference-document)

4. [确定元件参考文档在 o3de.org 文件夹中的所属位置。](#where-does-the-component-reference-document-live)

5. [将文档中的图像存储在正确的文件夹中。](#storing-image-files)

6. [更新 O3DE 组件引用以列出您的文档。](#updating-the-o3de-component-reference)

7. [将您的文档提交到 O3DE 文档。](#submit-your-component-reference-document)

所有文档必须遵守 O3DE [风格指南](/docs/contributing/to-docs/style-guide/) 和 [术语](/docs/contributing/to-docs/terminology/) 中概述的标准。如果您需要任何帮助，请在 [Discord](https://discord.com/invite/o3de) 上联系文档和社区特别兴趣小组 （#sig-docs-community）。


## 我的文档是否属于 O3DE Component Reference？

首先，确定您的文档是否属于 O3DE 用户指南的 [组件参考](/docs/user-guide/components/reference/) 部分。组件参考文档的目的正是，用户在 O3DE Editor 中使用组件时可以快速引用的页面。

补充信息（例如跨不同工作流使用组件）属于其他文档。该文档可以组织在用户指南的适当部分中，例如[交互性和模拟](/docs/user-guide/interactivity/), [脚本编程](/docs/user-guide/scripting/), [网络](/docs/user-guide/networking/), 或 [可视化](/docs/user-guide/visualization/) 章节。

## 设置仓库并下载组件模板

要为 O3DE 文档做出贡献，您必须完成 [设置本地 o3de.org 存储库](../get-started#setting-up-a-local-o3deorg-repo)的您可以在编写组件参考文档之前或之后执行此操作。但是，您必须进行此设置以将文档和图像放在正确的文件夹中。

在新选项卡中，导航到 o3de/sig-docs-community 存储库中的文件 [组件参考模板](https://raw.githubusercontent.com/o3de/sig-docs-community/main/doc-templates/component-reference.md) 并将其保存到本地计算机。在文本编辑器中打开模板，并在填写模板时参考此页面。


## 编写组件参考文档

以下部分将指导您如何填写组件引用模板的每个部分。有些部分是可选的，您的组件可能不需要，因此您可以根据需要删除这些部分。

### Metadata

与 O3DE 文档中的所有 Markdown (`.md`) 文件一样，您的组件参考文档必须在文件开头包含元数据。 请参考 [Metdata](/docs/contributing/to-docs/style-guide/metadata/) 来帮助你填写本节。

{{< note >}}
`weight` 参数是可选的。如果省略，则默认行为是按字母顺序对标题进行排序。
{{< /note >}}

See `component-reference.md`:
```yaml
---
linkTitle: \[Component Name\]
title: \[Component Name\] Component
description: The [component name], which does [function], is provided by the [Gem name] in Open 3D Engine (O3DE). # Example
weight: 200 # (Optional) Example
toc: true
---
```


### 简介

介绍组件及其功能。

**规格**：

* 从用户的角度描述组件，而不是设计实现。

* 您可以总结作为功能核心的技术信息，但不需要包括您做出该设计决策的原因。

* 如果组件是 Level 组件，或者是应用于 Level 实体的组件，请明确提及。

* 简介是页面上的第一段，紧跟在元数据之后。它可以是一到几个段落，具体取决于组件的复杂程度。

查看 `component-reference.md`:
```md

<!-- Introduction - Describe the component and what it does. -->

```


### Provider

编写提供此组件的 Gem 的名称，并链接到相应的 [Gem 参考文档](/docs/user-guide/gems/reference/)。

**规格**：

* 将 “Gem” 大写。

* 按照 [链接](/docs/contributing/to-docs/style-guide/format/#links)指南设置链接格式。


See `component-reference.md`:
```md
## Provider

[Gem name](_/docs/user-guide/gems/reference/<path-to-gem-docs>_)

```

### 依赖项（可选）

如果您的组件依赖于其他组件才能工作，请列出相关组件的名称并将其链接到相应的组件参考文档。

**规格**：

* 如有必要，请解释此组件与其依赖项的关系。

* 按照 [链接](/docs/contributing/to-docs/style-guide/format/#links) 指南设置链接格式。

**示例**：

* [Diffuse 组件](/docs/user-guide/components/reference/atom/diffuse-probe-grid/): 具有一个依赖项的简单示例。

* [Light 组件](/docs/user-guide/components/reference/atom/light/#dependencies): Light （光源） 组件可以依赖于不同的 Shape （形状） 组件。此示例描述了不同的依赖关系及其条件。

查看 `component-reference.md`:
```md
## Dependencies

<!-- (Optional) List the component's dependencies -->

```

### 用例（可选）

编写用户可能希望使用此组件的用例。本部分有助于阐明何时使用此组件，尤其是在可能存在混淆的情况下。

**规格**：

* 如果您提供多个使用案例，请确保它们彼此唯一。

* 一个有效的用例提供了情境背景，并清楚地说明了组件在情境中的角色。

* 写出完整的句子并详细说明任何技术术语。

**示例**：

* [PhysX Shape Collider 组件](/docs/user-guide/components/reference/physx/shape-collider/#use-cases) - 阐明了何时使用 PhysX Shape Collider （PhysX 形状碰撞器） 组件与 PhysX Primitive Collider （PhysX 基元碰撞器） 组件。

查看 `component-reference.md`:
```md
## Use cases

<!-- (Optional) Write the use cases in which a user may want to use this component. -->

```


### 限制（可选）

写下这个组件可能具有的限制。

**规格**：

* 如果可能，说明用户如何解决限制。

* 写出完整的句子并详细说明任何技术术语。

* 不包括任何“未来工作”。更好的地方是该功能的路线图，可以在相应的 [SIG 存储库](https://www.o3de.org/contribute/#join-a-special-interest-group)。 

**示例**:

* [PhysX Shape Collider 组件](/docs/user-guide/components/reference/physx/shape-collider/#limitations)
* [HDRi Skybox 组件](/docs/user-guide/components/reference/atom/hdri-skybox/)


查看 `component-reference.md`:
```md

## 限制

<!-- (Optional) Write the limitations of this component. Don't include any "future work". -->

```

### 属性

属性部分按顺序包含以下内容：

1. 组件卡的图像
2. 属性表
3. （可选）附加属性表


#### 组件卡的图片

您必须包含此组件的组件卡的图像。在 Editor 中，当选择具有此组件的实体时，可以在 **Entity Inspector （实体检查器） 面板中看到组件卡。

**规格**：

* 使用屏幕截图工具捕获图像。

* 图像必须以默认状态显示组件卡。

* 裁剪图像以仅显示此组件的组件卡。

* [存储图像文件](#storing-image-files) 添加到正确的文件夹中。 

* 遵循 [将媒体提交到 Open 3D Engine 文档中的准则](/docs/contributing/to-docs/style-guide/media/). 


#### 创建一个或多个属性表

根据属性在组件卡中的排列方式，您可以有一个或多个属性表。

*简单属性集* 是指在组件卡的内容层次结构的第一级显示其所有属性的组件卡。在这种情况下，您可以查看所有属性，而无需打开下拉列表或以特定方式配置一组属性。

另一方面，*复杂的属性集* 不会立即显示所有可用的属性。下拉列表下的属性组中可能隐藏了属性，或者仅当以特定方式配置其他属性时才会显示条件属性。

| 示例：简单的属性集 |示例：复杂的属性集| 
| - | - |
| ![Box Shape component properties](/images/user-guide/components/reference/shape/box-shape-component-ui-01.png) | ![PhysX Primitive Collider component interface.](/images/user-guide/components/reference/physx/physx-collider-ui-01.png) |

{{< tabs >}} 
{{% tab name="Simple set of properties" %}}

对于具有一组简单属性的组件卡，您只需要一个属性表。

**示例**:

* [Box Shape 组件](/docs/user-guide/components/reference/shape/box-shape/): 一个简单的示例，其中所有属性都显示在组件卡内容层次结构的第一级。

* [Bloom 组件](/docs/user-guide/components/reference/atom/bloom/): 即使 Bloom 组件的属性包含下拉列表，也只需要一个属性表。这是因为下拉列表默认打开，并用于包含值数组。另一个例外是 **Overrides** 下拉列表，该列表未在用户的主工作流程中使用。因此，对 **Overrides-Enabled Override** 属性的描述就足够了。

查看 `component-reference.md`:
```md

## 属性

<!-- This example is for a simple set of properties. 
If you use this section, then delete the other Properties section. -->

![[component name] interface](/images/<path-to-image>)

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
|   |   |   |   |
|   |   |   |   |
|   |   |   |   |

```

{{% /tab %}}
{{% tab name="Complex set of properties" %}}  

对于具有一组复杂属性的组件卡，您可能需要多个属性表：基本属性，每个属性组或配置一个属性表。第一个属性表称为“基本属性”，包含在组件卡的内容层次结构的第一级显示的所有属性。您可以在组件卡中的属性组名称或配置类型之后命名后续属性表。

另一种选择是将属性表分成文档上的 [标签页](/docs/contributing/to-docs/style-guide/format/#tabs) /format/#tabs）。当内容位于选项卡中时，只有活动选项卡显示内容，而其余选项卡则处于隐藏状态。由于此行为，如果属性集为用户可能会选择也可能不会选择的不同工作流定义配置，则建议使用选项卡。

**示例**:
* [Light 组件](/docs/user-guide/components/reference/atom/light/): 包含多个与 **Light type** 属性没有 1-1 关系的配置。某些属性组可用于多种光源类型，因此它们记录在不同的属性表中。每个属性表都指定了哪些光源类型支持这些属性。

* [PhysX Primitive Collider 组件](/docs/user-guide/components/reference/physx/collider/): 包含多个配置，这些配置取决于 **Shape** 属性。每个配置都会影响可用的属性，因此它们记录在不同的属性表中，并分为多个选项卡。

* [Camera Rig 组件](/docs/user-guide/components/reference/camera/camera-rig/): 包含可添加的属性组，每个属性组中有多个工作流。因此，每个属性组都记录在单独的属性表中，并且每个工作流都进一步分为选项卡。 

查看 `component-reference.md`:
```md
## 属性

<!-- This example is for a complex set of properties. 
If you use this section, then delete the other Properties section. -->

![<component name> interface](/images/<path-to-image>)


### Base 属性

| Property | Description | Value | Default |
| - | - | - | - |
|   |   |   |   |
|   |   |   |   |
|   |   |   |   |


### <Property group> 属性

![[property group] interface](/images/<path-to-image>)

| Property | Description | Value | Default |
| - | - | - | - |
|   |   |   |   |
|   |   |   |   |
|   |   |   |   |


### <Configuration> 属性

{{</* tabs */>}} 
{{%/* tab name="[configuration 1] properties" */%}}

![[configuration 1] interface](/images/<path-to-image>)

| Property | Description | Value | Default |
| - | - | - | - |
|   |   |   |   |
|   |   |   |   |
|   |   |   |   |

{{%/* /tab */%}}
{{%/* tab name="[configuration 2] properties" */%}}  

![[configuration 2] interface](/images/<path-to-image>)

| Property | Description | Value | Default |
| - | - | - | - |
|   |   |   |   |
|   |   |   |   |
|   |   |   |   |

{{%/* /tab */%}}
{{</* /tabs */>}}

```

{{% /tab %}}
{{< /tabs  >}}



#### 属性表的内容

在属性表中，按照属性在组件卡中的显示顺序将属性排序到行中。然后，为每个属性填写以下列：

* **Property**: 属性的名称。这必须是组件卡中显示的确切名称，格式为粗体。
* **Description**: 描述属性的用途及其配置的内容。简洁明了，并解释任何技术术语。 
* **Value**: 定义此属性的值的类型。如果是数值，请包括接受值的范围。
* **Default**: The property's default value. 

#### 值和默认值的格式设置

| 值 | 默认值 | 注释 |
| --- | --- | --- |
| Int: 0 - 10 | 0 |  |
| Float: 0.0 to Infinity | `100.0` | 如果值范围包含单词，例如 “-infinity” 或 “infinity”，请在范围之间使用 “to”。 |
| Float: 0.0 - 10000.0 | `0.5` | 如果值的范围只是数值，请在范围之间使用连字符 （-）。 |
| Float: 0.0 - 1.0 |Kernel Size: <br><ul><li>0: 0.04</li><li>1: 0.08</li><li>2: 0.16</li><li>3: 0.32</li><li>4: 0.64</li></ul> | 描述内核大小的浮点数组。 |
| String | None |  |
| Boolean | Enabled | 布尔值的默认值可以是 “Enabled” 或 “Disabled”。 |
| Vector3: -Infinity to Infinity |X: 0.0, Y: 0.0, Z: 0.0 |  |
| Vector3: <br><ul><li> X: 0.0 - 1.0 </li><li>Y: -Infinity to Infinity </li><li>Z: -5.0 - 5.0 </li></ul> | X: 0.0, Y: 0.0, Z: 0.0 | 每个参数具有不同默认值的 Vector3。 |
| Entity Id | None | 对关卡中实体的引用。 |
| 在项目的 [碰撞图层](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-collision-layers/) 中定义的任何碰撞图层。 | Default | 如果值是对象，请提供上下文，以便用户可以识别对象。 |
| `.physmaterial` 库产品资源。 | 全局项目`.physmaterial`库。 | |
| 产品资产 `.pxmesh` PhysX mesh. | None |  |
| `PhysicsAsset`, `Sphere`, `Box`, `Capsule` | `PhysicsAsset` | 一组枚举值。  |


### 组件功能部分（可选）

您可以编写其他部分来解释有关组件的特定功能，例如在视区或其他系统编辑器中编辑它们、它们的不同模式以及其他值得注意的规范或概念。

**规格：**

* 使用简洁而适当的标题，以便用户可以了解此部分的预期内容。

* 包含为用户提供服务并帮助他们使用组件的信息。

* 避免包含对用户无用的设计决策或实现细节。

* 根据需要对每个附加主题重复此部分。

**示例**：

* [PhysX Primitive Collider 组件](/docs/user-guide/components/reference/physx/collider#collider-component-mode): 包含有关在视区中编辑碰撞体和将碰撞体用作触发器的部分。

* [PhysX Shape Collider 组件](/docs/user-guide/components/reference/physx/shape-collider#complex-polygon-prism-shapes): 包含一个部分，该部分解释了使用复杂多边形棱柱形状时的复杂性。

* [Light 组件](/docs/user-guide/components/reference/atom/light#light-types): 包含描述不同光源类型及其功能的部分。此部分位于页面的开头附近，因为它包含了解组件属性所需的信息。

查看 `component-reference.md`:
```md

## <Component feature>

<!-- (Optional) Explain specific features of your component. Repeat this section for each additional topic, as needed. -->

```

### 请求总线 / 通知总线（可选）

request 和 notification bus 部分包含用户可以在其项目的 logic 和 scripts 中调用的函数。

在 request 或 notification bus 表中，列出行中的函数并填写以下列：

* **Request/Notification Name**: 函数的名称，格式为 'code'。
* **Description**: 描述函数的作用。简洁明了，并解释任何技术术语。 
* **Parameter**: 此函数接受的参数（如果有）。如果没有，则写入 “None”。
* **Return**: 此函数返回的内容。如果它不返回任何内容，则写入 “None”。
* **Scriptable**: 此函数是否可编写脚本。写下 “Yes” 或 “No”。

**规格**：

* 添加一个句子，指定事件总线接口的名称以及事件总线接口与哪些组件通信。（某些组件组共享相同的事件总线。

* 将请求功能分离到“请求总线”部分，将通知功能分离到“通知总线”部分。

* 将各部分放在页面末尾。

**示例**:

* [Terrain Physics Heightfield Collider 组件](/docs/user-guide/components/reference/terrain/terrain-physics-collider/)
* [Vegetation Altitude Filter 组件](/docs/user-guide/components/reference/vegetation-filters/vegetation-altitude-filter/)
* [Shape Reference 组件](/docs/user-guide/components/reference/shape/shape-reference/)


查看 `component-reference.md`:
```md

## [EBus interface name]RequestBus

<!-- (Optional) Example: "Use the following request functions with the <Request EBus name> EBus interface to communicate with <component name> in your game." -->

| Method Name | Description | Parameter | Return | Scriptable |
| - | - | - | - | - |
|   |   |   |   |   |
|   |   |   |   |   |
|   |   |   |   |   |

## [EBus interface name]NotificationBus

<!-- (Optional) Example: "Use the following notification functions with the <Notification EBus name> EBus interface to communicate with <component name> in your game." -->

| Method Name | Description | Parameter | Return | Scriptable |
| - | - | - | - | - |
|   |   |   |   |   |
|   |   |   |   |   |
|   |   |   |   |   |

```


## 组件参考文档位于何处？

组件参考文档位于文件夹`o3de.org/docs/user-guide/components/reference/`中。它们被组织到子文件夹中，这些子文件夹按类型对组件进行分组。您的元器件参考文档可能属于现有组，也可能完全属于新组。 

请随时联系 [Discord](https://discord.com/invite/o3de) 或 [GitHub](https://github.com/o3de/sig-docs-community)上的 sig-docs-community 寻求帮助。


## 存储图像文件

`static/images`目录的结构反映了 O3DE 站点存储库的`content/docs/`目录。因此，您必须将组件参考文档的图像存储在相应的组件组文件夹中：`static/images/user-guide/components/reference/<group>/`。


## 更新 O3DE 组件引用

请记得更新 [组件参考](/docs/user-guide/components/reference/) 部分并列出您的新组件参考文档。

1. 在文本编辑器中，打开 o3de.org存储库中的`content/docs/user-guide/components/reference.md`。

2. 在相应的组件组表中，创建新行并填写以下列：

    * **Component**: 组件的名称和指向组件参考文档的链接。按照 [链接](/docs/contributing/to-docs/style-guide/format/#links) 指南设置链接格式。

    * **Description:** 用一句话简要描述组件。



## 提交您的组件参考文档

恭喜你编写了你的组件参考文档！它几乎可以发布了。[开始为 Docs 做贡献](/docs/contributing/to-docs/get-started) 部分可以帮助您完成交付文档的最后步骤。
