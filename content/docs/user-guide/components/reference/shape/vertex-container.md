---
title: Vertex Container Type
linktitle: Vertex Container
description: ' 使用 Open 3D Engine （O3DE） 中的顶点容器来访问、更新和删除顶点。 '
weight: 500
---



`VertexContainer`是一种具体类型和接口，由多个 **Open 3D Engine （O3DE）** 组件总线使用。它由 [Polygon Prism Shape](/docs/user-guide/components/reference/shape/polygon-prism-shape/) 组件和 [Spline](/docs/user-guide/components/reference/shape/spline/) 组件直接实现，并由 [Navigation Area](/docs/user-guide/components/reference/ai/nav-area) 组件间接使用。

## Vertex Container 接口

顶点容器提供访问、修改、添加和删除顶点的功能。顶点容器具有多个相互构建的接口，包括：

### FixedVertices 接口

'`FixedVertices`' 支持容器中顶点的 '`GetVertex`' 和 '`UpdateVertex`' 函数，以及返回容器中顶点数的 '`Size`' 函数。

由于顶点数是固定的，因此无法添加或删除顶点。您可以使用数组或 '`AZStd::fixed_vector`' 实现此接口。

### VariableVertices 接口

'`VariableVertices`' 接口支持 '`FixedVertices`' 的所有功能，但也提供 '`AddVertex`'、'`InsertVertex`' 和 '`RemoveVertex`' 函数。'`VariableVertices`' 还提供了一个名为 '`Empty`' 的实用函数，用于检查容器是否包含任何元素或为空。

要实现 '`VariableVertices`' 接口，你可以使用 '`AZSt::vector`' 或 '`VertexContainer`'。

### VertexContainerInterface

'`VertexContainerInterface`' 为前两个接口和 '`VertexContainer`' 类型提供的所有功能提供了一个接口。为方便起见，'`VertexContainerInterface`' 还提供了 '`SetVertices`' 和 '`ClearVertices`' 函数，它们可以在一个操作中更新所有顶点或删除所有顶点。'`SetVertices`' 函数采用一个 '`vertices`' 参数，该参数包含要存储的所有顶点的列表。

{{< note >}}
'`VertexContainer`' 拥有它有权访问的顶点;它们不会存储在其他位置（'`VertexContainer`' 不是视图）。
{{< /note >}}

有关“`VertexContainerInterface`”中的接口的详细信息，请参阅`o3de\Code\Framework\AzCore\AzCore\Math\VertexContainerInterface.h`文件中的代码和代码注释。

有关“`VertexContainer`”类型的详细信息，请参阅`o3de\Code\Framework\AzCore\AzCore\Math\VertexContainer.h`文件中的代码和代码注释。

{{< note >}}
'`VertexContainer`' 可以存储 '`Vector2`' 或 '`Vector3`' 类型。向量类型是在创建类型时在编译时确定的。这对于某些不允许在 Z （垂直） 轴上修改点并且仅在二维中处理点的组件非常有用。Polygon Prism Shape （多边形棱柱形状） 组件需要 '`Vector2`' 类型。
{{< /note >}}
