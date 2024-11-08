---
linkTitle: Vegetation
title: Vegetation Gem
description: Vegetation Gem 提供在 Open 3D Engine （O3DE） 中放置自然植被的工具。
weight: 400
toc: true
---

**Vegetation Gem** 提供了用于以程序方式填充景观和环境的工具。这些工具由系统和编辑器组件组成，这些组件使用数据驱动的方法在运行时动态地自动选择、放置和管理植被对象。你可以使用这些工具来替换或补充手动放置、编辑和保存关卡中的每个实例。

动态植被系统的许多功能都依赖于其他 Gem 和组件来提供有关环境的数据，例如表面、图像和渐变信号，以指导植被的显示位置和方式。

## 依赖

- [Surface Data Gem](/docs/user-guide/gems/reference/environment/surface-data/)
- [Gradient Signal Gem](/docs/user-guide/gems/reference/utility/gradient-signal/)
- [FastNoise Gem](/docs/user-guide/gems/reference/utility/fast-noise/) (可选)

[Surface Data Gem](/docs/user-guide/gems/reference/environment/surface-data/) 允许表面（如地形或网格）发出传达其表面类型的信号或标签。例如，使用 [Vegetation Surface Mask Filter](/docs/user-guide/components/reference/vegetation-filters/vegetation-surface-mask-filter) 组件，您可以使用包含和排除列表来选择可以在特定表面上放置哪些类型的植被。您还可以使用 [Surface Mask Gradient](/docs/user-guide/components/reference/gradients/surface-mask-gradient/)组件将标签重新捕获为渐变信号。

[Gradient Signal Gem](/docs/user-guide/gems/reference/utility/gradient-signal/)提供将数据定向到植被系统的组件，从而控制动态植被的外观。将梯度信号与植被修饰符或过滤器结合使用，例如[Position Modifier](/docs/user-guide/components/reference/vegetation-modifiers/vegetation-position-modifier) 和[ Distribution Filter](/docs/user-guide/components/reference/vegetation-filters/vegetation-distribution-filter)  组件，可在世界中生成逼真的随机植被表达式。

## 提供

组件实体：
- [Vegetation Asset List](/docs/user-guide/components/reference/vegetation/vegetation-asset-list)
- [Vegetation Asset List Combiner](/docs/user-guide/components/reference/vegetation/vegetation-asset-list-combiner)
- [Vegetation Asset Weight Selector](/docs/user-guide/components/reference/vegetation/vegetation-asset-weight-selector)
- [Vegetation Layer Blender](/docs/user-guide/components/reference/vegetation/vegetation-layer-blender)
- [Vegetation Layer Blocker](/docs/user-guide/components/reference/vegetation/vegetation-layer-blocker)
- [Vegetation Layer Blocker (Mesh)](/docs/user-guide/components/reference/vegetation/vegetation-layer-blocker-mesh)
- [Vegetation Layer Debugger](/docs/user-guide/components/reference/vegetation/vegetation-layer-debugger)
- [Vegetation Layer Spawner](/docs/user-guide/components/reference/vegetation/layer-spawner)
- [Vegetation Position Modifier](/docs/user-guide/components/reference/vegetation-modifiers/vegetation-position-modifier)
- [Vegetation Rotation Modifier](/docs/user-guide/components/reference/vegetation-modifiers/vegetation-rotation-modifier)
- [Vegetation Scale Modifier](/docs/user-guide/components/reference/vegetation-modifiers/vegetation-scale-modifier)
- [Vegetation Slope Alignment Modifier](/docs/user-guide/components/reference/vegetation-modifiers/vegetation-slope-alignment-modifier)
- [Vegetation Altitude Filter](/docs/user-guide/components/reference/vegetation-filters/vegetation-altitude-filter)
- [Vegetation Distance Between Filter](/docs/user-guide/components/reference/vegetation-filters/vegetation-distance-between-filter)
- [Vegetation Distribution Filter](/docs/user-guide/components/reference/vegetation-filters/vegetation-distribution-filter)
- [Vegetation Shape Intersection Filter](/docs/user-guide/components/reference/vegetation-filters/vegetation-shape-intersection-filter)
- [Vegetation Slope Filter](/docs/user-guide/components/reference/vegetation-filters/vegetation-slope-filter)
- [Vegetation Surface Mask Depth Filter](/docs/user-guide/components/reference/vegetation-filters/vegetation-surface-mask-depth-filter)
- [Vegetation Surface Mask Filter](/docs/user-guide/components/reference/vegetation-filters/vegetation-surface-mask-filter)

