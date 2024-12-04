---
linkTitle: Prefab系统
title: O3DE Prefab系统开发人员指南
description: 了解 O3DE 中默认的场景制作系统--Prefab系统。
weight: 100
---

## 术语
| 名称              | 说明                                                                                                                                                                                                                                                                         |
|:----------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Entity          | 定义实体属性和行为的组件集合。                                                                                                                                                                                                                                                            |
| Component       | 有助于定义实体行为的一组属性。                                                                                                                                                                                                                                                            |
| Prefab Template | Prefab在内存中的表示。它表示加载到内存中的 prefab 的当前状态。这包括从磁盘加载的文件以及在其上应用的任何更改。                                                                                                                                                                                                                |
| Prefab Instance | 使用Prefab模板实例化的实际Prefab游戏对象。单个Prefab模板可用于实例化任意数量的Prefab实例。                                                                                                                                                                                                                              |
| Link            | 用于表达源模板和目标模板之间连接的对象。它包含一个补丁列表。                                                                                                                                                                                                                                             |
| Patch           | 代表应用于嵌套Prefab实例顶部的 JSON 补丁，以区别于从同一模板（源模板）实例化的其他嵌套实例。                                                                                                                                                                                                                          |
| DOM             | 即文档对象模型。它是一种与语言无关的逻辑模型，以树状分层结构表示任何对象数据，树中的每个节点都包含数据的一些相关信息。在预制系统中，DOM 值由 [RapidJSON](https://rapidjson.org/) 对象表示。预制 DOM 类型在[`PrefabDomTypes`](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/Prefab/PrefabDomTypes.h)中定义。 |




## 概述
<!-- <a name="overview"> -->

要了解什么是Prefab，我们首先需要了解什么是实体。实体是基础游戏对象，封装了构建游戏世界所需的逻辑。实体由定义其属性的组件组成。有关实体和组件的更多信息，请参阅[组件开发指南](/docs/user-guide/programming/components)。

Prefab的作用是轻松构建和修改大型复杂的游戏世界。创建大型世界通常需要在世界中复制大量实体。虽然只需创建大量实体并在编辑器中进行复制即可实现这一目的，但要对其进行修改却成了一件麻烦事，因为您必须进入每个实体并进行相同的修改。您还必须建立自定义跟踪机制，以确定在世界的哪些区域修改哪些实体。此外，您可能还希望某些实体保留其原有属性，而不与其他实体一起更改。所有这些问题都可以通过Prefab来解决。

简单地说，Prefab就是实体和嵌套Prefab的集合。接下来的章节将介绍如何使用实体和嵌套Prefab的集合来解决上述问题以及其他更多问题。


### Prefab文件格式
<!-- <a name="prefab-file-format"> -->

Prefab文件使用 JSON 格式。预制文件包含以下字段。

- **Container Entity**  : 包装实体，是包含在 prefab 中的所有其他实体和嵌套 prefab 的父实体。在<a href="/docs/user-guide/editor/entity-outliner/">Entity Outliner</a>中，这也用于区分 prefab 和普通实体。一个 prefab 必须有一个容器实体。

- **Entities** : prefab 拥有的实体的容器。一个 prefab 可能包含零个或多个嵌套实体。

- **Instances** : 一个 prefab 拥有的嵌套 prefab 的容器。一个 prefab 可以包含零个或多个嵌套 prefab。

    - **Source** : 嵌套的 prefab 需要加载的 prefab 文件的相对路径。

    - **Patches** : 应用于嵌套Prefab顶部的更改列表。

例如，这是一个包含单个实体和单个嵌套Prefab的Prefab：

<details>
<summary>Car.prefab</summary>

```json
{
    "ContainerEntity": {
        "Id": "ContainerEntity",
        "Name": "Car",
        "Components": {
            "Component_[12748530028122391598]": {
                "$type": "SelectionComponent",
                "Id": 12748530028122391598
            },
            "Component_[13392385766383931996]": {
                "$type": "EditorOnlyEntityComponent",
                "Id": 13392385766383931996
            },
            "Component_[14257310886211380255]": {
                "$type": "EditorInspectorComponent",
                "Id": 14257310886211380255
            },
            "Component_[14614886844957924847]": {
                "$type": "EditorEntitySortComponent",
                "Id": 14614886844957924847,
                "Child Entity Order": [
                    "Entity_[1245636963768]"
                ]
            },
            "Component_[14902063945494347948]": {
                "$type": "EditorPendingCompositionComponent",
                "Id": 14902063945494347948
            },
            "Component_[1719713518280800294]": {
                "$type": "EditorVisibilityComponent",
                "Id": 1719713518280800294
            },
            "Component_[1762295132015522048]": {
                "$type": "EditorEntityIconComponent",
                "Id": 1762295132015522048
            },
            "Component_[17657882952245439542]": {
                "$type": "{27F1E1A1-8D9D-4C3B-BD3A-AFB9762449C0} TransformComponent",
                "Id": 17657882952245439542,
                "Parent Entity": ""
            },
            "Component_[18430199545970438248]": {
                "$type": "EditorLockComponent",
                "Id": 18430199545970438248
            },
            "Component_[4138408407469934805]": {
                "$type": "EditorDisabledCompositionComponent",
                "Id": 4138408407469934805
            },
            "Component_[6522218679447316383]": {
                "$type": "EditorPrefabComponent",
                "Id": 6522218679447316383
            }
        }
    },
    "Entities": {
        "Entity_[1245636963768]": {
            "Id": "Entity_[1245636963768]",
            "Name": "Engine",
            "Components": {
                "Component_[10189105590972799159]": {
                    "$type": "EditorInspectorComponent",
                    "Id": 10189105590972799159,
                    "ComponentOrderEntryArray": [
                        {
                            "ComponentId": 15807988307212229253
                        }
                    ]
                },
                "Component_[1115340051766175588]": {
                    "$type": "EditorVisibilityComponent",
                    "Id": 1115340051766175588
                },
                "Component_[11897666328730413911]": {
                    "$type": "EditorDisabledCompositionComponent",
                    "Id": 11897666328730413911
                },
                "Component_[12786378132283821322]": {
                    "$type": "EditorEntitySortComponent",
                    "Id": 12786378132283821322
                },
                "Component_[1300055106168418593]": {
                    "$type": "EditorPendingCompositionComponent",
                    "Id": 1300055106168418593
                },
                "Component_[15030307130975739594]": {
                    "$type": "SelectionComponent",
                    "Id": 15030307130975739594
                },
                "Component_[15807988307212229253]": {
                    "$type": "{27F1E1A1-8D9D-4C3B-BD3A-AFB9762449C0} TransformComponent",
                    "Id": 15807988307212229253,
                    "Parent Entity": "ContainerEntity",
                    "Transform Data": {
                        "Translate": [
                            0.0,
                            -4.999996185302734,
                            0.0
                        ]
                    }
                },
                "Component_[5492938519155614766]": {
                    "$type": "EditorEntityIconComponent",
                    "Id": 5492938519155614766
                },
                "Component_[8499539744869616376]": {
                    "$type": "EditorOnlyEntityComponent",
                    "Id": 8499539744869616376
                },
                "Component_[9386536730188669959]": {
                    "$type": "EditorLockComponent",
                    "Id": 9386536730188669959
                }
            }
        }
    },
    "Instances": {
        "Instance_[1211277225400]": {
            "Source": "Prefabs/Wheel.prefab",
            "Patches": [
                {
                    "op": "replace",
                    "path": "/ContainerEntity/Components/Component_[16182207544533401071]/Parent Entity",
                    "value": "../ContainerEntity"
                },
                {
                    "op": "replace",
                    "path": "/ContainerEntity/Components/Component_[16182207544533401071]/Transform Data/Translate/1",
                    "value": 4.999995231628418
                },
                {
                    "op": "replace",
                    "path": "/ContainerEntity/Components/Component_[16182207544533401071]/Transform Data/Translate/2",
                    "value": 0.012969493865966797
                }
            ]
        }
    }
}
```
</details>

要查看Prefab和实体在实体大纲视图中的样子，请参阅 [实体和Prefab基础知识](/docs/learning-guide/tutorials/entities-and-prefabs/entity-and-prefab-basics/)。

### Prefab模板
<!-- <a name="prefab-template"> -->

**Prefab模板**是Prefab在内存中的表示。它代表加载到内存中的Prefab的当前状态。这包括从磁盘加载的文件 DOM 和所需的附加元数据。加载后，您可以继续对模板 DOM 进行更多更改，而无需每次编辑都将模板保存到文件中。此外，模板对象还存储了一个链接对象容器，代表此模板与其他模板之间的连接。

每个模板都有一个唯一的标识符 `TemplateId`，它是一个由 `PrefabSystemComponent`分配的无符号整数。模板 ID 与模板对象的映射关系由系统组件维护。Prefab模板与Prefab文件之间是一对一的关系。

例如，如果有一个名为`Bike.prefab`的Prefab文件，则只有一个模板与之关联。只有将Prefab加载到编辑器中，才会创建模板。

![Template-file relationship](images/PrefabTemplate.png)

| 代码                                                                                                                                                     | 说明                       |
|:-------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------|
| [Template](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/Prefab/Template/Template.h)                  | 表示加载Prefab时创建的模板对象的类。       |
| [PrefabSystemComponent](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/Prefab/PrefabSystemComponent.h) | 管理模板、链接和实例等Prefab对象的单例系统组件。 |


### Prefab实例
<!-- <a name="prefab-instance"> -->

**Prefab实例**定义了一个Prefab对象，它代表了存储在Prefab模板中的 DOM。与模板 DOM 类似，Prefab实例也有用于容器实体、实体和嵌套实例的成员变量。Prefab模板和Prefab实例之间是一对多的关系。

例如，如果文件中有一个 “Car”Prefab，那么就会定义一个 “Car”模板。使用该单一汽车模板可以生成许多实例。可以通过使用 `InstanceSerializer::Load()` 方法将 DOM 反序列化为实例对象来填充Prefab实例。Prefab实例也可以通过直接添加实体从头开始构建。与模板类似，Prefab实例也可以通过使用 `InstanceSerializer::Store()` 方法转换为 DOM 格式。Prefab序列化和反序列化将在下一节详细讨论。

![Prefab-Instance Relationship](images/PrefabInstance.png)

| 代码                                                                                                                                                          | 说明                                    |
|:------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------|
| [Instance](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/Prefab/Instance/Instance.h)                       | 表示从Prefab模板实例化的实例对象的类。                   |
| [InstanceSerializer](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/Prefab/Instance/InstanceSerializer.cpp) | 类，用于将 DOM 加载到Prefab实例中，并将Prefab实例存储到 DOM 中。 |


### Prefab链接
<!-- <a name="prefab-link"> -->

**Prefab链接**定义了两个模板的连接方式。链接存在于两个模板之间，由 `LinkId`（类似于 `TemplateId`的唯一整数）标识，并由源模板 id 和目标模板 id 定义。目标模板中的嵌套实例指向源模板。

```
Bike       (Bike.prefab)
| Wheel    (Wheel.prefab)
```

在上述 “Bike”和 “Wheel”模板之间的链接示例中，目标模板是与 “Bike.prefab ”文件相对应的模板，源模板是与 “Wheel.prefab ”文件相对应的模板。加载目标模板时，Prefab系统组件会在两个相关模板之间创建一个链接，并维护链接 id 到链接对象的映射。

链接对象的关键成员之一是链接 DOM，它包含源模板的文件路径和要应用到源模板上的补丁列表。链接 DOM 可能包含补丁，也可能不包含补丁。在没有补丁的情况下，目标模板只需获取源模板的模板 DOM。

{{< note >}}
在最新的开发分支中，链接会维护一棵`PrefabOverridePrefixTree`树，而不是链接 DOM。链接 DOM 将由链接树即时生成。这支持新的 **Prefab Override** 功能。
{{< /note >}}

![Link creation](images/PrefabLink.png)

| 代码                                                                                                                        | 说明                  |
|:--------------------------------------------------------------------------------------------------------------------------|:--------------------|
| [Link](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/Prefab/Link/Link.h) | 表示连接两个Prefab模板的链接对象的类。 |





## 加载和保持Prefab
<!-- <a name="loading-and-saving"> -->

加载Prefab文件时，我们会创建一个与之相关的Prefab模板。同样，当需要将Prefab存储到文件中时，我们会提取相应的模板并将其保存到文件中。这一过程由 `PrefabLoader` 实现，它遵循处理模板 DOM 的三个关键步骤：

1. 从JSON字符串创建模板DOM
1. 展开嵌套实例
1. 处理Prefab模板


### 从JSON字符串创建模板DOM
<!-- <a name="create-template-dom-from-json-string"> -->

在给定文件路径的情况下，`PrefabLoader`能够从预制文件中读取原始的JSON字符串。为了方便有效地操作字符串中的数据，加载器将字符串转换为 `PrefabDom`，即 RapidJSON 文档对象。

下面是一个简化的 `Level.prefab` DOM 示例：

```json
{
    "ContainerEntity": {
        "Id": "Entity_[1146574390643]",
        "Name": "Level",
        "Components": { }
    },
    "Entities": {
        "Entity_1": { } 
    }
    "Instances": {
        "Nested_Instance_1": {
            "Source": "filepath",
            "Patches": [ ]
        }
    }
}
```

转换后，DOM 只是 JSON 字符串的 1-1 表示法。实际内容不会改变。此时，嵌套实例 DOM 与文件中的样子一模一样。加载器将按照下一步展开每个嵌套实例中的内容。

### 展开嵌套实例
<!-- <a name="unfold-nested-instances"> -->

如上例所示，该文件仅存储嵌套预制文件的路径和补丁。但是，要完全加载预制模板，我们需要展开任何嵌套实例 DOM，并用源模板生成的完全展开 DOM 替换压缩版 DOM。之所以需要展开，是因为我们需要在编辑器操作过程中使用嵌套模板中描述的确切实体和组件。

`PrefabLoader::LoadNestedInstances()` 方法定义了如何展开所有嵌套实例。如果嵌套实例的源模板尚未加载，则会在展开之前递归加载源模板。为避免循环依赖问题，加载方法会维护一个文件路径集，以检查模板文件是否正在处理中。

嵌套实例展开后，将创建一个链接对象来表示目标模板与源模板嵌套实例之间的连接。你可以认为实例和链接之间有一个 1-1 的映射关系。一个目标模板可能包含多个嵌套实例和链接。

将模板保存到磁盘时，我们会将实例 DOM 压缩回只显示嵌套模板中的 **Source** 和 **Patches** 字段的状态。

![Conversion between prefab file and template](images/PrefabNestedInstances.png)


### 处理Prefab模板
<!-- <a name="sanitize-prefab-templates"> -->

JSON 序列化器的默认行为是将类型的默认值排除在序列化到 JSON 之外。例如，如果一个组件的属性默认值为零，除非该值不为零，否则在组件序列化生成 JSON 时，该属性将被剔除。这样做的目的是使文件中的序列化 JSON 字符串保持紧凑。遗憾的是，这使得从文件中加载 JSON 字符串变得有点麻烦。如果文件中没有某个值，就根本无法读取。

为防止这种情况发生，您可以定义自定义序列化器，以指示在加载时某个字段应使用的默认值。不过，我们不能指望所有组件和类型都能确定定义一个自定义序列化器。因此，在加载过程中，我们会添加一个 sanitization 阶段，在此阶段我们会临时序列化同一文件，并将默认值写入其中，以便从中读取。而在保存过程中，我们会暂时序列化文件，删除默认值。

![Prefab Sanitization](images/PrefabSanitization.png)

| 代码                                                                                                                                   | 说明                          |
|:-------------------------------------------------------------------------------------------------------------------------------------|:----------------------------|
| [PrefabLoader](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/Prefab/PrefabLoader.h) | 类，用于将Prefab文件加载到模板和将模板保存到预制文件。 |




## 关卡Prefab
<!-- <a name="level-as-prefab"> -->

所有实体和Prefab都放置在关卡之下。关卡只是另一种Prefab。与其他Prefab一样，关卡Prefab必须有一个容器实体，并且可以包含实体和实例（如果有的话）。

```json
{
    "ContainerEntity": {
        "Id": "Entity_[1146574390643]",
        "Name": "Level",
        "Components": { }
    },
    "Entities": { },
    "Instances": { }
}
```

与其他 prefab 不同，从关卡模板创建的实例只能有一个，即关卡实例（或根实例）。它是一个由 `PrefabEditorEntityOwnershipService` 拥有和管理的实例，其容器实体的名称被硬编码为 “Level”。

加载和保存关卡的过程与我们上面描述的加载和保存模板的过程相同。由于它拥有所有内容，因此首先加载它，然后递归加载所有其他嵌套模板。

| 代码                                                                                                                                                                               | 说明                |
|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------|
| [PrefabEditorEntityOwnershipService](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/Entity/PrefabEditorEntityOwnershipService.h) | 拥有并管理关卡Prefab实例的单例类。 |





## 传播
<!-- <a name="propagation"> -->

Prefab支持的关键机制之一是传播。这意味着修改一个Prefab实例后，可以轻松地将相同的更改复制到所有其他Prefab实例中。例如，修改一个车轮实例的颜色，并将更改复制到关卡中的所有车轮。我们可能想要传播不同类型的更改，如修改实体、删除组件、添加嵌套Prefab等。但无论哪种类型，传播事件的流程都如下所示：

![High-level propagation workflow](images/PrefabPropagation.png)


传播是一种重要机制，可在应用更改后保持模板、实例和链接同步。我们首先对一个实例进行更改，然后把更改带到它所属的模板，最后把更改传播到模板注册的所有实例。更改流程可分为三个部分：
1. 补丁生成
1. 模板-模板传播
1. 模板-实例传播


### 补丁生成
<!-- <a name="patch-generation"> -->

在对实例进行更改（例如更新其拥有实体的位置）后，我们希望将这些更改带到其对应的模板中。`InstanceToTemplatePropagator`提供了几个辅助函数，可在应用更改之前和之后从实例对象生成 DOM，在生成的 DOM 之间创建 JSON 差异补丁，并将补丁应用到模板 DOM。

```
Level
| Car        (Car.prefab)
  | Engine   <-- change position
```

在上面的示例中，我们有一个包含引擎实体的 Car prefab 实例。我们更新了引擎的位置。在对汽车应用更改之前和之后，会生成两个实例 DOM。生成的 JSON 补丁表示两个 DOM 之间的差异，即引擎的位置。然后，该补丁将应用到汽车模板 DOM，这样模板就能反映最新状态。模板更新后，就可以进行模板-模板传播，这将在下一节中介绍。

这里的目标是生成一个补丁，有多种方法可以做到这一点。随着时间的推移，我们对补丁生成过程进行了多次优化，以加快某些操作的速度。

#### 将实例对象序列化两次
1. 在应用更改之前序列化实例，以生成 **前状态 实例 DOM**。
1. 应用更改。
1. 在应用更改后序列化实例，生成**后状态实例 DOM**。
1. 比较两种状态并生成补丁。

![Patch generation by serializing the instance objects twice](images/PrefabPatchGeneration1.png)


#### 只序列化相关实体

上述方法可行，但缺点是需要花费大量时间将实例对象转换为 DOM，尤其是当我们进行小改动时，比如修改Prefab中 1,000 个实体中的一个实体。要解决这个问题，我们可以按照以下步骤操作：

1. 在应用更改之前序列化实体，生成**前状态实体 DOM**。
1. 应用更改
1. 在应用更改后序列化实体，生成**后状态实体 DOM**。
1. 比较两种状态并生成补丁。
1. 添加所需的路径前缀，使修补程序有效。
    * 例如，生成的补丁路径为 `ComponentA/...`，但我们还需要在其中添加实体路径前缀，以便知道我们所指的是Prefab中的哪个实体。带有前缀的路径将变成 `/Entities/Entity1/Components/ComponentA/...`。

{{< note >}}
不过，要做到这一点，我们需要知道，我们将只修改一个实体或一组实体。因此，这种方法只适用于实体更新等变更。
{{< /note >}}

![Patch generation by serializing relevant entities only](images/PrefabPatchGeneration2.png)


#### 使用模板 DOM 作为前状态

一个有趣的现象是，我们不需要序列化实例或实体来获取前状态 DOM，因为这些信息已经存在于相应的预制模板中。因此，我们可以跳过序列化步骤。

1. 从相应模板获取**实例或实体 DOM的前状态**。
1. 应用更改。
1. 在应用更改后序列化实例或实体，生成**后状态 DOM**。
1. 比较两种状态并生成补丁。
1. 添加所需的路径前缀，使补丁有效，就像我们在前一种情况中所做的那样。

#### 无需序列化即可生成补丁

如果我们知道要做什么操作，比如添加或删除实体，我们就不需要序列化整个实例或实体来获取 DOM 的后状态。我们可以聪明地自己创建一个补丁。

我们会根据操作创建一个补丁，其中包含 **op操作**、**path路径**和**value值**字段，以符合 JSON 补丁标准。

{{< note >}}
这只适用于添加或删除实体等简单操作，但可能不适用于修改特定组件等更复杂的用例。不过，有了**Document Property Editor (DPE)** 补丁，这种情况将来会有所改变。
{{< /note >}}


#### 尽力修补

我们使用 JSON 序列器的默认修补程序机制在实例或实体 DOM 上打补丁。如果在应用补丁数组时发生错误，补丁应用会立即停止。不过，对于Prefab来说，可能会有过期的补丁，或者系统可能由于错误而生成了错误的补丁。在这种情况下，为了防止数据丢失，我们会尽力打补丁，一次只打一个补丁，跳过导致错误的补丁。在补丁应用结束时，我们会发送一条警告信息，说明哪些补丁无法应用。

| 代码                                                                                                                                                                            | 说明                       |
|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------|
| [InstanceToTemplatePropagator](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/Prefab/Instance/InstanceToTemplatePropagator.h) | 定义从实例和实体生成 DOM 值的辅助函数的类。 |



### 模板-模板传播
<!-- <a name="template-template-propagation"> -->

在模板-模板传播中，我们使用最新的 DOM 值更新所有依赖模板。目标模板 DOM 包括从源模板 DOM 生成的实例 DOM。源模板 DOM 首先必须是最新的，这样目标模板 DOM 才能接收到最新的变化。

```
Level
| Bike1       (Bike.prefab)
  | Wheel1    (Wheel.prefab)
  | Wheel2    (Wheel.prefab)
| Bike1       (Bike.prefab)
  | Wheel1    (Wheel.prefab)
  | Wheel2    (Wheel.prefab)
```

例如，如果我们有一个类似上面的关卡，并且我们更新了 Wheel prefab 实例模板的 DOM。这就是模板-模板传播过程：

![Template-template propagation](images/PrefabTemplatePropagation.png)

| 代码                                                                                                                                                     | 说明                                                                                      |
|:-------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------|
| [PrefabSystemComponent](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/Prefab/PrefabSystemComponent.h) | `PrefabSystemComponent::PropagateTemplateChanges()` 方法使用最新的 DOM 值更新链接，从而重新生成所有嵌套实例 DOM。 |



### 模板-实例传播
<!-- <a name="template-instance-propagation"> -->

如上所述，当一个模板被更新时，它将触发依赖模板的更新。该过程结束后，我们仍需更新场景中的实际Prefab实例。这就是模板实例传播的作用所在。由Prefab模板创建的所有Prefab实例都将被放入一个队列。下一次系统运行时，队列中的所有实例都将通过有选择地反序列化模板 DOM，更新为已应用补丁的Prefab实例对象。

在上面的例子中，关卡有两个 Bike Prefab，这就是模板-实例传播的过程：

![Template-instance propagation](images/PrefabInstancePropagation.png)

用于反序列化的实例 DOM 与从层次结构中最顶层模板（层）看到的实例相对应，因为它考虑到了可能应用于实例的所有可能覆盖。如果被编辑的Prefab不是层级，而是层级中的某个嵌套Prefab，那么该Prefab将成为最顶层的模板，所使用的 DOM 将是从该模板看到的 DOM。

#### 选择性反序列化

JSON 反序列化器是累加式的，没有提供开箱即用的解决方案来选择性地只反序列化修改过的值。因此，过去需要清除Prefab实例的内容并从头开始反序列化。然而，随着预制构件规模的扩大或预制构件实例数量的增加，这种方法并不能很好地扩展。此外，其他几个系统也会监听和响应实体的变化，从头开始重新创建实体也会增加这些系统的计算成本。为了解决这些问题，我们引入了_选择性反序列化_来完成以下工作：

1. 我们会在实例对象中缓存用于反序列化的 DOM。如果实例中没有缓存的实例 DOM，我们就会像以前一样清除并重新加载所有内容。但如果有缓存的实例 DOM，我们就会使用它与当前 DOM 进行比较，并生成 JSON 补丁。
1. 然后，我们遍历补丁，通过检查补丁路径来确定哪些实体需要重新加载。
1. 反序列化器只清除这些特定实体的内容，并从提供的 DOM 中反序列化它们。
1. 在添加和删除实体时，它会利用实例下已存储的实体别名来修改相关实体，而不会影响其他实体。
1. 对于存储在使用实例别名的Prefab实例下的嵌套实例，请重复第 3 和第 4 步。


| Code | Description                                                                                                                 |
| :-   |:----------------------------------------------------------------------------------------------------------------------------|
| [InstanceUpdateExecutor](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/Prefab/Instance/InstanceUpdateExecutor.h) | 管理模板-实例传播的类。在 `PrefabComponentSystem::PropagateTemplateChanges()` 方法的末尾， 它将模板实例传播任务委托给 `InstanceUpdateExecutor`。            |
| [InstanceSerializer](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/Prefab/PrefabPublicInterface.h) | 类定义了如何将 DOM 反序列化为实例对象。反序列化在`PrefabDomUtils::LoadInstanceFromPrefabDom()`方法中进行，而 prefab 的 JSON 序列化器由`InstanceSerializer`定义。  |





## Prefab编辑器工作流程
<!-- <a name="prefab-editor-workflows"> -->

以上章节涵盖了Prefab的核心 API 功能，并介绍了Prefab系统的工作原理。本节将介绍如何将Prefab集成到 O3DE 编辑器中，以及如何在内部创建、修改和删除实体和Prefab。

一般来说，以下所有工作流程都遵循这种模式：

![Prefab editor workflow](images/PrefabGenericEditorWorkflow.png)

{{< note >}}
唯一的例外是重复工作流，在这种情况下，我们不首先更新Prefab实例，而是采用 DOM 授权方式，让传播来创建重复对象。
{{< /note >}}

| 代码                                                                                                                                                     | 说明                                                                                                                                           |
|:-------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------|
| [PrefabPublicInterface](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/Prefab/PrefabPublicInterface.h) | 公共处理程序实现的接口。PrefabPublicInterface 中公开暴露的函数可通过 Python 脚本调用，并用于编写Prefab的自动测试。                                                                     |
| [PrefabPublicHandler](https://github.com/o3de/o3de/blob/development/Code/Framework/AzToolsFramework/AzToolsFramework/Prefab/PrefabPublicHandler.cpp)   | 实现 prefab 公共接口的类。 `PrefabPublicHandler` 是实现Prefab编辑器工作流程的类。在内部，它调用在 `PrefabEditorEntityOwnershipService` 和 `PrefabSystemComponent` 中定义的Prefab API。 |



### 创建实体
<!-- <a name="create-entity"> -->

您可以创建一个由 prefab 拥有的新实体。该 prefab 称为拥有的 prefab 实例。默认情况下，新实体将创建在正在编辑的 prefab 的容器实体下，否则新实体将创建在所选实体下。

下图演示了如何将新的 “Engine  ”实体添加到 “Car  ”Prefab实例中：

![Editor workflow - Create entity](images/PrefabCreateEntityWorkflow.png)


### 创建Prefab
<!-- <a name="create-prefab"> -->

您可以选择多个实体和Prefab，这些实体和Prefab共享一个所有的Prefab并共享一个根父，然后将它们放在一个新的Prefab下。为此，应将所选实体和Prefab从所属模板中移除，然后移入新创建的模板中。

```
RaceTrack    (RaceTrack.prefab)
| Wheel      (Wheel.prefab)      <-- select this
| Engine                         <-- select this
| Road
```

在上面的示例中，如果您想选择 “Wheel  ”Prefab和 “Engine  ”实体，并将它们制作成一个Prefab，工作流程如下：

![Editor workflow - Create prefab](images/PrefabCreatePrefabWorkflow.png)

当一个模板从另一个模板中删除时，我们也需要删除这两个模板之间的链接。这意味着要删除 `PrefabSystemComponent` 中的 `Link` 对象。在上述示例中，WheelPrefab和 RaceTrack 之间的链接已被删除。

同样，必须在新创建的Prefab和其所属的Prefab之间创建一个新的 `Link` 对象。在上例中，Car prefab 和 RaceTrack prefab 之间创建了一个新链接。传播后，RaceTrack prefab 的所有其他实例也将获得这些更改。如果有十个 RaceTrack 实例，那么在所有这些实例中都会出现一个新的 Car 实例。

### 实例化Prefab
<!-- <a name="instantiate-prefab"> -->

您可以从磁盘上的Prefab模板创建新的Prefab实例。在实例化之前，可以加载或不加载模板；如果不加载，`PrefabLoader` 将首先加载模板。默认情况下，新的Prefab实例是在被编辑Prefab的容器实体下创建的，否则新实例将在所选实体下创建。

与**创建实体**不同的是，此工作流程包括在模板实例化和所属Prefab之间创建链接的额外工作。

下图演示了如何将WheelPrefab实例化并作为嵌套实例添加到CarPrefab中：

![Editor workflow - Instantiate prefab](images/PrefabInstantiatePrefabWorkflow.png)


### 删除实体和嵌套的Prefab
<!-- <a name="delete-entities-and-nested-prefabs"> -->

您可以选择多个实体和Prefab，并将它们从所属模板中移除。被选中的项目需要共享一个共同的自有Prefab，否则操作不会成功。删除实体时，工作流程也会删除该实体的所有后代实体和 prefab 实例。

```
Car        (Car.prefab)
| Wheel    (Wheel.prefab)
| Engine                   <-- select this
  | Piston
  | Distributor
| Seats
  | Seat   (Seat.prefab)   <-- select this
```

在上例中，如果您选择了 Engine 实体和 Seat Prefab，就会生成两个供删除的列表：
1. **Entity List:** Engine, Piston, Distributor
1. **Nested Prefab List:** Seat

下图展示了工作流程的样子：

![Editor workflow - Delete entities and nested prefabs](images/PrefabDeleteWorkflow.png)


### 分离Prefab
<!-- <a name="detach-prefab"> -->

您可以在Entity Outliner中选择一个Prefab实例并将其分离。分离的意思是：

1. 将所选Prefab的容器实体转换为常规实体。该实体的父实体与容器实体的父实体相同。
1. 将所有实体和嵌套Prefab实例（由所选Prefab拥有）置于该实体之下。
   它与**创建Prefab**的操作相反。.

{{< note >}}
一次只能分离一个Prefab。如果选择了多个Prefab，**Detach Prefab...** 选项将不会出现在右键菜单中。
{{< /note >}}

```
Car       (Car.prefab)
| Wheel   (Wheel.prefab)    <-- select this
Wheel     (Wheel.prefab)
| Tire    (Tire.prefab)
| Rim
| Hub
```

在上例中，如果选择 Car prefab 下的 Wheel prefab 并将其分离，Wheel prefab 的所有子实体都将被置于 Car prefab 实例下的新 Wheel 实体中。下图演示了该工作流程：

![Editor workflow - Detach prefab](images/PrefabDetachWorkflow.png)


### 复制实体和Prefab
<!-- <a name="duplicate-entities-and-prefabs"> -->

您可以在Entity Outliner中选择多个实体和Prefab并复制它们。所有选中的实体和Prefab应为同一Prefab实例所有，否则操作不会成功。

{{< note >}}
复制一个实体将复制其所有后代。
{{< /note >}}

```
Garage_Small
| Brooms                     <-- select this
  | Broom_1
  | Broom_2
Garage_Large
| Car         (Car.prefab)   <-- select this
```

在上述示例中，如果您选择 “Brooms ”实体和 “Car ”Prefab并复制它们，复制将导致实体和Prefab实例的创建：

1. 将创建一个新实体 Brooms（包含 Broom_1 和 Broom_2）。Brooms 的父实体是 Garage_Small。
1. 将创建一个新的嵌套 prefab 实例 Car。该Prefab的父节点是 Garage_Large。

下图展示了工作流程的样子。

![Editor workflow - Duplicate entities and prefabs](images/PrefabDuplicateWorkflow.png)

{{< note >}}
第二步用点线包围。如上所述，对于这一特殊操作，我们目前会生成补丁，并直接在模板 DOM 上应用补丁。这是一种模板 DOM 授权方法。在传播过程中，prefab 实例对象将被更新。
{{< /note >}}
