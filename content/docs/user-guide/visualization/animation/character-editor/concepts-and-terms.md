---
description: null
title: '动画编辑器概念和术语'
---

以下概念和术语用于**动画编辑器**：

**Actor**
至少有一个骨骼的角色称为**Actor**。一个Actor由一个层次结构中的一组节点组成。每个节点都是一个变换（位置、旋转和缩放），并可包含一个网格。
Actor的实例称为*Actor实例*。例如，一个士兵被实例化 100 次，就能创建一支相同角色的军队。您可以分别为Actor实例制作动画，这样每个实例就会有不同的表现。每个Actor实例都有独一无二的变换，但其层次结构与被实例化的Actor相同。
Actor文件具有`.actor`扩展名 \(例如, `hero.actor`\)。

**Motions**
动作是单独的动画片段，如行走循环、空闲动作等。动作包含变形动画和/或变形目标权重动画。一个动作可以包含骨骼结构和变形目标的动画数据。
动作文件具有`.motion` 扩展名 \(例如, `Walk.motion`\)。

**Animation graphs**
动画网络被称为动画图。动画图包含状态机、转换、条件、混合树和其他节点。动画图是分层的。
Animation graph 文件具有.`animgraph` 扩展名 \(例如, `Main.animgraph`\)。

**Motion sets**
动作集包含一个动作列表，其中每个动作都有一个唯一的字符串 ID \(例如，`walk`, `idle`等\)。动画图中的节点可以根据字符串 ID 引用动作。
动作集可以分级。子集可以覆盖角色的特定动作，同时与父集共享其余动作。
动作集具有`.motionset` 扩展名 \(例如, `MainSet.motionset`\)。

**Motion events**
动作文件中特定时间值的标记称为**动作事件**（也称为通知器或通知）。动作事件有一个类型字符串（如 “SOUND”）和参数字符串（如 “Footstep”）。
动作事件可以有固定的时间值或范围。具有单一时间值的事件称为**tick events**。具有指定开始和结束时间的事件称为**ranged events**。
您可以指定事件预设，这是预先设置好的事件类型，您可以将其拖放到事件轨道中。**event track**是一组可以启用或禁用的事件。例如，您可以将所有音效添加到专门用于声音的事件轨道中。
动作事件数据被存储在动作FBX的`.assetinfo`文件中。

**Synchronization**
您可以使用 **full clip-base sync** 或 **sync tracks ** 来同步动作片段，以便在混合时保持角色动作同步。例如，如果角色正在奔跑，同步有助于保持左右脚同步。
基于片段的完全同步会扭曲动作，使子动作的播放时间不断变化。
同步音轨使用动作事件，事件标记特定时刻（例如，左右脚着地的位置）。这种系统也称为相位匹配，可动态控制播放速度。

**Floats**
浮点数是带小数点的数字（例如 1.35 或 1.0）。布尔数和整数也是浮点数，因此可以作为权重浮点数输入传递给混合节点。如果浮点数被四舍五入，**动画编辑器**总是将其向下舍入。例如，2.99 将变为 2。

**Time**
所有时间值和持续时间都以秒为单位。例如，您可以将过渡时间设置为 0.3 或 300 毫秒。

## 关于 Animation Graph 

动画图定义了游戏角色的动画行为。动画图包含角色可能具有的状态，并定义了这些状态之间的转换。每个过渡都可以有一组条件，这些条件定义了过渡背后的逻辑。

动画图形包含节点和节点之间的连接。这些连接定义了节点之间的数据传递方式或节点之间的转换方式。

动画图形有两种主要节点类型：
+ 状态机
+ 混合树

由于动画图是分层的，因此节点可以嵌套。例如，你可以在一个混合树中包含一个状态机，而混合树又包含另一个混合树和状态机，以此类推。层次结构的级数是无限的，但最佳做法是将层次结构限制在 20 级以内。

每个动画图都有一个根节点，它是一个状态机。这个根节点是默认的，不能删除。一个简单的动画图可以在根状态机中包含一个状态。例如，单个状态可以是一个动作节点，它输出一个应用于角色的姿势，如空闲动作。

在向动画图形添加节点之前，必须先创建一个动作集。创建动作集后，可以创建动画图形，然后使用**Anim Graph**窗口中的**Resource Management**窗格将动作集分配给动画图形。

### Animation Graph 节点 

在状态机中，可以从**Sources**类别中添加以下节点：
+ **Blend Tree**
+ **Entry**
+ **Exit Node**
+ **Hub**
+ **Motion**
+ **State Machine**
+ **Bind Pose**

在混合树中，您可以从以下六个类别中添加其他节点

1. **Sources**
   + **Float Constant**
   + **Parameters**
   + **Blend Tree**
   + **Entry**
   + **Motion Frame**
   + **Exit Node**
   + **Motion**
   + **State Machine**
   + **Bind Pose**

1. **Blending**
   + **Pose Subtract**
   + **Morph Target**
   + **Pose Mask**
   + **Blend N**
   + **Blend Two (Legacy)**
   + **Blend Space 2D**
   + **Blend Space 1D**
   + **Blend Two**

