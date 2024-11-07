---
linkTitle: Gem 参考
title: Open 3D Engine中的Gem参考
description: 由 Open 3D Foundation 提供的 Gem，用于将代码和资产功能添加到您的 Open 3D Engine （O3DE） 项目中。
weight: 100
toc: true
---

**Gem** 是可再发行的包，其中包含源代码和资产，您可以将其包含在 **Open 3D Engine （O3DE）** 项目中以添加新功能。O3DE 提供以下 Gem：

## Animation

| Gem | 说明 |
| - | - |
| [EMotion FX Animation](./animation/emotionfx) | EMotion FX Animation Gem 为绑定角色提供 Open 3D Engine 的动画系统，并包括 Animation Editor，这是一个用于创建动画行为、模拟对象和绑定角色碰撞器的工具。 |
| [Maestro Cinematics](./animation/maestro) | Maestro Cinematics Gem 提供 Track View、Open 3D Engine 的动画序列和电影编辑器。 |

## Artificial Intelligence

| Gem | 说明 |
| - | - |
| [Kythera AI](./kythera-ai/) | Kythera AI Gem 为 Open 3D Engine （O3DE） 中的 Kythera AI 功能提供支持，并包括演示 Kythera AI 功能的演示项目、关卡和资产。 |
| [Recast Navigation](./ai/recast/recast-navigation) | Recast Navigation Gem 支持在这些导航网格中构建导航网格和计算可行走路径。它使用开源库 [Recast Navigation](https://github.com/recastnavigation/recastnavigation)。可以在 Navigation Sample 关卡的 AutomatedTesting 项目中找到其使用示例。 |

## Assets

| Gem | 说明 |
| - | - |
| [Asset Validation](./assets/asset-validation) | Asset Validation Gem 提供与种子相关的命令，以确保资产具有用于资产捆绑的有效种子。 |
| [Custom Asset Example](./assets/custom-asset-example) | 自定义资产示例 Gem 提供了为 Open 3D Engine 的资产管道创建自定义资产的示例代码。 |
| [Dev Textures](./assets/dev-textures) | Dev Textures Gem 提供了一组可用于原型和预生产的通用纹理资源。 |
| [Prefab Builder](./assets/prefab) | Prefab Builder Gem 为Prefab提供 Asset Processor 模块，Prefab是通过组合较小的实体构建的复杂资产。 |
| [Primitive Assets](./assets/primitive-assets) | Primitive Assets Gem 提供启用了物理特性的原始形状网格对象。 |
| [Scene Processing](./assets/scene-processing) | Scene Processing Gem （场景处理 Gem） 提供 FBX Settings（FBX 设置），该工具可用于指定用于处理角色、网格、运动和 PhysX 的 .fbx 文件的默认设置。 |
| [Test Asset Builder](./assets/test-asset-builder) | 测试 Asset Builder Gem 用于对 Asset Processor 进行功能测试。 |

## Audio

| Gem | 说明                                                                |
| - |-------------------------------------------------------------------|
| [Audio Engine Wwise](./audio/wwise/audio-engine-wwise) | Wwise Audio Engine Gem 支持 Audiokinetic Wave Works 互动声音引擎 （Wwise）。 |
| [Audio System](./audio/audio-system) | 音频系统 Gem 提供音频转换层 （ATL），它在 Open 3D Engine 项目中添加了对音频的支持。            |
| [Microphone](./audio/microphone) | Microphone Gem 支持通过麦克风输入音频。                                       |
| [MiniAudio](./audio/miniaudio/miniaudio) | MiniAudio Gem 支持使用 [MiniAudio](https://miniaud.io)。               |

## AWS

| Gem | 说明 |
| - | - |
| [AWS Client Auth](./aws/aws-client-auth) | AWS Client Auth Gem 提供客户端身份验证和 AWS 授权的解决方案。 |
| [AWS Core](./aws/aws-core) | AWS Core Gem 提供基本的共享 AWS 功能，例如 AWS 开发工具包初始化和客户端配置。 |
| [AWS Metrics](./aws/aws-metrics) | AWS Metrics Gem 为 AWS 指标提交和分析提供了解决方案。|
| [AWS GameLift](./aws/aws-gamelift) | AWS GameLift Gem 提供了一个框架，用于扩展 O3DE 联网层和多人游戏 Gem 以与 Amazon GameLift 配合使用。 |

## Core

| Gem | 说明 |
| - | - |
| [O3DE Core (LmbrCentral)](./o3de-core) | O3DE Core （LmbrCentral） Gem 提供运行 Open 3D Engine Editor 所需的代码和资产。 |

## Debug

| Gem | 说明 |
| - | - |
| [Crash Reporting](./debug/crash-reporting) | Crash Reporting Gem 为 Open 3D Engine 项目的外部崩溃报告提供支持。 |
| [Debug Draw](./debug/debug-draw) | Debug Draw Gem 为 Open 3D Engine 提供 Editor 和运行时调试可视化功能。 |
| [Immediate Mode GUI (IMGUI)](./debug/imgui) | 即时模式 GUI Gem 提供了第三方库 IMGUI，该库可用于创建运行时即时模式叠加层，以便在 Open 3D Engine 中调试和分析信息。 |
| [Remote Tools](./debug/remote-tools) | 远程工具 Gem 有助于 Open 3D Engine 应用程序之间的连接以进行调试。 |
<!-- | [Automated Launcher Testing](./debug/automated-launcher-testing) | Automated Launcher Testing Gem 管理自动化的 Open 3D Engine （O3DE） 启动器测试。| -->

## Design

| Gem | 说明 |
| - | - |
| [White Box](./design/white-box) | White Box Gem 为 Open 3D Engine 提供 White Box 快速设计组件。 |

## Environment

| Gem | 说明 |
| - | - |
| [Landscape Canvas](./environment/landscape-canvas) | Landscape Canvas Gem 提供 Landscape Canvas 编辑器;一个基于节点的图形工具，用于编写工作流以使用动态植被填充景观。 |
| [Surface Data](./environment/surface-data) | Surface Data Gem 提供了从表面 （如网格和 terrain） 发出信号或标签的功能。 |
| [Vegetation](./environment/vegetation) | Vegetation Gem 提供了在 Open 3D Engine 中放置自然植被的工具。 |
| [Terrain](./environment/terrain) | Terrain Gem 提供了一个地形系统，该系统将高度、颜色和表面数据映射到世界各个区域。它还提供基于渐变和基于形状的创作工具和工作流程，与物理特性集成以进行物理模拟，并高效渲染地形。 |
<!-- | [Vegetation Gem Assets](./environment/vegetation-gem-assets) | Vegetation Assets Gem 提供植被模型、纹理以及用于 Vegetation Gem 和 Landscape Canvas 的其他资产和示例。 | -->

## Framework

| Gem | 说明 |
| - | - |
| [Graph Canvas](./framework/graph-canvas) | Graph Canvas Gem 提供了一个 C++ 框架，用于为 Open 3D Engine 创建基于图形节点的自定义编辑器。 |
| [Graph Model](./framework/graph-model) | 图形模型 Gem 为 Open 3D Engine 提供了一个通用的节点图形数据模型框架。|

## Gameplay

| Gem | 说明 |
| - | - |
| [Achievements](./gameplay/achievements) | 成就 Gem 提供了一个与目标平台无关的界面，用于检索成就详细信息和解锁成就。 |
| [Game State](./gameplay/game-state) | Game State Gem 提供了在 Open 3D Engine 项目中确定和管理游戏状态的功能。 |
| [Game State Samples](./gameplay/game-state-samples) | Game State Samples Gem 提供了一组示例游戏状态（基于 Game State Gem 构建），包括主要用户选择、主菜单、关卡加载、关卡运行和关卡暂停。 |
| [Tick Bus Order Viewer](./gameplay/tick-bus-order-viewer) | Tick Bus Order 控制台变量 Gem 提供了一个控制台变量，用于显示运行时 tick 事件的顺序。 |

## Input

| Gem | 说明 |
| - | - |
| [Gestures](./input/gestures) | Gestures Gem 提供对基于手势的常见输入操作的检测。 |
| [Local User](./input/local-user) | 本地用户 Gem 提供将本地用户 ID 映射到本地玩家槽位和管理本地用户配置文件的功能。 |
| [Starting Point Input](./input/starting-point-input) | Starting Point Input Gem 提供了将低级别输入事件映射到高级操作的功能。 |
| [Starting Point Movement](./input/starting-point-movement) | Starting Point Movement Gem 提供了一系列 Lua 脚本，用于侦听和响应输入事件。 |
| [Virtual Gamepad](./input/virtual-gamepad) | Virtual Gamepad Gem 提供在触摸屏设备上模拟游戏手柄的控件。 |

## Multiplayer

| Gem | 说明 |
| - | - |
| [Multiplayer](./multiplayer/multiplayer-gem) | 多人游戏 Gem 提供高级多人游戏功能，例如实体复制、本地预测和服务器端向后对帐。 |
| [Multiplayer Compression](./multiplayer/multiplayer-compression) | 多人游戏压缩 Gem 提供与多人游戏 Gem 一起使用的开源压缩器。 |

## Network

| Gem | 说明 |
| - | - |
| [Certificate Manager](./network/certificate-manager) | Certificate Manager Gem 提供对身份验证文件的访问，以便从 Amazon S3、磁盘上的文件和其他第三方来源进行安全游戏连接。 |
| [Http Requestor](./network/http-requestor) | HTTP 请求者 Gem 提供通过用户提供的回调函数发出异步 HTTP/HTTPS 请求和返回数据的功能。|
| [Metastream](./network/twitch/metastream) | Metastream Gem 为 HTTP 服务器提供功能，允许广播公司使用来自游戏会话的统计数据和事件数据的叠加来自定义游戏流。 |
| [Presence](./network/presence) | Presence Gem 为 Presence 服务提供与目标平台无关的界面。 |
| [Twitch](./network/twitch/twitch) | Twitch Gem 提供对 Twitch API v5 SDK 的访问，包括社交功能、频道和其他 API。 |

## Physics

| Gem | 说明 |
| - | - |
| [NVIDIA Cloth (NvCloth)](./physics/nvidia/nvidia-cloth) | NVIDIA Cloth （NvCloth） Gem 提供了创建逼真的布料和织物模拟的功能。 |
| [PhysX](./physics/nvidia/physx) | PhysX Gem 通过 NVIDIA PhysX 提供物理模拟，包括静态和动态刚体模拟、力区域、人偶和动态 PhysX 关节。 |
| [PhysX Debug](./physics/nvidia/physx-debug) | The PhysX Debug Gem provides debugging functionality and visualizations for PhysX in Open 3D Engine (O3DE) projects. |
<!--| [NVIDIA Blast](./physics/nvidia/nvidia-blast) | NVIDIA Blast Gem 提供了在 Houdini 中创作破裂网格资产的工具，以及在 Open 3D Engine 中创建逼真破坏模拟的功能。 | 隐藏，直到修复 blast 工具并更新 blast 文档。-->

## Rendering

| Gem | 说明 |
| - | - |
| [Atom Common Features](./rendering/atom/atom) | Atom Gem 提供 Atom 渲染器及其相关工具（例如材质编辑器）、实用程序、库和接口。 |
| [Atom Content](./rendering/atom/atom-content) | Atom 内容 Gem 提供包括模型、纹理和材质在内的资源，可用于在 Open 3D Engine 中测试 Atom 渲染器。 |
| [Atom O3DE Integration](./rendering/atom/atom-o3de-integration) | Atom O3DE 集成 Gem 提供组件、库和功能，以支持 Atom Renderer 并将其集成到 Open 3D Engine 中。 |
| [Atom TressFX](./rendering/amd/atom-tressfx) | Atom TressFX Gem 在 Atom 和 Open 3D Engine 中提供逼真的头发和毛发模拟和渲染。|
| [Camera](./rendering/camera) | Camera Gem 提供了一个基本的摄像机组件，用于定义运行时渲染的视锥体。 |
| [Camera Framework](./rendering/camera-framework) | Camera Framework Gem 为实现更复杂的相机系统提供了基础。 |
| [PBR Reference Materials](./rendering/pbr-reference-materials) | PBR Reference Materials Gem 为 Open 3D Engine （O3DE） 项目提供基于物理的参考资料。 |
| [Stars](./rendering/stars) | 星星 Gem 提供基于物理的动画、与分辨率无关的遥远星星。 |
| [Starting Point Camera](./rendering/starting-point-camera) | Starting Point Camera Gem （起点摄像机 Gem） 提供与 Camera Framework Gem 一起使用的行为，以定义摄像机装备。 |
| [Video Playback Framework](./rendering/video-playback-framework) | 视频播放框架 Gem 提供了播放视频的接口。 |

## Robotics

| Gem | 说明 |
| - | - |
| [Human Worker](./robotics/humanworker.md) | Human Worker Gem 提供了一组可用于机器人模拟的动画人类工作人员资产。 |
| [OTTO Robots](./robotics/otto-robots.md) | OTTO Robots Gem 提供了一组可用于机器人模拟的自主移动机器人资产。 |
| [RGL](./robotics/rgl.md) | 用于 Open 3D 引擎 （O3DE） 的 Robotec GPU Lidar （RGL） Gem 支持机器人技术的 GPU 加速 LiDAR 仿真。 |
| [ROS&nbsp;2](./robotics/ros2.md) | ROS 2 Gem 提供与 [机器人操作系统 （ROS） 2](https://docs.ros.org/en/rolling/index.html) 库的集成，并支持机器人系统的仿真设计。 |
| [UR Robots](./robotics/ur-robots.md) | UR Robots Gem 提供了一系列可用于机器人仿真的机械臂资产。 |

## Script

| Gem | 说明 |
| - | - |
| [Editor Python Bindings](./script/python/editor-python-bindings) | 编辑器 Python 绑定 Gem 为 Open 3D Engine Editor 函数提供 Python 命令。 |
| [Expression Evaluation](./script/expression-evaluation) | 表达式评估 Gem 提供了一种解析和执行字符串表达式的方法。|
| [Python Asset Builder](./script/python/python-asset-builder) |Python 资产生成器 Gem 提供了在 Python 中为 Asset Processor 实施自定义资产生成器的功能。|
| [Script Canvas](./script/script-canvas) | Script Canvas Gem 提供 Open 3D Engine 的可视化脚本环境 Script Canvas。 |
| [Script Canvas Developer](./script/script-canvas-developer) | Script Canvas 开发人员 Gem 提供了一套用于开发和调试 Script Canvas 系统的实用程序功能。 |
| [Script Canvas Physics](./script/script-canvas-physics) | Script Canvas 物理特性 Gem 为物理特性场景查询（如光线投射）提供 Script Canvas 节点。 |
| [Script Canvas Testing](./script/script-canvas-testing) | Script Canvas 测试 Gem 提供了一个框架，用于测试 Script Canvas 以及使用 Script Canvas 进行测试。 |
| [Scripted Entity Tweener](./script/scripted-entity-tweener) | 脚本化实体补间 Gem 为 Open 3D Engine 项目提供脚本驱动的动画系统。 |
| [Script Events](./script/script-events) | 脚本事件 Gem 提供了一个框架，用于创建可从 Open 3D Engine 中的任何脚本解决方案中使用的事件资产。 |
| [Qt for Python](./script/python/qt-for-python) | Qt for Python Gem 提供 PySide2 Python 库来管理 Qt 小部件。 |

## SDK

| Gem | 说明 |
| - | - |
| [In-App Purchases](./sdk/in-app-purchases) | In-App Purchases Gem 为 Open 3D Engine 项目中的应用程序内购买提供功能。 |

## UI

| Gem | 说明 |
| - | - |
| [LyShine](./ui/lyshine) | LyShine Gem 为 Open 3D Engine 项目提供运行时 UI 系统和创建工具。 |
| [LyShine Examples](./ui/lyshine-examples) | LyShine 示例 Gem 提供了 LyShine 的示例代码和资源，LyShine 是 Open 3D Engine 项目的运行时 UI 系统和编辑器。 |
| [Message Popup](./ui/message-popup) | Message Popup Gem 提供了 Open 3D Engine 项目中弹出消息的示例实现。 |
| [UI Basics](./ui/ui-basics) | UI 基础知识 Gem 提供了一组可与 LyShine、Open 3D Engine 运行时用户界面系统和编辑器一起使用的资源。 |

## Utility

| Gem | 说明 |
| - | - |
| [Fast Noise](./utility/fast-noise) | Fast Noise Gradient Gem 使用第三方开源 FastNoise 库来提供各种高性能噪声生成算法。 |
| [Gradient Signal](./utility/gradient-signal) | Gradient Signal Gem 提供了许多用于生成、修改和混合梯度信号的组件。 |
| [Save Data](./utility/save-data) | Save Data Gem 提供了一个 API，用于将运行时数据保存在 Open 3D Engine 项目中。 |
| [Scene Logging Example](./utility/scene-logging-example) | 场景日志记录示例 Gem 演示了通过向管道添加其他日志记录来扩展 Open 3D Engine SceneAPI 的基础知识。 |
| [Texture Atlas](./utility/texture-atlas) | 纹理图集 Gem 为 Open 3D Engine 中的纹理图集提供格式设置。 |
