---
linkTitle: UI Spawner
description: ' 使用 Open 3D Engine UI Spawner 组件在实体的位置生成运行时动态切片，并带有可选的偏移量。 '
title: UI Spawner 组件
---

使用 **UISpawner** 组件在实体的位置生成一个运行时动态 [slice](/docs/user-guide/interactivity/user-interface/slices) \(\*.`dynamicslice`\) ，并带有可选的偏移量。结合脚本，您可以使用 **UISpawner** 组件随时生成任何动态切片，并生成同一动态切片的多个实例。

![UISpawner component with an example slice file.](/images/user-guide/interactivity/user-interface/components/ui-editor-components-uispawner.png)

## UISpawner 组件属性

**UISpawner** 组件具有以下属性：

****Dynamic Slice****
选择要生成的切片资产。

****Spawn on Activate****
如果选中，则在激活时生成所选切片。

## EBus 请求总线接口

将以下请求函数与 **UiSpawnerBus** 事件总线接口结合使用，以便与游戏的其他组件进行通信。

有关使用事件总线 （EBus） 接口的更多信息，请参阅 [使用事件总线 （EBus） 系统](/docs/user-guide/programming/messaging/ebus/).

### Spawn 

在实体位置生成组件中指定的 UI 切片。

**参数**
None

**返回值**
此生成的切片实例化ticket。

**可脚本化**
Yes

### SpawnRelative 

在实体位置生成组件中指定的 UI 切片，并具有指定的相对偏移量。

**参数**
`Relative` - 带有 spawner 组件的实体的偏移位置。

**返回值**
此生成的切片实例化票证。

**可脚本化**
Yes

### SpawnViewport 

在指定的视区位置生成组件中指定的切片。

**参数**
`Pos` - 生成切片的视区位置。

**返回值**
此生成的切片实例化票证。

**可脚本化**
Yes

### SpawnSlice 

在实体的位置生成指定的切片。

**参数**
`slice` - 指定要生成的切片资产。

**返回值**
此生成的切片实例化票证。

**可脚本化**
No

### SpawnSliceRelative 

在具有相对偏移量的实体位置生成给定的切片。

**参数**
`slice` - 指定要生成的切片资产。
`relative` - 带有 spawner 组件的实体的偏移位置。

**返回值**
此生成的切片实例化票证。

**可脚本化**
No

### SpawnSliceViewport 

在指定的视区位置生成指定的切片。

**参数**
`slice` - 指定要生成的切片资产。
`pos` - 生成切片的视区位置。

**返回值**
此生成的切片实例化票证。

**可脚本化**
Yes

## EBus 通知总线接口

将以下通知函数与 UiSpawnerNotificationBus 事件总线接口结合使用，以便与游戏的其他组件进行通信。

有关使用事件总线 （EBus） 接口的更多信息，请参阅 [使用事件总线 （EBus） 系统](/docs/user-guide/programming/messaging/ebus/).

### OnSpawnBegin 

宣布已生成切片，但尚未激活实体。[OnEntitySpawned](#onentityspawned)事件即将被调度。

**参数**
`ticket` - spawn 函数返回的切片实例化票证。可以比较这些内容，以了解它与哪个 spawn 请求相关。

**可脚本化**
Yes

### OnSpawnEnd 

宣布已生成切片。此函数为每个生成请求调用一次。所有 [OnEntitySpawned](#onentityspawned)事件都已调度。

**参数**
`ticket` - spawn 函数返回的切片实例化票证。可以比较这些内容，以了解它与哪个 spawn 请求相关。

**可脚本化**
Yes

### OnEntitySpawned 

宣布在生成期间已创建实体。对于在生成切片时创建的每个实体，将调用此函数一次。

**参数**
`ticket` - spawn 函数返回的切片实例化票证。可以比较这些内容，以了解它与哪个 spawn 请求相关。
`spawnedEntity` - 指定生成的实体的 ID。

**可脚本化**
Yes

### OnEntitiesSpawned 

提供在生成期间创建的所有实体的列表。对于每个 spawn 请求，此函数仅调用一次。该函数在 [OnEntitySpawned](#onentityspawned) 调用之后和 [OnSpawnEnd](#onspawnend)调用之前调用。

**参数**
`ticket` - spawn 函数返回的切片实例化票证。可以比较这些内容，以了解它与哪个 spawn 请求相关。
`spawnedEntities` - 指定生成的实体的 ID。

**可脚本化**
Yes

### OnTopLevelEntitiesSpawned 

提供在生成期间创建的所有顶级实体的列表。对于每个 spawn 请求，此函数仅调用一次。该函数在 [OnEntitySpawned](#onentityspawned) 调用之后和 [OnSpawnEnd](#onspawnend) 调用之前调用。

顶级实体是在切片中没有任何父实体的实体。通常，每个切片只有一个顶级实体。

**参数**
`ticket` - spawn 函数返回的切片实例化票证。可以比较这些内容，以了解它与哪个 spawn 请求相关。
`spawnedEntities` - 指定生成的顶级实体的 ID。

**可脚本化**
Yes

### OnSpawnFailed 

宣布生成请求失败。

**参数**
`ticket` - spawn 函数返回的切片实例化票证。可以比较这些内容，以了解它与哪个 spawn 请求相关。

**可脚本化**
Yes
