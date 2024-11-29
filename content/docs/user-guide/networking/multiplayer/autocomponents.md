---
linktitle: 自动组件
title: Multiplayer 自动组件
description: 通过自动组件定义 Open 3D Engine （O3DE） 多人游戏状态的参考。
weight: 400
---

*自动组件* 提供了一种创建**Open 3D Engine （O3DE）** 多人游戏组件的便捷方式。这些组件定义与网络同步相关的状态。使用 [AzAutoGen](/docs/user-guide/programming/autogen) 系统，您可以在构建过程中处理在项目中找到的自动组件文件，以自动生成组件的 C++ 类，并创建提供网络复制和远程函数调用的控制器。自动组件还管理 [编辑](/docs/user-guide/programming/components/reflection/edit-context/) 和 [行为](/docs/user-guide/programming/components/reflection/behavior-context/) 上下文绑定，以便绑定组件显示在 **O3DE 编辑器** 中并与 O3DE 脚本一起使用。

要为您的项目启用自动组件构建，请按照 [多人游戏：项目配置](./configuration) 中的说明进行操作。


## 文件结构

自动组件在 XML 文件中定义，并放置在多人游戏 Gem 的`Code\Source\Autogen` 目录中。根据命名约定，自动组件文件名必须以后缀`.AutoComponent.xml`。

## 属性

多人游戏组件可以包含定义其功能和接口的各种属性，例如网络属性、RPC、网络输入和原型属性。

### Component

`Component` 标签定义多人游戏组件的名称、命名空间、包含路径和 C++ 覆盖行为。这是必需的，并且只应定义一次。

| 属性 |描述 |类型 |
|---|---|---|
| Name | 生成的 auto-component 的类名。该值必须是有效的 C++ 类名。 | `string` |
| Namespace | 生成的 auto-component 将放置在其中的命名空间。对于给定的 Gem，所有定义的 auto-components 都必须在同一命名空间中定义。在 Gem 中混合命名空间将导致编译器错误。该值必须是有效的 C++ 命名空间名称。 | `string` |
| OverrideComponent | 如果 `true`，生成的组件将是一个基类，实现多人游戏组件的开发人员将负责提供在编辑器和运行时使用的最终派生组件。 | `bool` |
| OverrideController | 如果 `true`，生成的控制器将是一个基类，实现多人游戏组件的开发人员将负责提供运行时中使用的最终派生控制器。 | `bool` |
| OverrideInclude | 如果 `OverrideComponent` 或 `OverrideController` 是 `true`，此值为 **required**，并且必须是包含具体实现的标头的路径（相对于 Gem 根）。实现最终组件或控制器类的开发人员必须提供在其中声明最终类的类头。 | `string` |

### ComponentRelation

`ComponentRelation` 标签表示该组件与其他组件的关系。使用此选项可定义此组件是否需要同一实体上的同级组件或是否与同一实体上的同级组件不兼容。例如，`NetworkCharacterComponent` 需要在相同的实体上有一个 `NetworkTransformComponent` 才能正常运行。

| 属性 |描述 |类型 |
|---|---|---|
| Name | 相关组件的名称。该值必须是有效的 C++ 类名。| `string` |
| Namespace | 在其中声明相关组件的命名空间。该值必须是有效的 C++ 命名空间名称。 | `string` |
| Include | 相关组件的 include 路径。 | `string` |
| Constraint | 相关组件与此组件的关系类型。允许的值为：<ul><li>**Required**: 相关组件必须存在于实体上。这些组件在 Editor 中显示为添加此 autocomponent 的硬性要求。</li><li>**Weak**: 相关组件不是必需的，但将在此自动组件之前在实体（如果存在）上激活。</li><li>**Incompatible**:相关组件与此 auto-component 不兼容。尝试将两个组件都放置在实体上将导致错误。</li></ul> | `Required`, `Weak`, `Incompatible` |
| HasController | 如果 `true`，则相关组件必须具有与之关联的多人游戏控制器。将此值设置为 true 将导致在控制器上生成控制器访问器。| `bool` |

对于具有 `Required` 或 `Weak`关系约束的组件，将在 auto-component 上生成名为 `Get<ComponentName>()` 的访问器。这些访问器返回一个缓存的指针，该指针指向在实体激活时创建的相关组件。

### Include

`Include`标签用于生成 C++ 代码的 `#include` 指令。`Include`标签包含单个头文件的路径。对生成的类将使用的每个头文件使用额外的`Include`标签。

| 属性 |描述 |类型 |
|---|---|---|
| File | 要添加为生成源的 `#include` 的标头的路径。 | `string` |


### Network 属性

