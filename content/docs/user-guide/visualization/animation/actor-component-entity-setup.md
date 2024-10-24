---
linkTitle: 设置Actor实体
description: ' 在 Open 3D Engine 中使用多个或单个 FBX 文件设置Actor实体。'
title: 设置Actor实体
---

将蒙皮网格设置为Actor后，就可以创建Actor实体。然后将附件作为主实体的父实体，并将附件与主Actor排成一行。

**要创建Actor实体**

1. 从**资产浏览器**中，选择并拖动主Actor文件和附件Actor文件到视口。

    ![From the Asset Browser, drag the main actor file and its attachment actor files into the viewport.](/images/user-guide/visualization/animation/component-actor-component-entity-setup-1.png)

   O3DE 会自动将每个文件添加为自己的Actor组件实体。

    ![Drag each actor file to the viewport results in three separate actor entities in the Entity Outliner.](/images/user-guide/visualization/animation/component-actor-component-entity-setup-2.png)

1. 在**Entity Outliner**中，单击并拖动附件实体到主实体，这样附件Actor实体就会成为主Actor实体的父实体。

    ![Parent the attachment entities by selecting and dragging them onto the main actor entity in the Entity Outliner.](/images/user-guide/visualization/animation/component-actor-component-entity-setup-2-parented.png)

    {{< note >}}
   附件实体可能与主实体不一致。下一步将解决这个问题。
{{< /note >}}

    **Example**

    ![View the main actor entity and the attachment entities in the viewport.](/images/user-guide/visualization/animation/component-actor-component-entity-setup-3.png)

1. 要使子实体与父实体对齐，请选择一个子实体（附件），然后在**Entity Inspector**中指定**Translate**值为 “0”。对其他子实体重复上述操作。

    ![Change the Translate values to 0 on each child entity to line it up with the parent.](/images/user-guide/visualization/animation/component-actor-component-entity-setup-4.png)

   现在，附件与主要Actor实体保持一致。

    ![Attachment entities lined up correctly with the main parent actor entity.](/images/user-guide/visualization/animation/component-actor-component-entity-setup-5.png)

1. 对每个子实体执行以下操作：

    1. 在**Actor**组件中，为**Attachment type**选择**Skin attachment**。

    1. 对于**Target entity**，点击 {{< icon "picker.svg" >}} ，选择用于附加蒙皮网格的主Actor (例如 **cowboyactor**)。

组件实体设置完成。当您的主Actor产生动画时，附加的蒙皮网格附件将与主要Actor的骨架一起产生动画。
