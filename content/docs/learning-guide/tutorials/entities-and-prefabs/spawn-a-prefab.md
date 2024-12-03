---
linktitle: 生成和取消生成
title: 生成和取消生成预制件
description: 学习在 Open 3D Engine （O3DE） 中使用 Script Canvas 生成和取消生成预制件。
weight: 400
toc: true
---

在本教程中，您将学习如何在 Open 3D Engine （O3DE） 中生成和取消消失预制件实例。您将创建一个要生成的预制件，创建一个生成器实体，使用 [Script Canvas Editor]](/docs/user-guide/scripting/script-canvas/get-started/editor-interface) 组装一个生成脚本，并测试结果。在完成本教程之前，您应该熟悉 [实体和预制件基础知识](entity-and-prefab-basics)。

| O3DE 体验 |完成时间 |功能聚焦 |最后更新 |
|--- |--- |--- |--- |
|初级 |25 分钟 |使用 Script Canvas 生成和取消消失预制件。 | January 13, 2023 |

## 先决条件

* 具备使用 [O3DE 编辑器](/docs/user-guide/editor) 的基本知识。
* 具备使用 [Script Canvas 编辑器](/docs/user-guide/scripting/script-canvas/editor-reference) 的基本知识。
* 从标准项目模板构建的项目，或包含标准模板中的 Gem 的项目。

## 创建预制件

