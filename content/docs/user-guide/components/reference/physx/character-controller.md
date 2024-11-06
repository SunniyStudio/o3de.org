---
linkTitle: PhysX Character Controller
description: 使用PhysX Character Controller 组件在 Open 3D Engine(O3DE)中实现基本角色交互。
title: PhysX Character Controller 组件
---



您可以使用 **PhysX Character Controller** 组件实现与物理世界的基本角色交互。例如，您可以阻止角色穿过墙壁或穿过地形。您还可以控制与斜坡和台阶的交互，并管理与其他角色的交互。

下图显示了 **PhysX Character Controller** 组件的一些功能。由于通常使用角色的脚部位置更方便，因此实体位置与控制器的基座重合。有关触点偏移的详细信息，请参见 [触点偏移](#contact-offset)。

![Contact offset of a PhysX Character Controller component in the O3DE Editor viewport.](/images/user-guide/component/physx/component-physx-character-controller-1.png)

**PhysX Character Controller** 组件需要 [PhysX](/docs/user-guide/gems/reference/physics/nvidia/physx/) gem。

**主题**
+ [使用PhysX Character Controller 组件](#using-the-physx-character-controller-component)
+ [PhysX Character Controller 属性](#physx-character-controller-properties)
+ [PhysX 和 Legacy Character Physics 组件之间的区别](#differences-between-physx-and-legacy-character-physics-components)

## 使用PhysX Character Controller 组件 

使用**PhysX Character Controller**组件，[将它添加到一个实体](/docs/user-guide/components/reference/#adding-components-to-an-entity)（表示角色的实体）。

您可以使用 Script Canvas、C++ API 或使用 C++ API 的动画系统来控制角色的移动。

## PhysX Character Controller 属性 

你可以在**[Entity Inspector](/docs/user-guide/editor/entity-inspector/)**中为**PhysX Character Controller**组件配置属性。

![PhysX Character Controller component properties in the Entity Inspector.](/images/user-guide/component/physx/ui-physx-character-controller-properties.png)

**PhysX Character Controller**具有以下组件属性。


****

| 属性 | 说明 |
| --- | --- |
|  **Collision Layer**  |  分配给控制器的碰撞层。默认值为**Default**。  |
|  **Collides With**  |  此角色控制器与之碰撞的碰撞层。可能的值是您在 PhysX 配置的 collision groups （碰撞组） 部分中定义的值。 您可以指定以下值：碰撞过滤器确定动态对象是否与控制器发生碰撞。一组单独的过滤器控制哪些对象可以阻止角色移动。移动过滤器目前是硬编码的，因此静态对象会阻碍角色移动。   |
|  **Physics Materials** | 为此角色控制器碰撞器选择物理材质。   |
|  **[Maximum Slope Angle](#maximum-slope-angle)**  |  角色控制器可以攀爬的最大坡度的角度（以度为单位）。   |
|  **[Step Height](#step-height)**  |  角色控制器可以遍历的步数高度（以米为单位）。   |
|  **Minimum Movement Distance**  |  距离（以米为单位），低于该距离时，控制器不会尝试移动。用于避免抖动。  |
|  **Collider Tag**  |  用于标识与角色控制器关联的碰撞体的标记字符串。  |
|  **Slope Behavior**  |  控制器在最大坡度以上的表面上的行为。 您可以指定以下值：默认值为 **Prevent Climbing**.  |
| **[Contact Offset](#contact-offset)** |  超出控制器的额外距离（以米为单位），以监控潜在接触。用于更平滑的接触解析。  |
| **Scale** |  相对于为控制器指定的尺寸缩放在 PhysX 中创建的碰撞器的大小。建议使用略小于 `1` 的值。 默认值为 `0.8`.  |
| **[Shape](#shape)** | 角色控制器的形状。您可以指定以下值：默认值为 Capsule。 |
| **Height (Capsule Only)** |  胶囊的高度（以米为单位）。  |
| **Radius (Capsule Only)** |  胶囊的半径（以米为单位）。  |
| **Dimensions (Box Only)** |  框的 x、y 和 z 维度（以米为单位）。  |

### Maximum Slope Angle 

最大坡度角是角色控制器可以爬升的最大坡度。角色不能在超过此斜率的方向上移动。如果角色站在高于最大坡度角度的斜坡上，则其行为取决于坡度行为设置。**Entity Inspector** 中允许的值范围是 `0` 到 `89` 度。

### Step Height 

最大坡度角度决定了控制器可以爬升的台阶高度。

**Example**
胶囊控制器可能能够爬上略高于台阶高度的台阶，因为弯曲的底部可以在台阶上向上滑动。请参阅下图：

![Step height determines the height of steps that the controller can climb.](/images/user-guide/component/physx/component-physx-character-controller-6.png)

### Contact Offset 

接触偏移是碰撞器形状和接触表面之间的距离填充。接触偏移允许仿真提供更平滑的碰撞行为。

{{< note >}}
接触偏移包含在脚位置的计算中。
{{< /note >}}

**Example**
在 **PhysX Character Controller** （PhysX 角色控制器） 组件的编辑器调试绘制中，接触偏移的效果由围绕碰撞器实体形状的线框表示，如下图所示。

![Wireframe showing the contact offset for a PhysX Character Controller in the O3DE Editor viewport.](/images/user-guide/component/physx/component-physx-character-controller-7.png)

### Shape 

您可以将角色控制器碰撞器与以下形状一起使用：
+ Capsule
+ Box

使用 **Entity Inspector **中的 **Shape** 属性选择所需的形状。执行此操作时，将显示相关维度以供编辑。维度设置与 **[PhysX Primitive Collider](/docs/user-guide/components/reference/physx/collider/)** 组件的胶囊和盒体选项相同。

## PhysX 和 Legacy Character Physics 组件之间的区别 

角色控制器通常是 **kinematic** 或 **simulated**。模拟角色控制器通过其速度或施加力进行控制。运动学角色控制器直接由位置控制。每种控制器类型都有优点和缺点。

有关详细信息，请参阅 NVIDIA 文档中的[Character Controllers](https://docs.nvidia.com/gameworks/content/gameworkslibrary/physx/guide/3.3.4/Manual/CharacterControllers.html) 。

由于 **PhysX Character Controller** 组件是运动学的，不受外力的影响，因此它不受开箱即用的重力影响。这种分离允许您使用 Script Canvas 或 C++ 实现重力和其他效果的自定义行为，因为该行为可能高度特定于游戏。有关某些游戏功能（如重力）的示例实现，请参阅 **[PhysX Character Gameplay](/docs/user-guide/components/reference/physx/character-gameplay/)** 组件。当模拟对象与运动控制器碰撞时，运动控制器的行为就像它们具有无限质量一样。您的自定义游戏逻辑决定了控制器如何响应碰撞，例如重击造成的后坐力。
