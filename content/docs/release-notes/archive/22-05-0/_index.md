---
linkTitle: 22.05.0 发行说明
title: Open 3D Engine 22.05.0 发行说明
description: Open 3D Engine （O3DE） 版本 22.05.0 的完整发行说明。
weight: 897
toc: true
---

**Open 3D Engine （O3DE）** 版本 22.05.0 代表了该项目 2022 年的第一个主要版本。此版本已提交 1,469 个代码，其中包含错误修复、生活质量改进和功能添加。从此版本开始，我们的发行说明将包括一个 [功能网格](./feature-state/)，表示 O3DE 中每个功能的当前开发状态。

## 22.05.0 的亮点

首先：Open 3D Engine 有了新 logo！

此版本还引入了用户定义的属性 （UDP），这是一种将元数据从源资产读取到 Asset Processor 的方法。可以在内容创建工具中分配 UDP，以存储有关层次结构节点（如网格、光照、动画节点等）的自定义属性，从而为 O3DE 的资产生成工作流程提供支持。有关更多信息，请阅读我们的 [关于 UDP 的博客文章](https://o3de.org/blog/posts/blog-udp/)。

我们还在此版本中包含一个用于运动匹配的实验性 Gem。运动匹配是一种数据驱动的动画技术，它根据现有动画数据以及当前角色和输入上下文合成运动。示例 Gem 包括一个角色预制件，可使用游戏手柄进行控制。有关支持的功能以及内部工作原理的更多详细信息，请参阅我们 GitHub 存储库中的 [Motion Matching Gem code](https://github.com/o3de/o3de/tree/development/Gems/MotionMatching)。

Atom 也得到了一些改进！Gem 现在可以在运行时将自定义通道注入渲染管道！以前，Gem 将新通道引入渲染管道很麻烦。项目开发人员必须修改现有资源或复制并粘贴整个渲染管道。在 22.05.0 中，现在提供了一组新的 API，以促进渲染管道的自定义。这项技术已经用于 TressFX、Terrain 和 LyShine Gems。有关 API 的更多信息，请阅读 [O3DE Wiki 文档，了解如何在 Gem 中开发通行证](https://github.com/o3de/o3de/wiki/Work-With-Passes-In-Gems)。

为了进一步减少复制和粘贴，材质类型现在在 Atom 中可以更好地重用。材质 JSON 文件现在可以引用另一个文件来加载基本属性，然后覆盖需要为子材质自定义的任何值。有关此功能的信息，请阅读 [可混合材质类型 RFC](https://github.com/o3de/sig-graphics-audio/issues/16)。

我们的最后一个主要功能说明是 Multiplayer Gem 现在包括对玩家实体生成器的支持。这使项目开发人员能够设置在加入会话时用户控制的实体的显示位置，并使用 Multiplayer Gem 将生成的网络预制件的所有权更改为项目代码。由于这是对使用 Multiplayer Gem 的任何项目的内存所有权的重要更改，因此我们建议开发人员参考 [O3DE Multiplayer Sample](https://github.com/o3de/o3de-multiplayersample) 中使用的生成器逻辑示例。

## 功能和错误修复

以下是 22.05.0 中包含的其他功能和值得注意的修复的列表。

* **音频**
  * 改进的音频控件编辑器：为音频引擎添加新的连接属性。 ([6480](https://github.com/o3de/o3de/pull/6480), [6303](https://github.com/o3de/o3de/pull/6303))
* **资产管线**
  * Prefabs 用于场景处理的产品：添加了一种通过在 Asset Pipeline 的场景生成器中生成预制件来创建预制件的新方法。
  * 场景文件的默认预制件生成：如果未发现场景清单，则预制件 Gem 将自动为每个源场景资源（例如 STL 或 FBX 文件）创建默认程序预制件。
  * O3DE 的 AssImp 集成现在可与 O3DE 的其他第 3 方库相匹配。([3p-package-source/75](https://github.com/o3de/3p-package-source/pull/75))
  * 自动化测试的完整扫描时间从约 2.5 分钟缩短至约 40 秒。 ([6619](https://github.com/o3de/o3de/pull/6619))
  * 作业对产品的依赖性：允许 Asset Builder 提供他们特别依赖的产品列表，以更好地安排和控制 Asset Processor 中的 CreateJobs 流程。 ([RFC](https://github.com/o3de/sig-core/issues/28))
* **角色**
  * Atom 渲染视口：动画编辑器现在使用 Atom。这显着提高了性能，动画编辑器中的渲染现在与游戏中的预期相当，并且动画编辑器视口现在与其他 O3DE 视口一致。动画编辑器的旧版 OpenGL 渲染视口现已弃用。
* **编辑器**
  * 通用 DOM：核心框架更新以支持文档属性编辑器。
* **实体 & Prefab**
  * 改进了 Script Canvas 对生成的支持：以前，只能使用 Script Canvas 和`SpawnAllEntities`来生成实体。此功能扩展了 Script Canvas 的可生成 API 功能。它提供了对生成工作流程和可生成生命周期的更细粒度的控制，以及对处理多个预制件的更好支持。
  * AutomatedTesting 中的切片弃用：AutomatedTesting 项目已转换为对所有测试使用预制件（以前，有许多项目使用以前的系统 Slices）。 ([7006](https://github.com/o3de/o3de/issues/7006))
* **物理**
  * 删除旧的计时/计时器系统：已删除旧的计时/计时器系统，并为 O3DE 添加了新的计时器。所有系统都已更新为使用新计时器。这是未来即时报价系统更新和改进的基础。之前的平滑逻辑也被删除。
  * 升级物理特性自动测试：所有物理特性自动测试均已更新为使用预制件而不是切片（包括 PhysX、Cloth 和 Blast）。在大多数情况下，测试使用的是新的测试框架。现在，几个项目也在并行运行。
* **安装程序**
  * 安装程序验证测试：这些测试验证安装程序内部版本是否有效。自动测试将验证：关键二进制文件是否存在、o3de.exe注册引擎、项目创建成功、项目编译成功、Asset Processor 批处理成功、编辑器运行、游戏启动器运行、卸载成功以及 o3de.exe 取消注册引擎。目前 O3DE 每晚运行一次，但测试旨在让任何人都可以将它们插入到他们的质量验证流程中。 ([7834](https://github.com/o3de/o3de/pull/7834))
* **Script Canvas**
  * Script Canvas 现在具有“迷你地图”功能，可帮助可视化和导航图形。 ([6263](https://github.com/o3de/o3de/pull/6263))
* **模板**
  * 默认组件模板：新模板可帮助您创建组件。使用 `o3de` CLI 脚本和`create-from-template`命令从 **DefaultComponent** 模板创建新的运行时组件。有关详细说明，请参阅 [在 O3DE 中创建组件](/docs/user-guide/programming/components/create-component/). ([8462](https://github.com/o3de/o3de/pull/8462))
* **视口**
  * 摄像机编辑器视图书签：*视图书签* 添加了使用关卡预制件存储本地视图转换的功能。它们被设计为扩展场景中所有预制件的坚实基础。视图书签系统旨在允许用户为根（级别）预制件存储不同的视图书签。这些书签可以存储在本地（当前在 Settings Registry 中）。将来，它们将能够在任何预制件中共享。书签可以使用 **Ctrl-&lt;功能键&gt;** 进行设置，并使用 **Shift-&lt;功能键&gt;** 进行恢复。目前，每个级别最多可以设置 12 个。 ([7855](https://github.com/o3de/o3de/pull/7855), [8400](https://github.com/o3de/o3de/pull/8400))
  * 实验性编辑器模式视觉反馈：为了使使用视窗的体验更加直观，我们正在研究其他渲染功能，以改善反馈和可用性。编辑器模式视觉反馈处于非常早期的阶段，可以在 Settings Registry 中打开 (`--regset=/Amazon/Preferences/EnableEditorModeFeedback=true`). ([8235](https://github.com/o3de/o3de/pull/8235), [3458](https://github.com/o3de/o3de/issues/3458))
  * 预制件聚焦模式改进：聚焦模式预制件编辑中添加了大量修复和生活质量改进。现在 **Ctrl-S** 将保存聚焦的预制件，而不是整个场景。焦点模式痕迹导航现在具有图标，右键单击上下文菜单分组已得到改进，并且还收到了其他几个小的修复和更新。
  * 操纵器的光标环绕模式：现在可以将光标设置为在 Editor 导航期间环绕视区。此功能默认处于关闭状态，可以在设置注册表中使用`--regset=/Amazon/Preferences/Editor/Manipulator/ManipulatorMouseWrapSetting=true`打开。 ([5155](https://github.com/o3de/o3de/pull/5155))
* **Atom**
  * 新的 Debug Rendering 关卡组件：有一个新的关卡组件，允许用户可视化和调试渲染的各个方面。它可以显示法线、仅渲染漫反射或镜面反射照明、覆盖场景中所有材质的特定属性等。
  * 管道全局通道附件引用：编写`.pass`文件进行渲染的用户现在可以以管道的全局方式声明和引用附件。
  * DiffuseProbeGrid 的可用性和性能改进：
    * DiffuseProbeGrid 可视化效果：将各个探针及其原始辐照度值显示为关卡中适当位置的球体。
    * 滚动 DiffuseProbeGrid：允许将 DiffuseProbeGrid 附加到摄像机，以便在摄像机所在的任何位置应用漫反射 GI。
    * DiffuseProbeGrid `NumberOfRaysPerProbe` 设置：现在可以在 DiffuseProbeGrid 属性中调整网格中每个探针投射的光线数。 每个网格可以设置不同的此值。
    * DiffuseProbeGrid Irradiance query（漫反射探针网格辐照度查询）：查询关卡中任意位置和方向的漫反射 GI。改进了多个网格重叠时和网格边缘周围的 DiffuseProbeGrid 混合，以便与全局 IBL 立方体贴图混合。
  * 新的 **CubeMapCapture** 组件：在关卡中的任何位置捕获漫反射或镜面卷积立方体贴图。
  * 现在，可以通过设置`MultisampleState/samples`参数在`MainRenderPipeline.azasset`文件中更改 MSAA 状态。 将样本设置为 `1`将禁用 MSAA。
  * 对漫反射 GI 和 ReflectionProbe 视觉质量进行了多项改进。
* **白盒**
  * Linux 现在支持白盒！ ([5075](https://github.com/o3de/o3de/pull/5075))
 
## 弃用
 
* **物理**
    * `TransformUniformScale` ([7573](https://github.com/o3de/o3de/issues/7573))
    * `BoxManipulatorRequestBus` ([7572](https://github.com/o3de/o3de/issues/7572))
* **EMFX (角色)**
    * OpenGL 渲染插件现已弃用，取而代之的是 EMFX Atom 视口。
 
## 已知问题
 
* **EMFX (角色)**
    * 如果您已在旧视区中保存布局，它将加载旧的 OpenGL 视区。解决方法是删除 OpenGL 小组件并添加新的 Atom 视口小组件，然后保存布局。 或者，您也可以从默认布局重新创建相同的布局。
 
* **Atom (图形)**
    * [Linux] 在 HDR 颜色分级组件中生成 LUT 后立即按下 **Activate LUT** 按钮时，编辑器崩溃。 ([9157](https://github.com/o3de/o3de/issues/9157))
 
* **资产处理器**
    * [Linux]: 在 Asset Processor 仍在运行时关闭 Editor 将导致重新启动 Editor 时发生崩溃。

* **Linux Ubuntu 20.04.4 LTS**

    在极少数情况下，在 Linux Ubuntu 20.04.4 LTS 上，可能无法从文件资源管理器启动 O3DE Editor、Project Manager、Asset Processor 或其他可执行文件，并显示错误。

    解决方法：使用命令行从其目录启动可执行文件，例如`sudo ./o3de`。请参阅此 [FOSTips 文章](https://fostips.com/double-click-run-elf-ubuntu/)了解其他可能的解决方案。 
    
    请参考 [issue 9502](https://github.com/o3de/o3de/issues/9502) 了解更多信息。

* **网络**
    * 网络实体层次结构仅限于最多具有 16 个实体的层次结构。
    * GameLift 服务器启动器可手动重新定位。目前没有自动生成或资产布局。
    * 目前不支持整体式发布服务器版本。
    * ImGui 键盘在服务器启动器中不起作用。
    * 如果您在使用多人游戏时遇到意外的客户端断开连接问题，请设置`sv_isTransient=False`以检查服务器自动终止逻辑是否导致了问题。有关其他设置，请参阅[用户指南：网络设置](https://www.o3de.org/docs/user-guide/networking/settings/)。 ([9328](https://github.com/o3de/o3de/issues/9328))
    * AWS Gems 中的所有 CDK 示例均适用于 CDK v1 (https://docs.aws.amazon.com/cdk/v1/guide/home.html)。CDK 应用程序目前与 CDK v2 不兼容。
