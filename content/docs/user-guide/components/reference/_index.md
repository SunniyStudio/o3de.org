---
title: 组件参考
linkTitle: 组件参考
description: Open 3D Engine (O3DE)组件参考索引
weight: 100
toc: true
---

组件为**开放三维引擎（O3DE）**中的实体添加功能。一个实体可以包含任意数量或组合的组件。有些组件允许每个实体只有一个实例，有些则依赖于其他组件才能运行。

组件由 Gems 提供。要在**O3DE 编辑器**中使用一个组件，必须添加提供该组件的 Gem。虽然组件可能属于同一类型，但它们可能不是由同一个 Gem 提供的。您可以在组件的参考主题中找到提供该组件的 Gem。

## 为实体添加组件

在 O3DE 编辑器中为实体添加组件：

1. 在 **Entity Outliner** 或 **Perspective**，点击一个实体来选择它。这将在**Entity Inspector**中显示实体的详细信息。
1. 在Entity Inspector中，点击**Add Component**。
1. 从组件列表中选择一个组件添加到实体中。

{{< note >}}
如果在**Add Component**列表中找不到组件，可能需要启用提供该组件的 Gem 并重建项目。
{{< /note >}}

## 组件
以下组件按 O3DE 编辑器中出现的类型分组。

<!--
### AI
| Component | Description | 
| - | - |
| [Navigation](/docs/user-guide/components/reference/ai/navigation/) | Provides basic path-finding and path-following services to an entity. |
| [Navigation Area](/docs/user-guide/components/reference/ai/nav-area/) | Configures the area used for navigation and pathfinding. |
| [Navigation Seed](/docs/user-guide/components/reference/ai/nav-seed/) | Determines the reachable navigation nodes along the path.  |
-->


### Animation

| 组件 | 说明 |
| - | - |
| [Actor](./animation/actor) | 添加与骨骼绑定的网格组，该网格组可由来自 Anim Graph 或 Simple Motion 组件的动画数据驱动。 |
| [Anim Graph](./animation/animgraph/) | 管理在 “动画编辑器”（Animation Editor）中创建的资产集，包括动画图、默认参数设置和为相关Actor分配的动作集。 |
| [Attachment](./animation/attachment/) | 允许一个实体连接到另一个实体骨架上的骨骼。 |
| [Simple LOD Distance](./animation/simple-lod-distance) | 为Actor的每个细节级别（LOD）设置与摄像机的距离。 |
| [Simple Motion](./animation/simple-motion/) | 为相关的 Actor 指定一个动作。这是动画图形组件的一种更简单的替代方案。  |


### Atom Renderer

