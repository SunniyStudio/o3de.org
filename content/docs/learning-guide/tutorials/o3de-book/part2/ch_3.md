---
linkTitle: 第3章 介绍实体和组件
title: 第3章 介绍实体和组件
description: 第3章 介绍实体和组件
---
# 第3章 介绍实体和组件

{{<note>}}
存在就是某种东西，与不存在的虚无不同，它是由特定属性组成的具有特定性质的实体。
—Ayn Rand
{{</note>}}

## 介绍

{{<note>}}
我们将在第 2 章 创建新项目 中继续，我们在编辑器中创建了“MyLevel”。本章的资源可以在 GitHub 上找到，网址为：
https://github.com/AMZN-Olex/O3DEBookCode2111/tree/ch03_intro_to_entities
{{</note>}}

本章包括以下内容：
* 什么是实体？
* 什么是组件？
* Transform 组件。
* Mesh 组件。
* 创建复合对象。

在 O3DE 中，游戏世界是由实体（C++ 中的 AZ::Entity）构成的，这些实体可能包含许多组件 （AZ::Component）。实体是一组独立的组件。从概念上讲，组件是实体的特定属性。例如，对于要在空间中拥有位置的实体，它必须具有转换组件 TransformComponent。同样，如果您希望创建作为可移动摄像机的实体，则它必须具有 transform 和 camera 组件。

实体本身是一个组件的空容器。除了附加的组件之外，它几乎没有什么东西。可以在以下位置找到实体源标头：C:\O3DE\21.11.2\Code\Framework\AzCore\AzCore\Component\Entity.h。

```c++
//! An addressable container for a group of components.
//! An entity creates, initializes, activates,
//! and deactivates its components.
//! An entity has an ID and, optionally, a name.
class Entity
```

实体的一个重要方面是它们的亮度。他们真正拥有的只是他们的标识符 AZ：：EntityId。它们接口的其余部分与交互和管理实体具有的组件有关。实体的所有行为都来自它所拥有的组件。这些组件描述实体的行为和属性。您可以自由地将实体组合为任意组件组合。

这是它在 O3DE Editor 中的样子。

图 3.1.具有 Transform 和 Camera 组件的实体

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_1.PNG)

在这里，我们在 Entity Inspector 中查看附加了 Transform 和 Camera 组件的实体。

{{<note>}}
您可以在以下位置找到有关实体和组件的官方参考：
https://www.o3de.org/docs/welcome-guide/key-concepts/#the-component-entity-system
{{</note>}}

可以在 C:\O3DE\21.11.2\Code\Framework\AzCore\AzCore\Component\Component.h 中找到 AZ::Component 的头文件。

```cpp
  /**
  * Base class for all components.
  */
  class Component
```

在我们开始深入研究 AZ::Component 的 C++ 代码和接口之前，我们先了解一下在 Editor 中使用实体和组件构建游戏对象的工作流程和常见方法。

## O3DE 组件
了解组件的一个好方法是查看 O3DE 附带的组件。您必然会遇到的前几个组件是 TransformComponent 和 MeshComponent。

{{<note>}}
有关如何创建实体和组件的 O3DE 编辑器的优秀入门指南，请访问 https://www.o3de.org/docs/learning-guide/tutorials/first-project/。

您应该完成本教程，并在 Editor 中自行修改，以了解界面。
{{</note>}}

本章的其余部分将介绍添加和修改实体的工作流程。

### Transform元件
在 Editor 中创建的所有实体都会自动添加一个 Transform 组件。
为什么实体默认获得 Transform 组件？如果要在运行时在游戏代码中创建实体，则该实体将完全为空。它甚至没有 Transform 组件，除非您在运行时自己添加它。但是，当您在 Editor 中创建一个实体时，该实体会隐式地将实体放置在关卡中的某个位置。Somewhere 表示一个位置，因此它必须具有 Transform （变换） 组件才能位于关卡中的某个位置。

{{<tip>}}
该规则的一个例外是特殊的 Level 实体及其 Level 组件。我们在第 6 章 什么是 AZ::Interface 中了解它们？
{{</tip>}}

图 3.2.编辑器：Entity Outliner

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_2.PNG)

“MyLevel” 已经包含了一些实体。下面是一个 Shader Ball，在 Entity Inspector 中查看。

图 3.3.编辑器：“简单”关卡中的球体实体

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_4.PNG)

{{<note>}}
Transform 组件的官方参考可以在以下位置找到： https://www.o3de.org/docs/userguide/components/reference/transform/ ，以及许多其他组件。
{{</note>}}

什么是“父实体”字段？组件可以引入自己的概念和关系。此字段根据实体在世界中的空间关系将一个实体与另一个实体相关联。子父对象的位置、方向和缩放将取决于其父实体的值。

#### “子”实体

您可以在 Entity Outliner 中创建“子”实体，方法是右键单击现有实体，然后选择 Create entity （创建实体）。这是它在 Editor 中的样子。

图 3.4.编辑器：创建（子）实体

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_3.PNG)

创建实体后，在 Entity Outliner 中选择它，然后在 Entity Inspector 中重命名为“Child of Shader Ball”。

图 3.5. 编辑器: 子实体

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_6.PNG)

实体名称是可选的，但它在这里提醒我们，此实体是 Shader Ball 实体的子实体，如 Transform 组件的 Parent entity 属性所示。

图 3.6.具有 Child Transform 的实体

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_5.PNG)

父实体指向 Shader Ball。这意味着 Translate、Rotate 和 Uniform Scale 的值将成为取决于父级值的局部值，即 Shader Ball 的 Transform 组件值。

我们要看的下一个常见组件是 Mesh 组件。

## Mesh 组件
Mesh 组件允许您将图形对象添加到实体。您可以使用 Entity Inspector 中的 Add Component 菜单将其添加到实体。