网络属性 表示有关在网络终端节点之间复制的组件的信息。
他们有两个重要的字段： `ReplicateFrom` 和 `ReplicateTo`. 
这些字段共同定义哪个角色可以复制到另一个特定角色。
您只能从 Authority 和 Autonomous 角色复制属性值。
`ReplicateFrom` 授权关系会创建一个服务器授权模型，这有助于确保您不会意外地从非特权客户端复制。 
属性可以复制到任何角色，因为会话中的所有参与者都可能需要来自任何其他参与者的信息。

以下是具有有效“from 和 to”关系的四种类型的网络属性及其可能的用例：

- **Authority-to-Client**: 处理 Client 端或 “simulated” 属性。

- **Authority-to-Autonomous**: 处理仅限自主的属性。

- **Authority-to-Server**: 处理主机迁移。

- **Autonomous-to-Authority**: 收集有关客户端指标的信息，例如监控客户端的运行状况。

对于从 Authority 复制的网络属性，将应用复制层次结构。replicate-to 规则按以下方式在层次结构中向上流动：Authority-to-Client 复制还会复制到 Autonomous 和 Server 角色。Authority-to-Autonomous 复制也会复制到 Server 角色。最后，Authority-to-Server 复制仅复制到 Server。此行为层次结构可确保在权限迁移到其他服务器时，其他服务器具有正确的属性信息。

`NetworkProperty` 标签具有以下属性：

