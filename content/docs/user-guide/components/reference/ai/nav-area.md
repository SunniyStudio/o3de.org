---
linkTitle: Navigation Area
description: ' 使用 Navigation Area组件和Polygon Prism Shape 组件 在 Open 3D Engine中创建导航网格。'
title: Navigation Area 组件
draft: True
---



**Navigation Area**组件定义了供人工智能系统使用的可导航区域或体积的特征。该组件与**[Polygon Prism Shape](/docs/user-guide/components/reference/shape/polygon-prism-shape/)**组件一起使用，后者定义了导航区域的体积。

{{< note >}}
当你添加**Navigation Area**组件时，还必须添加**[Polygon Prism Shape](/docs/user-guide/components/reference/shape/polygon-prism-shape/)**组件。
{{< /note >}}

对于如何调整**Polygon Prism Shape**组件，请参阅[Polygon Prism Shape](/docs/user-guide/components/reference/shape/polygon-prism-shape/)。

创建**Navigation Area**时，在[渲染导航网格](#render-navigation-mesh)时，指定AI [**Agent Type**](#component-nav-area-properties)可穿越的所有区域都会显示为蓝色。任何不可穿越的区域都会显示为空白区域，例如深坑和陡坡。禁区和静态物体周围的区域也会呈现为空白区域。即使导航区域被静态物体、地形特征和禁区分割成不同的部分，每个可穿越区域也会呈现蓝色。

![Navigation Area](/images/user-guide/component/component-navigation-mesh.png)

您可以使用 **[Navigation Seed](/docs/user-guide/components/reference/ai/nav-seed/)** 组件来微调AI的可访问性。

**要添加Navigation Area** {#create-navigation-area}

1. 在视口中，靠近要创建导航区域的位置，右键单击并选择 **Create entity**。

1. 通过在**Entity Outliner**中选中新实体，向它添加**Navigation Area**组件。

![Navigation Area component properties.](/images/user-guide/component/component-nav-area-1.png)

1. 在**Navigation Area**组件中，在**Agent Types**旁，点击 **+**。

   在 **\[0\]**旁，选择**MediumSizedCharacters**。此属性定义可以在此区域内可以导航的 [AI代理类型](#component-nav-area-properties)。

1. 添加**Polygon Prism** 组件。调整**Polygon Prism**的[大小、形状和高度](/docs/user-guide/components/reference/shape/polygon-prism-shape)。确保您的地形和物体与多边形棱柱的体积相交。

   如果您的多边形棱镜悬浮在地形上方，并且没有与地形完全相交，导航系统就无法生成适当的可穿越区域。下面的示例显示了一个高于地形过高的导航区域（1）和一个适当位于地形上的导航区域（2）。如果导航区域过高，请使用**移动**工具降低实体的 Z（上下）位置。

   ![Enable Show Navigation Areas and View Agent Type in O3DE Editor.](/images/user-guide/component/component-nav-area.png)

**要查看生成的导航区域网格** {#render-navigation-mesh}

1. 在 O3DE 编辑器中，选择 **Game**, **AI**, **Show Navigation Areas**.

1. 在 O3DE 编辑器中，选择 **Game**, **AI**, **View Agent Type**，然后启用要显示的AI代理类型。

1. 在 O3DE 编辑器中，选择 **Game**, **AI**, **Continuous Update** 来显示您修改地形或区域时导航网格的变化。

![Enable Show Navigation Areas, View Agent Type, and Continuous Update in O3DE Editor.](/images/user-guide/component/component-nav-area-gameai-menu-items.png)

导航网格以蓝色显示可穿越区域。

## Navigation Area 组件属性

**Navigation Area** 组件具有以下属性： {#component-nav-area-properties-agent-types}

**Agent Types**
指定可以穿越此导航区域的人工智能类型。这些代理类型在项目的 `Scripts\AI\Navigation.xml` 文件中定义。要为该区域指定多种代理类型，请点击 **+** 图标。
使用该属性可以限制哪些代理可以在该区域内导航。例如，您可以允许角色在狭窄的走廊内导航，但限制车辆。
要在人工智能上定义代理类型，请参阅 [Navigation](/docs/user-guide/components/reference/ai/navigation/) 组件。

**Exclusion**
选中时，创建一个减法导航区域。这将在现有导航网格内创建一个切口。更多信息，请参阅 [创建导航网格禁区](#component-nav-area-exclusion)。

## 围绕静态物体导航

当 O3DE 创建导航网格时，它会自动排除一些不应该被穿越的区域，例如大石头或树干。为确保导航系统能正确检测到这些区域，您必须将其**Transform**组件指定为静态。

**将实体标记为静态**

1. 在**Entity Outliner**中，选择实体。这可以是一棵树、一块巨石、一栋建筑或任何你不想让AI穿过的物体。

1. 在**[Transform](/docs/user-guide/components/reference/transform/)**组件中，选择**Static** 属性。

   下面的示例显示了未选择**Static**属性时巨石周围的导航网格。

![Navigation mesh goes through the boulder because Static is not selected on the boulder's Transform component](/images/user-guide/component/component-nav-area-4.png)

下面的示例显示了相同的导航网格，但在巨石上选择了**Static**属性。

![Navigation mesh creates an exclusion area around the boulder, because Static is selected on the boulder's Transform component](/images/user-guide/component/component-nav-area-5.png)

## 创建导航网格排除区域

您可以使用**Navigation Area**组件来手动创建导航网格的排除区域。这意味着人工智能代理无法穿越这些区域。为此，您需要创建一个导航区域并选择**Exclusion**属性，如下图所示。

![Select the Exclusion property to create a navigation area that subtracts from the navigation mesh](/images/user-guide/component/component-nav-area-8.png)

下面的示例显示了导航网格（1）和带有排除区域（2）的导航网格。

![Navigation Area component, when marked as an exclusion, subtracts or creates a hole or non-traversable area in the navigation mesh](/images/user-guide/component/component-nav-area-6.png)

**要创建一个排除区域**

1. 如果还没有，您必须首先 [创建一个导航区域](#create-navigation-area)，该区域不能是排除区域。

1. 创建一个实体，向它添加 **Navigation Area** 和 **Polygon Prism Shape** 组件。

1. 在 **Navigation Area** 组件中，选择 **Exclusion** 属性。

1. 在 **Viewport**中，在导航网格中放置排除区域。

1. 将多边形塑造成禁区的首选形状。

## 导航物理集成 

导航系统根据物理系统提供的所有静态物理碰撞体（包括地形）构建导航网格。

**物理集成详情**
启用`AZ::Physics`集成模式后，导航网格体素生成器会发出`WorldRequestBus::Overlap`静态查询，以收集边界框内的碰撞体。新的 `AZ::Shape::GetGeometry()` 方法会返回形状几何体。地形、形状和网格的 PhysX 碰撞体将在获取几何体数据的同时获取 PhysX 场景读取锁的操作中提供三角形数据。

## NavigationAreaRequestBus EBus 接口 

使用以下带有 `NavigationAreaRequestBus` EBus 接口的请求函数与游戏中的其他组件进行通信。

有关使用事件总线（EBus）接口的更多信息，请参阅 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus/)

### RefreshArea 

您可以使用 [PolygonPrismShapeComponentRequestBus](/docs/user-guide/components/reference/shape/polygon-prism-shape/#polygonprismshapecomponentrequestbus) 通过添加、删除和更新顶点位置来修改多边形棱镜区域。更改区域后，使用 `RefreshArea` 更新导航区域。

**参数**
无

**返回值**
`AZ::ConstPolygonPrismPtr`

**可脚本化**
否

{{< note >}}
Navigation Area 组件依赖于 Polygon Prism组件，而Polygon Prism组件也使用`VertexContainer`函数。更多信息，请参阅 [顶点容器](/docs/user-guide/components/reference/shape/vertex-container/)。
{{< /note >}}