1. **Controllers**
   + **Transform**
   + **LookAt**
   + **TwoLink IK**
   + **AccumTransform**

1. **Logic**
   + **Pose Switch**
   + **Float Condition**
   + **Float Switch**
   + **Bool Logic**

1. **Math**
   + **Direction to Weight**
   + **Range Remapper**
   + **Vector3 Compose**
   + **Vector2 Compose**
   + **Vector4 Decompose**
   + **Vector4 Compose**
   + **Vector3 Math1**
   + **Vector3 Decompose**
   + **Smoothing**
   + **Float Math2**
   + **Vector2 Decompose**
   + **Vector3 Math2**
   + **Float Math1**

1. **Misc**
   + **Mirror Pose**

## 关于参数 

创建动画图形时，可以使用参数来控制动画在不同状态之间的转换。

每个过渡都可以应用一组条件。这些条件定义了过渡的逻辑规则以及动画如何融合在一起。

每个过渡条件都由一组参数控制。您的 O3DE 游戏设置会将参数值发送到动画图形。Actor对传入的参数做出反应。游戏将参数值发送到动画图形，然后动画图形会自动对变化做出反应。例如，您可以指定速度、方向、武器类型等参数值。

您可以在游戏关卡中使用**Entity Inspector**为实体添加一个**Actor**和一个**Animation**组件来设置。

更多信息，请参阅 [动画编辑器组件](/docs/user-guide/visualization/animation/character-editor/components/)。

### 向动画图添加参数 

您可以在**Parameters**窗格中为动画图形添加参数。

**为动画图形添加参数**

1. 在 O3DE 编辑器中，选择 **Tools**, **Animation Editor**。

1. 在 **Parameters** 面板中，点击绿色的 **+** 图标。

1. 在 **Create Parameter** 对话框中，指定参数名，描述，和值的类型。

   您可以指定以下值类型，为动画图形节点提供输入：
   + **Float (slider)**
   + **Float (spin box)**
   + **Boolean (checkbox)**
   + **Tag (checkbox)**
   + **Integer (slider)**
   + **Integer (spin box)**
   + **Vector2**
   + **Vector3**
   + **Vector3 gizmo**
   + **Vector4**
   + **String**
   + **Color**
   + **Rotation**
   + **Group**

您可以为参数类型命名，以确定控件的用途。例如，您可以为  `movement_speed`, `movement_direction`, `jumping` 和 `attacking`等参数命名。作为艺术家和游戏设计师，您可以指定最能控制动画图形的参数。

![Create parameters for an animation graph in the Animation Editor.](/images/user-guide/actor-animation/animation-editor-parameters-pane.png)

### 在混合树中添加参数节点

在**Parameter**窗格中创建参数后，您可以将参数节点添加到混合树中。

**在混合树中添加参数节点**

1.在 **Animation Editor**中，右击动画图表网格，选择 **Create Node**, **Sources**, **Parameters**.。

1. 在 **Attributes** 面板中，点击 **select parameter** ，并指定你想要的参数。

![Select your parameter in your animation graph in the Animation Editor.](/images/user-guide/actor-animation/animation-editor-attributes-pane-02.png)

您可以重命名参数节点，并指定它们为其他节点提供输入。在下面的示例中，**speed\_parameter** 节点为混合树提供了输入。

![Use parameter nodes in the Animation Editor to specify parameter types and values for your animation graph.](/images/user-guide/actor-animation/animation-editor-blend-tree.png)

## 关于 Motion Sets 

动作集是动作的集合，其中每个动作指代一个特定的动作文件，并用字符串 ID 标识，如 **idle/\_motion1**。创建动作节点时，指定的是动作的字符串 ID，而不是动作文件本身。您可以将不同的动作集与相同的动画图形结合使用。例如，您可以创建一个动画图形，为可控的人类角色定义动画行为，并将同一动画图形应用于青蛙。由于青蛙的动作与人类角色不同，因此要为青蛙指定不同的动作集。您可以共享角色的动画图形；无需为每种角色类型创建唯一的动画图形。

应用于给定Actor实例的动画图形与指定动作集的组合称为**动画图形实例**。每个动画图形实例都有一组独特的参数值。例如，一支由 100 名士兵组成的军队由 100 个不同的动画图实例控制，这样就可以为每个士兵制作独立的动画。

动作集也可以分级。子动作集可以覆盖父动作集的某些动作。将子动作集应用到角色时，除了为子动作集指定的动作外，角色将使用父动作集共享的所有动作。例如，一个角色可以共享父角色 90% 的相同动作，但也有该角色特有的自定义动作。

## 关于状态机 

状态机包含一组通过转换连接起来的状态。转换从一个节点到另一个节点，并具有属性，如转换所需的时间。在过渡期间，当动画从一个状态移动到另一个状态时，两个状态的输出之间会进行混合。

过渡条件是与给定过渡相关联的条件。例如，它们可以比较一个参数值和另一个值，看给定参数是否大于指定值。如果满足条件，就会触发转换。例如，如果速度参数大于 0，字符就会从空闲状态过渡到运行状态。您可以对单个过渡应用多个条件。只有当所有条件都满足时，才会发生转换。
