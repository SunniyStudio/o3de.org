---
description: ' 了解如何在 O3DE 动画编辑器中使用动画图形标签。
 '
title: 使用动画图形标签
---

在**动画编辑器**中，您可以使用标记来描述角色的当前状态，并控制不同状态之间的转换。标签是布尔标志，要么处于激活状态（启用），要么处于非激活状态（禁用）。一些标记的例子包括 “开心”、“持剑 ”和 “左腿受伤”。

## 添加标签

标签由动画图参数表示。定义参数时，可以为使用相同动画图和参数的每个实体指定不同的值。例如，你可以为使用动画图的每个实体指定不同的**Speed**参数值。同样，也可以为每个实体指定不同的标签。例如，一个实体激活了 “持剑 ”标签，而另一个实体激活了 “快乐 ”标签。有关参数的更多信息，请参阅 [关于参数](/docs/user-guide/visualization/animation/character-editor/concepts-and-terms/#animation-graph-parameters).。

**创建标签**

1. 在 O3DE 编辑器中，选择 **Tools**, **Animation Editor**。

1. 在 **Animation Editor**中，在 **Parameters** 面板中，点击 **+** 按钮。

![Add Parameter Icon](/images/user-guide/actor-animation/anim-graph-parameters-pane.png)

1. 在 **Create Parameter** 对话框中，完成以下事项：

   1. 在**Value type**中，选择**Tag**。

   1. 在**Name**中，输入标记的名称。

   1. 在**Description**中，输入标签的可选描述。

   1. 对于 **Default**，选择复选框以启用标记。

      ![New Paramter configuration](/images/user-guide/actor-animation/anim-graph-create-parameter-dialog-box.png)

1. 点击 **Create**。

## 为标签添加条件

使用标签条件可使状态机改变活动状态。例如，您可以根据活动标签选择特定的跳跃动画。要过渡到 “Awesome Jump”，需要启用 “Freaky”、“Awesome ”和 “Happy  ”标签。

![Anim Graph with Tag conditions on transitions](/images/user-guide/actor-animation/anim-graph-tag-conditions-example.png)

您还可以将标记与通配符转换结合使用，选择其他状态可以访问的特定状态。通配符转换是可以从任何节点开始的转换。在上例中，**Idle** 左边的箭头代表通配符转换。这意味着只要满足通配符转换的条件，就可以从任何状态转换到**Idle**状态。

标签条件有两个属性：测试功能和标签。

![Example Tag condition](/images/user-guide/actor-animation/anim-graph-tag-conditions-attributes.png)

**测试函数**
指定通过条件的标记状态。

您可以选择以下选项：
+ **All tags active** - 所有标签必须处于激活状态，否则条件会阻止更改。
+ **One or more tags inactive** - 必须至少有一个标签处于非活动状态，否则条件会阻止更改。
+ **One or more tags active** - 必须至少有一个标签处于活动状态，否则条件会阻止更改。
+ **No tag active** - 所有标签必须处于非激活状态，否则条件会阻止更改。

**Tags**
指定条件检查的标记。

要在条件中添加标记，请选择节点之间的过渡线。在**Attributes**窗格中，选择要使用的值。

![Attribute pane of transition](/images/user-guide/actor-animation/anim-graph-tag-conditions-values.png)

{{< note >}}
您只能从**Parameters**窗格中可用的标签中进行选择。更多信息，请参阅 [添加标签](#animation-editor-adding-tags)。
{{< /note >}}
