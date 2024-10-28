---
description: ' 在 Open 3D Engine 的 <guilabel>Track View</guilabel> 编辑器中创建一个组件实体节点，并将其添加到轨迹中。 '
title: Component Entities and Component Nodes
---

**主题**
- [命名和识别组件实体](#naming-and-identifying-component-entities)
- [在组件实体中添加或移除组件](#adding-or-removing-components-from-a-component-entity)
- [在组件实体上为组件制作动画](#animating-components-on-a-component-entity)

在Track View中，**组件实体节点**是**组件节点**的容器。

使用轨迹视图添加动画时，动画轨迹会应用到组件节点。组件实体节点不直接具有轨迹或关键属性。

**示例
组件实体**GameObject**包含 **Transform** 、**Mesh**和**Point Light**组件。在轨迹视图中将**GameObject**组件实体添加到序列中时，可以在节点浏览器中看到所有组件。组件实体节点是序列中动画组件的引用。

**示例**
组件实体**GameObject**包含**Transform**、**Mesh**和**Point Light**组件。在轨迹视图中将 **GameObject** 组件实体添加到序列中时，可以在节点浏览器中看到所有组件。组件实体节点是序列中动画组件的引用。

![Track View and the Entity Inspector with the same component entities.](/images/user-guide/cinematics/cinematics-component-entities-nodes-track-view-editor-1.png)

可以制作动画的组件节点作为子节点嵌套在相关的实体节点下。您可以为这些组件节点添加动画轨迹。

**为动画节点添加轨道**

1. 在轨迹视图中，创建或选择一个序列。

1. 在节点浏览器中，右键单击并选择**Add Track**，然后选择组件属性。

并非所有组件都能在轨迹视图中显示动画。例如，您只能为**Mesh**组件添加**Visibility**轨迹。**Point Light**组件有多个轨道，您可以添加到序列中。在下面的示例中，**Color**、**DiffuseMultiplier**和**Visible**轨迹被添加到序列中。

![Add tracks from component entity nodes](/images/user-guide/cinematics/cinematics-component-entities-nodes-track-view-editor-2.png)

有关在轨迹视图中添加节点支持的更多信息，请参阅 [为动画在轨迹视图中公开自定义组件](/docs/user-guide/programming/components/expose-animation)。

## 命名和识别组件实体

O3DE 使用实体 ID 来标识组件实体，这意味着你可以随意命名你的组件实体。这包括对多个实体重复使用相同的名称。在 “轨迹视图 ”中，如果组件实体节点共享相同的名称，名称后会附加一个数字。这不会改变关卡中组件实体的名称，但可能难以确定要将哪个实体制作成动画。

**示例***

![Duplicate entities in the node browser have numbers appended to the name.](/images/user-guide/cinematics/cinematics-component-entities-nodes-track-view-editor-3.png)

## 从组件实体中添加或删除组件

当您在 O3DE 编辑器中为一个组件实体添加组件时，该组件会自动添加到轨迹视图中的任何组件实体节点上。移除组件时，组件和任何动画数据也会从轨迹视图中移除。

{{< important >}}
从组件实体中移除组件时要小心，因为这可能会影响现有序列。例如，如果从作为序列一部分的实体中移除**Simple Motion**组件，动画将不再引用指定的动画。
{{< /important >}}

## 动画组件实体上的组件

可以制作动画的组件节点作为子节点嵌套在相关的组件实体节点下。你可以为这些组件节点添加动画轨迹。

**为组件节点添加动画轨迹**

1. 在 “轨迹视图 ”中，选择或创建一个序列。

1. 在节点浏览器中，右键单击组件节点并选择**Add Track**，然后选择所需的轨道。

    {{< note >}}
   某些组件仅支持数量有限的轨道，这些轨道可以在序列中进行动画处理。有关组件特定属性的更多信息，请参阅 [组件参考](/docs/user-guide/components/reference/)。

并非所有组件都能在轨迹视图中显示动画。有关在轨迹视图中添加节点支持的更多信息，请参阅 [将自定义组件显示在轨迹视图中以制作动画](/docs/user-guide/programming/components/expose-animation)。
{{< /note >}}

将组件实体节点添加到序列中并指定要制作动画的轨道后，就可以在时间轴上添加关键帧了。在关键帧中，你可以指定要在时间轴的哪个位置制作动画，并编辑其属性。

更多信息，请参阅 [在轨道上添加和删除动画关键帧](/docs/user-guide/visualization/cinematics/adding-removing-animation-keys-on-tracks)。