| 属性 | 说明 | 值 |
| - | - | - |
| Name | 网络属性的名称。 | `string` |
| Description | 描述 network 属性的功能。 | `string` |
| Type | 属性的类型必须是有效的 C++ 类型或类名。 | `string` |
| Init | 属性的初始值。 | `string` |
| ReplicateFrom | 告知网络哪个角色可以发送有关对此属性所做更改的信息。 | `Authority`, `Autonomous` | 
| ReplicateTo | 告知网络哪个角色可以接收有关对此属性所做更改的信息。 | `Server`, `Authority`, `Autonomous`, `Client` | 
| Container | 存储此网络属性的 holder 对象的类型。 | `Object`, `Array`, `Vector` |
| Count | 元素数，当 `Container` 是 `Array` 或 `Vector`。必须是整数值或整数类型变量。 | `string` |
| IsPublic | 如果 `true`，则此属性的访问修饰符设置为 `public`，并且可以从类外部访问。 | `bool` |
| IsRewindable | 如果 `true`中，网络模拟会记录每个网络时间时钟周期的此网络属性的历史值。如果网络属性在网络模拟中起着至关重要的作用，则需要回退功能，因为它与其他网络实体相关。例如，网络转换组件的网络属性是可倒回的。这一点很重要，因为如果玩家 A 在网络时间第 10 刻开枪，则主机必须将所有其他网络实体的转换组件倒回它们在第 10 刻的位置，以检查谁被击中。 | `bool` |
| IsPredictable | 如果为 true，则允许 Autonomous 播放器编辑此属性，即使 **ReplicateFrom** 是 `Authority` 而不是 `Autonomous`。自主播放器无法控制此网络属性，但他们可以在本地预测和更改其值。例如，使用 Network Transform 组件，允许自主玩家在本地更改其玩家的位置，甚至在获得服务器的明确移动许可之前。这会使玩家的动作感觉灵敏. | `bool` |
| ExposeToEditor | 如果 `true`，此属性可在 O3DE 编辑器中访问。 | `bool` |
| ExposeToScript | 如果 `true`，则可通过 Lua 和 ScriptCanvas 访问此属性，以便运行时编写脚本。  | `bool` |
| GenerateEventBindings | 如果 `true`，每当此网络属性的值发生变化时，都会触发 [AZ::Event](https://www.o3de.org/docs/user-guide/programming/az-event/) 回调。 | `bool` |

### Remote procedure calls (RPCs)

RPC 具有两个重要属性：`InvokeFrom` 和 `HandleOn`。这些属性描述从哪个角色调用 RPC 以及在哪个角色上处理 RPC。

以下是四种类型的 RPC，具有有效的“invoked from and handled on”关系及其可能的用例：

- **Authority-to-Autonomous**: 在示例用例中，它向用户发送有关游戏状态的更正。

- **Authority-to-Client**: Authority 向所有客户端发送调用。例如，它发送一个请求，以便在所有客户端上播放粒子效果。

- **Server-to-Authority**: 这是在实体之间传递信息所必需的。例如，假设在多服务器设置中，实体 A 归 ServerA 所有，实体 B 归 ServerB 所有。如果实体 A 直接与实体 B 通信，则实体 A 将与代理通信，而不是真正的实体 B。Server-to-Authority 确保消息始终找到具有权限的实体。例如，当玩家想要造成伤害时，Autonomous 会通知 Server，然后 Server 将 DealDamage 函数发送给 Authority。

- **Autonomous-to-Authority**: 发送影响用户输入的用户设置信息，这些信息在输入处理时而不是输入创建时使用，例如鼠标敏感度和输入控件。

`RemoteProcedure` 标签具有以下属性：

| 属性 | 说明 | 值 |
| - | - | - |
| Name | RPC 的名称。 | `string` |
| Param | 可以与 RPC 一起发送的参数和值对。您可以没有参数或无限数量的参数。您必须指定参数的类型和名称。 |  |
| Description | 描述 RPC 的功能。 | `string` |
| InvokeFrom | 告知网络哪个角色调用此 RPC。 | `Authority`, `Server`, `Autonomous` | 
| HandleOn | 告知网络哪些角色处理此 RPC。如果为 Authority，则 RPC 由对该实体具有权限的服务器处理。如果 Autonomous（自主），则 RPC 仅由相关玩家的本地客户端处理。如果为 Client，则在所有客户端上处理 RPC。 | `Authority`, `Autonomous`, `Client` |
| IsPublic | 如果 `true`，则此属性的访问修饰符设置为 '`public`'，允许其他人通过网络发送此 RPC。 | `bool` |
| IsReliable | 如果 `true`，RPC 是可靠的，并使用队列来保证消息的传递。否则，RPC 不可靠，并通过“即发即弃”方法发送。请注意，RPC 可以可靠地发送，但不能保证按任何特定顺序到达。  | `bool` |
| GenerateEventBindings | 如果 `true`, [AZ::Event](https://www.o3de.org/docs/user-guide/programming/az-event/) 每当此网络属性的值发生更改时，都会触发回调。 | `bool` |


### Archetype 属性

`ArchetypeProperty`tag 定义只能在编辑时配置的属性。当生成多人游戏组件时，可以通过组件菜单在 O3DE 编辑器中访问其原型属性。多人游戏组件可以没有原型属性，也可以有无限数量的原型属性。

| 属性 | 说明 | 值 |
| - | - | - |
| Name | 属性的名称。 | `string` |
| Type | 属性的类型必须是有效的 C++ 类型或类名。 | `string` |
| Init | 属性的初始值。 | `string` |
| Container | 存储此属性的 holder 对象的类型。 | `Object`, `Array`, `Vector` |
| Count | 元素的数量，当`Container` 是 `Array` 或 `Vector`.必须是整数值或整数类型变量。 | `string` |
| ExposeToEditor | 如果 `true`, 这可以在 O3DE 编辑器中访问。建议启用此设置，以使 archetype 属性有用。 如果你不想让用户访问此属性，禁用此设置，提供一个硬编码值 `Init`。  | `bool` |


### Network 输入

**网络输入**用于将输入数据从播放器发送到权威服务器。网络输入类似于特殊的 [远程过程调用 （RPC）](/docs/user-guide/networking/multiplayer/overview#remote-procedure-calls-rpcs)，多人游戏 Gem 在网络时钟周期的每一帧创建、处理和记录。当颁发机构收到来自播放器的网络输入时，它会使用帧编号标记网络输入。记下帧编号允许颁发机构 *倒回 *，这意味着它将所有可倒回的网络属性返回到历史上的那个时刻。在处理输入之前倒回对于保持多人游戏模拟同步至关重要。

| 属性 | 说明 |
| --- | --- |
| **Name** | 输入的名称。在代码中写入或读取输入时，将使用此名称。 |
| **Type** | 输入存储为的数据类型。 |
| **Init** | 数据的初始值。 |

#### 例如

```xml
<NetworkInput Type="StickAxis" Name="ForwardAxis" Init="0.0f" />
<NetworkInput Type="bool"      Name="Crouch"      Init="false" />
<NetworkInput Type="uint8_t"   Name="ResetCount"  Init="0" />
```

有关更完整的示例，请参阅 [`NetworkPlayerMovementComponent.AutoComponent.xml`](https://github.com/o3de/o3de-multiplayersample/blob/development/Gem/Code/Source/AutoGen/NetworkPlayerMovementComponent.AutoComponent.xml#L21-L28).

#### 在游戏逻辑中使用网络输入

在 C++ 和脚本中，具有网络输入的自动组件要求您实现以下多人游戏组件控制器函数：

- `CreateInput`: 定义此函数以返回一个填充的网络输入，其中包含自上次 tick 以来发生的所有记录的设备输入。多人游戏系统在每个网络时钟周期自动为自主玩家调用 CreateInput。这一点很重要，因为与单人游戏不同，多人游戏系统无法在收到设备输入后立即执行操作。相反，多人游戏系统会跟踪所有设备输入并将其存储在网络输入中。

- `ProcessInput`: 定义并使用此函数处理所有网络输入。在调用此函数之前，您应该只通过 CreateInput 记录设备输入，这还不会导致世界发生任何变化。调用 ProcessInput 时，服务器和客户端播放器将在同一网络时钟周期处理相同的网络输入。此函数需要自主玩家和权限。


### 例如

[`NetworkWeaponsComponent.AutoComponent.xml`](https://github.com/o3de/o3de-multiplayersample/blob/development/Gem/Code/Source/AutoGen/NetworkWeaponsComponent.AutoComponent.xml)是一个 auto-component 示例，该组件在多人游戏会话中同步表示武器状态的组件。

```xml
<?xml version="1.0"?>

<Component
    Name="NetworkWeaponsComponent"
    Namespace="MultiplayerSample"
    OverrideComponent="true"
    OverrideController="true"
    OverrideInclude="Source/Components/NetworkWeaponsComponent.h"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

    <ComponentRelation Constraint="Required" HasController="true" Name="NetworkAnimationComponent" Namespace="MultiplayerSample" Include="Source/Components/NetworkAnimationComponent.h" />

    <Include File="Source/Weapons/WeaponTypes.h" />

    <NetworkInput Type="bool" Name="Draw" Init="false" />
    <NetworkInput Type="WeaponActivationBitset" Name="Firing"  Init="" />

    <NetworkProperty Type="AZ::Vector3"  Name="TargetPosition"   Init="AZ::Vector3::CreateZero()" Container="Object"       ReplicateFrom="Authority" ReplicateTo="Client"     IsPublic="false" IsRewindable="false" IsPredictable="false" ExposeToEditor="false" GenerateEventBindings="false" Description="Target position the weapons component is currently aiming at" />
    <NetworkProperty Type="FireParams"   Name="ActivationParams" Init=""  Container="Array" Count="MaxWeaponsPerComponent" ReplicateFrom="Authority" ReplicateTo="Client"     IsPublic="false" IsRewindable="false" IsPredictable="false" ExposeToEditor="false" GenerateEventBindings="false" Description="Parameters for the current weapon activation" />
    <NetworkProperty Type="uint8_t"      Name="ActivationCounts" Init="0" Container="Array" Count="MaxWeaponsPerComponent" ReplicateFrom="Authority" ReplicateTo="Client"     IsPublic="false" IsRewindable="false" IsPredictable="false" ExposeToEditor="false" GenerateEventBindings="false" Description="The number of activations" />
    <NetworkProperty Type="WeaponState"  Name="WeaponStates"     Init=""  Container="Array" Count="MaxWeaponsPerComponent" ReplicateFrom="Authority" ReplicateTo="Autonomous" IsPublic="false" IsRewindable="false" IsPredictable="true"  ExposeToEditor="false" GenerateEventBindings="false" Description="The predictable states of the weapons" />

    <ArchetypeProperty Type="WeaponParams" Name="WeaponParams"  Init=""           Container="Array" Count="MaxWeaponsPerComponent" ExposeToEditor="true" Description="Parameters for the weapons attached to this NetworkWeaponsComponent" />
    <ArchetypeProperty Type="AZ::Name"     Name="FireBoneNames" Init="AZ::Name()" Container="Array" Count="MaxWeaponsPerComponent" ExposeToEditor="true" Description="Name of the bone to attach to for fire events" />

    <RemoteProcedure Name="SendConfirmHit" InvokeFrom="Authority" HandleOn="Client" IsPublic="false" IsReliable="false" GenerateEventBindings="false" Description="Single hit event confirmed by the server" >
        <Param Type="WeaponIndex" Name="WeaponIndex" />
        <Param Type="HitEvent"    Name="HitEvent" />
    </RemoteProcedure>

    <RemoteProcedure Name="SendConfirmProjectileHit" InvokeFrom="Authority" HandleOn="Client" IsPublic="false" IsReliable="false" GenerateEventBindings="false" Description="Fired by projectile entities on confirmed hit" >
        <Param Type="WeaponIndex" Name="WeaponIndex" />
        <Param Type="HitEvent"    Name="HitEvent" />
    </RemoteProcedure>
</Component>
```

## 构建自动组件

自动组件在编译和生成项目时进行处理。无论何时更新自动组件 XML 文件，都必须重新配置并重新编译 O3DE Editor、Game Launcher 和 Server Launcher。这是因为 XML 用于生成 C++ 文件，并且必须编译 C++ 文件。有关配置构建的更多信息，请参阅 [配置和构建](/docs/user-guide/build/configure-and-build).

与其他 O3DE 组件一样，请确保将 auto-component 文件添加到项目的 CMake 文件中，以便可以构建它们。同样，在更新任何 CMake 文件后，您必须重新配置并重新编译。

以下 `<your-project>_files.cmake` 示例列出了自动组件文件：

```cmake
set(FILES
    ...
    Source/AutoGen/NetworkTestPlayerComponent.AutoComponent.xml
    Source/AutoGen/MySimpleNetPlayerComponent.AutoComponent.xml    
)    
```
