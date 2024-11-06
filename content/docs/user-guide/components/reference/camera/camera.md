---
linkTitle: Camera
title: Camera 组件
description: ' 使用Camera组件可将实体用作Open 3D Engine (O3DE)中的摄像机。 '
---

**Camera** 组件向实体添加摄像机。

## 提供方 ##

[Camera Gem](/docs/user-guide/gems/reference/rendering/camera)

## Camera 属性 

![Camera component properties in the Entity Inspector.](/images/user-guide/components/reference/camera/camera-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Be this camera** | 选择 **Be this camera** 按钮可激活视口中的摄像机视图。 更多信息，请参阅 [视口操纵器](/docs/user-guide/editor/viewport#manipulators)。 |||
| **Orthographic** | 如果设置为启用，此摄像机将使用正投影而不是透视投影。无论物体与摄像机的距离如何，它们看起来都是一样大。 | Boolean | `Disabled` |
| **Field of view** | 垂直视场角（度）。 | 0.0 - 180.0 | `75` |
| **Near clip distance** | 到视锥近夹平面的距离（米）。必须小于**Far clip distance**。 | 0.001 to Infinity | `0.2` |
| **Far clip distance** | 到视锥近远平面的距离，以米为单位。必须大于 **Near clip distance**。 | 0.001 to Infinity | `1024` |
| **Make active camera on activation** | 如果设置为启用，则在组件激活时，该摄像机将成为活动的渲染摄像机。 | Boolean | `Enabled` |
| **Debug - Frustrum length**| 圆锥曲线形状的长度占**Far clip distance**的百分比。 | 0.01 - 100.0 | `1.0` |
| **Debug - Frustrum color** | 蘑菇头形状的颜色。 | 每通道 8 位颜色： 0-255 | `255,255,0` |

## CameraRequestBus

| 请求名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetFarClipDistance` | 返回摄像机的**Far clip distance**，单位为米。 | None | Far Clip Distance: Float | Yes |
| `GetFov` | 以度数为单位返回摄像机的**Field of view**。 | None | FOV: Float | Yes |
| `GetFovDegrees` | 以度数为单位返回摄像机的**Field of view**。 | None | FOV: Float | Yes |
| `GetFovRadians` | 以弧度为单位返回摄像机的**Field of view**。 | None | FOV: Float | Yes |
| `GetNearClipDistance` | 返回摄像机的**Near clip distance**，单位为米。 | None | Near Clip Distance: Float | Yes |
| `GetOrthographicHalfWidth` | 返回摄像机的正交半宽。 | None | Half-Width: Float | Yes |
| `IsActiveView` | 如果摄像机是当前活动视图，则返回 `True`。 | None | Boolean | Yes |
| `IsOrthographic` | 如果摄像机设置为使用正交透视，则返回 `True`。 | None | Boolean | Yes |
| `MakeActiveView` | 将摄像机设置为活动视图。| None | None | Yes |
| `SetFarClipDistance` | 设置摄像机的**Far clip distance**，单位为米。 | Far Clip Distance: Float | None | Yes |
| `SetFov` | 设置摄像机的**Field of view**，单位为度。 | FOV: Float | None | Yes |
| `SetFovDegrees` | 设置摄像机的**Field of view**，单位为度。 | FOV: Float | None | Yes |
| `SetFovRadians` | 以弧度为单位设置摄像机的**Field of view**。 | FOV: Float | None | Yes |
| `SetNearClipDistance` | 设置摄像机的**Near clip distance**，单位为米。 | Near Clip Distance: Float | None | Yes |
| `SetOrthographic` | 如果为 `True`，则设置摄像机使用正交透视。 | Boolean | None | Yes |
| `SetOrthographicHalfWidth` | 设置摄像机的正交半宽。| Half-Width: Float | None | Yes |


## CameraNotificationBus

| 请求名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `OnActiveViewChanged` | 通知侦听者新摄像机已成为活动视图。 | None | EntityId | Yes |
| `OnCameraAdded` | 通知侦听者关卡中启用了新的摄像机。 | None | EntityId | Yes |
| `OnCameraRemoved` | 通知侦听者关卡中的摄像头已被停用。 | None | EntityId | Yes |

## CameraSystemRequestBus

| 请求名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| `GetActiveCamera` | 返回活动摄像机的 EntityId。 | None | EntityId | Yes |

更多信息，请参阅 [使用事件总线 (EBus) 系统](/docs/user-guide/programming/messaging/ebus/)。

## Lua 脚本示例

以下是使用`CameraRequestBus`的 Lua 脚本示例。

```lua
local camerasample =
{
    Properties =
    {
    }
}

function camerasample:OnActivate()
    CameraRequestBus.Event.SetFov(self.entityId, 85)
    local nearClip = CameraRequestBus.Event.GetNearClipDistance(self.entityId)
    CameraRequestBus.Event.SetFarClipDistance(self.entityId, nearClip + 1024)
end

return camerasample
```

## 从视图创建摄像机实体

右键单击视口中的实体并选择**Create camera entity from view**，即可从特定实体创建静态摄像机视图。这样就会在同一点放置一个带有摄像机组件的新实体。您可以通过修改摄像机的变换组件来调整摄像机的视图。
