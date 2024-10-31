---
title: Material 组件
linktitle: Material
description: 'Open 3D Engine (O3DE) Material 组件参考。'
toc: true
---

使用 **Material** 组件可以检查和自定义具有兼容渲染组件的实体上的材质和属性。目前，只有 [Mesh 组件](/docs/user-guide/components/reference/atom/mesh/) 和 [Actor 组件](/docs/user-guide/components/reference/animation/actor/) 支持材质组件。不过，任何组件都可以通过实现所需的总线和服务来支持 “材质 ”组件。材质组件使用材质资产，您可以使用 “材质编辑器 ”创建和配置材质资产。另一种生成材料资产的方法是资产处理器和模型资产生成器。这可以导出在源模型（如 FBX 文件）中定义的材质。

## 提供者 ##

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## Material 组件属性
默认情况下，Material组件只包含**Default Material**属性。材质组件通过 `MaterialConsumerRequestBus` 显示其他属性组，即**Model Materials**和**LOD Materials**。例如，网格组件实现了 `MaterialConsumerRequestBus` 来描述其模型资产所需的材料。每当材料需求发生变化时，网格组件就会通过 `MaterialConsumerNotificationBus` 发送通知。如果模型资产需要五种独特的材料，那么**Model Materials**属性组就会显示五个模型材料插槽。此外，如果模型资产包含三个 LOD，那么**LOD Materials**属性组就会显示三个 LOD 材料组及其各自的材料插槽。
1
| 说明 | 截图 |
|-|-|
| Default | ![material-component-base-properties](/images/user-guide/components/reference/atom/material/material-base-properties-ui.png) |
| Simple | ![material-component-slot-properties](/images/user-guide/components/reference/atom/material/material-slot-properties-ui.png) |
| Complex | ![material-component-extended-slot-properties](/images/user-guide/components/reference/atom/material/material-extended-slot-properties-ui.png) |

## Material组件功能

| 说明 | 截图 |
|-|-|
| Material 组件上下文菜单包含许多用于清除所有材质、模型材质和 LOD 材质的选项。菜单中的操作可用于消除先前分配的模型中可能保留的未使用的材料分配和属性，或者将材质组件复制并粘贴到一个新实体中。这些选项大多与下面详细介绍的 MaterialComponentRequestBus 上的功能相对应。 | ![material-componen-menu](/images/user-guide/components/reference/atom/material/material-menu-ui.png) |
| 每个材料槽还有一个上下文菜单。该菜单提供的选项可用于打开专用的 “材质编辑器”、打开Material Instance Inspector窗口以自定义所选实体的材质属性等。 | ![material-component-slot-menu](/images/user-guide/components/reference/atom/material/material-slot-menu-ui.png) |
| 通过**Generate/Manage Source Material**对话框，您可以从材料组件导出并自动分配`.material`源文件。如果您的工作流程不需要`.material`源文件，那么您可以使用**Material Instance Inspector**直接在实体上自定义材质。<br><br>您可以通过单击上下文菜单中的**Generate/Manage Source Materials...**或 Material 组件顶部的按钮来访问此对话框。该对话框显示与每个材料槽相关联的唯一材料列表。您可以配置将材料保存为目标文件名，也可以使用默认文件名。随后，对话框将生成`.material`源文件，并自动将其分配给每个匹配的材质槽。然后，您可以使用 “材质编辑器 ”打开和编辑“.材质`.material` 并与其他实体共享。 <br><br>尽管您并不需要使用此对话框，但它有助于简化为多个实体共享的材料进行分配的过程。例如，如果您选择了共享同一组材质插槽的多个实体，并从它们的上下文菜单中打开对话框，那么生成的材质就会自动分配到每个实体上。该对话框还可用于根据 FBX 或其他源中嵌入的材质建立材质库。 | ![material-componen-generate](/images/user-guide/components/reference/atom/material/material-generate-ui.png) |
| **Material Instance Inspector** 提供的功能与材料编辑器中的检查器相同。不过，该工具并不是创建子材质，而是通过覆盖分配给材质槽的资产值来自定义材质属性。您所做的更改会实时应用到实体，并在视口和预览中可见。<br><br>您可以从上下文菜单中选择 **Open Material Instance Editor…** 或 **Open Material Editor...**，打开Material Instance Inspector。 该窗口保持打开并锁定实体和材料槽选择，直到手动关闭为止。如果实体或所选材料槽无效或不可用，该窗口将被禁用。| ![material-componen-inspector](/images/user-guide/components/reference/atom/material/material-inspector-ui.png) |
1
| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Generate/Manage Source Materials...** | 这个按钮打开 **Generate/Manage Source Materials** 对话框。 | | |
| **Default Material** | 默认材质适用于整个模型，但已指定更高优先级模型或 LOD 材质的部件除外。 | 材质资产 | None |
| **Model Materials** | 除非分配了优先级更高的 LOD 材质，否则分配给这些槽的材质将应用于模型中具有相同材质槽名称的每个部分。 | 材质资产的容器 | Empty |
| **Enable LOD Materials** | 启用此标记后，可按 LOD 定制材质。 | Boolean | `Disabled` |
| **LOD Materials** | 分配给 LOD 材质槽的材质会覆盖给定 LOD 的**Default Material** 和 **Model Material**分配。如果您更改了默认材质或模型材质，但没有看到这些更改被应用，那么请确认没有 LOD 材质具有更高的优先级，或者完全禁用它们。  | 由LOD组织的材质资产的容器 | Empty |

