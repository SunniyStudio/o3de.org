---
description: ' 使用 Physics 材质自定义对象在 Open 3D Engine 项目中撞击表面时的反应。'
title: 物理材质
weight: 200
---

物理材质定义 PhysX 碰撞器如何通过摩擦力和恢复（反弹）等属性对碰撞做出反应。在 O3DE 中，您可以使用 **Asset Editor** 创建物理材质，并将它们分配给 PhysX 碰撞器。

**主题**
+ [Physics （物理） 材质属性](#physics-material-properties)
+ [创建物理材质](#create-a-physics-material)
+ [分配物理材质](#assign-a-physics-material)
  + [为每个面分配物理材质](#assign-physics-materials-per-face)

## 物理材质属性

![Physics material interface.](/images/user-guide/physx/physx/ui-physx-material-A.png)

****Dynamic Friction****
PhysX 碰撞器移动时的摩擦系数。
**0.0**: 无摩擦。

****Static Friction****
PhysX 碰撞器静止时的摩擦系数。
**0.0**: 无摩擦。

****Restitution****
PhysX 碰撞器在碰撞（反弹）时保留的能量。
**0.0**: 无弹跳。
**1.0**: 最大弹跳。

****Friction combine****
定义碰撞时 PhysX 碰撞器之间的物理材质摩擦力组合方式。
**Average**: 接触材料的平均值。这是默认值。
**Minimum**: 接触材料的较小值。
**Maximum**: 接触材料的值中的较大者。
**Multiply**: 接触材料值的乘积。

****Restitution combine****
定义碰撞时 PhysX 碰撞器之间如何组合物理材质恢复。
**Average**: 接触材料的平均值。这是默认值。
**Minimum**: 接触材料的较小值。
**Maximum**: 接触材料的值中的较大者。
**Multiply**: 接触材料值的乘积。

****Density****
定义材料的紧密度。

****Compliant Contact Mode (仅在启用 [PhysX 5.1](/docs/user-guide/interactivity/physics/nvidia-physx/#physx-version-support)时可用)****

+ ****Enable****
启用后，将使用隐式弹簧计算接触的法向力。启用 Compliant Contact Mode 时，不使用 **恢复属性**。

+ ****Damping****
较高的阻尼值会产生海绵状接触。

+ ****Stiffness****
刚度值越高，产生的弹簧越硬，其行为更像刚性接触。物体的质量越高，需要的刚度就越高，以减少穿透。

****Debug Color****
Debug （调试） 视图中物理材质的显示颜色。

{{< note >}}
当材质发生碰撞时，**Friction combine** 和 **Restitution combine** 使用以下顺序定义应用的摩擦力和恢复力的值：

1. **Average**
2. **Minimum**
3. **Multiply**
4. **Maximum**
{{< /note >}}

## 创建物理材质

物理材质定义 PhysX 碰撞器的物理属性。

**创建物理材质**

1. 在 **Tools** 菜单中旋转Asset Editor。

1. 在 Asset Editor 中，从**File**菜单中选择 **New**, **PhysX Material** 。

   ![Create a physics material in the Asset Editor.](/images/user-guide/physx/physx/ui-physx-material-B.png)

1. 根据需要设置材料属性。

1. 从 Asset Editor 的 **File** 菜单中选择 **Save As** 以保存物理材质。

## 分配物理材质

使用 **PhysX Primitive Collider** （PhysX 基元碰撞器） 组件时，可以为整个碰撞器分配一个物理材质。

使用 **PhysX Mesh Collider** 组件时，如果 PhysX 资产是三角形网格，则可以将物理材质分配给整个碰撞器或按面分配。启用 **Physics Materials from Asset** 属性后，将根据网格的 PhysX 资产中的物理材质自动设置此碰撞器的物理材质（请参阅 [FBX 设置 PhysX 选项卡](/docs/user-guide/assets/scene-settings/physx-tab/)）。要在碰撞器中手动设置物理材质，请取消选中 **Physics Materials from Asset** 属性，然后从 **Physics Materials** 属性列表中选择物理材质进行分配。

![PhysX Primitive Collider, setting the physics materials.](/images/user-guide/physx/physx/ui-physx-material-F.png)

### 为每个面分配物理材质

具有 PhysX 三角形网格资产的实体可以为每个面分配物理材质。您可以通过在内容创建应用程序中将材质分配给三角形碰撞网格的面来定义材质放置。这些材质在 FBX Settings （FBX 设置） 的 PhysX 组和 PhysX Mesh Collider （PhysX 网格碰撞器） 组件中列出，其中每个材质都分配有一个物理材质。

有关创建 PhysX 网格资源的更多信息，请参阅 [FBX 设置的 PhysX 选项卡](/docs/user-guide/assets/scene-settings/physx-tab/).

在下面的示例中，PhysX 网格资产是在内容创建应用程序中使用六种材质创作的：*黄色*、*红色*、*蓝色*、*蓝绿色*、*绿色* 和 *橙色*。在 FBX Settings （FBX 设置） 中的 PhysX （PhysX） 选项卡下，可以为每个材质分配一个物理材质。

![PhysX Asset, per face physics materials.](/images/user-guide/physx/physx/ui-physx-material-G.png)

在 **PhysX Mesh Collider Component** 中，当 **Physics Materials from Asset** 属性被禁用时，可以手动将物理材质分配给每个材质。启用 **Physics Materials from Asset** 属性后，将自动使用在 FBX Settings 中分配的物理材质。

![PhysX Mesh Collider, per face physics materials.](/images/user-guide/physx/physx/ui-physx-material-H.png)
