---
linkTitle: Transform
description: ' 使用 Transform （变换） 组件在 Open 3D Engine 中移动、旋转和缩放实体。 '
title: Transform 组件
---

**Transform** 组件控制 3D 世界中实体的平移、旋转和缩放信息。当您在 O3DE Editor 中创建实体时，将自动添加 **Transform** 组件。平移是实体的坐标位置（x、y 和 z 轴）。旋转是实体绕其中心旋转的程度。统一比例是实体的尺寸与其原始大小相比，在每个方向上均匀应用。

*World space* 是指实体在关卡中的绝对平移、旋转和缩放。如果子实体附加到父实体，则 *local space* 是指实体相对于其父实体的平移、旋转和缩放。

## Transform组件属性 

**Transform** 组件具有以下属性：

**Parent entity**
分配为父级的实体。如果指定了父实体，则 **Transform** 组件将遵循父实体。

### 值 

**Transform** 组件具有以下值：

**Translate**
本地位置（相对于父级），以米为单位。

**Rotate**
局部旋转（相对于父级），以度为单位。

**Uniform Scale**
局部比例，在每个方向上统一应用单个值。点击 **Add non-uniform scale** 按钮以添加 [Non-uniform Scale](/docs/user-guide/components/reference/non-uniform-scale) 组件，该组件允许在实体的每个轴上使用不同的缩放值。

**Parent activation**
配置父实体激活时的转换行为。

**Static**
在运行时无法移动的实体。O3DE 中的某些系统处理静态实体的方式与处理可移动实体的方式不同（例如，渲染器可以优化静态实体，从而减少绘制它们所占用的资源）。

## EBus 请求总线接口

**TransformBus** 是 **Transform** 组件的请求总线。实体的变换是平移、旋转和缩放信息。

有关使用事件总线 （EBus） 接口的更多信息，请参阅 [使用事件总线 （EBus） 系统](/docs/user-guide/programming/messaging/ebus/).

将以下请求函数与 EBus 接口结合使用，以便与游戏的其他组件进行通信。

### GetLocalTM 

返回实体的本地转换。不包括父实体的转换。

**参数**
None

**返回值**
实体的本地转换。

### SetLocalTM 

设置实体相对于其父实体的本地转换，并通知所有侦听器。

**参数**
实体的本地转换。

**返回值**
None

### GetWorldTM 

返回实体的世界变换，包括父实体的变换。

**参数**
None

**返回值**
实体的世界转换。

### SetWorldTM 

设置世界变换并通知所有侦听器。

**参数**
实体的世界转换。

**返回值**
None

### GetLocalAndWorld 

检索实体的 local 和 world 转换。

**参数**
Transform \[out\] - Local transform，相对于父实体。
Transform \[out\] - World transform.

**返回值**
None

### SetWorldTranslation 

设置实体的世界空间平移。

**参数**
新的世界空间位置，以 x、y 和 z 坐标表示。
Type: Vector3

**返回值**
None

### SetLocalTranslation 

设置实体的局部空间平移，该平移相对于其父实体。

**参数**
新的本地空间位置，以 x、y 和 z 坐标为单位。
Type: Vector3

**返回值**
None

### GetWorldTranslation 

获取实体的世界空间平移。

**参数**
None

**返回值**
实体的世界空间，以 x、y 和 z 坐标表示。
Type: Vector3

### GetLocalTranslation 

获取实体的局部空间平移，该平移相对于其父实体。

**参数**
None

**返回值**
实体的局部空间，以 x、y 和 z 坐标为单位。
Type: Vector3

### MoveEntity 

在世界空间内移动实体。

**参数**
在世界空间、x、y 和 z 坐标中的偏移。
Type: Vector3

**返回值**
None

### SetWorldX 

设置实体在世界空间中的平移 x 轴坐标。

**参数**
世界空间中的 X 轴坐标。
Type: Float

**返回值**
None

### SetWorldY 

设置实体在世界空间中的平移 y 轴坐标。

**参数**
世界空间中的 Y 轴坐标。
Type: Float

**返回值**
None

### SetWorldZ 

设置实体在世界空间中的平移 z 轴坐标。

**参数**
世界空间中的 Z 轴坐标。
Type: Float

**返回值**
None

### GetWorldX 

获取实体在世界空间中的平移 x 轴坐标。

**参数**
None

**返回值**
世界空间中的 X 轴坐标。
Type: Float

### GetWorldY 

获取实体在世界空间中的平移 y 轴坐标。

**参数**
None

**返回值**
世界空间中的 Y 轴坐标。
Type: Float

### GetWorldZ 

设置实体在世界空间中的平移 z 轴坐标。

**参数**
None

**返回值**
世界空间中的 Z 轴坐标。
Type: Float

### SetLocalX 

设置实体在本地空间中的平移 x 轴坐标。

**参数**
局部空间中的 X 轴坐标。
Type: Float

**返回值**
None

### SetLocalY 

设置实体在本地空间中的平移 y 轴坐标。

**参数**
本地空间中的 Y 轴坐标。
Type: Float

**返回值**
None

### SetLocalZ 

设置实体在本地空间中的平移 z 轴坐标。

**参数**
局部空间的 Z 轴坐标。
Type: Float

**返回值**
None

### GetLocalX 

获取实体在本地空间中的平移 x 轴坐标。