| 组件 | 说明 |
| - | - |
| [Bloom](/docs/user-guide/components/reference/atom/bloom/) | 模拟真实世界中的光渗入或发光。 |
| [Chromatic Aberration](/docs/user-guide/components/reference/atom/chromatic-aberration) | 模拟透镜效应，将波长不同的光线聚焦到不同的点上。 |
| [CubeMap Capture](/docs/user-guide/components/reference/atom/cubemap-capture/) | 捕捉实体位置上的Specular IBL 或Diffuse IBL Cubemap。 |
| [Debug Rendering](/docs/user-guide/components/reference/atom/debug-rendering/) | 用于可视化检查和调试场景的关卡组件。 |
| [Decal (Atom)](/docs/user-guide/components/reference/atom/decal/) | 将纹理材质沿单一方向投射到网格表面。 |
| [Deferred Fog](/docs/user-guide/components/reference/atom/deferred-fog/) | 创建屏幕空间雾化效果，可用作场景雾化或分层/地面雾化，并可选择云噪湍流。 |
| [Depth of Field](/docs/user-guide/components/reference/atom/depth-of-field/) | 模拟真实世界中相机的镜头效果，聚焦于特定区域。 |
| [Diffuse Global Illumination](/docs/user-guide/components/reference/atom/diffuse-gi/) | 控制**Diffuse Probe Grid**组件提供的全局照明质量水平。  | 
| [Diffuse Probe Grid](/docs/user-guide/components/reference/atom/diffuse-probe-grid/) | 创建一个光探针体积，在指定区域内提供漫射全局照明。 |
| [Directional Light](/docs/user-guide/components/reference/atom/directional-light/) | 光线从无限远的一点射向一个方向，类似于太阳光。 |
| [Display Mapper](/docs/user-guide/components/reference/atom/display-mapper/) | 为场景配置色调映射和调色。 |
| [Entity Reference](/docs/user-guide/components/reference/atom/entity-reference/) | 允许您为一个实体提供对其他实体的引用。 |
| [Exposure Control](/docs/user-guide/components/reference/atom/exposure-control/) | 调整相机在场景中曝光的光量。 |
| [Global Skylight (IBL)](/docs/user-guide/components/reference/atom/global-skylight-ibl/) | 创建基于图像的全局照明效果，使用 HDR 天空盒图像为场景计算光照。 |
| [Grid](/docs/user-guide/components/reference/atom/grid/) | 为场景添加自定义网格。 |
| [HDRi Skybox](/docs/user-guide/components/reference/atom/hdri-skybox/) | 使用 HDR 图像在场景中创建天空盒。 |
| [Light](/docs/user-guide/components/reference/atom/light) | 通过创建各种类型的点光源和区域光源，模拟柔和的演播室光线。 |
| [Look Modification](/docs/user-guide/components/reference/atom/look-modification/) | 配置调色查找表 (LUT)。 |
| [Material](/docs/user-guide/components/reference/atom/material/) | 管理和定制应用于模型、Actor和其他兼容组件的材质。 |
| [Mesh](/docs/user-guide/components/reference/atom/mesh/) | 指定要渲染的模型。 |
| [Occlusion Culling Plane](/docs/user-guide/components/reference/atom/occlusion-culling-plane/) | 创建一个遮挡器，当它位于摄像机和网格之间时，可以阻止网格被渲染。 |
| [Physical Sky](/docs/user-guide/components/reference/atom/physical-sky/) | 调整场景的物理环境，如天空、太阳和雾。 |
| [Post-processing Modifiers](/docs/user-guide/components/reference/atom/post-processing-modifiers/) | 定义图层、体积、面积和权重的组件集合，用于后期特效处理（PostFX）。|
| [Reflection Probe](/docs/user-guide/components/reference/atom/reflection-probe/) | 在探头（捕捉点）周围的环境中产生镜面反射。 |
| [SSAO](/docs/user-guide/components/reference/atom/ssao/) | 通过使用屏幕空间环境遮蔽技术，近似场景中的间接照明。 |
| [Stars](/docs/user-guide/components/reference/atom/stars/) | 提供与分辨率无关的基于物理的遥远恒星动画。|

### Audio

| 组件 | 说明 |
| - | - |
| Audio Animation | 增加了在动画事件发生时执行音频触发器的功能。 |
| [Audio Area Environment](/docs/user-guide/components/reference/audio/area-environment/) | 使在形状周围和整个形状中移动的实体能够在其触发的任何声音中应用环境效果。 |
| [Audio Environment ](/docs/user-guide/components/reference/audio/environment/) | 应用环境效果，如混响或回声。 |
| [Audio Listener](/docs/user-guide/components/reference/audio/listener/) | 允许在环境中放置虚拟麦克风。 |
| [Audio Preload](/docs/user-guide/components/reference/audio/preload/) | 加载和卸载音频转换层预载中包含的声音库。|
| [Audio Proxy](/docs/user-guide/components/reference/audio/proxy/) | 如果在一个实体中添加了多个音频组件，则需要音频代理组件来确保其他音频组件与同一个音频对象进行通信。 |
| [Audio Rtpc](/docs/user-guide/components/reference/audio/rtpc/) | 提供基本的**实时参数控制**（Real-time Parameter Control，RTPC）功能，可让您实时调整音效。 |
| [Audio Switch](/docs/user-guide/components/reference/audio/switch/) | 提供基本的音频转换层开关功能，用于指定实体的状态。 |
| [Audio Trigger](/docs/user-guide/components/reference/audio/trigger/) | 提供音频转换层触发器，允许您播放或停止音频。 |
| [Multi-Position Audio](/docs/user-guide/components/reference/audio/multi-position/) | 可通过多个位置播放声音。|
| [MiniAudio Listener](/docs/user-guide/components/reference/audio/mini-audio-listener/) | 提供为空间音频播放设置 MiniAudio 监听器的功能。|
| [MiniAudio Playback](/docs/user-guide/components/reference/audio/mini-audio-playback/) | 提供使用 MiniAudio 播放声音的功能。|


