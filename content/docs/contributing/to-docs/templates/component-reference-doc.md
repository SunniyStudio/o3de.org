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


## Does my document belong in the O3DE Component Reference?

First, determine whether or not your document belongs in the [Component Reference](/docs/user-guide/components/reference/) section of the O3DE User Guide. The purpose of the component reference document is exactly that, a page that users can quickly reference when using the component in **O3DE Editor**.

Supplemental information, such as using the component across different workflows, belongs in other documentation. That document can be organized in an appropriate section of the User Guide, such as the [Interactivity and Simulation](/docs/user-guide/interactivity/), [Scripting](/docs/user-guide/scripting/), [Networking](/docs/user-guide/networking/), or [Visualization](/docs/user-guide/visualization/) sections.

## Setting up repository and downloading component template

To contribute to O3DE documentation, you must complete the steps in [Setting up a local o3de.org repo](../get-started#setting-up-a-local-o3deorg-repo). You can do this before or after writing your component reference document. However, you must set this up to place your document and images in the right folder.

In a new tab, navigate to the file [component reference template](https://raw.githubusercontent.com/o3de/sig-docs-community/main/doc-templates/component-reference.md) in the o3de/sig-docs-community repository and save it to your local machine. Open the template in a text editor and refer to this page as you fill it out.


## 编写组件参考文档

以下部分将指导您如何填写组件引用模板的每个部分。有些部分是可选的，您的组件可能不需要，因此您可以根据需要删除这些部分。

### Metadata

Like all Markdown (`.md`) files in the O3DE Documentation, your component reference document must contain metadata at the beginning of the file. Refer to [Metdata](/docs/contributing/to-docs/style-guide/metadata/) to help you fill in this section. 

{{< note >}}
The `weight` parameter is optional. If omitted, the default behavior is alphabetical sorting of the title.
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

Write the name of the Gem that provides this component and link to the corresponding [Gem reference document](/docs/user-guide/gems/reference/).

**Specifications**:

* Capitalize "Gem".

* Follow the [Links](/docs/contributing/to-docs/style-guide/format/#links) guideline for formatting the link.


See `component-reference.md`:
```md
## Provider

[Gem name](_/docs/user-guide/gems/reference/<path-to-gem-docs>_)

```

### Dependencies (optional)

If your component depends on other components to work, list the names of the dependent components and link it to the corresponding component reference document.

**Specifications**:

* Explain how this component relates to its dependencies, if necessary.

* Follow the [Links](/docs/contributing/to-docs/style-guide/format/#links) guideline for formatting the link.

**Examples**:

* [Diffuse component](/docs/user-guide/components/reference/atom/diffuse-probe-grid/): A straightforward example with one dependency.

* [Light component](/docs/user-guide/components/reference/atom/light/#dependencies): The Light component can depend on different Shape components. This example describes the varying dependency and its criteria.

See `component-reference.md`:
```md
## Dependencies

<!-- (Optional) List the component's dependencies -->

```

### Use cases (optional)

Write the use cases for which a user may want to use this component. This section can help clarify when to use this component, especially if there's possible confusion. 

**Specifications**:

* If you're providing multiple use cases, make sure they are unique from each other. 

* An effective use case provides situational context and clearly states the role of the component in the situation. 

* Write in complete sentences and elaborate on any technical terminology.

**Examples**:

* [PhysX Shape Collider component](/docs/user-guide/components/reference/physx/shape-collider/#use-cases) - Clarifies when to use a PhysX Shape Collider component versus a PhysX Primitive Collider component. 

See `component-reference.md`:
```md
## Use cases

<!-- (Optional) Write the use cases in which a user may want to use this component. -->

```


### Limitations (optional)

Write about the limitations this component may have. 

**Specifications**:

* Explain how users can work around a limitation, if possible. 

* Write in complete sentences and elaborate on any technical terminology.

* Don't include any "future work". A better place for that is the feature's roadmap, which can be found in the corresponding [SIG repository](https://www.o3de.org/contribute/#join-a-special-interest-group). 

**Examples**:

* [PhysX Shape Collider component](/docs/user-guide/components/reference/physx/shape-collider/#limitations)
* [HDRi Skybox Component](/docs/user-guide/components/reference/atom/hdri-skybox/)


See `component-reference.md`:
```md

## Limitations

<!-- (Optional) Write the limitations of this component. Don't include any "future work". -->

```

### Properties

The properties section contains the following contents in order: 

1. An image of the component card
2. A property table
3. (optional) Additional property tables


#### Image of component card

 You must include an image of the component card of this component. In the Editor, when an entity with this component is selected, the component card can be seen in the **Entity Inspector** panel.

 **Specifications**:

* Use a screenshot tool to capture the image. 

* The image must show the component card at its default state.

* Crop the image to show only the component card of this component.

* [Store the image files](#storing-image-files) in the correct folder. 

* Follow the guidelines in [Submitting Media to Open 3D Engine Documentation](/docs/contributing/to-docs/style-guide/media/). 


#### Creating one or multiple property tables

Depending on how properties are arranged in your component card, you can have one or multiple property tables. 

A *simple set of properties* refers to a component card that displays all of its properties at the first level in the component card's content hierarchy. In this case, you can view all of the properties without opening a drop down list or configuring a set of properties a specific way. 

On the other hand, a *complex set of properties* doesn't show all available properties immediately. There may be properties hidden in a property group under a drop down list or conditional properties that only appear when other properties are configured a specific way. 

| Example: Simple set of properties | Example: Complex set of properties | 
| - | - |
| ![Box Shape component properties](/images/user-guide/components/reference/shape/box-shape-component-ui-01.png) | ![PhysX Primitive Collider component interface.](/images/user-guide/components/reference/physx/physx-collider-ui-01.png) |

{{< tabs >}} 
{{% tab name="Simple set of properties" %}}

For a component card with a simple set of properties, you only need one property table. 

**Examples**:

* [Box Shape component](/docs/user-guide/components/reference/shape/box-shape/): A straightforward example where all properties are displayed at the first level in the component card's content hierarchy.

* [Bloom component](/docs/user-guide/components/reference/atom/bloom/): Even though the Bloom component's properties contain dropdown lists, only one property table is needed. This is because the drop down lists are opened by default and are used to contain an array of values. Another exception is the **Overrides** dropdown list, which isn't used in the user's main workflow. So, a description of the **Overrides-Enabled Override** property is sufficient. 

See `component-reference.md`:
```md

## Properties

<!-- This example is for a simple set of properties. 
If you use this section, then delete the other Properties section. -->

![[component name] interface](/images/<path-to-image>)

| Property | Description | Value | Default |
| - | - | - | - |
|   |   |   |   |
|   |   |   |   |
|   |   |   |   |

```

{{% /tab %}}
{{% tab name="Complex set of properties" %}}  

For a component card with a complex set of properties, you may need multiple property tables: base properties, and one for each property group or configuration. The first property table is called "Base properties" and contains all of the properties that are displayed at the first level in the component card's content hierarchy. You can name subsequent property tables after the property group name in the component card or the type of configuration. 

Another option is to separate the property tables into [tabs](/docs/contributing/to-docs/style-guide/format/#tabs) on your document. When content is in tabs, only the active tab displays content, while the rest is hidden. Because of this behavior, tabs are recommended if the sets of properties define configurations for different workflows that the user may or may not choose. 

**Examples**:
* [Light component](/docs/user-guide/components/reference/atom/light/): Contains several configurations that don't have a 1-1 relationship with the **Light type** property. Some property groups are available for more than one light type, so they are documented in different property tables. Each property table specifies which light types support those properties.

* [PhysX Primitive Collider component](/docs/user-guide/components/reference/physx/collider/): Contains several configurations, which depend on the **Shape** property. Each configuration affect what properties are available, so they are documented in different property tables separated into tabs. 

* [Camera Rig component](/docs/user-guide/components/reference/camera/camera-rig/): Contains addable property groups with multiple workflows in each. So, each property group is documented in separate property tables and each workflow is further separated into tabs. 

See `component-reference.md`:
```md
## Properties

<!-- This example is for a complex set of properties. 
If you use this section, then delete the other Properties section. -->

![<component name> interface](/images/<path-to-image>)


### Base properties

| Property | Description | Value | Default |
| - | - | - | - |
|   |   |   |   |
|   |   |   |   |
|   |   |   |   |


### <Property group> properties

![[property group] interface](/images/<path-to-image>)

| Property | Description | Value | Default |
| - | - | - | - |
|   |   |   |   |
|   |   |   |   |
|   |   |   |   |


### <Configuration> properties

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



#### Contents of the property table

In the property table, sort the properties into rows in the order they're shown in the component card. Then, fill in the following columns for each property: 

* **Property**: The name of the property. This must be the exact name as shown in the component card, formatted in bold. 
* **Description**: Describes the purpose of the property and what it configures. Be clear and concise, and explain any technical terminology. 
* **Value**: The type of value that defines this property. If it's a numerical value, include the range of accepted values. 
* **Default**: The property's default value. 

#### Formatting examples for values and default values

| Value | Default | Note |
| --- | --- | --- |
| Int: 0 - 10 | 0 |  |
| Float: 0.0 to Infinity | `100.0` | If the range of values includes words, such as "-infinity" or "infinity", use "to" between the range. |
| Float: 0.0 - 10000.0 | `0.5` | If the range of values is only numerical, use a hyphen (-)  between the range. |
| Float: 0.0 - 1.0 |Kernel Size: <br><ul><li>0: 0.04</li><li>1: 0.08</li><li>2: 0.16</li><li>3: 0.32</li><li>4: 0.64</li></ul> | An array of floats that describes kernel sizes. |
| String | None |  |
| Boolean | Enabled | The default value of a boolean can be "Enabled" or "Disabled". |
| Vector3: -Infinity to Infinity |X: 0.0, Y: 0.0, Z: 0.0 |  |
| Vector3: <br><ul><li> X: 0.0 - 1.0 </li><li>Y: -Infinity to Infinity </li><li>Z: -5.0 - 5.0 </li></ul> | X: 0.0, Y: 0.0, Z: 0.0 | A Vector3 with different default values per parameter. |
| Entity Id | None | A reference to an entity in the level. |
| Any collision layer defined in the project's [Collision Layers](/docs/user-guide/interactivity/physics/nvidia-physx/configuring/configuration-collision-layers/). | Default | If the value is an object, provide context so users can identify the object. |
| A `.physmaterial` library product asset. | The global project `.physmaterial` library. | |
| Product asset `.pxmesh` PhysX mesh. | None |  |
| `PhysicsAsset`, `Sphere`, `Box`, `Capsule` | `PhysicsAsset` | A set of enumerated values.  |


### Component feature sections (optional)

You can write additional sections to explain specific features about your component, such as editing them in the viewport or another system editor, their different modes, and other notable specifications or concepts. 

**Specifications:**

* Use a concise and appropriate title such that users can know what to expect from this section. 

* Include information that serves the users and helps them use the component.

* Avoid including design decisions or implementation details that aren't useful to the user. 

* Repeat this section for each additional topic as needed. 

**Examples**:

* [PhysX Primitive Collider component](/docs/user-guide/components/reference/physx/collider#collider-component-mode): Contains sections on editing colliders in the viewport and using colliders as triggers.

* [PhysX Shape Collider component](/docs/user-guide/components/reference/physx/shape-collider#complex-polygon-prism-shapes): Contains a section that explains intricacies when working with complex polygon prism shapes.

* [Light component](/docs/user-guide/components/reference/atom/light#light-types): Contains a section that describes the different light types and their features. This section is located near the beginning of the page because it contains information that's necessary to understand the component's properties. 

See `component-reference.md`:
```md

## <Component feature>

<!-- (Optional) Explain specific features of your component. Repeat this section for each additional topic, as needed. -->

```

### Request Bus / Notification Bus (optional)

The request and notification bus sections contain the functions that users can call in their project's logic and scripts. 

In the request or notification bus table, list the functions in rows and fill in the following columns:

* **Request/Notification Name**: The name of the function, formatted as `code`.
* **Description**: Describes what the function does. Be clear and concise, and explain any technical terminology. 
* **Parameter**: What parameters this function accepts, if any. If none, write "None".
* **Return**: What this function returns. If it returns nothing, write "None". 
* **Scriptable**: Whether or not this function is scriptable. Write "Yes" or "No". 

**Specifications**:

* Add a sentence specifying the name of the EBus interface and which components the EBus interface communicates with. (Some groups of components share the same EBus.)

* Separate request functions into the "Request Bus" section and notification functions into the "Notification Bus" section. 

* Place the sections at the end of the page. 

**Examples**:

* [Terrain Physics Heightfield Collider component](/docs/user-guide/components/reference/terrain/terrain-physics-collider/)
* [Vegetation Altitude Filter component](/docs/user-guide/components/reference/vegetation-filters/vegetation-altitude-filter/)
* [Shape Reference component](/docs/user-guide/components/reference/shape/shape-reference/)


See `component-reference.md`:
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


## Where does the component reference document live?

Component reference documentation live in the folder: `o3de.org/docs/user-guide/components/reference/`. They are organized into subfolders that group the components by type. Your component reference document may belong to an existing group, or a new group completely. 

Feel free to reach out to sig-docs-community on [Discord](https://discord.com/invite/o3de) or [GitHub](https://github.com/o3de/sig-docs-community) for assistance.


## Storing image files

The `static/images` directory is structured to reflect the `content/docs/` directory of the O3DE site repository. So, you must store images for your component reference document in the appropriate component group folder: `static/images/user-guide/components/reference/<group>/`.


## Updating the O3DE Component Reference

Remember to update the [Component Reference](/docs/user-guide/components/reference/) section and list your new component reference document. 

1. In a text editor, open the file `content/docs/user-guide/components/reference.md` from your clone of the o3de.org repo.

2. In the appropriate component group table, create a new row and fill in the following columns: 

    * **Component**: The name of your component and a link to your component reference document. Follow the [Links](/docs/contributing/to-docs/style-guide/format/#links) guideline for formatting the link. 

    * **Description:** Briefly describe the component in a single sentence. 



## Submit your component reference document

Congratulations on writing your component reference document! It's almost ready to publish. The [Get Started Contributing to Docs](/docs/contributing/to-docs/get-started) section can help you through the final steps to deliver your document.