## MaterialComponentRequestBus
该请求总线提供了与Material组件交互的接口。它包含许多与检查或自定义材料和属性相关的功能。所有这些功能都可以在编辑器、游戏和模拟中使用。其中许多功能对于扩展与材料组件相关的工具和脚本可能更有用。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| 'GetDefautMaterialMap' | 返回可用材料槽及其默认分配的映射。 | None | 未修改的材料槽布局: MaterialAssignmentMap | Yes |
| 'FindMaterialAssignmentId' | 搜索并返回与 LOD 索引和标签子串匹配的 MaterialAssignmentId。默认材料和模型材料的 LOD 索引为-1。如果没有找到匹配的材料，将返回默认材料。 | LOD Index: Integer, Label: String | Material Assignment Slot ID: MaterialAssignmentId | Yes |
| 'GetDefaultMaterialAssetId' | 返回材料槽的默认材料资产 | Material Assignment Slot ID: MaterialAssignmentId | Material Asset ID: AssetId | Yes |
| GetMaterialLabel' | 返回材料槽的唯一显示名称 | Material Assignment Slot ID: MaterialAssignmentId | Label: String | Yes |
| 'SetMaterialMap' | 替换材料槽的所有材料和属性定制数据 | Material Overrides: MaterialAssignmentMap | None | Yes |
| 'GetMaterialMap' | 返回材料槽的所有材料和属性 | None | Material Overrides: MaterialAssignmentMap | Yes |
| 'ClearMaterialMap' | 清除所有材料和属性定制 | None | None | Yes |
| 'ClearMaterialsOnModelSlots' | 清除非 LOD 材质定制 | None | None | Yes |
| 'ClearMaterialsOnLodSlots' | 清晰的 LOD 材质定制 | None | None | Yes |
| 'ClearMaterialsOnInvalidSlots' | 清除与可用材料槽不符的剩余材料 | None | None | Yes |
| 'ClearMaterialsWithMissingAssets' | 清除任何引用缺失资产的材料定制 | None | None | Yes |
| 'RepairMaterialsWithMissingAssets' | 通过为每个插槽分配默认资产，修复引用缺失资产的材料 | None | None | Yes |
| 'RepairMaterialsWithRenamedProperties' | 在可能的情况下，通过自动重命名来修复引用缺失属性的材料 | None | Number of properties updated: Integer | Yes |
| 'SetMaterialAssetIdOnDefaultSlot' | 为默认材料槽分配材料的便捷功能 | Material Asset ID: AssetId | None | Yes |
| 'GetMaterialAssetIdOnDefaultSlot' | 返回分配给默认材料槽的材料的方便函数 | None | Material Asset ID: AssetId | Yes |
| 'ClearMaterialAssetIdOnDefaultSlot' | 清除分配给默认材料槽的材料的便捷功能 | None | None | Yes |
| 'SetMaterialAssetId' | 设置材质资产 | Material Assignment Slot ID: MaterialAssignmentId, Material Asset ID: AssetId | None | Yes |
| 'GetMaterialAssetId' | 获得材质资产 | Material Assignment Slot ID: MaterialAssignmentId | Material Asset ID: AssetId | Yes |
| 'ClearMaterialAssetId' | 清除材质资产 | Material Assignment Slot ID: MaterialAssignmentId | None | Yes |
| 'SetPropertyValue' | 设置由 AZStd::any 包装的材料属性值 | Material Assignment Slot ID: MaterialAssignmentId, Property Name: String, Property Value: AZStd::any | None | Yes |
| 'GetPropertyValue' | 获取由 AZStd::any 包装的材料属性值 | Material Assignment Slot ID: MaterialAssignmentId, Property Name: String | Property Value: AZStd::any | Yes |
| 'ClearPropertyValue' | 清除材料槽的属性值 | Material Assignment Slot ID: MaterialAssignmentId, Property Name: String | None | Yes |
| 'ClearPropertyValues | 清除材料槽的属性值 | Material Assignment Slot ID: MaterialAssignmentId | None | Yes |
| 'ClearAllPropertyValues' | 清除所有属性值 | None | None | Yes |
| 'SetPropertyValues' | 为材料槽设置属性值 | Material Assignment Slot ID: MaterialAssignmentId , Material Properties: MaterialPropertyValueMap | None | Yes |
| 'GetPropertyValues' | 获取材料槽的属性值 | Material Assignment Slot ID: MaterialAssignmentId | Material Properties: MaterialPropertyValueMap | Yes |
| 'SetModelUvOverrides' | 为材料槽设置模型 UV 值 | Material Assignment Slot ID: MaterialAssignmentId , Material Model UV Overrides: MaterialModelUvOverrideMap | None | Yes |
| 'GetModelUvOverrides' | 获取材料槽的模型 UV 值 | Material Assignment Slot ID: MaterialAssignmentId | Material Model UV Overrides: MaterialModelUvOverrideMap | Yes |
| 'SetPropertyValueT | 封装 SetPropertyValue 的模板便捷函数，用于类型安全的行为上下文绑定 | Material Assignment Slot ID: MaterialAssignmentId, property name: String, Property Value: T | None | Yes |
| 'GetPropertyValueT | 封装 GetPropertyValue 的模板便捷函数，用于类型安全行为上下文绑定 | Material Assignment Slot ID: MaterialAssignmentId, property name: String | Property Value: T | Yes |

