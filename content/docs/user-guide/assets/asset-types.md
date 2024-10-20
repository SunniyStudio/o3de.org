---
linkTitle: 资产类型
title: 资产类型 
description: Open 3D Engine (O3DE)资产管道支持的资产类型。
weight: 700
toc: true
---

下表描述了**Open 3D Engine (O3DE)** 中**资产管道**支持的一些最常见的源资产类型，并列出了**资产处理器**为每种源资产类型生成的产品资产。该表还列出了处理特定源资产类型的构建器，以及构建器在源代码中的位置。构建器可以位于 O3DE 引擎代码、Gems 或项目代码中。

{{< note >}}
有些源资产只是被复制到**资产缓存**，因此产品资产类型与源资产类型相同。
{{< /note >}}

## 资产构建器

| 源资产 | 构建器 | 构建器位置 | 说明  | 产品资产 | 
| - | - | - | - | - |
| `.animgraph` | AnimGraphBuilderWorker | [EMotionFX Gem](https://github.com/o3de/o3de/tree/development/Gems/EMotionFX) | [Animation graphs](docs/user-guide/visualization/animation/character-editor/concepts-and-terms) 定义角色的动画行为。 | `.animgraph` |
| `.attimage`, `.azasset`, `.azbuffer` | 任何资产构建器 | [Atom Gem](https://github.com/o3de/o3de/blob/development/Gems/Atom/RPI/Code/Source/RPI.Builders/Common/AnyAssetBuilder.h) | - | 和源资产相同， `.attimage` 生成 `.attimage` 产品资产。 |
| `.texatlas` | Atlas Worker Builder | [Texture Atlas Gem](https://github.com/o3de/o3de/blob/development/Gems/TextureAtlas/Code/Source/Editor/AtlasBuilderWorker.h) | [Texture atlases](docs/user-guide/interactivity/user-interface/canvases/texture-atlases) 用于减少用户界面中的绘制调用。 | `.texatlasidx` and `.streamingimage` |
| `.bmp`, `.dds`, `.exr`, `.gif`, `.jpeg`, `.jpg`, `.png`, `.tga`, `.tiff`, `.tif` | Atom Image Builder | [Atom Gem](https://github.com/o3de/o3de/blob/development/Gems/Atom/Asset/ImageProcessingAtom/Code/Source/ImageBuilderComponent.h) | 处理图像供 O3DE 使用，主要用作[纹理](docs/user-guide/assets/texture-settings/texture-assets)。 | 生成的产品资产取决于设置，但通常包括 `.imagemipchain` 和 `.streamingimage`。 |
| `.materialtype`, `.material` | Atom Material Builder | [Atom Gem](https://github.com/o3de/o3de/blob/development/Gems/Atom/RPI/Code/Source/RPI.Builders/Material/MaterialBuilder.h) | [材质](docs/atom-guide/look-dev/materials) 包含的属性可定义对象的渲染方式。 | `.materialtype` 输出 `.material` 和 `.azmaterialtype` 产品资产。 `.material` 输出 `.azmaterial` 产品资产。 |
| `.resourcepool` | Atom Resource Pool Asset Builder | [Atom Gem](https://github.com/o3de/o3de/blob/development/Gems/Atom/RPI/Code/Source/RPI.Builders/ResourcePool/ResourcePoolBuilder.h) | - | `.pool` 或 `.streamingimagepool` |
| `.benchmarksettings`, `.benchmark` | Benchmark Asset Worker Builder | [LmbrCentral Gem](https://github.com/o3de/o3de/blob/development/Gems/LmbrCentral/Code/Source/Builders/BenchmarkAssetBuilder/BenchmarkAssetBuilderComponent.h) | - | - |
| `.blast` | Blast Scene Builder | [Blast Gem](https://github.com/o3de/o3de/blob/development/Gems/Blast/Editor/Scripts/blast_asset_builder.py) | - | `.blast`, `.assetinfo.generated` |
| `.cfg` | CFG Builder Worker | [LmbrCentral Gem](https://github.com/o3de/o3de/blob/development/Gems/LmbrCentral/Code/Source/Builders/CopyDependencyBuilder/CfgBuilderWorker/CfgBuilderWorker.h) | - | `.cfg` |
| `.emfxworkspace` | EmfxWorkspaceBuilderDescriptor | [LmbrCentral Gem](https://github.com/o3de/o3de/blob/development/Gems/LmbrCentral/Code/Source/Builders/CopyDependencyBuilder/EmfxWorkspaceBuilderWorker/EmfxWorkspaceBuilderWorker.h) | - | `.emfxworkspace` |
| `.fontfamily`, `.font` | FontBuilderWorker | [LmbrCentral Gem](https://github.com/o3de/o3de/blob/development/Gems/LmbrCentral/Code/Source/Builders/CopyDependencyBuilder/FontBuilderWorker/FontBuilderWorker.h) | [字体](docs/user-guide/interactivity/user-interface/fonts) 被用于渲染文本。 | 和源资产相同，`.font` 生成 `.font` 产品资产。 |
| `.tfx` | HairAssetBuilder | [AtomTressFX Gem](https://github.com/o3de/o3de/blob/development/Gems/AtomTressFX/Code/Builders/HairAssetBuilder.h) | [TressFX](docs/user-guide/gems/reference/rendering/amd/atom-tressfx) 提供逼真的毛发和皮毛模拟。 | `.tfxhair` |
| `.lua` | Lua Worker Builder | [LmbrCentral Gem](https://github.com/o3de/o3de/blob/development/Gems/LmbrCentral/Code/Source/Builders/LuaBuilder/LuaBuilderWorker.h) | [Lua](docs/user-guide/scripting/lua) 是一种脚本语言，可用于为项目编写逻辑。 | `.luac` |
| `.mock` | Mock Builder | [Automated Testing Project](https://github.com/o3de/o3de/blob/development/AutomatedTesting/Gem/PythonTests/PythonAssetBuilder/mock_asset_builder.py) | 该构建器与 AutomatedTesting 项目一起运行，为自动化测试提供帮助。对于希望在自己的项目中使用 Python 编写自定义资产类型构建器的团队来说，这是一个有用的示例。 | `.mock_asset` |
| `.motionset` | MotionSetBuilderWorker | [EmotionFX Gem](https://github.com/o3de/o3de/blob/development/Gems/EMotionFX/Code/EMotionFX/Pipeline/EMotionFXBuilder/MotionSetBuilderWorker.h) | 一个 [动作集](docs/user-guide/visualization/animation/character-editor/concepts-and-terms) 包含一个动作列表。 | `.motionset` |
| `.pass` | Pass Asset Builder | [Atom Gem](https://github.com/o3de/o3de/blob/development/Gems/Atom/RPI/Code/Source/RPI.Builders/Pass/PassBuilder.h) | [通道](docs/atom-guide/dev-guide/passes) 是渲染工作的逻辑分组，有明确的输入和输出。 | `.pass` |
| `.precompiledshader` | Precompiled Shader Builder | [Atom Gem](https://github.com/o3de/o3de/blob/development/Gems/Atom/Asset/Shader/Code/Source/Editor/AzslShaderBuilderSystemComponent.h) | - | `.azshader` |
| `.prefab` | Prefab Builder | [Prefab Gem](https://github.com/o3de/o3de/blob/development/Gems/Prefab/PrefabBuilder/PrefabBuilderComponent.h) | [Prefab](docs/user-guide/interactivity/prefabs) 是构建 O3DE 项目的基础元素，Prefab是实体的集合。| `.spawnable` |
| `.ts` | Qt Translation File Builder | [LmbrCentral Gem](https://github.com/o3de/o3de/blob/development/Gems/LmbrCentral/Code/Source/Builders/TranslationBuilder/TranslationBuilderComponent.h) | Qt 翻译文件。Qt 仅在编辑器工具中启用，因此这些文件对于将工具（而非项目）翻译成不同语言非常有用。 | `.qm` |
| `.fbx`, `.glb`, `gltf`, `stl` | Scene Builder | [SceneProcessing Gem](https://github.com/o3de/o3de/blob/development/Gems/SceneProcessing/Code/Source/SceneBuilder/SceneBuilderWorker.h) | [场景文件](docs/user-guide/assets/scene-settings/scene-format-support) 用于定义网格和动画等 3D 内容。 | `.motion`, `.procprefab`, `.azmaterial`, `.azbuffer`, `.azlod`, `.azmodel`, `.blast_chunks`, `.pxmesh`, `.skinmeta`, `.actor` |
| `.xmlschema` | SchemaBuilderWorker | [LmbrCentral Gem](https://github.com/o3de/o3de/blob/development/Gems/LmbrCentral/Code/Source/Builders/CopyDependencyBuilder/SchemaBuilderWorker/SchemaBuilderWorker.h) | XML 模式文件有助于在 XML 文件中查找产品依赖关系。O3DE 编辑器中的 XML 模式编辑工具可以定义模式来匹配 XML 格式文件，并在这些文件中搜索产品依赖关系。 | `.xmlschema` |
| `.scriptcanvas` | Script Canvas Builder | [ScriptCanvas Gem](https://github.com/o3de/o3de/blob/development/Gems/ScriptCanvas/Code/Builder/ScriptCanvasBuilderWorker.h) | [Script Canvas](docs/user-guide/scripting/script-canvas) 是 O3DE 的通用可视化脚本环境。 | `.luac`, `.scriptcanvas_compiled`, `.scriptcanvas_fn_compiled` |
| `.scriptevents` | Script Events Builder | [ScriptEvents Gem](https://github.com/o3de/o3de/blob/development/Gems/ScriptEvents/Code/Builder/ScriptEventsBuilderWorker.h) | [Script Events](docs/user-guide/scripting/script-events) 允许脚本之间相互通信。 | `.scriptevents` |
| `.seed` | SeedBuilderDescriptor | [LmbrCentral Gem](https://github.com/o3de/o3de/blob/development/Gems/LmbrCentral/Code/Source/Builders/DependencyBuilder/DependencyBuilderWorker.h) | [Seed 资产列表](docs/user-guide/packaging/asset-bundler) 用于资产捆绑流程，即如何将内容打包，以便向最终用户分发项目。 | `.seed` |
| `engine.json` | Settings Registry Builder | [Asset Processor](https://github.com/o3de/o3de/blob/development/Code/Tools/AssetProcessor/native/InternalBuilders/SettingsRegistryBuilder.h) | - | `engine.json`, `.setreg` |
| `.shader` | Shader Asset Builder | [Atom Gem](https://github.com/o3de/o3de/blob/development/Gems/Atom/Asset/Shader/Code/Source/Editor/AzslShaderBuilderSystemComponent.h) | [Shaders](docs/atom-guide/dev-guide/shaders) 被渲染系统用于确定 3D 几何图形的渲染方式。 | `.json`, `.azslin`, `.azshadervariant`, `.hlsl` |
| `.shadervariantlist` | Shader Variant Asset Builder | [Atom Gem](https://github.com/o3de/o3de/blob/development/Gems/Atom/Asset/Shader/Code/Source/Editor/AzslShaderBuilderSystemComponent.h) | [Shader variants](docs/atom-guide/dev-guide/shaders/azsl/shader-variant-options) 是着色器常量，仅用于可静态优化的条件语句中。 | `.azshadervarianttree`, `.azshadervariant` | 
| `.uicanvas` | UI Canvas Builder | [LyShine Gem](https://github.com/o3de/o3de/blob/development/Gems/LyShine/Code/Pipeline/LyShineBuilder/UiCanvasBuilderWorker.h) | [UI Canvas](docs/user-guide/interactivity/user-interface) 文件为项目定义二维视觉元素，通常用于用户界面。 | `.uicanvas` |
| `.ent`, `.vegdescriptorlist`, 以及任何与正则表达式匹配的内容：`(?!.*libs\/gameaudio\/).*\.xml` | XmlBuilderWorker | [LmbrCentral Gem](https://github.com/o3de/o3de/blob/development/Gems/LmbrCentral/Code/Source/Builders/CopyDependencyBuilder/XmlBuilderWorker/XmlBuilderWorker.h) | XML 文件作为产品资产直接复制到缓存中。XML 生成器为扫描 XML 文件以查找产品依赖关系提供了一个入口点，在 [资产打包](docs/user-guide/packaging/asset-bundler) 过程中使用，以便在向最终用户分发项目时打包内容。 | `.xml` |

## 复制任务

复制任务用于没有额外处理要求的资产，它们将源资产直接复制到资产缓存中。在这种情况下，产品资产与源资产完全相同。复制任务是通过设置注册表定义的，引擎的默认复制任务可在[AssetProcessorPlatformConfig.setreg](https://github.com/o3de/o3de/blob/development/Registry/AssetProcessorPlatformConfig.setreg)中找到。
