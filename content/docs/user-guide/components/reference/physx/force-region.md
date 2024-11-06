---
linkTitle: PhysX Force Region
description: ' 使用 PhysX Force Region （PhysX 力区域） 组件指定将物理力应用于实体的区域。'
title: PhysX Force Region 组件
---



您可以使用 **PhysX Force Region** （PhysX 力区域） 组件指定将物理力应用于实体的区域。对于每个物理模拟帧，该组件将力应用于区域边界内的实体。您可以使用此组件来模拟效果，例如模拟重力、减慢速度或将实体偏转到另一个方向。

要创建力区域，您必须执行以下操作：
+ 为游戏项目启用 [PhysX](/docs/user-guide/gems/reference/physics/nvidia/physx/) gem
+ 向相同的实体添加一个 **[PhysX Primitive Collider](/docs/user-guide/components/reference/physx/collider/)** 组件
+ 对于 **PhysX Primitive Collider** 组件，你必须选择 **Trigger** 属性，以使力区域生效

**PhysX Primitive Collider** 组件的形状、大小和方向表示对传入实体施加力的区域。

{{< note >}}
也可以使用 **PhysX Mesh Collider** 组件，这需要 **PxMesh** 资产。但是，如果资产内部的碰撞器不是凸面网格（例如，它是一个三角形网格），则碰撞器将不能用作碰撞的触发器。
{{< /note >}}

