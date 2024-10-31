---
linkTitle: Audio Preload
title: Audio Preload 组件
description: 使用 Open 3D Engine 中的Audio Area Environment组件，可将环境效果应用于实体触发的声音。
toc: true
---

使用**Audio Preload**组件，您可以加载和卸载 ATL 预加载，其中包含对声音库的引用。您可以将加载类型指定为自动或手动。自动加载类型是指在组件激活时加载预载，停用时卸载预载。手动加载类型是指在用户提出请求之前，组件不采取任何行动。

## Audio Preload 属性

**Audio Preload** 组件具有以下属性：

**Preload Name**
该组件加载或卸载的默认 ATL 预载的名称。修改此属性可定义自定义默认 ATL 预加载。
默认值: Blank

**Load Type**
设置为 **Auto** 或 **Manual**.
如果设置为 **Auto**，当组件激活时加载预加载，当组件停用时卸载预加载。
如果设置为 **Manual**，只有当用户向界面提出请求时，预载才会加载和卸载。
默认值: Auto

## EBus请求总线接口

使用 EBus 接口的下列请求功能可与游戏的其他组件进行通信。

有关使用事件总线（EBus）接口的更多信息，请参阅 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus)。


**Load**

|  |  |
| --- |--- |
| 名称 | Load |
| 说明 | 加载默认 ATL 预加载（如果已设置） |
| 参数 | None |
| 返回值 | None |
| 可脚本化 | Yes |


**Load**

|  |  |
| --- |--- |
| 名称 | Unload |
| 说明 | 卸载默认 ATL 预加载（如果已设置）。 |
| 参数 | None |
| 返回值 | None |
| 可脚本化 | Yes |


**Load**

|  |  |
| --- |--- |
| 名称 | LoadPreload |
| 说明 | 按名称加载 ATL 预加载 |
| 参数 |  **preloadName**  要加载的 ATL Preload 的名称  |
| 返回值 | None |
| 可脚本化 | Yes |


**Load**

|  |  |
| --- |--- |
| 名称 | UnloadPreload |
| 说明 | 按名称卸载 ATL 预加载 |
| 参数 |  **preloadName**  要卸载的 ATL Preload 的名称  |
| 返回值 | None |
| 可脚本化 | Yes |


**Load**

|  |  |
| --- |--- |
| 名称 | IsLoaded |
| 说明 | 返回该组件是否已加载 ATL 预加载 |
| 参数 |  **preloadName**  要检查的 ATL Preload的名称  |
| 返回值 | bool 如果该组件已加载指定的预加载，则为 true，否则为 false |
| 可脚本化 | Yes |