按照本节中的步骤创建要生成的预制件。如果您已经有用于本教程的预制件，请跳到下一部分 [创建一个生成器实体](#create-a-spawner-entity)。

1. 在 **O3DE 编辑器 ** 中，创建具有 **Mesh** 组件的实体。您可以在本教程中使用任何可用的网格。将 `.azmodel` 从 **Asset Browser** 拖到视区中，以创建具有 **Mesh** 组件的实体。

1. 右键单击 **Entity Outliner** 中的新实体，然后从上下文菜单中选择 **Create Prefab...**。

    {{< image-width "/images/learning-guide/tutorials/entities-and-prefabs/create-prefab.png" "450" "Creating a prefab from an entity." >}}

1. 在 **Save As...** 窗口中，导航到项目中的目录（如 `Prefabs`目录），提供预制件的名称，然后选择 **Save**。

1. 您创建的实体已转换为您刚刚保存的预制件的实例。您将创建一个脚本来生成预制件实例，以便您可以从关卡中删除此实例。在 Entity Outliner 中选择预制件实例，然后按 **Delete** 将其删除。

## 创建一个生成器实体

在本节中，您将创建一个实体，用于运行生成预制件实例的脚本。

1. 创建一个名为 `Spawner`的新实体。

1. 通过使用平移 Gizmo 在视区中拖动实体，确保将新实体放置在摄像机视图中。您可以通过选择视区顶部的 {{< icon helpers.svg >}} **Helpers** 按钮并从列表中选择 **Helpers** 来显示摄像机的视图视锥体。

    {{< image-width "/images/learning-guide/tutorials/entities-and-prefabs/viewport-helpers.png" "900" "Placing an entity in the camera frustum." >}}

1. 将 **Script Canvas** 组件添加到生成器实体。在 **Entity Inspector** 中，选择 **Add Component**，然后从组件列表中选择 **Script Canvas**。

1. 按 **Ctrl + S** 保存带有生成器实体的关卡。

1. 在 **Script Canvas** 组件中，点击 {{< icon open-in-internal-app.svg >}} **Open application** 按钮打开 Script Canvas 编辑器。

    ![Open Script Canvas Editor from the Script Canvas component.](/images/learning-guide/tutorials/entities-and-prefabs/open-script-canvas-editor.png)

## 生成预制件

在本节中，您将创建一个生成预制件的脚本。您将学习 Script Canvas Editor 的基础知识，包括创建变量、放置和连接节点以及使用引用。

1. Script Canvas Editor 将打开一个空的图形画布窗口。对于此脚本，您将使用 **On Graph Start** 作为入口点，以便在激活生成器实体时脚本逻辑立即运行。在 **Node Palette** 中，展开 **Timing** 组。将 **On Graph Start** 节点拖动到图形画布窗口中。Script Canvas 会自动创建一个新的节点图。

    {{< video src="/images/learning-guide/tutorials/entities-and-prefabs/on-graph-start.mp4" info="Add On Graph Start node in script canvas." autoplay="true" loop="true" >}}

1. 该脚本需要引用要生成的预制件。通过以下步骤创建变量以引用预制件：

    1. 在 **Variable Manager**中，选择 **Create Variable**, 然后从变量类型列表中选择 **SpawnableScriptAssetRef**。

    2. 为**SpawnableScriptAssetRef**变量命名。在**Node Inspector**中，在**Name**属性中输入`PrefabRef`。

    3. 将要生成的预制件添加到变量中。在 Node Inspector 中，选择 {{< icon file-folder.svg >}} **File** 按钮，然后选择要为 **asset** 属性生成的预制件。

    ![Setting up a SpawnableScriptAssetRef variable.](/images/learning-guide/tutorials/entities-and-prefabs/prefabref-variable.png)

1. 该脚本需要一个变量，该变量保存有关生成的预制件实例的信息。使用以下步骤创建一个：

    1. 在 Variable Manager （变量管理器） 中，选择 **Create Variable（创建变量）**，然后从变量类型列表中选择 **EntitySpawnTicket**。

    2. 为 **EntitySpawnTicket** 变量命名。在**Node Inspector**中，在**Name**属性中输入`SpawnTicket`。

1. 将 **Create Spawn Ticket** 节点添加到图形画布。使用 Node Palette （节点调色板） 顶部的搜索字段筛选节点列表。将 **Create Spawn Ticket** 节点拖动到 **On Graph Start** 节点右侧的图形画布中。

    {{< note >}}您还可以在图形画布中右键单击以访问节点列表。
    {{< /note >}}

1. 连接节点。从 **On Graph Start** 节点上的 **Out** 引脚拖动到 **Create Spawn Ticket** 节点上的 **Create Ticket** 引脚以连接节点。

    {{< video src="/images/learning-guide/tutorials/entities-and-prefabs/connect-nodes.mp4" info="Click and drag to connect Script Canvas nodes." autoplay="true" loop="true" >}}

1. 将预制件引用添加到 **Create Spawn Ticket** 节点。右键单击 **Prefab** 引脚，然后从上下文菜单中选择 **Convert to Reference**。该属性将自动设置为您在步骤 1 中创建的 **PrefabRef** 变量。如有必要，您可以选择 **Prefab**字段中的 {{< icon settings.svg>}} **Settings** 按钮来选择变量引用。

    ![Set the prefab reference on the Create Spawn Ticket Script Canvas Node.](/images/learning-guide/tutorials/entities-and-prefabs/set-prefab-reference.png)

1. 将 **Prefab/Spawning** 节点组中的 **Spawn** 节点添加到图形画布，并通过以下步骤将其连接到 **Create Spawn Ticket** 节点：

    1. 将 **Create Spawn Ticket** 节点的 **Ticket Created** 引脚连接到 **Spawn** 节点的 **Request Spawn** 引脚。

    2. 将 **Create Spawn Ticket** 节点的 **SpawnTicket** 引脚连接到 **Spawn** 节点的 **SpawnTicket** 引脚。

    {{< image-width "/images/learning-guide/tutorials/entities-and-prefabs/connect-spawn-node.png" "900" "Connect the Spawn node to the Create Spawn Ticket node.">}}

1. 除了生成预制件实例外，**Spawn** 节点还需要使用数据填充 **SpawnTicket** 变量。在 **Spawn** 节点上，右键单击 **SpawnTicketOut** 引脚，然后从上下文菜单中选择 **Convert to Reference**。该属性将自动设置为您在步骤 2 中创建的 **SpawnTicket** 变量。如有必要，您可以选择 **SpawnTicketOut** 字段中的 {{< icon settings.svg>}} **Settings** 按钮来选择变量引用。

    ![Set the prefab reference on a Create Spawn Ticket Script Canvas Node.](/images/learning-guide/tutorials/entities-and-prefabs/spawn-ticket-out-ref.png)

1. 按 **Ctrl + S** 保存脚本。在 **Save As** 窗口中，将文件命名为`spawn-despawn-prefab.scriptcanvas`，然后选择 **Save**。

到目前为止，您创建的脚本在激活时会在生成器实体的位置生成一个预制件实例。您可以将脚本添加到生成器实体的 **Script Canvas** 节点，然后按 **Ctrl + G** 来测试脚本。在本教程的最后步骤中，您将学习如何取消 prefab。

## 延迟和消失

在本节中，您将为脚本添加 3 秒的延迟，之后，预制件将消失。

1. 将 **Timing** 组中的 **Delay** 节点添加到图形画布中，并通过以下步骤进行连接：

    1. 将 **Spawn** 节点的 **On Spawn Completed** 引脚连接到 **Delay** 节点的 **Start** 引脚。

    1. 在 **Delay** 节点上，将 **Start Time** 属性设置为“3”以创建 3 秒的延迟。

1. 将 **Prefab/Spawning** 组中的 **Despawn** 节点添加到图形画布，并通过以下步骤进行连接：

    1. 将 **Delay** 节点的 **Done** 引脚连接到 **Despawn** 节点的 **Request Despawn** 引脚。

    1. 在 **Despawn** 节点上，右键单击 **SpawnTicket** 引脚，然后从上下文菜单中选择 **Convert to Reference**。该属性将自动设置为您创建的 **SpawnTicket** 变量。如有必要，您可以选择 **SpawnTicket** 字段中的 {{< icon settings.svg>}} **Settings** 按钮来选择变量引用。

1. 按 **Ctrl + S** 保存脚本。下图显示了完整的 Script Canvas 图表。

    {{< image-width "/images/learning-guide/tutorials/entities-and-prefabs/final-script-canvas.png" "900" "The final script canvas graph.">}}

## 测试结果

现在，您将脚本添加到 spawner 实体并测试脚本。

1. 在 O3DE 编辑器中，选择生成器实体。

1. 在 Entity Inspector 的 **Script Canvas** 组件中，选择 {{< icon browse-edit-select-files.svg >}} 文件浏览器图标，然后选择您创建的`spawn-despawn-prefab.scriptcanvas` 文件。

    ![Set the script canvas file property](/images/learning-guide/tutorials/entities-and-prefabs/set-script-canvas-source.png)

1. 按 **Ctrl + G** 运行关卡并测试脚本。

    {{< video src="/images/learning-guide/tutorials/entities-and-prefabs/spawn-despawn-finished.mp4" info="Click and drag to connect Script Canvas nodes." autoplay="true" loop="true" >}}

## 结论和下一步

祝贺！您已经学习了如何生成和取消生成预制件实例。这些是在 O3DE 中工作的关键概念。以下是基于您在本教程中学到的知识进行构建的一些想法：

* 您能否在取消预制件实例之前随机化延迟？
* 看看您是否能弄清楚如何在您选择的位置生成预制件实例。提示：在 **Spawn** Script Canvas 节点上使用变换属性。
* 是否可以从单个生成器实体在不同位置生成多个预制件实例？