图 3.7.具有 Mesh （网格） 组件的实体

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_7.PNG)

{{<tip>}}
我已经折叠了 Transform 组件以使其不碍事，但它仍然存在。您可以使用组件名称旁边的小三角形切换折叠状态。
{{</tip>}}

在这种情况下，Mesh asset （网格资源） 已经设置好了，但您始终可以使用单击 Mesh Asset （网格资源） 属性右侧的文件夹按钮时打开的对话框来更改它。

指定网格资产的另一种方法是直接在 Mesh Asset 文本字段中键入。键入时，将出现一个下拉列表，匹配项

图 3.8.通过交互式搜索选择网格资产

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_9.PNG)

Add Material Component （添加材质组件） 按钮允许您为网格分配自定义材质。在这种情况下，实体没有材料组件，因此使用网格的默认材料。

图 3.9.编辑器：_Sphere_1x1 实体

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_8.PNG)

请注意，Mesh （网格） 组件没有任何 offset 或 position 属性。Mesh 组件不提供任何属性来指定网格的定位方式。隐式地，网格放置在实体的中心，即 （0， 0， 0） 的局部位置。想象一下，网格资产以某个非零局部点为中心。然后我们需要做一些额外的工作来正确地居中它。我们将在下一节中解决这个问题。

## 使用实体组合对象
我们已经看到，我们可以使用 Transform 组件将一个实体与另一个实体相关联。在某些情况下，单个独立实体将完成其工作，但在实践中，您通常需要创建复杂的复合对象。

图 3.10.编辑器：Composite Object

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_12.PNG)

我们将从一个空的 “root” 实体开始，该实体将成为我们将创建的所有其他实体的父实体来表示该对象。这通常很有用，因此我们可以通过移动 “root” 实体在 Editor 中一起移动整个组。它仍然是一个实体，就像任何其他实体一样，但 Transform component parent entity 字段会将它们放在一起。

在 Editor 中，可以通过右键单击主视区中的空白区域并选择 Create entity 来创建实体。

图 3.11.编辑器：创建实体

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_11.PNG)

Editor 将为其分配一些自动生成的名称，例如 Entity11，但我们可以在 Entity Inspector 中重命名它。我将选择 Root Entity。

为复合对象中间的球创建 Root Entity 的子实体。

图 3.12.编辑器：创建子实体

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_10.PNG)

通过创建另外两个子实体来创建两个圆柱体网格。这一次，父级将是 Ball Entity。将子实体命名为 Cylinder 1 Entity 和 Cylinder 2 Entity。大纲将显示它们的结构。

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_15.PNG)

实体的结构

现在，我们可以将各种网格组件分配给球和圆柱体实体。以下是向 Ball Entity 添加网格组件的方法。选择 Ball Entity。在 Entity Inspector 中，单击 Add Component （添加组件）。

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_14.PNG)

Add Component 菜单

这将列出给定您当前项目配置的所有可用组件。

{{<tip>}}
我发现在 Search...字段快速找到我要添加的组件。
{{</tip>}}

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_13.PNG)

添加组件时的搜索字段

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_17.PNG)

查找 Mesh 组件

Atom/Mesh 是我们想要的组件。单击它会将其添加到实体中。然后我们可以将 Mesh asset 指定为 _Sphere_1x1就像我们之前看到的其他实体一样。

{{<note>}}
并非所有组件都可以多次添加到同一实体中。当您使用静态 C++ 方法 GetIncompatibleServices 编写组件时，您可以自己指定该限制，第 9 章 使用 AZ：：TickBus 中对此进行了介绍。以下是如果您考虑将第二个 Mesh 组件添加到同一实体，则 Add Component 菜单的外观
{{</note>}}

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_16.PNG)

Editor 会自动将 （1） 添加到组件的名称中。这表示您的实体上已经有一个 Mesh 组件。如果组件配置为将自身限制为每个实体一个实例，则您将无法再在 Add Component 菜单中找到它。

这是完成的 Ball 实体

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_18.PNG)

具有 Mesh （网格） 组件的实体

圆柱体实体遵循相同的结构，但具有不同的 Mesh 资产和 Transform （变换） 组件中的自定义值，以便将它们移动到一侧并更改它们在 X 和 Y 维度上的缩放。具体而言，Ball Entity 的 Translate 值为 （0， 0， 0），它是对象的中心。但圆柱体的 Translate 值为 （0.5， 0， 0） 和 （-0.5， 0， 0）。

{{<note>}}
Non-uniform Scale 是一个特殊组件，允许您配置非均匀缩放。否则，实体将在所有方向上均匀缩放。
{{</note>}}

图 3.13. Cylinder 1 实体

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_19.PNG)

具有圆柱体网格的实体

图 3.14. Cylinder 2 实体

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_21.PNG)

## 小结

{{<note>}}
本章的资源可以在 GitHub 上找到，网址为：
https://github.com/AMZN-Olex/O3DEBookCode2111/tree/ch03_intro_to_entities
{{</note>}}

您可以通过对象的 Root Entity （根实体） 将对象移动到摄像机实体的视图中。完成更改后，您需要保存关卡。快捷键是 CTRL+S 或从编辑器的主菜单 File → Save 中。

然后，您可以按 CTRL+G 在编辑器中测试关卡。我们在本章中创建的复合对象将如下所示：

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_20.PNG)

由多个父实体组成的对象

本章介绍了 O3DE 中实体和组件的一般概念。真正的强大之处在于创建您的组件。为了创建自定义组件，必须用 C++ 代码编写它们。下一章将首先介绍 O3DE C++ 项目的整体结构，以及如何用 C++ 从头开始编写全新的组件。

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_22.PNG)

我们在本章中创建的新实体
