---
linkTitle: Entity Outliner
title: Entity Outliner
description: 使用 Open 3D Engine (O3DE) 中的Entity Outliner管理实体。
weight: 200
---

**Entity Outliner** 显示关卡中的所有实体和Prefab。您可以查看父级层次结构、锁定选择、在视口中显示和隐藏实体，并创建搜索过滤器。

## 打开Entity Outliner

1. 在O3DE编辑器中，选择**Tools**, **Entity Outliner**。

1. 在Entity Outliner中，您可以创建、选择、搜索和过滤实体。

   ![Use the Entity Outliner to view all the entities in your level.](/images/user-guide/editor/interface-entity-outliner.png)

   您可以在**Entity Outliner**中执行以下操作之一来选择多个实体：

- 使用 **Ctrl + 鼠标左键** 一次选择多个实体，使其成为多重选择的一部分。
- 使用 **Shift + 鼠标左键** 选择一系列实体。

## 对实体重新排序

在 Entity Outliner 中创建实体或实例化 prefab 时，如果没有选择任何实体，它们会显示在列表的底部。但如果选中了一个实体，新实体或预制件将成为选中实体的子实体。

1. 要移动实体，右键单击实体，然后选择**向上移动**或**向下移动**。

     ![Move an entity up and down the Entity Outliner.](/images/user-guide/component/entity_system/component-entity-outliner-reorder.png)

1. 您还可以将一个或多个实体拖放到首选顺序中。选择并拖动实体，直到在首选位置出现一条白线。

     ![Drag and drop to reorder an entity.](/images/user-guide/component/entity_system/component-entity-outliner-reorder-drag-drop.png)

1. 要使一个实体成为另一个实体的子实体，请选择并拖动该实体到其父实体。父实体周围会出现一个白框。

     ![Drag and drop to parent an entity.](/images/user-guide/component/entity_system/component-entity-outliner-parenting-drag-drop.png)

## 隐藏和显示实体

您可以隐藏实体，使其不出现在视口中，这样视口中只显示您想要的实体。如果隐藏父实体，所有子实体也会被隐藏。您还可以隐藏父实体中的子实体。

1. 在**Entity Outliner**中，单击实体旁边的眼睛图标。划线的眼睛图标表示实体不在视口中显示。

1. 要显示实体，请再次点击图标。

     ![Hide and show entities in the viewport.](/images/user-guide/component/entity_system/component-entity-outliner-hiding.png)

## 锁定实体

您可以锁定实体，使其无法在视口中被选中。 这样就可以只选择要编辑的实体。如果锁定父实体，所有子实体也会被锁定。您还可以锁定父实体中的单个子实体。

1. 在**Entity Outliner**中，单击实体旁边的锁定图标。当实体被锁定时，图标显示为白色。

     ![Lock entities in the Entity Outliner so that that can't be selected in the viewport.](/images/user-guide/component/entity_system/component-entity-outliner-locking.png)

1. 要解锁实体，请再次单击锁定图标。

## 搜索和筛选实体

对于有许多实体的关卡，您可以搜索和筛选所需的实体。在搜索过滤框中输入文字，即可找到特定实体。

**按名称搜索实体**

1. 在**Entity Outliner**中，输入实体的名称。

1. 要按组件筛选，请单击筛选器图标。

1. 在出现的搜索字段中输入组件名称，或滚动并选择所需的实体。任何具有指定组件的实体都会出现在结果中。
**示例**

     出现的实体都附加了**Camera**或**Trigger Area**组件。

     ![Search for entities in the Entity Outliner.](/images/shared/shared-entity-outliner-search-filter.png)

1. 要清除搜索结果，单击 **Clear**。

**按ID搜索实体**

1. 在**Entity Outliner**中，输入实体的完整 ID。不支持部分匹配和通配符搜索。

   您还可以对实体进行排序，以便它们按您希望的顺序出现在**Entity Outliner**中。

**要排序实体**

1. 在**Entity Outliner**中。点击排序图标。

     ![Sort entities in the Entity Outliner.](/images/shared/shared-entity-outliner-sort-filter.png)

1. 选择以下选项。

     - **Sort: Manually** - 手动组织实体。
     - **Sort: A to Z**- 按字母升序对实体进行排序。
     - **Sort: Z to A** - 按字母降序对实体进行排序。
     - **Scroll to Selected** - 在视口中选择一个实体时，**Entity Outliner**会滚动到该实体。如果您选择了多个实体，**Entity Outliner**会滚动到最后选定的实体。
     - **Expand Selected** - 在视口中选择一个实体时，**Entity Outliner**会展开层次结构以显示任何子实体。

   您还可以在**Entity Outliner**中选择一个实体，以便在视口中或反向查找该实体。这项功能可以帮助您找到想要的实体，尤其是在大型图层中。

**要找到实体或Prefab**

1. 在**Entity Outliner**中，右击一个实体，并选择**Find in viewport**。视口会导航到相应的实体。

     ![Find prefabs or entities in the viewport from the Entity Outliner.](/images/shared/shared-search-find-in-outliner.png)

1. 在视口中，右键单击Prefab或实体，然后选择**Find in Entity Outliner**。**Entity Outliner**会导航到相应的项目。

     ![Find entities in the Entity Outliner from the viewport.](/images/shared/shared-viewport-search-find-in-outliner.png)
