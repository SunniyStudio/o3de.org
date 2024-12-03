---
linkTitle: 2D 导航
title: 使用 Kythera AI 进行 2D 导航
description: 在 Open 3D Engine （O3DE） 中使用 Kythera AI Gem 进行 2D 导航的教程
weight: 100
toc: true
---

本教程介绍如何使用 [Kythera AI Gem](/docs/user-guide/gems/reference/kythera-ai/)  创建一个可由 Kythera AI 代理导航的关卡区域，以及一个遵循可视化脚本化行为树的简单 AI 代理。

您需要从自己的项目开始，并启用 Kythera AI Gem，或者从 Kythera AI Demo 项目开始。有关设置 Kythera AI 演示项目的信息，请参阅 [Kythera AI Gem 设置](/docs/user-guide/gems/reference/kythera-ai/kythera-ai-gem-setup/)。

## 配置文件

要使此示例正常工作，您首先需要创建两个 XML 配置文件，一个用于代理行为的 *Profile*，以及一个用于定义导航区域参数的 *NavMesh*。

### Profile

AI 代理的行为在其配置文件中定义。配置文件中最重要的设置之一是代理的默认 *行为树*，它是一个可视化脚本化的“程序”，供代理运行。

配置文件以 XML 格式在项目的`Scripts`目录的`Profiles.xml`中定义。打开（或创建）此文件并添加新配置文件`WanderProfile`：

```xml
<Profiles>
    <WanderProfile type="bb">
        <CharacterFeatures type="bb">
            <GroundNav type="bool">true</GroundNav>
            <GroundAvoidance type="bool">true</GroundAvoidance>
            <Movement type="bool">true</Movement>
            <Behavior type="bool">true</Behavior>
        </CharacterFeatures>
        <DefaultBehavior type="string">WanderBehavior</DefaultBehavior>
    </WanderProfile>
</Profiles>
```

如果您在编辑器或游戏运行时更改了 `Profiles.xml`，则可以使用 console 命令重新加载文件`kyt_LoadProfiles`.

### 导航网格

2D 代理在 *导航网格* （导航网格） 上运行，该网格是一个 2D 图块网络，用于定义关卡的可导航区域。Kythera AI 根据`NavMesh.xml`中定义的参数自动生成这些导航网格。Kythera AI 允许不同类型的代理使用多种类型的导航网格。例如，大型生物的导航网格类型会阻止它们在小空间中导航。

导航网格可以是全局的，这意味着它们在项目的所有级别中都可用，也可以是特定于级别的。全局`NavMesh.xml` 放置在项目的 `Scripts` 目录中。对于特定于关卡的导航网格，可以在关卡的子目录中放置一个`NavMesh.xml`。

```xml
<NavMeshParams>
    <DefaultNavMeshName type="string">Default</DefaultNavMeshName>
    <DefaultNavMeshType type="string">MediumSizedCharacters</DefaultNavMeshType>
    <NavMeshNames type="bb">
        <Default type="bba">
            <Entry0 type="string">MediumSizedCharacters</Entry0>
        </Default>
    </NavMeshNames>
    <NavMeshTypes type="bb">
        <MediumSizedCharacters type="bb">
            <AgentHeight type="float">1.720000</AgentHeight>
            <AgentMaxClimb type="float">0.700000</AgentMaxClimb>
            <AgentMaxSlope type="float">45.000000</AgentMaxSlope>
            <AgentRadius type="float">0.600000</AgentRadius>
            <CellHeight type="float">0.050000</CellHeight>
            <CellSize type="float">0.100000</CellSize>
        </MediumSizedCharacters>
    </NavMeshTypes>
</NavMeshParams>
```

## 创建一个关卡并引用行为树

1. 创建一个新的空关卡。

1. 删除 **ShaderBall** 实体。

1. 选择 **Grid** 实体，并向其添加新的 **PhysX Primitive Collider** 组件。将包含在 NavMesh 中的所有实体都需要 PhysX 碰撞器。

    1. 在碰撞器上，将 shape 属性设置为 `Box`.

    1. 在碰撞器上，将 X 和 Y scale （X 和 Y 缩放） 设置为 '`32`'，以便框包含网格。

    1. 如果你激活了Debug Helpers (视口右上角的 ![debug helper icon](/images/learning-guide/tutorials/ai/debug-helpers.png)) 并且 **Draw Collider** （绘制碰撞器） 选项在 PhysX Primitive collider （PhysX 基元碰撞器） 组件上处于活动状态，您应该会看到碰撞器调试可视化效果：
    
        ![PhysX collider debug draw](/images/learning-guide/tutorials/ai/collider.png)

