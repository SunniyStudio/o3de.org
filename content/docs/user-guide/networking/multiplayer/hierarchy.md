---
linktitle: 网络实体层次结构
title: 多人游戏网络实体层次结构
description: 在 Open 3D Engine （O3DE） 中创建网络实体层次结构的参考。
weight: 600
---

*网络层次结构* 提供了一种将相关实体分组到一个逻辑组下的便捷方法。此类层次结构中的实体将其网络输入作为一个整体处理，包括其输入的创建、处理和回滚。

{{<important>}}
网络层次结构专为一小组实体而设计。

默认情况下，限制设置为 16 个实体。您可以使用 CVar 修改此限制： `bg_hierarchyEntityMaxLimit`.
{{</important>}}

## 分层组件

为了声明网络层次结构，您必须使用分层组件的组合：`NetworkHierarchyRootComponent`和`NetworkHierarchyChildComponent`。`NetworkHierarchyRootComponent`将实体标记为层次结构的根，并且应将其放置在实体预制件或一组实体的最高父级上。`NetworkHierarchyRootComponent`将实体标记为层次结构的子实体。应将其添加到具有 `NetworkHierarchyRootComponent`的实体下的子实体上。

可以在 Editor 中创建网络层次结构，也可以通过在运行时为实体设置父子关系来创建。

### 在编辑器中创建网络层次结构

1. 创建具有所有网络通用组件的网络实体：
    - Network Binding 组件
    - Network Transform 组件


1. 在 Editor 中添加 Mesh 组件以获取视觉信息。

1. 以下是一个简单的网络实体示例：

    ![Starting Network Entity](/images/user-guide/networking/multiplayer/starting_network_entity.png)

1. 要使其成为分层根实体，请添加 **Network Hierarchy Root** 组件：

    ![Starting Hierarchy Root Entity](/images/user-guide/networking/multiplayer/starting_hierarchy_root_entity.png)

    生成的实体是分层子实体的父实体。

1. 通过添加 **Network Hierarchy Child** 组件来形成分层子实体：

    ![Starting Hierarchy Child Entity](/images/user-guide/networking/multiplayer/starting_hierarchy_child_entity.png)

    请注意，此实体是根实体的子实体。

    ![Simple Hierarchy Example](/images/user-guide/networking/multiplayer/simple_hierarchy.png)

    通常，层次结构是您希望在运行时生成的一组实体。在这些情况下，您希望从实体中创建预制件。

    ![Simple Hierarchy Prefab](/images/user-guide/networking/multiplayer/simple_hierarchy_prefab.png)

    具有 **Network Hierarchy Root** 组件的实体必须位于预制件中的顶部父实体，这一点很重要。所有其他实体上都应具有 **Network Hierarchy Child** 组件。


### ImGui 层次结构调试器

O3DE 多人游戏 Gem 附带一个调试器，可帮助您对 O3DE 应用程序中的层次结构进行故障排除和分析。可以通过调出 ImGui 菜单（默认为 **HOME** 键），然后选择 **Multiplayer** > **Multiplayer Hierarchy Debugger** 来启用它。启用层次结构调试器后，您将能够浏览层次结构，并通过层次结构的根实体在世界中获取调试文本。

使用此调试器，您可以找到有关层次结构的以下信息：
- 其根实体名称
- 层次结构中的实体总数
- 层次结构中每个成员的实体名称
- 每个成员的 `AZ::EntityId`
- 每个成员的网络 ID，`Multiplayer::NetEntityId`
- 它在层次结构中的分层角色，可以是 child、root 或在嵌套层次结构中充当 child 的内部根。


### 将输入处理添加到网络层次结构中

任何具有执行多人游戏输入处理组件的组件的实体都需要具有：
- **Local Prediction Input**
- **Network Hierarchy Root**
- **Network Hierarchy Child**

例如，下面是一个定义中包含`NetworkInput`字段的示例组件。

```xml
<?xml version="1.0"?>

<Component
    Name="NetworkInputProcessingExampleComponent"
    Namespace="MultiplayerSample"
    OverrideComponent="false"
    OverrideController="false"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

    <NetworkInput Type="float" Name="InputValue" Init="0.0f" />

</Component>
```

添加一个组件以提供多人游戏输入处理：

![Hierarchy Debug Overlay](/images/user-guide/networking/multiplayer/hierarchy_child_entity_with_input_processing_component.png)

作为一般规则，层次结构必须在其顶部父实体中有一个 **Local Prediction Input** 组件才能处理多人游戏输入。但是，分层子实体只需要一个 **Network Hierarchy Child** 组件来处理其多人游戏输入。


### 在运行时创建网络层次结构

每当具有层次结构根或层次结构子组件的实体之间的父子关系发生变化时，网络层次结构就会在运行时自动更新、创建和解散。当使用`AZ::TransformBus`事件总线更改实体的附件时，层次结构将在运行时更新。

### 层次结构的层次结构

通过将一个层次结构的根附加到另一个层次结构中的实体，可以从多个层次结构中创建层次结构。在这种情况下，内部根组件将成为此新父级的分层子级，直到它与层次结构分离。当使用不同的预制件构建车辆、武器和玩家时，这非常有用，这些预制件在运行时根据需要附加和分离。


{{<note>}}
有关网络层次结构的更多信息，请阅读 [ServerHierarchyTests 源](https://github.com/o3de/o3de/blob/development/Gems/Multiplayer/Code/Tests/ServerHierarchyTests.cpp)。
{{</note>}}