### Camera

| 组件 | 说明 | 
| - | - |
| [Camera](/docs/user-guide/components/reference/camera/camera/) | 允许将实体用作摄像头。 |
| [Camera Rig](/docs/user-guide/components/reference/camera/camera-rig/) | 管理驱动摄像机实体的行为。 |

<!--### Destruction
| Component | Description | 
| - | - |
| [Blast Family](/docs/user-guide/components/reference/destruction/blast-family/) | Enables destruction simulation using the [NVIDIA Blast library](https://developer.nvidia.com/blast). |
| [Blast Family Mesh Data](/docs/user-guide/components/reference/destruction/blast-family-mesh-data/) | Sets the mesh and material assets for NVIDIA Blast entities. | Hiding until blast tools are fixed, and blast docs are updated.-->

### Editor

| 组件 | 说明 | 
| - | - |
| [Comment](/docs/user-guide/components/reference/editor/comment/) | 允许您为组件实体添加文本注释。 |

### Gameplay

| 组件 | 说明 | 
| - | - |
| [Fly Camera Input](./gameplay/fly-camera-input/) | 允许您使用鼠标和按键输入来控制摄像机。 |
| [Look At](./gameplay/look-at/) | 强制实体始终查看指定目标。 |
| [Simple State](/docs/user-guide/components/reference/gameplay/simple-state/) | 提供了一个简单的状态机，可以激活和停用相关实体。|
| [Tag](./gameplay/tag/) | 允许您对实体应用一个或多个标签。|
| [Input](./gameplay/input/) | 将原始输入与游戏中的事件绑定。 |

### Gradient Modifiers

| 组件 | 说明 |
| - | - |
| [Dither Gradient Modifier](./gradient-modifiers/dither-gradient-modifier) | 对输入灰阶进行有序抖动。 |
| [Gradient Mixer](./gradient-modifiers/gradient-mixer) | 结合其他灰阶生成新灰阶。 |
| [Gradient Transform Modifier](./gradient-modifiers/gradient-transform-modifier) | 根据**Shape**组件为灰阶创建一个坐标空间。输入渐变的修改器将在此空间中应用。 |
| [Invert Gradient Modifier](./gradient-modifiers/invert-gradient-modifier) | 反转灰阶值。 |
| [Levels Gradient Modifier](./gradient-modifiers/levels-gradient-modifier) | 利用低点/中点/高点修改输入灰阶信号，并可箝位最小/最大输出值。 |
| [Posterize Gradient Modifier](./gradient-modifiers/posterize-gradient-modifier) | 将输入的灰阶信号划分为指定数量的波段。|
| [Smooth-Step Gradient Modifier](./gradient-modifiers/smooth-step-gradient-modifier) | 生成的灰阶落差能平滑输入灰阶。 |
| [Threshold Gradient Modifier](./gradient-modifiers/threshold-gradient-modifier) | 将阈值应用于输入灰阶，生成只有两个值的输出灰阶。高于阈值的输入灰阶值设置为 1，低于阈值的输入值设置为 0。 |

### Gradients

| 组件 | 说明 |
| - | - |
| [Altitude Gradient](./gradients/altitude-gradient) | 在一定范围内根据高度生成渐变。 |
| [Constant Gradient](./gradients/constant-gradient) | 采样时以渐变形式返回指定值。 |
| [FastNoise Gradient](./gradients/fastnoise-gradient) | 使用[FastNoise](https://github.com/Auburn/FastNoiseLite)生成渐变值，[[FastNoise](https://github.com/Auburn/FastNoiseLite)是一个包含多种实时噪声算法的噪声生成库。 |
| [Image Gradient](./gradients/image-gradient) | 通过对图像资产采样生成渐变。 |
| [Perlin Noise Gradient](./gradients/perlin-noise-gradient) | 通过对培林噪声发生器采样生成渐变。 |
| [Random Noise Gradient](./gradients/random-noise-gradient) | G通过对随机噪音发生器进行采样，产生渐变。|
| [Reference Gradient](./gradients/reference-gradient) | 引用另一个渐变。 |
| [Shape Falloff Gradient](./gradients/shape-falloff-gradient) | 根据与形状的距离生成渐变。 |
| [Slope Gradient](./gradients/slope-gradient) | 根据曲面角度生成渐变效果。 |
| [Surface Mask Gradient](./gradients/surface-mask-gradient) | 根据底层曲面类型生成渐变。 |

### Multiplayer

| 组件 | 说明 |
| - | - |
| [Simple Network Player Spawner](./multiplayer/simple-player-spawner) | 在网络多人游戏会话中，执行处理玩家加入和离开事件的基本设置。 |


### Non-uniform Scale

| 组件 | 说明 |
| - | - |
| [Non-uniform Scale](/docs/user-guide/components/reference/non-uniform-scale/non-uniform-scale/) | 允许实体在 x、y 和 z 轴上以不同大小缩放。默认情况下，实体只能在各坐标轴上等比例缩放。 |

### NVIDIA PhysX

| 组件 | 说明 |
| - | - |
| [Cloth](/docs/user-guide/components/reference/physx/cloth/) | 将网格顶点视为具有物理特性的布粒子，从而模拟布的行为。 |
| [PhysX Ball Joint](/docs/user-guide/components/reference/physx/ball-joint/) | 模拟动态球形关节，将实体约束在关节上，并可绕关节的 Y 轴和 Z 轴自由旋转。|
| [PhysX Character Controller](/docs/user-guide/components/reference/physx/character-controller/) | 实现角色与物理世界的基本互动。 |
| [PhysX Character Gameplay](/docs/user-guide/components/reference/physx/character-gameplay/) | 配置游戏中角色的一般属性，例如角色的重力强度。 |
| [PhysX Dynamic Rigid Body](/docs/user-guide/components/reference/physx/rigid-body/) | 将可移动实体定义为可移动的刚性物体，它是实体，可以与其他 PhysX 实体发生碰撞。 |
| [PhysX Fixed Joint](/docs/user-guide/components/reference/physx/fixed-joint/) | 创建一个动态固定关节，将实体约束在关节上，在任何轴上都没有自由度。 |
| [PhysX Force Region](/docs/user-guide/components/reference/physx/force-region/) | 对指定区域内的物体施加物理力。 |
  [PhysX Heightfield Collider](/docs/user-guide/components/reference/physx/heightfield-collider/) | 根据Axis-Aligned Box组件创建一个几何碰撞体。 |
| [PhysX Hinge Joint](/docs/user-guide/components/reference/physx/hinge-joint/) | 创建动态铰链关节，将实体约束在关节上，并可绕关节的 X 轴自由旋转。|
| [PhysX Mesh Collider](./physx/mesh-collider/) | 允许您指定用于计算实体间碰撞的 PhysX 网格资产。 |
| [PhysX Primitive Collider](./physx/collider/) | 允许您指定用于计算实体间碰撞的原始形状。 |
| [PhysX Prismatic Joint](/docs/user-guide/components/reference/physx/prismatic-joint/) | 创建动态棱柱关节，将实体约束在关节上，保持相同的旋转，但允许实体沿一条轴自由移动。|
| [PhysX Ragdoll](/docs/user-guide/components/reference/physx/ragdoll/) | 通过创建由关节连接的刚体层次结构来模拟布偶物理。 |
| [PhysX Static Rigid Body](/docs/user-guide/components/reference/physx/static-rigid-body/) | 将实体定义为不可移动的刚性物体，它是实体，可以与其他 PhysX 实体发生碰撞。 |
| [PhysX Shape Collider](/docs/user-guide/components/reference/physx/shape-collider/) | 根据**Shape**组件创建一个几何碰撞体。 |

### Scripting

| 组件 | 说明 |
| - | - |
| [Lua Script](/docs/user-guide/components/reference/scripting/lua-script/) | 允许您使用 Lua 代码添加自定义逻辑和功能。 |
| [Script Canvas](/docs/user-guide/components/reference/scripting/script-canvas/) | 允许您使用 Lua 代码添加自定义逻辑和功能。 |

### Shape  

| 组件 | 说明 |
| - | - |
| [Axis Aligned Box Shape](/docs/user-guide/components/reference/shape/axis-aligned-box-shape/) | 创建始终与轴线对齐的方框几何体。 |
| [Box Shape](/docs/user-guide/components/reference/shape/box-shape/) | 为体积和触发器生成方框几何图形。|
| [Capsule Shape](/docs/user-guide/components/reference/shape/capsule-shape/) | 为体积和触发器生成胶囊几何图形。 |
| [Compound Shape](/docs/user-guide/components/reference/shape/compound-shape/) | 用简单的形状构建复杂的几何体，用于体积和触发器。 |
| [Cylinder Shape](/docs/user-guide/components/reference/shape/cylinder-shape/) | 为体积和触发器生成圆柱体几何图形。 |
| [Disk Shape](/docs/user-guide/components/reference/shape/disk-shape/) | 为区域和触发器生成圆盘几何图形。 |
| [Polygon Prism Shape](/docs/user-guide/components/reference/shape/polygon-prism-shape/) | 为体积和触发器生成 n 边棱柱几何图形。 |
| [Quad Shape](/docs/user-guide/components/reference/shape/quad-shape/) | 为区域和触发器生成四平面几何。 |
| [Shape Reference](/docs/user-guide/components/reference/shape/shape-reference) | 使实体能够引用和重复使用形状组件。 |
| [Sphere Shape](/docs/user-guide/components/reference/shape/sphere-shape/) | 为体积和触发器生成球形几何体。 |
| [Spline](/docs/user-guide/components/reference/shape/spline/) | 为路径生成直线和曲线。 |
| [Tube Shape](/docs/user-guide/components/reference/shape/tube-shape/) | 为体积和触发器生成管道几何形状。 |
| [White Box](/docs/user-guide/components/reference/shape/white-box/) | 可以在 O3DE 编辑器中绘制 3D 代理网格草图。 |
| [White Box Collider](/docs/user-guide/components/reference/shape/white-box-collider/) | 支持白盒网格的碰撞层和物理材质。 |

### Surface Data   

| 组件 | 说明 |
| - | - |
| [Gradient Surface Tag Emitter](surface-data/gradient-surface-tag-emitter) | 启用渐变以发射表面标记。 |
| [Mesh Surface Tag Emitter](surface-data/mesh-surface-tag-emitter) | 使静态网格能发出表面标记。 |
| [PhysX Collider Surface Tag Emitter](surface-data/physx-collider-surface-tag-emitter) | 启用物理碰撞体发射表面标记。 |
| [Shape Surface Tag Emitter](surface-data/shape-surface-tag-emitter) | 使形状可以发出表面标记。 |

### Terrain  

| 组件 | 说明 |
| - | - |
| [Terrain Physics Heightfield Collider](./terrain/terrain-physics-collider) | 以高度场和表面到材质映射的形式向物理碰撞体提供地形数据。 |
| [Terrain Layer Spawner](./terrain/layer_spawner) | 生成一个包含在可配置边界内的地形区域，并允许对重叠的地形层进行优先级排序。 |
| [Terrain Macro Material](./terrain/terrain-macro-material) | 为地形区域提供低保真色彩数据。 |
| [Terrain Height Gradient List](./terrain/height_gradient_list) | 根据梯度列表提供地形高度数据。 |
| [Terrain Surface Materials List](./terrain/surface-material-list) | 定义曲面类型与渲染材质之间的映射关系。 |
| [Terrain Surface Gradient List](./terrain/surface-gradient-list) | 定义地形图层上梯度和表面类型之间的映射。 |
| [Terrain World](./terrain/world) | 允许设置 “地形世界 ”的边界和高度查询分辨率。 |
| [Terrain World Debugger](./terrain/world-debugger) | 提供显示 “地形世界 ”线框或边界表示的方法。|
| [Terrain World Renderer](./terrain/world-renderer) | 在 “地形世界 ”范围内渲染地形。 |

### Test  

| 组件 | 说明 |
| - | - |
| AssetCollectionAsyncLoaderTest | 允许您测试 AssetCollectionAsyncLoader 提供的 API。 |

### UI  

| 组件 | 说明 |
| - | - |
| [UI Canvas Asset Ref](/docs/user-guide/components/reference/ui/canvas-asset-ref/) | 允许将 UI 画布与实体关联。 |
| [UI Canvas Proxy Ref](/docs/user-guide/components/reference/ui/canvas-proxy-ref/) | 允许您将一个实体与另一个管理 UI 画布的实体关联起来。|
| [UI Canvas on Mesh](/docs/user-guide/components/reference/ui/canvas-on-mesh/) | 允许您在 3D 世界中的实体上放置 UI 画布，玩家可以通过光线投射与该实体进行交互。 |

### Vegetation  

| 组件 | 说明 |
| - | - |
| Landscape Canvas | 提供基于节点的编辑器，用于创作动态植被。  |
| [Vegetation Asset List](vegetation/vegetation-asset-list) | 提供一组植被描述符。 |
| [Vegetation Asset List Combiner](vegetation/vegetation-asset-list-combiner) | 提供植被描述符提供者的列表. |
| [Vegetation Asset Weight Selector](vegetation/vegetation-asset-weight-selector) | 根据权重选择植被资产。 |
| [Vegetation Layer Blender](vegetation/vegetation-layer-blender) | 组合一系列植被区域，并按指定顺序应用。 |
| [Vegetation Layer Blocker](vegetation/vegetation-layer-blocker) | 定义一个不能放置动态植被的区域。|
| [Vegetation Layer Blocker (Mesh)](vegetation/vegetation-layer-blocker-mesh) | 防止植被进入网格。 |
| [Vegetation Layer Debugger](vegetation/vegetation-layer-debugger) | 启用植被层的调试可视化器。 |
| [Vegetation Layer Spawner](vegetation/layer-spawner) | 在指定区域创建动态植被。 |

### Vegetation Filters  

| 组件 | 说明 |
| - | - |
| [Vegetation Altitude Filter](vegetation-filters/vegetation-altitude-filter) | 限制在指定高度范围内的地表种植植被。 |
| [Vegetation Distance Between Filter](vegetation-filters/vegetation-distance-between-filter) | 定义植被实例之间的最小距离。 |
| [Vegetation Distribution Filter](vegetation-filters/vegetation-distribution-filter) | 将植被的位置限制在梯度分布中的指定值范围内。  |
| [Vegetation Shape Intersection Filter](vegetation-filters/vegetation-shape-intersection-filter) | 将植被的位置限制在与指定形状相交的表面上。 |
| [Vegetation Slope Filter](vegetation-filters/vegetation-slope-filter) | 将植被的放置范围限制在指定的表面角度范围内。 |
| [Vegetation Surface Mask Depth Filter](vegetation-filters/vegetation-surface-mask-depth-filter) | 将植被的放置范围限制在两个表面标签之间指定深度范围内的表面。 |
| [Vegetation Surface Mask Filter](vegetation-filters/vegetation-surface-mask-filter) | 根据表面掩膜-标签映射，筛选出植被。 |

### Vegetation Modifiers  

| 组件 | 说明 |
| - | - |
| [Vegetation Position Modifier](vegetation-modifiers/vegetation-position-modifier) | 偏移植被的位置。 |
| [Vegetation Rotation Modifier](vegetation-modifiers/vegetation-rotation-modifier) | 偏移植被的旋转。 |
| [Vegetation Scale Modifier](vegetation-modifiers/vegetation-scale-modifier) | 偏移植被的比例。 |
| [Vegetation Slope Alignment Modifier](vegetation-modifiers/vegetation-slope-alignment-modifier) | 偏移植被相对于地表角度的方向。 |

## Supplemental Information

以下各页提供了有关各组成部分使用的数据和系统的更多信息。

| 组件 | 说明 |
| - | - |
| [Paint Brush](paintbrush/paintbrush) | 某些组件使用的绘制数据操作器。 |
| [Terrain Detail Material](./terrain/terrain-detail-material) | 为地形区域提供高保真材质数据。 |