1. 创建名为 `NavBounds` 的新实体。这将定义生成导航网格的关卡区域。

    1. 在 entity Inspector 中添加 Kythera **NavMesh Bounds** 组件。

    1. 添加 **Polygon Prism Shape** 组件，然后选择 **Edit** 按钮以进入棱柱的编辑模式。

    1. 使用四个红色圆圈增加棱柱的大小，使其包含大部分网格。
    
    1. 增加棱柱的高度，使蓝色箭头位于中心。

        ![Navmesh boundaries](/images/learning-guide/tutorials/ai/navmesh-boundaries-edit.png)
    
    1. 选择 **Done** 退出编辑模式。

1. Kythera AI 工具栏具有本教程其余部分所需的一些功能。右键单击 O3DE 工具栏，然后选择 Kythera AI 工具栏，如果 Kythera AI 工具栏不可见，则添加它。
    
    ![Kythera AI toolbar](/images/learning-guide/tutorials/ai/kythera-toolbar.png)

1. 在 Kythera 工具栏的下拉菜单中激活`Basic Debug Draw`。这是工具栏中的第一个下拉列表，默认显示`NavMesh Dbg off`。

1. 选择Kythera AI工具栏中的**Generate navmesh** 按钮 (![Generate navmesh icon](/images/user-guide/gems/kythera-ai/toolbar-generate-navmesh.png))。

1. 创建名为 `Agent` 的新实体。这将是 AI 角色。

    1. 添加 **Mesh** 组件并选择任何可用的网格。您可以使用 AtomLyIntegration Gem 中的 `Lucy_low` 。

    1. 将 **Kythera** 组件添加到实体。此组件向 Kythera 注册实体。

    1. 将 Kythera **Agent** 组件添加到实体。
    
    1. 在 Agent 组件中，将 **Profile** 属性设置为 `WanderProfile`，如 `Profiles.xml` 中所定义。

    1. 添加 **SimpleMovementController** 组件。此组件通过实现`MovementRequestBus`，将来自 Kythera AI 的移动请求转换为 Agent 实体的移动。有关更多信息，请参阅 [角色移动 API](/docs/user-guide/gems/reference/kythera-ai/character-movement-apis).
    
{{< note >}}
尚不支持动画。
{{< /note >}}

## 使用 Inspector 创建行为树

[Inspector](/docs/user-guide/gems/reference/kythera-ai/introduction-to-the-inspector/) 是 Kythera AI 基于浏览器的调试器和行为树创作工具。初始化 Kythera Gem 时，Inspector Web 服务器将在本地计算机上启动。当 Kythera AI 运行时，Inspector 位于 [http://localhost:8081/](http://localhost:8081/)。

我们将定义一个非常基本的行为树，它在 NavMesh 上随机生成一个位置，然后移动到该位置。到达后，它将生成另一个位置并移动到那里，以无限循环的方式。

1. 打开 Inspector

1. 选择 **BT Editor** 选项卡。

1.  创建一个名为 `WanderBehavior` 的新行为树 (如 `Profiles.xml` 中的 `DefaultBehavior` 节点中引用的).

1. 添加 **Repeater** 节点。此节点执行 **iterations** 属性指定的子节点次数。默认情况下，iterations 设置为 '`0`'，这意味着无限迭代。保留默认值 '`0`'。

1. 添加 **Sequence** 节点作为 Repeater 节点的子节点。此节点将依次执行附加到它的所有子节点。但是，如果一个失败，执行将停止。

1. 添加 **Character_RandomPointInRange** 节点作为 Sequence 节点的子节点。此节点在导航网格上查找随机点。

    1. 设置 **Range** 为 `100`.

    1. 将 **Point** 输出设置为 '`NextPoint`'。这是包含找到的点的变量的名称。

1. 添加 '`Character_Goto`' 节点。此节点将角色移动到特定位置。此节点已完全集成到 Kythera 导航系统中，因此它将使用导航网格来寻向目标的路径。

    1. 设置 **Destination** 为 `NextPoint`.

    1. 设置 **Speed** 为 `5`.

完成的行为树应如下图所示：

![Behavior tree example](/images/learning-guide/tutorials/ai/behavior-tree.png)
        
返回编辑器，激活 Simulate （Ctrl+P） 并查看代理移动！

您还可以激活代理沿 Kythera AI 工具栏上的 navpaths 按钮(![navpaths button icon](/images/learning-guide/tutorials/ai/toolbar-navpaths.png))行走的路径的调试绘制 .

以下是完成的示例，其中 NavMesh 调试绘制设置从`Basic` 更改为 `Color Tiles`并激活了 Nav Path 调试绘制：

![2D Navigation tutorial finished](/images/learning-guide/tutorials/ai/finished.png)

## 故障排除

如果某些内容不起作用，您可以检查`<project directory>/user/log/Kythera.log`中的 Kythera AI 日志文件以获取更多信息。
