---
linkTitle: Entity Inspector
title: Entity Inspector
description: 在 Open 3D Engine (O3DE) 中使用Entity Inspector为实体添加组件并修改其属性。
weight: 300
---

**Entity Inspector**管理每个实体的所有组件。在**Entity Outliner**或视口中选择一个实体，就能在**Entity Inspector**中看到所附的组件。

使用**Entity Inspector**进行以下操作：

- 为实体添加组件
- 修改组件属性
- 删除、复制、剪切和粘贴组件
- 设置实体状态
- 自定义实体图标
- 钉住实体的检查器
- 为自定义组件创建自己的帮助主题

{{< note >}}
有关可用组件的列表和说明，请参阅[组件参考](/docs/user-guide/components/reference)。您还可以点击每个组件标题下的**Help**图标，打开帮助主题。
{{< /note >}}

## 打开Entity Inspector

**打开Entity Inspector**

1. 在 O3DE 编辑器中，选择 **Tools**，**Entity Inspector**。

1. 在视口或**Entity Outliner**中选择一个实体。

1. 在**Entity Inspector**中，您可以看到以下内容：

  * **Name** - 实体名称。您可以为实体输入不同的名称。
  * **Entity Icon** - 可定制的图标，帮助你识别视口中的实体。
  * **Status** - 实体的活动状态。关卡开始时，实体可能处于活动、非活动或仅在编辑器模式下处于活动状态。
  * **Entity ID** - 如果该实体 ID 在消息、错误或断言中被调用，您可以在 **Entity Outliner**
  * 中搜索该实体。
  * 实体的附属组件显示在下方。

![Find entities and its attached components in the Entity Inspector.](/images/user-guide/component/entity_system/component-entity-inspector.png)

## 设置实体状态

默认情况下，实体在关卡中以激活状态开始。创建游戏时，您可以指定实体保持非活动状态，直到通过脚本或玩家操作等机制激活为止。如果您想在游戏模式中禁用实体，或想为在游戏中工作的用户创建测试实体或视觉注释，也可以将实体设置为编辑器。

**设置实体的状态**

1. 在**Entity Outliner**或视口中，选择一个实体。

1. 在**Entity Inspector**中，选择**Status**下拉菜单，选择以下一个选项：
      - **Start active** - 关卡开始时，实体处于激活状态。
      - **Start inactive** - 关卡开始时，实体处于非激活状态。
      - **Editor only** - 实体仅在编辑器模式下激活。

      ![Specify whether component is active, inactive, or active in editor mode only.](/images/shared/shared-component-entity-inspector-startactive.png)

1. 将实体设置为**Start Inactive**或**Editor only**时，选择实体可在**Entity Outliner**和视口中查看其状态。

    **示例 Start Inactive**

   不活动的实体会有一个删除线图标，视口中也会显示不活动的文本。

    ![Specify whether component is active, inactive, or active in editor mode only.](/images/shared/shared-component-entity-inspector-inactive-example.png)
    
    **示例 Editor only**

   只显示编辑器实体的图标没有阴影，视口中只显示编辑器文本。

    ![Specify whether component is active, inactive, or active in editor mode only.](/images/shared/shared-component-entity-inspector-editor-only-example.png)
   
## 固定Entity Inspector

您可以固定一个实体的检查器，使其在您选择另一个实体时仍保持打开和可见状态。您可以固定多个实体的检查器，也可以固定同一实体的多个检查器实例。这可以帮助您将实体及其组件相互比较。

Entity Inspector具有以下功能：
- 即使您选择了其他实体，也会始终显示已固定的实体。
- 功能与主**Entity Inspector**窗口类似。
- 打开不同的层或退出 O3DE 时关闭。
- 如果将松散实体转换为切片，固定的检查器会指向与之前松散实体相对应的新切片实体。
- 在进入和退出游戏模式时持续存在。
- 当您修改某个实体时，会更新该实体的所有固定检查器。

你可以从**Entity Outliner** 或 **Entity Inspector**固定一个检查器。

**要固定一个检查器**

1. 选择一个实体。

1. 请执行以下操作之一：

     1. 在**Entity Outliner**中，右击该实体，然后选择**Open pinned Inspector**。

         ![In the Entity Outliner, choose Open pinned Inspector to pin an inspector for that entity.](/images/user-guide/component/entity_system/component-entity-inspector-pin-1.png)

     1. 在**Entity Inspector**中，点击图钉图标。
   
         ![In the Entity Inspector, click the pin icon to pin an inspector for the entity.](/images/user-guide/component/entity_system/component-entity-inspector-pin-2.png)

1. 在O3DE编辑器中，你可以查看固定的Entity Inspector。

    **示例**

    ![Multiple pinned inspectors open in a level.](/images/user-guide/component/entity_system/component-entity-inspector-pin.png)

## 自定义实体图标

没有添加任何组件的实体的默认图标是**Transform**( ![Transform Component Icon](/images/user-guide/component/entity_system/entity-inspector-transform-icon.png))组件的图标。当你添加另一个组件时，该图标会变成你添加到该实体的第一个组件。

您还可以指定自己的图标。

**自定义实体图标**

1. 在**Entity Inspector**中，单击顶部的图标图像。

1. 选择**Set custom icon**。

     !['Set custom icon' menu item](/images/user-guide/component/entity_system/component-working-customize.png)

1. 从你的游戏项目目录选择一个图标。