**主题**
- [PhysX Force Region 组件属性](#physx-force-region-component-properties)
  - [Force Types](#force-types)
    - [Linear Damping](#linear-damping)
    - [Local Space](#local-space)
    - [Point](#point)
    - [Simple Drag](#simple-drag)
    - [Spline Follow](#spline-follow)
    - [World Space](#world-space)
- [Creating a Force Region](#creating-a-force-region)

## PhysX Force Region 组件属性

![Force Region component properties.](/images/user-guide/component/physx/ui-physx-force-region-component-properties.png)


| 属性 | 说明 |
| --- | --- |
| Visible |  该组件始终显示在视区中，即使未选择实体也是如此。  |
| Debug Forces | 在游戏模式中绘制调试箭头。调试箭头指示力区域内每个实体的净力方向。 |
| Forces |  指定作用在力区域中的力类型。您可以为同一组件添加多个力类型。  |

### Force Types 力类型

您可以向组件添加多个力类型。当实体进入力区域时，该实体将根据您指定的力的净值进行移动。

**内容**
- [PhysX Force Region Component Properties](#physx-force-region-component-properties)
  - [Force Types](#force-types)
    - [Linear Damping](#linear-damping)
    - [Local Space](#local-space)
    - [Point](#point)
    - [Simple Drag](#simple-drag)
    - [Spline Follow](#spline-follow)
    - [World Space](#world-space)
- [创建力区域](#creating-a-force-region)

#### Linear Damping线性阻尼

在与实体的速度相反的方向上应用力。例如，您可以创建模拟沼泽或泥浆的力。


| 属性 | 说明 |
| --- | --- |
| Damping |  要应用的阻尼量。您不能指定负值。指定较高的值可应用更多的阻尼。指定 `0` 表示无阻尼。  |

#### Local Space局部空间

在局部空间中相对于力区域的方向应用力。例如，您可以创建一个模拟吹风机或真空吸尘器的力。


| 属性 | 说明 |
| --- | --- |
| Direction |  力区域局部空间中的力方向。  您可以指定一个介于 `-1000000` 到 `1000000` 之间的值，但 O3DE 编辑器会将该值标准化为 `-1` 和 `1` 的范围。  |
|  **Magnitude**  |  要应用的力的大小。 指定负值以沿相反方向应用力。 |

#### Point点

相对于力中心区域应用力。大小确定力是向内还是向外。例如，您可以创建模拟爆炸或黑洞的力。


| 属性 | 说明 |
| --- | --- |
| Magnitude |  要应用的力的大小。为向外的力指定一个正值，为向内力指定一个负值。  |

#### Simple Drag简单阻力

应用模拟空气阻力的力。**Simple Drag** 始终在碰撞实体的相反方向上施加力。更大、速度更快的实体会受到更大的阻力。实体近似为球体。


| 属性 | 说明 |
| --- | --- |
|  **Region Density** | 体积的密度。指定较高的值可增加拖动力。 您只能指定正值。   |

#### Spline Follow样条线跟随

应用力使实体跟随样条曲线。该力使用比例导数 （PD） 控制器，该控制器模拟沿样条曲线移动的弹簧。例如，您可以创建模拟水滑梯的力。

{{< note >}}
对于力区域实体，如果更改 **Transform** 组件的 **Scale** 属性，则缩放必须均匀，以便 x、y 和 z 缩放值匹配。如果缩放不均匀，则样条曲线无法正确反映力的路径。
样条曲线的末端必须位于力区域之外，以便实体可以在跟随样条曲线后退出。
{{< /note >}}


| 属性 | 说明 |
| --- | --- |
|   **Damping Ratio**   |  当实体进入样条曲线时，减慢实体的振动速度。 值越高，振动减慢的速度就越快。值为 `1` 会快速减慢样条曲线周围的振动。  |
|  **Frequency**  |  实体进入样条曲线时的振动频率。  |
|  **Target Speed** |  实体在穿过样条曲线的节点时尝试达到的速度。指定负值以沿相反方向应用力。  |
|  **Lookahead**  |  实体在其路径中向前看以到达样条上节点的距离（以米为单位）。 **Lookahead** 值确定实体在多远前寻找下一个要转向的节点。在每个物理模拟帧中，样条中的实体会改变其方向，以将自身重新对齐到已识别的节点。如果样条节点彼此靠近，则可以指定较小的 **Lookahead**值，以便实体可以检测下一个节点。如果节点相距较远，您可以指定更高的 **Lookahead** 值，以便实体可以检测下一个节点。   |

#### World Space 世界空间

在世界空间中应用力。World Space Force 不考虑实体的方向。例如，您可以创建模拟重力的力。

{{< note >}}
您可以定义世界空间的方向，以便它始终沿您想要的方向施加力，而不管碰撞实体如何。
{{< /note >}}


| 属性 | 说明 |
| --- | --- |
|  **Direction** |  力在世界空间中的方向。  |
|  **Magnitude**  |  要应用的力的大小。 指定负值以沿相反方向应用力。  |

{{< note >}}
选择力类型时，请记住以下几点：
对于 **Simple Drag**，您无法定义力的方向。**Simple Drag** 始终在与实体移动相反的方向上工作。相反，您可以使用 **World Space** 定义力的方向，无论移动实体的方向如何，它始终作用于您指定的方向。

为了确定要施加的力，**Linear Damping** 会考虑碰撞实体的速度和质量，但不考虑其形状。相比之下，**Simple Drag** 考虑了碰撞实体的速度、横截面积和力区域的 **Region Density**。
{{< /note >}}

## 创建 Force Region

您可以创建一个力区域，以便将力应用于进入该区域的另一个实体。

**创建PhysX Force Region组件**

1. 在 O3DE 编辑器中，创建一个实体。

1. 为此实体输入一个名称，例如 *ForceRegion*。

1. 在 **Entity Inspector**中，选择 **Add Component** ，然后选择一个 **PhysX Force Region** 组件。

1. 在 **PhysX Force Region** 组件中，选择**Add Required Component** 并选择 **PhysX Primitive Collider** 组件。

1. 在 **PhysX Primitive Collider** 组件中，选择 **Add Required Component** 并选择 **PhysX Static Rigid Body** 组件。

1. 对于 **PhysX Primitive Collider** 组件，完成以下操作：

   1. 选择 **Trigger** 属性。

   1. 对于 **Shape**，选择一个形状，例如 **Box**。

1. 对于 **PhysX Force Region** 组件，完成以下操作：

   1. 选择 **Visible** 和 **Debug Forces** 属性。

   1. 对于**Forces**，点击 **+** 图标，对于 **Force Type**，选择一种力，例如 **Local Space**。

   1. 对于 **Magnitude**，输入一个值，例如 **20**。

      实体上会显示指示力方向的蓝色箭头。

      ![Direction of the PhysX Force Region.](/images/user-guide/component/physx/force-region-component-local-force.png)

1. 要将实体与力区域碰撞，请创建名为 **Sphere** 的动态实体，并附加 **PhysX Primitive Collider** 和 **PhysX Dynamic Rigid Body** 组件。这些组件使实体能够与其他 PhysX 实体交互。

1. (可选) 添加 **Mesh** 组件，然后对于 **Mesh asset**，选择一个网格资产，例如`_sphere_1x1.fbx`.

1. 选择并拖拽 **Sphere** 实体，使其位于力区域上方。

    ![An entity entering the force region.](/images/user-guide/component/physx/force-region-component-local-force-2.png)

1. 在创建动态实体后，按下 **Ctrl**+**G** 进入游戏模式。
**例如**

   球体下落并与力区域碰撞。力区域施加力并将球体推向相反方向。

   ![PhysX Force Region component animation.](/images/user-guide/component/physx/animation-force-region-component.gif)

1. 要离开游戏模式，按下 **Esc**。

{{< note >}}
要显示 PhysX 调试可视化效果，请参阅 [调试 PhysX](/docs/user-guide/interactivity/physics/debugging/)。
有关使用 PhysX 组件的更多信息，请参阅[使用 PhysX 系统模拟物理行为](/docs/user-guide/interactivity/physics/nvidia-physx/)。
{{< /note >}}
