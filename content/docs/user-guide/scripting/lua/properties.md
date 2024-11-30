---
linkTitle: 属性表
title: Lua Script Properties 表
description: 指定 Open 3D Engine 的 O3DE 编辑器中显示的 Lua 脚本组件的属性。
toc: True
weight: 400
---

Properties Table 在 **Entity Inspector** 中配置 **Lua Script** 组件的用户界面，以自定义 Lua 脚本的行为。使用 Properties Table，您可以修改数值、选择状态以及打开和关闭标志。该表甚至可以引用您的脚本可以与之交互的其他实体。

Properties Table 中的属性将向 **O3DE Editor **公开。Properties Table 之外的属性是私有的，不会显示在编辑器中。

以下示例是角色控制器的 Properties 表。

```lua
-- Properties Table Example
Properties = {
    MoveSpeed = {
        default = 3.0,
        suffix = "m/s",
        description = "Character movement speed."
    },
    RotationSpeed = {
        default = 360.0,
        suffix = "deg/sec",
        description = "Character turning speed."
    },
    CameraFollowDistance = {
        default = 5.0,
        suffix = "m",
        description = "Minimum follow-distance between the camera and character."
    },
    CameraFollowHeight = {
        default = 1.0,
        suffix = "m",
        description = "Camera height-offset from the character."
    },
    CameraLerpSpeed = {
        default = 5.0, 
        description = "Coefficient that affects the camera's rate of closure."
    },
    Camera = {
        default = EntityId()
    },
    InitialState = "Idle",
    DebugStateMachine = false
}
```

在用户界面中呈现时，属性按字母顺序排序，而不管它们在脚本中的顺序如何。

结果是 Lua Script 组件中的以下 **Properties** 用户界面：

![Properties in O3DE Editor defined by the Properties table](/images/user-guide/scripting/lua/character-controller.png)

您作为默认值提供的类型决定了属性在 **Properties** 用户界面中的显示方式。您可以通过以表格格式指定其他属性，在 O3DE Editor 中进一步自定义属性的表示形式。所有属性类型都支持将鼠标悬停在属性名称上时显示的描述字段。

## 支持的类型

属性可以具有本节中描述的类型。

### 布尔值 （True， False）

以下示例是布尔值。

```lua
-- Boolean Examples
Properties = {
    DebugMovement = false,
    AllowMovement = { 
        default = true, 
        description = "Toggles object movement."
    }
}
```

在 O3DE 编辑器中，布尔值由复选框表示。

### 数值（整数或浮点数）

以下示例是数值。

```lua
-- Numeric Examples
Properties = {
    Count = 5,
    Velocity = { 
        default = 1.0, 
        suffix = "m/s", 
        description = "Initial velocity of the object."
    },
    Distance = { 
        default = 5.0, 
        suffix = "m", 
        min = 2.0, 
        max = 10.0, 
        step = 2.0, 
        description = "Maximum distance an object can travel."
    }
}
```

在 O3DE 编辑器中，数值由带有增/减箭头的编辑字段表示。其他属性可能包含在数字属性定义中，这些属性：
+ 提供自定义后缀以指示单位。
+ 设置最小值和最大值。
+ 提供步长值（当用户单击编辑字段右侧的箭头时，该值增加或减少的程度）。

### 字符串

以下示例是字符串。

```lua
-- String Examples
Properties = {
    DebugPrefix = "d_",
    StartingState = { 
        default = "Idle", 
        description = "Sets the starting state."
    }
}
```

在 O3DE 编辑器中，字符串值由文本编辑框表示。

### 反射类

您可以使用同时反映到`BehaviorContext`和 `EditContext` 的任何类作为属性。一个很好的示例是引用实体的 `EntityId` 类型。

```lua
-- Entity Examples
Properties = {
    Target = EntityId(),
    ParentEntity = { 
        default = EntityId(), 
        description = "Sets the entity to track."
    }
}
```

编辑器表示是 reflected 类型的默认编辑器。例如，对于“EntityId”，它是实体引用选取器。对于大多数反射类型，它是类型属性的树。

```lua
...
-- Reflected Type Examples
Properties = {
    Vector2 = { 
        default = Vector2(1, 2), 
        min = 0, 
        max = 5, 
        step = 0.5 
    },
    Vector3 = { 
        default = Vector3(3, 4, 5)
    },
    Vector4 = { 
        default = Vector4(6, 7, 8, 9)
    },
    Color = { 
        default = Color(100, 200, 100)
    },
    SurfaceTag = { 
        default = SurfaceTag()
    }
}
```

![Reflected types as properties](/images/user-guide/scripting/lua/reflected-types.png)

### 数组

属性可以包含上述任何类型的可调整大小的数组。要创建数组，请将默认值声明为无键值表。例如，以下代码中的属性定义生成下图所示的属性。

```lua
-- Array Example
Properties = {
    ExampleArray = { 
        default = { 
            1, 2, 3, 4 
        } 
    }
}
```

![Property array](/images/user-guide/scripting/lua/array-example.png)

在Entity Inspector中，点击 {{< icon "add.svg" >}} 和 {{< icon "delete.svg" >}} 以实时添加和删除条目。您还可以将 `EntityId()`用于数组的值，以使数组元素成为实体引用。

## 分组属性

下面的代码示例演示如何使用 Properties Table 中的变量来公开属性的命名分组。

```lua
-- Grouped Properties Example
Properties = {
		Movement = {
			  TopSpeed = 4,
			  Acceleration = 2,
			  TurnSpeed = 12
		},
		Combat = {
			  ProjectileDamage = 50,
			  RateOfFire = 3,
			  AmmoCapacity = 12
		}
}
```

![Grouped properties](/images/user-guide/scripting/lua/property-groups.png)

## 属性

通过将属性包含在属性定义中的默认值中，将属性添加到属性中。属性键不区分大小写。

### 常见属性

Common 属性可以添加到任何属性。

|属性 |描述 |
| --- | --- |
| Description | 一个字符串，它是属性的工具提示的文本。 |
| UI | 指定 （覆盖） 属性使用的 UI 处理程序。  |

### 数字属性

数字属性只能添加到具有数字表示的属性中。

|属性 |描述 |
| --- | --- |
| Suffix |表示属性的度量单位的字符串。|
| Min | 属性可在 O3DE 编辑器中设置为的最小值。 |
| Max | 属性可以在 O3DE 编辑器中设置为的最大值。|
| Step | 在 O3DE 编辑器中更改属性值时将递增的量。 |