**参数**
None

**返回值**
局部空间中的 X 轴坐标。
Type: Float

### GetLocalY 

获取实体在本地空间中的 y 轴坐标。

**参数**
None

**返回值**
本地空间中的 Y 轴坐标。
Type: Float

### GetLocalZ 

获取实体在本地空间中的 z 轴坐标。

**参数**
None

**返回值**
局部空间的 Z 轴坐标。
Type: Float

### GetWorldRotation 

获取世界变换围绕其旋转的每个主轴的弧度角度，顺序如下：z 轴、y 轴、x 轴。

**参数**
None

**返回值**
以弧度为单位的欧拉角，表示绕每个主轴的旋转程度。
Type: Vector3

### GetWorldRotationQuaternion 

获取表示世界旋转的四元数。

**参数**
None

**返回值**
表示世界旋转的四元数。
Type: Quaternion

### SetLocalRotation 

按以下顺序设置围绕每个主轴的局部旋转：z 轴、y 轴、x 轴。

**参数**
Vector3 表示围绕每个主轴旋转的弧度角。
Type: Vector3

**返回值**
None

### SetLocalRotationQuaternion 

使用四元数设置局部旋转矩阵。

**参数**
局部旋转矩阵。
Type: Quaternion

**返回值**
None

### RotateAroundLocalX 

围绕局部 x 轴旋转弧度角。

**参数**
绕局部 x 轴旋转的弧度角度。
Type: Float

**返回值**
None

### RotateAroundLocalY 

围绕局部 y 轴旋转弧度角。

**参数**
绕局部 y 轴旋转的弧度角度。
Type: Float

**返回值**
None

### RotateAroundLocalZ 

围绕局部 z 轴旋转弧度角。

**参数**
绕局部 z 轴旋转的弧度角度。
Type: Float

**返回值**
None

### GetLocalRotation 

获取按以下顺序旋转局部变换所围绕的每个主轴的弧度角度：z 轴、y 轴、x 轴。

**参数**
None

**返回值**
指示围绕每个主轴旋转的弧度量。
Type: Vector3

### GetLocalRotationQuaternion 

获取表示局部旋转的四元数。

**参数**
None

**返回值**
表示局部旋转的四元数。
Type: Quaternion

### SetLocalUniformScale 

设置变换的局部统一缩放。

**参数**
变换的局部统一缩放，均匀应用于每个轴。
Type: Float

**返回值**
None

### GetLocalUniformScale 

获取均匀缩放值，均匀应用于局部空间中的每个轴。

**参数**
None

**返回值**
Uniform scale 值，均匀应用于局部空间中的每个轴。
Type: Float

### GetWorldUniformScale 

获取均匀缩放值，均匀应用于世界空间中的每个轴。

**参数**
None

**返回值**
Uniform scale 值，均匀应用于世界空间中的每个轴。
Type: Float

### GetParentId 

返回父实体的 ID。如果实体没有父项，则实体 ID 无效。

**参数**
None

**返回值**
父级的 EntityID
Type: Int

### SetParent 

设置实体的父实体并通知所有侦听器。实体的局部变换将移动到父实体的空间中，以保留实体的世界变换。

**参数**
EntityId - Parent entity ID
Type: Int

**返回值**
None

### SetParentRelative 

设置实体的父实体，相对于父实体移动转换，并通知所有侦听器。此函数使用世界转换作为本地转换，并相对于父实体移动转换。

**参数**
EntityId - Parent entity ID
Type: Int

**返回值**
None

### GetChildren 

返回实体的直接子项的实体 ID。

**参数**
None

**返回值**
Vector of EntityIds

### GetAllDescendants 

返回实体的所有后代的实体 ID。后代是实体的子项、子项的子项等。实体 ID 按宽度顺序排列。

**参数**
None

**返回值**
Vector of EntityIds

### GetEntityAndAllDescendants 

返回实体及其所有后代的实体 ID。后代是实体的子项、子项的子项等。实体 ID 按广度排序排在最前面，此实体的 ID 是列表中的第一个。

**参数**
None

**返回值**
Vector of EntityIds

### IsStaticTransform 

返回转换是否为静态。静态转换不会移动，也不会响应移动它的请求。

**参数**
None

**返回值**
Boolean

## EBus Notification Bus Interface 

**TransformNotificationBus** 是 **Transform** 组件的通知总线。将以下通知函数与 EBus 界面结合使用，以便与游戏的其他组件进行通信。

有关使用事件总线 （EBus） 接口的更多信息，请参阅 [使用事件总线 （EBus） 系统](/docs/user-guide/programming/messaging/ebus/).

### OnTransformChanged 

指示实体的 local 或 world 转换已更改。

**参数**
Transform （变换） - 实体的新局部变换
Transform （转换） - 实体的新世界转换

### OnParentChanged 

表示实体的父级已更改。

**参数**
EntityId - 前一个父级的实体 ID。如果上一个父项没有，则实体 ID 无效。
EntityId - 新父级的实体 ID。如果没有新的父项，则实体 ID 无效。

### OnChildAdded 

表示已将子项添加到实体。

**参数**
EntityId - 添加的子项的实体 ID

### OnChildRemoved 

表示已从实体中删除子项。

**参数**
EntityId - 已删除子项的实体 ID