关卡组件：
- Vegetation System Settings 组件
- Vegetation Debugger 组件

系统组件：
- [Vegetation Area System](/docs/user-guide/components/reference/vegetation/vegetation-area-system)
- [Vegetation Instance System](/docs/user-guide/components/reference/vegetation/vegetation-instance-system)

API: [Vegetation Gem API 参考](https://www.o3de.org/docs/api/gems/vegetation/index.html)

源代码: [`/Gems/docs/user-guide/components/reference/Vegetation/`](https://github.com/o3de/o3de/tree/development/Gems/Vegetation/Code)

## Vegetation Areas

动态植被系统围绕 *植被区域* 的概念构建，该概念描述了植被将在表面上生成的内容、位置、方式以及是否生成。
引擎中的其他系统记录和处理 Vegetation 区域。
他们可以检查世界中的点并对其执行操作，例如放置或阻止植被实例。

在 **O3DE 编辑器** 中，植被区域称为 *植被层*。
您可以使用描述、选择、拒绝或改变潜在植被实例的组件将植被行为添加到植被图层。
这些行为分别称为植被描述符提供者、选择器、过滤器和修饰符。
每个行为都有一个接口，该接口使用新功能扩展系统。
Vegetation Gem 包含每种类型的多个版本，每种类型对其生成的植被实例都有独特的用途和影响。
这些组件公开了一些控件，这些控件允许快速、基于规则、程序地填充世界上任何任意大小的部分，其中包含装饰性内容。
根据其配置，单个植被层可以生成一小片花朵，或者智能地用令人信服的植物和物体的种类和分组覆盖整个世界。

## 植被实例

*植被实例* 是放置在世界各地的对象。创建它们时，您可以使用植被描述符配置它们的信息。
其中包括唯一 ID、转换、其他属性和对源植被描述符的引用。

在 O3DE 编辑器中，植被实例显示在植被区域内。它们是根据植被区域的配置以程序方式生成的。

## 植被描述符

*植被描述符* 是指定表示一种植被类型所需的所有常见细节的结构。它包括网格和材质资产的数据、它创建的植被实例的类型，以及许多高级参数，这些参数可以启用这些参数来覆盖大多数过滤器和修饰器的行为。在 [Vegetation Asset List](/docs/user-guide/components/reference/vegetation/vegetation-asset-list)  组件中或通过 **资产编辑器** 创建 **植被描述符**。

|组名称 |参数名称 |描述 |类型 |
| --- | --- | --- | --- |
|     | **Weight** | 权重在生成和选择过程中用作乘数，用于更改选择一个描述符而不是另一个描述符的可能性。 | Float |
|     | **Advanced** | 启用后，将显示以下通常隐藏的高级设置。| Bool |
| Position |     | 用于控制对[Position Modifier](/docs/user-guide/components/reference/vegetation-modifiers/vegetation-position-modifier)组件的逐实例覆盖的设置。|     |
|     | **Position Override Enabled** | 启用后，[Position Modifier](/docs/user-guide/components/reference/vegetation-modifiers/vegetation-position-modifier)  组件可以使用描述符中指定的每个实例覆盖值，而不是组件配置。 | Bool |
|     | **Position Min X** | 覆盖 X 轴上最小位置偏移的值。 | Float |
|     | **Position Max X** | 覆盖 X 轴上最大位置偏移的值。 | Float |
|     | **Position Min Y** | 覆盖 Y 轴上最小位置偏移的值。 | Float |
|     | **Position Max Y** | 覆盖 Y 轴上最大位置偏移的值。 | Float |
|     | **Position Min Z** | 覆盖 Z 轴上最小位置偏移的值。 | Float |
|     | **Position Max Z** | 覆盖 Z 轴上最大位置偏移的值。| Float |
| Rotation |     | 用于控制 [Rotation Modifier](/docs/user-guide/components/reference/vegetation-modifiers/vegetation-rotation-modifier) 组件的逐实例覆盖的设置。 |     |
|     | **Rotation Override Enabled** | 启用后，[Rotation Modifier](/docs/user-guide/components/reference/vegetation-modifiers/vegetation-rotation-modifier) 组件可以使用描述符中指定的每实例覆盖值，而不是组件配置。 | Bool |
|     | **Rotation Min X** | 覆盖 X 轴上最小旋转偏移的值。 | Float |
|     | **Rotation Max X** | 覆盖 X 轴上最大旋转偏移的值。| Float |
|     | **Rotation Min Y** | 覆盖 Y 轴上最小旋转偏移的值。| Float |
|     | **Rotation Max Y** | 覆盖 Y 轴上最大旋转偏移的值。 | Float |
|     | **Rotation Min Z** | 覆盖 Z 轴上最小旋转偏移的值。 | Float |
|     | **Rotation Max Z** | 覆盖 Z 轴上最大旋转偏移的值。 | Float |
| Scale |     | 用于控制 [Scale Modifier](/docs/user-guide/components/reference/vegetation-modifiers/vegetation-scale-modifier) 组件的逐实例覆盖的设置。 |     |
|     | **Scale Override Enabled** | 启用后，[Scale Modifier](/docs/user-guide/components/reference/vegetation-modifiers/vegetation-scale-modifier)组件可以使用描述符中指定的每个实例覆盖值，而不是组件配置。 | Bool |
|     | **Scale Min** | 覆盖最小缩放乘数的值。 | Float |
|     | **Scale Max** | 覆盖最大缩放乘数的值。 | Float |
| Altitude Filter |     | 用于控制 [Altitude Filter](/docs/user-guide/components/reference/vegetation-filters/vegetation-altitude-filter) 组件的每实例覆盖的设置。 |     |
|     | **Altitude Filter Override Enabled** | 启用后，[Altitude Filter](/docs/user-guide/components/reference/vegetation-filters/vegetation-altitude-filter)组件可以使用描述符中指定的每个实例覆盖值，而不是组件配置。 | Bool |
|     | **Altitude Filter Min** | 覆盖过滤器接受的最小高度的值。 | Float |
|     | **Altitude Filter Max** | 过滤器接受的最大高度的覆盖值。 | Float |
| **Distance Between Filter (Radius)** |     | 用于控制[Distance Between Filter](/docs/user-guide/components/reference/vegetation-filters/vegetation-distance-between-filter)组件的每个实例覆盖的设置。 |     |
|     | **Distance Between Filter Override Enabled** | 启用后，[Distance Between Filter](/docs/user-guide/components/reference/vegetation-filters/vegetation-distance-between-filter)组件可以使用描述符中指定的每个实例覆盖值，而不是组件配置。| Bool |
|     | **Bound Mode** | 在两个实例之间执行距离检查时，此设置确定是否使用网格资源的半径，而不是手动输入的半径<br><br>MeshRadius<br><br>。 |     |
|     | **Radius Min** | 用户定义的距离检查半径。| Float |
| Surface Slope Alignment |     |     |     |
|     | **Surface Slope Alignment Override Enabled** | 启用后，[Slope Alignment Modifier](/docs/user-guide/components/reference/vegetation-modifiers/vegetation-slope-alignment-modifier)组件可以使用描述符中指定的每个实例覆盖值，而不是组件配置。 | Bool |
|     | **Surface Slope Alignment Min** | 覆盖最小对齐强度。 | Float |
|     | **Surface Slope Alignment Max** | 覆盖最大对齐强度。| Float |
| Slope Filter |     |     |     |
|     | **Slope Filter Override Enabled** | 启用后，[Slope Filter](/docs/user-guide/components/reference/vegetation-filters/vegetation-slope-filter)  组件可以使用描述符中指定的每个实例覆盖值，而不是组件配置。 | Bool |
|     | **Slope Filter Min** | 覆盖过滤器接受的最小斜率。 | Float |
|     | **Slope Filter Max** | 覆盖过滤器接受的最大斜率。 | Float |
| Surface Mask Filter |     |     |     |
|     | **Override Mode** | 控制 [Surface Mask Filter](/docs/user-guide/components/reference/vegetation-filters/vegetation-surface-mask-filter)  组件如何考虑覆盖<br><br>Disable - 覆盖将被完全忽略<br><br>Replace - 覆盖将替换组件中指定的覆盖<br><br>Extend - 覆盖将添加到组件中指定的覆盖。 |     |
|     | **Inclusion Tags** | 一组表面标签，用于指示可以放置植被的位置。 | SurfaceTagVector |
|     | **Exclusion Tags** | 一组表面标签，用于指示不放置植被的位置。 | SurfaceTagVector |
| Surface Mask Depth Filter |   |  用于控制[Surface Mask Depth Filter](/docs/user-guide/components/reference/vegetation-filters/vegetation-surface-mask-depth-filter)组件的逐实例覆盖的设置。 |     |
|     | **Surface Tags** | 一组用于深度比较的表面标签。 | SurfaceTagVector |
|     | **Upper Distance Range (m)** | 用于与具有匹配表面标签的最近点进行垂直距离比较的范围。 | Float |
|     | **Lower Distance Range (m)** | 用于与具有匹配表面标签的最近点进行垂直距离比较的范围。 | Float |

File: [`/Gems/Vegetation/Code/Source/Descriptor.cpp`](https://github.com/o3de/o3de/blob/900bd390b9b11c83a77ac3de651dc9eb3d897270/Gems/Vegetation/Code/Include/Vegetation/Descriptor.h)


## EBus 接口

### Vegetation::AreaInfoBus

|请求名称 |描述 |参数 |返回 |
| --- | --- | --- | --- |
| `GetLayer` | 获取植被区域的 *layer* 或宏优先级值。当存在多个重叠时，“`GetLayer`”和“`GetPriority`”可用于识别植被区域。 | None | Float |
| `GetPriority` | 获取层中的微优先级值。当存在多个重叠时，“`GetLayer`”和“`GetPriority`”可用于识别植被区域。 | None | Float |
| `GetEncompassingAabb` | 获取整个植被区域的轴对齐边界框。 | None | AZ::Aabb |
| `GetProductCount` | 获取此植被区域当前生成的实例数。 | None | AZ::u32 |
| `GetChangeIndex` | 获取一个递增数字，该数字表示自创建以来 Blocker 区域刷新的次数。 | None | AZ::u32 |

File: [`/Gems/Vegetation/Code/Include/Vegetation/Ebuses/AreaInfoBus.h`](https://github.com/o3de/o3de/blob/0121a0675b53104cfe1d9f6d87bf9b8ac59a4382/Gems/Vegetation/Code/Include/Vegetation/Ebuses/AreaInfoBus.h)

### Vegetation::AreaRequestBus

|请求名称 |描述 |参数 |返回 |
| --- | --- | --- | --- |
| `PrepareToClaim` | 运行任何索赔前检查或逻辑，与位置无关。 | EntityIdStack& stackIds | Bool |
| `ClaimPositions` | 处理一组用于种植植被或执行其他操作的点。 | EntityIdStack& stackIds, ClaimContext& context | None |
| `UnclaimPosition` | 每当系统释放已声明的点时处理清理。 | const ClaimHandle handle | None |

文件: [`/Gems/Vegetation/Code/Include/Vegetation/Ebuses/AreaRequestBus.h`](https://github.com/o3de/o3de/blob/0fb9727c67c9ad7885b4af538860920f5ba53bfa/Gems/Vegetation/Code/Include/Vegetation/Ebuses/AreaRequestBus.h)


### Vegetation::DescriptorProviderRequestBus

*Vegetation descriptor providers* 为 [Vegetation Layer Spawners](/docs/user-guide/components/reference/vegetation/layer-spawner) 和 [Blenders](/docs/user-guide/components/reference/vegetation/vegetation-layer-blender)  提供来自已定义来源的植被描述符列表。
植被 Gem 附带的组件可以从直接在组件中定义的列表中提供描述符，从外部创建的资产中引用描述符列表，或者将多个描述符列表组合在一起。

|请求名称 | 描述                                  |参数 |返回 |
| --- |-------------------------------------| --- | ---  |
| `GetDescriptors` | 当组件准备就绪并完全加载其所有资产时，此方法会创建其活动描述符的列表。 | DescriptorPtrVec& descriptors | None |

文件: [`/Gems/Vegetation/Code/Include/Vegetation/Ebuses/DescriptorProviderRequestBus.h`](https://github.com/o3de/o3de/blob/0fb9727c67c9ad7885b4af538860920f5ba53bfa/Gems/Vegetation/Code/Include/Vegetation/Ebuses/DescriptorProviderRequestBus.h)

### Vegetation::DescriptorSelectorRequestBus

当植被描述符提供程序提供多个选项时，[Vegetation Layer Spawner](/docs/user-guide/components/reference/vegetation/layer-spawner)  组件可以使用植被描述符选择器。有一个选择器[Vegetation Asset Weight Selector](/docs/user-guide/components/reference/vegetation/vegetation-asset-weight-selector)组件，它根据植被描述符的选择权重字段进行选择。

|请求名称 |描述 |参数 |返回 |
| --- | --- | --- | ---  |
| `SelectDescriptors` | 使用输入梯度信号和其他参数将描述符集减少到符合选择条件的描述符集。 | None | const DescriptorSelectorParams&  <br>DescriptorPtrVec&|

文件: [`/Gems/Vegetation/Code/Include/Vegetation/Ebuses/DescriptorSelectorRequestBus.h`](https://github.com/o3de/o3de/blob/0fb9727c67c9ad7885b4af538860920f5ba53bfa/Gems/Vegetation/Code/Include/Vegetation/Ebuses/DescriptorSelectorRequestBus.h)

### Vegetation::ModifierRequestBus

*Vegetation modifiers* 通过更改位置、旋转、缩放、对齐或任何公开的字段，为每个植被实例添加唯一性或变化。
这允许在整个植被区域中使用相同的植被描述符，并且描述符的每个实例都以不同的方式显示。

|请求名称 |描述 |参数 |返回 |
| --- | --- | --- | ---   |
| `Execute` | 根据配置修改单个植被实例。 | Vegetation::InstanceData | None |
| `GetModifierStage` | Internal（内部）：确定运行 Vegetation Modifier 组件的顺序。 | None | Vegetation::ModifierStage |

文件: [`/Gems/Vegetation/Code/Include/Vegetation/Ebuses/ModifierRequestBus.h`](https://github.com/o3de/o3de/blob/0fb9727c67c9ad7885b4af538860920f5ba53bfa/Gems/Vegetation/Code/Include/Vegetation/Ebuses/ModifierRequestBus.h)

### Vegetation::FilterRequestBus

植被区域使用 *植被过滤器* 来评估每个植被实例数据，并决定是否应该进行任何活动。
[Vegetation Layer Spawners](/docs/user-guide/components/reference/vegetation/layer-spawner) 使用过滤器来确定是否创建植被实例。
他们可以选择在植被修饰符运行之前或之后评估过滤器。
在修饰符之前评估过滤器的性能会更好，因为它会跳过不必要的处理，但是当发生位置更改时，它产生的结果不太准确。
在修饰符之后评估过滤器是准确的，因为它会评估实例数据的最终版本。

|请求名称 |描述 |参数 |返回 |
| --- | --- | --- | ---   |
| `Evaluate` | 评估 InstanceData 中描述的植被是否满足筛选器设置的要求。 | InstanceData | Bool |
| `GetFilterStage` | 获取评估过滤器时的过滤器阶段 （PreProcess/PostProcess）。 | None | FilterStage |
| `SetFilterStage` | 设置评估过滤器时的过滤器阶段 （PreProcess/PostProcess）。 | FilterStage | None |

文件: [`Gems/Vegetation/Code/Include/Vegetation/Ebuses/FilterRequestBus.h`](https://github.com/o3de/o3de/blob/0fb9727c67c9ad7885b4af538860920f5ba53bfa/Gems/Vegetation/Code/Include/Vegetation/Ebuses/FilterRequestBus.h)
