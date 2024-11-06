---
title: Terrain Macro Material 组件
linktitle: Terrain Macro Material
description: 'Open 3D Engine (O3DE) Terrain Macro Material 组件参考。'
weight: 100
---

**Terrain Macro Material** 组件将 Terrain Base Color 纹理应用于其体积内的所有 Terrain 区域。

## 提供者

[Terrain Gem](/docs/user-guide/gems/reference/environment/terrain)

## 依赖

[Terrain World](/docs/user-guide/components/reference/terrain/world)  
[Terrain World Renderer](/docs/user-guide/components/reference/terrain/world-renderer)  
[Axis Aligned Box Shape](/docs/user-guide/components/reference/shape/axis-aligned-box-shape)

**Terrain Macro Material** 还依赖于其体积中至少存在的一个 **Terrain Layer Spawner** ，因为此组件增强了现有的地形数据。

## 属性

![Terrain Macro Material component properties](/images/user-guide/components/reference/terrain/terrain-macro-material-component.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Color Texture** | 将在 terrain 上渲染的图像。 | Texture | None |
| **Normal Texture** | 将用作法线贴图的纹理。| Texture | None |
| **Normal Flip X** | 设置为 true 可围绕 X 轴翻转法线。 | Boolean | `False` |
| **Normal Flip Y** | 设置为 true 可绕 Y 轴翻转法线。| Boolean | `False` |
| **Normal Factor** | 调整法线值的强度。 | Float | `1.0` |
| **Priority** | 宏材质数据相对于其他 **Terrain Macro Material** 组件的优先级。值越大，优先级越高。 | Integer | `0` |
| **Save Mode** | 指定如何选择在编辑任何图像后保存图像的路径。 | `Save As...`, `Auto Save`, `Auto Save With Incrementing Names` | `Auto Save` |

## 用法

**Terrain Macro Material** 组件为体积内的地形系统提供低保真颜色信息。颜色数据用作[**Terrain World Renderer**](/docs/user-guide/components/reference/terrain/world-renderer) 组件上定义的 **细节材质渲染距离** 之外的地形的任何部分的唯一颜色源。在 **Detail material render distance** 内，颜色数据与 [地形细节材质](/docs/user-guide/components/reference/terrain/terrain-detail-material) 混合，以便在细节纹理在表面上重复时提供颜色变化。

该组件还为地形系统提供可选的宏法线信息，以便远距离地形可以在较低多边形 LOD 上具有更高质量的法线。宏法线很难正确创作，因此不建议将其用于典型用途。宏法线必须位于世界空间中，并在所有维度上以与 O3DE 关卡中的地形相同的地形比例生成。如果 O3DE 中的地形在任何方向上调整大小不均匀，则法线将不再与地形几何体对齐，并且地形表面照明将不正确。

您可以通过调整实体上的 [Axis Aligned Box Shape](/docs/user-guide/components/reference/shape/axis-aligned-box-shape)  组件来配置体积的尺寸。您可以通过将纹理资产拖动到 **Color Texture** 或 **Normal Texture** 字段，或者单击 {{< icon "file-folder.svg" >}} 来分配颜色纹理和可选的法线纹理。分配纹理后，您可以使用 **Normal Flip X** 和 **Normal Flip Y** 切换以及 **Normal Factor** 滑块来调整法线的方向和幅值。

如果方便，**Terrain Macro Material** 可以与 **Terrain Layer Spawner** 组件存在于同一实体上，但这不是必需的。此组件可用于任何世界区域，无论它与多个刷怪箱重叠、单个刷怪箱或根本没有刷怪箱。宏材质数据将应用于出现在其体积内的任何地形数据，并且不会在缺少地形的地方进行渲染。这种灵活性允许以与其他地形数据不同的大小和分辨率来创作、加载和卸载地形宏材质。

如果两个 **Terrain Macro Material** 体积重叠，则 **Priority** 字段指示将使用哪种宏材质数据。值越大，优先级越高。

### 创建新图像

要创建新映像，请按 **Create New Image...** 按钮。系统将提示您输入图像的宽度和高度（以像素为单位），然后输入保存图像的位置。如果图像保存到项目使用的 [源资产目录](/docs/user-guide/assets/pipeline/scan-directories/)中，则地形宏材质将自动使用保存的图像填充 **Color Texture** 字段。

## 编辑图像

要编辑现有颜色纹理，请按 Terrain Macro Material 组件上的 **Edit** 按钮，或在视区的 **Component Switcher** 中选择 Terrain Macro Material 组件的图标。这将进入 Paint Brush 模式。有关如何使用 Paint Brush 的更多详细信息，请参阅 [Paint Brush](/docs/user-guide/components/reference/paintbrush/paintbrush)文档。

编辑完成后，按 **Esc**、按 Terrain Macro Material 组件上的 **Done** 按钮或在视区的 **Component Switcher** 中选择其他组件的图标来结束 Paint Brush 模式。此时，图像更改将被保存，由 Asset Processor 重新处理，并在处理完成后重新加载。

**Save Mode** 确定图像的保存位置。

| 保存模式 | 说明 |
| - | - |
| `Save As...` | 每次保存图像时，都会提示输入存储位置。 |
| `Auto Save` | 在加载关卡后首次保存图像时提示输入保存位置，但在后续每次保存时，它都会自动覆盖该位置的图像。 |
| `Auto Save With Incrementing Names` | 自动保存名称末尾带有递增数字的图像，并且仅当已有具有该名称的现有图像时，才会提示输入存储位置。<br><br>例如，如果最初选择的图像是 '`image.tif`'，这会将其保存为 '`image.0000.tif`'，然后是 '`image.0001.tif`'，然后是 '`image.0002.tif`'，依此类推。 |

## MacroMaterialData

此结构在发送有关宏材质设置的信息时使用。

| 字段 | 说明 | 类型 |
|-|-|-|
| **m_entityId** | 拥有实体的 EntityId。 | `AZ::EntityId` |
| **m_bounds** | 此宏材质组件影响的区域的边界。 | `AZ::Aabb` |
| **m_colorImage** | 应应用于地形的图像。 | `AZ::Data::Instance<AZ::RPI::Image>` |
| **m_normalImage** | 要在此区域中使用的法线贴图。 | `AZ::Data::Instance<AZ::RPI::Image>` |
| **m_normalFlipX** | 法线贴图是否应绕 X 轴翻转。 | `bool` |
| **m_normalFlipY** | 法线贴图是否应绕 Y 轴翻转。 | `bool` |
| **m_normalFactor** | 法线贴图的强度。 | `float` |

## TerrainMacroMaterialRequestBus

'`TerrainMacroMaterialRequestBus`' 是一个内部系统总线，仅用于地形渲染和编辑系统与 Terrain Macro Material 组件之间的通信。其他系统通常不需要使用此 EBus，因为 terrain 系统之外的任何内容都不需要来自各个组件实例的任何信息。但是，如果出现使用案例，可以使用“`TerrainMacroMaterialRequestBus`”事件总线接口上的以下请求函数来查询各个地形宏材质组件。

| 方法名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `GetTerrainMacroMaterialData` | 返回分配给 **Terrain Macro Material** 组件的 '`MacroMaterialData`' 结构。 | None | [MacroMaterialData](#macromaterialdata) | Yes |
| `GetTerrainMacroColorImageSize` | 返回颜色纹理的高度/宽度/深度（以像素为单位）。 | None | RHI::Size | No |
| `GetMacroColorImagePixelsPerMeter` | 返回世界空间中每米的颜色纹理像素数。 | None | Vector2 | No |

## TerrainMacroMaterialNotificationBus

“`TerrainMacroMaterialNotificationBus`”是地形渲染和编辑系统使用的内部 EBus，用于监控地形宏材质的更改。其他系统通常不需要使用此 EBus，因为 terrain 系统之外的任何内容都不需要有关单个 Terrain Macro Materials 的任何信息。但是，如果出现使用案例，可以使用“`TerrainMacroMaterialNotificationBus`”事件总线接口上的以下通知函数来监控地形宏材质更改。

| 通知名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `OnTerrainMacroMaterialCreated` | 在创建新的宏素材时通知侦听器。 | None | EntityId; [MacroMaterialData](#macromaterialdata) | Yes |
| `OnTerrainMacroMaterialChanged` | 当宏素材发生更改时通知侦听器。 | None | EntityId; [MacroMaterialData](#macromaterialdata) | Yes |
| `OnTerrainMacroMaterialDestroyed` | 在删除宏素材时通知侦听器。 | None | EntityId | Yes |
| `OnTerrainMacroMaterialRegionChanged` | 当宏素材的边界区域发生变化时通知侦听器。 | None | EntityId; Old Region: Aabb; New Region: Aabb | Yes |

## TerrainMacroColorModificationBus

'`TerrainMacroColorModificationBus`' 是地形编辑系统用于修改地形宏材质颜色纹理的内部事件总线。这是一个较低级别的 API，大多数系统应该使用更高级别的 [Paint Brush](/docs/user-guide/components/reference/paintbrush/paintbrush)绘画 API 来修改纹理。

| 通知名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `StartMacroColorImageModification` | 启动图像修改会话。 | None | None | No |
| `EndMacroColorImageModification` | 完成图像修改会话。 | None | None | No |
| `GetMacroColorPixelIndicesForPositions` | 给定世界位置列表，将像素索引列表返回到图像中。 | positions: 要查询的位置列表, outIndices: 像素索引的输出列表 | None | No |
| `GetMacroColorPixelValuesByPosition` | 获取位置列表中的图像像素值。 | positions: 要查询的位置列表, outValues: 像素颜色的输出列表 | None | No |
| `GetMacroColorPixelValuesByPixelIndex` | 获取像素索引列表中的图像像素值。 | indices: 要查询的像素索引列表, outValues: 像素颜色的输出列表 | None | No |
| `StartMacroColorPixelModifications` | 开始一系列像素修改。 | None | None | No |
| `EndMacroColorPixelModifications` | 完成一系列像素修改。 | None | None | No |
| `SetMacroColorPixelValuesByPixelIndex` | 在像素索引列表中设置图像像素值。 | indices: 要为其设置值的像素索引列表, outValues: 要设置的像素颜色列表 | None | No |

## TerrainMacroColorModificationNotificationBus

“`TerrainMacroColorModificationNotificationBus`”是地形编辑系统用于修改地形宏材质颜色纹理的内部事件总线。这是一个较低级别的 API，大多数系统应该使用更高级别的 [Paint Brush](/docs/user-guide/components/reference/paintbrush/paintbrush)绘画 API 来修改纹理。

| 通知名称 | 说明 | 参数 | 返回值 | 脚本化 |
|-|-|-|-|-|
| `OnTerrainMacroColorBrushStrokeBegin` | 通知任何侦听器，画笔描边已在宏彩色图像上开始。 | None | None | No |
| `OnTerrainMacroColorBrushStrokeEnd` | 通知任何侦听器，画笔描边已在宏彩色图像上结束。 | changedDataBuffer: 指向包含已更改数据的 ImageTileBuffer 的指针, dirtyRegion: 定义受画笔描边影响的世界空间区域的 AABB | None | No |
