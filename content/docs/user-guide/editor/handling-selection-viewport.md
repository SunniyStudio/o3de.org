---
description: ' 更新您的组件，使其知道如何处理 Open 3D 引擎视口中的选择。 '
title: '步骤 5：处理视口中的选区'
draft: true
---

在以下步骤中，请修改代码，以便双击视口中的组件进入组件模式。

在组件模式下，您可以直接在视口中修改**Point Light**组件的尺寸。

**处理视口中的选择**

1. 在文本编辑器中，打开`EditorPointLightComponent.h`文件。

1. 对最后的参数，添加`EditorComponentSelectionRequestsBus::Handler`。

   ```
   class EditorPointLightComponent
           : public EditorLightComponent
           , private AzToolsFramework::EditorComponentSelectionRequestsBus::Handler
   ```

1. 要实现`EditorComponentSelectionRequests`，必须重写以下4个函数：

   1. `GetEditorSelectionBoundsViewport` - 返回包含组件可见范围的 AABB

   1. `EditorSelectionIntersectRayViewport` - 为组件执行选择的位置

   1. `SupportsEditorRayIntersect` - 重载此函数，如果执行了 `EditorSelectionIntersectRayViewport`，则返回 `true`

   1. `GetBoundingBoxDisplayType` - 用于调试，以确保 AABB 适合正确。此示例将函数设置为 `NoBoundingBox`

   ```
   // EditorComponentSelectionRequests
   AZ::Aabb GetEditorSelectionBoundsViewport(
       const AzFramework::ViewportInfo& viewportInfo) override;
   bool EditorSelectionIntersectRayViewport(
       const AzFramework::ViewportInfo& viewportInfo,
       const AZ::Vector3& src, const AZ::Vector3& dir, AZ::VectorFloat& distance) override;
   bool SupportsEditorRayIntersect() override;
   AZ::u32 GetBoundingBoxDisplayType() override;
   ```

1. 保存文件。

1. 在文本编辑器中，打开`EditorPointLightComponent.cpp`文件。

1. 在组件的`Activate` 和 `Deactivate` 函数中，分别连接和断开连接到`EditorComponentSelectionRequestsBus`。

   ```
   void EditorPointLightComponent::Activate()
   {
       ...
       AzToolsFramework::EditorComponentSelectionRequestsBus::Handler::BusConnect(GetEntityId());
       ...
   }

   void EditorPointLightComponent::Deactivate()
   {
       ...
       AzToolsFramework::EditorComponentSelectionRequestsBus::Handler::BusDisconnect();
       ...
   }
   ```

1. 向你的代码添加以下变更：
    + 添加`SupportsEditorRayIntersect`的实现，返回`true`。默认情况下，此函数返回`false`。
    + 添加`GetBoundingBoxDisplayType`的实现，返回`AzToolsFramework::EditorComponentSelectionRequests::BoundingBoxDisplay::NoBoundingBox`。

    ```
    bool EditorPointLightComponent::SupportsEditorRayIntersect()
    {
       return true;
    }

       AZ::u32 EditorPointLightComponent::GetBoundingBoxDisplayType()
    {
           return AzToolsFramework::EditorComponentSelectionRequests::BoundingBoxDisplay::NoBoundingBox;}
    ```
    {{< note >}}
可以返回`AzToolsFramework::EditorComponentSelectionRequests::BoundingBoxDisplay:BoundingBox`以进行调试，但不应该启用它。
{{< /note >}}

   接下来的两个函数展示了如何实现提取和选择支持。

1. 为`GetEditorSelectionBoundsViewport`函数添加实现。

1. 以组件为中心创建一个覆盖其外延的 AABB。在这种情况下，获取实体在世界空间中的位置，然后以点光源的半径创建一个 AABB。由于点光源是以球体表示的，因此应使用 `GetPointMaxDistance` 函数。
**示例**

   您的代码应如下所示。

   ```
   AZ::Aabb EditorPointLightComponent::GetEditorSelectionBoundsViewport(
       const AzFramework::ViewportInfo& viewportInfo)
   {
       AZ::Vector3 worldTranslation = AZ::Vector3::CreateZero();
       AZ::TransformBus::EventResult(
           worldTranslation, GetEntityId(), &AZ::TransformInterface::GetWorldTranslation);

       return AZ::Aabb::CreateCenterRadius(worldTranslation, GetPointMaxDistance());
   }
   ```

   下一步，更改 `EditorSelectionIntersectRayViewport` 函数。
**示例**

   ```
   // top of file
   <AzToolsFramework/Picking/Manipulators/ManipulatorBounds.h>


   ...

   bool EditorPointLightComponent::EditorSelectionIntersectRayViewport(
       const AzFramework::ViewportInfo& viewportInfo,
       const AZ::Vector3& src, const AZ::Vector3& dir, AZ::VectorFloat& distance)
   {
       AZ::Transform worldFromLocal = AZ::Transform::CreateIdentity();
       AZ::TransformBus::EventResult(
           worldFromLocal, GetEntityId(), &AZ::TransformInterface::GetWorldTM);

       const float minorRadius = 0.1f;
       const float majorRadius = GetPointMaxDistance();

       const AZ::Vector3 axes[] = {
           AZ::Vector3::CreateAxisX(), AZ::Vector3::CreateAxisY(), AZ::Vector3::CreateAxisZ()
       };

       enum { AxisCount = 3 };
       float distances[AxisCount] = { FLT_MAX, FLT_MAX, FLT_MAX };
       bool intersection = false;
       for (size_t axisIndex = 0; axisIndex < AxisCount; ++axisIndex)
       {
            intersection = intersection
                || AzToolsFramework::Picking::IntersectHollowCylinder(
                    src, dir, worldFromLocal.GetTranslation(), axes[axisIndex],
                    minorRadius, majorRadius, distances[axisIndex]);
       }

       distance = AZ::GetMin(AZ::GetMin(distances[0], distances[1]), distances[2]);

       return intersection;
   }
   ```

1. 获取实体在世界空间中的位置，并用近似的环形或扁平空心圆柱体来表示**Point Light**组件的环。小半径对应于圆环的管状部分，也就是它的厚度。

   ```
   const float minorRadius = 0.1f;
   const float majorRadius = GetPointMaxDistance();
   ```

1. 您需要一个合理大小的半径，以便在视口中轻松选择。主半径是指从圆环中心到圆管中间的距离。由于每个轴都有一个环，因此请检查每个轴是否都使用了`IntersectHollowCylinder`函数，该函数基本上近似于一个环。

1. 为每个轴测试一个环，并存储交点距离，以找到最近的交点。

   ```
   {
       intersection = intersection
            || AzToolsFramework::Picking::IntersectHollowCylinder(
                src, dir, worldFromLocal.GetTranslation(), axes[axisIndex],
                minorRadius, majorRadius, distances[axisIndex]);
   }
   ```

   如果交叉成功，则返回最短的距离。

1. 保存文件。