## MaterialComponentNotificationBus
这些通知会在编辑和运行时根据材料组件的状态变化发送。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| 'OnMaterialsEdited' | 此通知用于编辑时间。每当使用实体检查器更改属性值之外的材料组件配置时，都应发送该通知。该通知用于同步修改后的材料和材料实例检查器。 | None | None | No |
| 'OnMaterialsUpdated' | 每当材料实例因属性分配或删除而重新创建或销毁时，材料组件都会发送此通知。如果有任何变化，该通知会排队，每帧只发送一次。与材质实例相关的任何内容，如 [Mesh 组件](/docs/user-guide/components/reference/atom/mesh/) 和 [Actor 组件](/docs/user-guide/components/reference/animation/actor/)都必须侦听此通知，以便为其绘制数据包分配正确的材质实例。 | Material Overrides: MaterialAssignmentMap | None | No |
| 'OnMaterialInstanceCreated' | 每当材料组件创建了一个单独的材料实例时，就会发送此通知。只有当材料组件配置指定了属性重载以自定义特定材料的行为时，才会创建唯一的材料实例。 | Material Override: MaterialAssignment | None | No |

## MaterialConsumerRequestBus
"材料消费者“是指任何接受来自Material 组件的材料和属性更改的组件。实现此总线可为材质组件提供有关可用材质插槽、默认材质资产、标签和其他信息的详细信息。这些信息用于填充材质组件用户界面。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| 'FindMaterialAssignmentId' | 搜索并返回与 LOD 索引和标签子串匹配的 MaterialAssignmentId。默认材料和模型材料的 LOD 索引为-1。如果没有找到匹配的材料，将返回默认材料。 | LOD Index: Integer, Label: String | Material Assignment Slot ID: MaterialAssignmentId | Yes |
| 'GetMaterialAssignments' | 返回可用材料槽及其默认分配的地图 | None | Unmodified Material Slot Layout: MaterialAssignmentMap | Yes |
| 'GetModelMaterialSlots' | 返回包含可用材料槽详细信息（如稳定 ID 和标签名称）的映射。[Mesh 组件](/docs/user-guide/components/reference/atom/mesh/) 和 [Actor 组件](/docs/user-guide/components/reference/animation/actor/) 模型资产中提取这些数据，但其他任何渲染组件也可以提供相同的数据。 | None | Material Slot Layout: ModelMaterialSlotMap | No |
| 'GetModelUvNames' | 返回可用的 UV 通道名称映射图，可使用Material 组件对每个材质进行重映射。 | None | Model UV Channel Names: AZStd::unordered_set<AZ::Name> | Yes |

## MaterialConsumerNotificationBus
该通知总线用于通知 Material 组件 及其对应的 “编辑器 ”更改可用材质插槽的布局和默认值。从该总线消耗的通知会告知编辑器Material 组件需要进行用户界面更改。

| 方法名称 | 说明 | 参数 | 返回值 | 可脚本化 |
|-|-|-|-|-|
| 'OnMaterialAssignmentsChanged' | 每当可用材料插槽发生变化时，材料消费者必须发送此通知。编辑器Material组件收到该通知后，就会为可用插槽重建用户界面，以反映材料消费者可定制的内容。 | None| None | No |
