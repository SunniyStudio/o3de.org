---
linkTitle: 功能网格
title: 22.05.0 功能网格快照
description: 22.05.0 版本的 Open 3D Engine （O3DE） 功能状态网格快照。
toc: true
---

**Open 3D Engine （O3DE）** 功能网格是 O3DE 中每个功能系统的状态及其当前支持状态的记录。此页面中包含的功能网格是针对 22.05.0 版本生成的。有关最新的功能网格，请参阅 [o3de/community 功能网格](https://o3de.github.io/community/features/form.html)。

功能通过每个单独的 O3DE 特别兴趣小组 （SIG） 进行报告。有关每个 SIG 及其职责的更多信息，请参阅 GitHub 上的 [o3de/社区存储库](https://github.com/o3de/community/)。
 
## SIG-Build 

### 构建系统

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Github 管线 | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟢 最优化 | 所有  |
| Jenkins 管线 | 🟡 活跃 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🔵 进行中 | 所有  |
| 安装包构建 | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟢 最优化 | Windows Linux  |
| 构建失败分析 | 🟡 活跃 | 🟢 完成 | 🟢 完成 | 🔵 进行中 | 🔵 进行中 | 所有  |
| 构建脚本 | 🟡 活跃 | 🟢 完成 | 🟢 完成 | 🔵 进行中 | 🔵 进行中 | 所有  |
| 构建环境 | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟢 最优化 | 所有  |
| 构建矩阵 | 🟡 活跃 | 🟢 完成 | 🟢 完成 | 🔵 进行中 | 🔵 进行中 | 所有  |
| 第3方软件包系统 | 🟡 活跃 | 🟢 完成 | 🟢 完成 | 🔵 进行中 | 🔵 进行中 | 所有  |

### 基础设施 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Jenkins | 🟡 活跃 | 🟢 完成 | 🟢 完成 | 🔵 进行中 | 🔵 进行中 | 所有  |
| Github | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟢 最优化 | 所有  |
| LFS | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟢 最优化 | 所有  |
| 许可证扫描 | 🟡 活跃 | 🟢 完成 | 🟢 完成 | 🔵 进行中 | 🔵 进行中 | 所有  |

## SIG-内容 

### 框架 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| AzToolsFramework | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有 |
| Lua | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有 |
| Prefabs | || || ||
| Qt for Python | || || ||

### 编辑器 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Asset Browser | 🟡 活跃 | 🔵 设计中 | ⭕ 不需要 | 🟢 文档 | 🔵 进行中 | Windows Linux MacOS  |
| Framework | || || ||
| Localization | || || ||
| Undo / Redo | || || ||
| Asset Editor | 🔵 待办 | 🟠 最小 | ⭕ 不需要 | 🟢 文档 | 🔵 进行中 | Windows Linux MacOS  |

### Canvas Tools 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Graph Model | 🟡 活跃 | 🟠 最小 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | Windows Linux MacOS  |
| Graph Canvas | 🟡 活跃 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | Windows Linux MacOS  |
| Landscape Canvas | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | Windows Linux MacOS  |

### Project Manager 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| 远程项目 | 🟡 活跃 | 🔵 设计中 | ⭕ 不需要 | 🔵 进行中 | 🔵 进行中 | Windows Linux  |
| 项目版本控制 | 🟡 活跃 | 🔵 设计中 | ⭕ 不需要 | 🔵 进行中 | 🔵 进行中 | Windows Linux  |
| 模板管理 | 🟠 已计划 | ❌ 无 | ⭕ 不需要 | ❌ 未被证实 | ❌ 未支持 | Windows Linux  |
| Gem 创建向导 | 🟠 已计划 | ❌ 无 | ⭕ 不需要 | ❌ 未被证实 | ❌ 未支持 | Windows Linux  |
| 远程 Gem 改进 (URI vs. URL) | 🟠 已计划 | ❌ 无 | ⭕ 不需要 | ❌ 未被证实 | ❌ 未支持 | Windows Linux  |
| 远程 Gem (初版) | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔵 进行中 | Windows Linux  |

### 脚本编程 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Expression Evaluation | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有 |
| Script Canvas | 🟡 活跃 | 🟡 部分 | ⭕ 不需要 | 🟠 易变 | 🔴 需要测试 | Windows Linux MacOS  |
| Script Canvas Developer | 🟡 活跃 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | Windows Linux MacOS  |
| Script Events | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有 |
| Script Canvas Testing | 🟢 完成 | 🟡 部分 | ⭕ 不需要 | 🟠 易变 | 🟢 最优化 | Windows Linux MacOS  |
| Lua Editor | 🟡 活跃 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | Windows Linux MacOS  |

### User Interface 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| LyShine (2D Render) | 🟡 活跃 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🔴 需要测试 | Windows Linux MacOS  |

### Animation 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Animation Playback Control | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Pose Blending | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Animation Syncing | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Motion Events | 🔵 待办 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Bone Masking | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Motion Extraction (Root Motion) | 🟡 活跃 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Motion Matching | 🟡 活跃 | 🟡 部分 | ⭕ 不需要 | 🟡 实验 | 🔵 进行中 | 所有  |
| Debug Rendering | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Animation Sharing | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Animation Compression | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Multi-threading | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Retargeting | 🟢 完成 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Inverse Kinematics (IK) | 🟢 完成 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| LOD | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Blend Tree/State Machine | 🟢 完成 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Transition Conditions | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Wildcard Conditions | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Debugging Tools (Anim Graph) | 🟢 完成 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Visual Tools | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Software Skinning (Linear, Dual-Quat) | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| GPU Skinning (Linear, Dual-Quat) | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Morph Target/Facial Animation | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| GPU Accelerated Morphing | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Simulated Objects/Dynamic Bones | 🟢 完成 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Ragdoll Runtime | 🟢 完成 | 🟡 部分 | ⭕ 不需要 | 🟡 实验 | 🔵 进行中 | 所有  |
| Cloth Authoring | 🟢 完成 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Collider Authoring Tools | 🟡 活跃 | 🟡 部分 | ⭕ 不需要 | 🔵 进行中 | 🔵 进行中 | 所有  |
| Attachments | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Skinned Attachments | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |

### World Building 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Terrain | 🟡 活跃 | 🟠 最小 | ❌ 无 | 🟡 实验 | 🟡 需要优化 | Windows |
| Dynamic Vegetation | 🟢 完成 | 🟢 完成 | 🟠 部分 | 🟢 文档 | 🟡 需要优化 | 所有 |

### Viewport 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Manipulators | 🟡 活跃 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | Windows Linux MacOS  |
| Component Mode | 🟡 活跃 | 🟡 部分 | ⭕ 不需要 | 🔵 进行中 | 🔴 需要测试 | Windows Linux MacOS  |
| Viewport UI | 🟡 活跃 | 🟠 最小 | ⭕ 不需要 | 🔵 进行中 | 🔴 需要测试 | Windows Linux MacOS  |
| Interaction Model | 🟡 活跃 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | Windows Linux MacOS  |
| Camera | 🟡 活跃 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | Windows Linux MacOS  |
| View Bookmarks | 🟡 活跃 | 🟠 最小 | ⭕ 不需要 | 🟡 实验 | 🔴 需要测试 | Windows Linux MacOS  |
| Manipulator Test Framework | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | Windows Linux MacOS  |
| Visibility | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | Windows Linux MacOS  |
| Editor Mode Visual Feedback | 🟡 活跃 | 🟠 最小 | ⭕ 不需要 | 🟡 实验 | 🔵 进行中 | Windows Linux MacOS  |

### White Box Tool 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Atom Integration | 🟢 完成 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Viewport Editing | 🟡 活跃 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | Windows Linux MacOS  |
| Triangulation | 🔵 待办 | ❌ 无 | ⭕ 不需要 | ❌ 未被证实 | ❌ 未支持 | Windows Linux MacOS  |
| Boolean Operations | 🔵 待办 | ❌ 无 | ⭕ 不需要 | ❌ 未被证实 | ❌ 未支持 | Windows Linux MacOS  |
| Custom UV Mapping | 🔵 待办 | ❌ 无 | ⭕ 不需要 | ❌ 未被证实 | ❌ 未支持 | Windows Linux MacOS  |

## SIG-Core 

### Core features 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| AzCore | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| AzFramework | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Math libraries | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| SDK Build | 🟢 完成 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | Windows Linux MacOS  |
| Reflection frameworks | 🟡 活跃 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🟡 需要优化 | 所有  |
| Streaming system | 🟡 活跃 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🔵 进行中 | 所有  |
| Input system | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Logging and tracing | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟡 需要优化 | 所有  |
| Profiling | 🟡 活跃 | 🟡 部分 | ⭕ 不需要 | 🟠 易变 | 🔵 进行中 | Windows  |
| Optimised standard library | 🟡 活跃 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔵 进行中 | 所有  |

### Physics API (最小, non-backend specific) 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Collision Filtering | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟠 易变 | 🔴 需要测试 ||
| Collision Filtering - Programmable Reserved Bits | 🔵 待办 | ❌ 无 |||||
| Joints | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 ||
| Rigid Bodies | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 ||
| Multiple Scenes | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟡 实验 | 🔴 需要测试 ||
| Character Controller | 🟢 完成 | 🟡 部分 | ⭕ 不需要 | 🟠 易变 | 🔴 需要测试 ||
| Ragdoll | 🟢 完成 | 🟡 部分 | ⭕ 不需要 | 🟠 易变 | 🔴 需要测试 ||
| Materials | 🟡 活跃 | 🟢 完成 | ⭕ 不需要 | 🟠 易变 | 🔴 需要测试 ||
| Shapes | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 ||
| Heightfields | 🟡 活跃 | 🟡 部分 | ⭕ 不需要 | 🟠 易变 | 🔴 需要测试 ||
| Wind | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟡 实验 | 🔴 需要测试 ||
| Scene Queries | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 ||

### Nvidia PhysX Integration 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Ticking | 🔵 待办 | 🟡 部分 | ⭕ 不需要 | 🟠 易变 | 🔴 需要测试 | 所有  |
| Rigid Body Simulation | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟡 需要优化 | 所有  |
| Continuous Collision Detection (CCD) | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Collision Asset Pipeline | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Convex Decomposition | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Primitive Fitting | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Primitive Colliders | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Asset Colliders | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Shape Colliders | 🟢 完成 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Heightfield Colliders | 🟡 活跃 | 🟠 最小 | ⭕ 不需要 | 🟠 易变 | 🟡 需要优化 | 所有  |
| Triggers | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Force Regions | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Wind | 🔵 待办 | 🟡 部分 | ⭕ 不需要 | 🟡 实验 | 🔴 需要测试 | 所有  |
| Materials | 🟡 活跃 | 🟢 完成 | 🟢 完成 | 🟠 易变 | 🔴 需要测试 | 所有  |
| Collision Filtering | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Joints | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Articulations | 🟠 已计划 | ❌ 无 | ⭕ 不需要 | ❌ 未被证实 | 🔴 需要测试 | 所有  |
| Character Controller | 🟢 完成 | 🟠 最小 | ⭕ 不需要 | 🟠 易变 | 🔴 需要测试 | 所有  |
| Ragdoll | 🟡 活跃 | 🟡 部分 | 🟢 完成 | 🟠 易变 | 🔴 需要测试 | 所有  |
| Scripting | 🟢 完成 | 🟠 最小 | ❌ 无 | 🟠 易变 | 🔴 需要测试 | 所有  |
| Scene Queries | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Multi-Scene | 🔵 待办 | ❌ 无 | ⭕ 不需要 | ❌ 未被证实 | 🔴 需要测试 | 所有  |
| PhysX Visual Debugger Integration | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | Windows  |
| Debug Visualization | 🟢 完成 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | Windows Linux MacOS  |
| Mesh Simplification | ❌ 未安排 | || || |

### Cloth - NvCloth Integration 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Generic API | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Support for Mesh Components | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔵 进行中 | 所有  |
| Support for Actor Components | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔵 进行中 | 所有  |
| Mesh Simplification | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Simulation Constraints | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Realtime Editing | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Wind | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Actor Colliders | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| CCD | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Self Collision | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Async Simulation | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Debug Visualization | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🔴 需要测试 | 所有  |
| Environmental Collision | 🔵 待办 | ❌ 无 | || ||
| Painting Tool | 🔵 待办 | ❌ 无 | || ||
| LOD | 🔵 待办 | ❌ 无 | || ||
| Mesh Collision | 🔵 待办 | ❌ 无 | || ||

### Destruction - Nvidia Blast Integration 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Authoring/Pipeline | 🔵 待办 | 🔵 设计中 | ⭕ 不需要 | 🟡 实验 | 🔴 需要测试 | Windows  |
| Geometry Destruction Simulation | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟡 实验 | 🔴 需要测试 | Windows  |
| Materials | 🟡 活跃 | 🟢 完成 | ⭕ 不需要 | 🟡 实验 | 🔴 需要测试 | Windows  |
| Scripting | 🟢 完成 | 🟠 最小 | ⭕ 不需要 | 🟡 实验 | 🔴 需要测试 | Windows  |
| Atom Integration | 🔵 待办 | 🟠 最小 | ⭕ 不需要 | 🟡 实验 | 🔴 需要测试 | Windows  |
| PhysX Integration | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟡 实验 | 🔴 需要测试 | Windows  |

### Vehicles 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Vehicles | ❌ 未安排 | || || |

### Fluids 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Fluids | ❌ 未安排 | || || |

### Soft Bodies 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Soft Bodies | ❌ 未安排 | || || |

## SIG-Graphics-Audio 

### Features 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Deferred Fog | 🟢 完成 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | 所有  |
| Tonemapping | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟡 需要优化 | 所有  |
| Direct Lighting / Area Lights | 🟢 完成 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟡 需要优化 | 所有  |
| Meshes | 🟡 活跃 | 🟡 部分 | 🟢 完成 | 🟢 文档 | 🟡 需要优化 | 所有  |
| Skinned Meshes | 🟡 活跃 | 🟡 部分 | 🟢 完成 | 🟢 文档 | 🟡 需要优化 | 所有  |
| Eye Adaptation | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟡 需要优化 | 所有  |
| Culling | 🟡 活跃 | 🟡 部分 | 🟢 完成 | 🔵 进行中 | 🟡 需要优化 | 所有  |
| HDR Pipeline | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟡 需要优化 | 所有  |
| Shadows | 🟡 活跃 | 🟡 部分 | 🟢 完成 | 🔵 进行中 | 🟡 需要优化 | 所有  |
| Skybox and Physical Sky | 🟢 完成 | 🟡 部分 | 🟢 完成 | 🟢 文档 | 🟡 需要优化 | 所有  |
| SSAO | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟢 最优化 | 所有  |
| Color Grading | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟡 需要优化 | 所有  |
| Depth of Field | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟡 需要优化 | 所有  |
| PBR Materials | 🟡 活跃 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟡 需要优化 | 所有  |
| Post Processing Volumes | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟡 需要优化 | 所有  |
| Decals | 🟡 活跃 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟡 需要优化 | 所有  |
| Screen Space Reflections | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟡 需要优化 | 所有  |
| Subsurface Scattering | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟡 需要优化 | 所有  |
| Motion Vectors | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟢 最优化 | 所有  |
| Temporal Anti-aliasing (TAA) | 🟡 活跃 | 🟡 部分 | 🟢 完成 | 🔵 进行中 | 🟡 需要优化 | 所有  |

### Render Hardware Interface 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| DirectX 12 | 🟡 活跃 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟡 需要优化 | |
| Vulkan | 🟡 活跃 | 🟢 完成 | ⭕ 不需要 | 🟢 文档 | 🟡 需要优化 | |
| Metal | 🟡 活跃 | 🟡 部分 | ⭕ 不需要 | 🟠 易变 | 🟡 需要优化 | |

### Audio 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Wwise Integration | 🟢 完成 | 🟡 部分 | ⭕ 不需要 | 🟢 文档 | 🟢 最优化 | |

## SIG-Network 

### Core Networking 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Transport API | 🟢 完成 | || || |
| Multiple network interface support | 🟢 完成 | || || |
| Compression (TCP/UDP) | 🟢 完成 | || || |
| Metrics support | 🟢 完成 | || || |
| UDP Core | 🟢 完成 | || || |
| UDP: DTLS support | 🟢 完成 | || || |
| UDP: Reliable queue support | 🟢 完成 | || || |
| UDP: Fragmentated packet support | 🟢 完成 | || || |
| TCP | 🟢 完成 | || || |
| TCP: TLS Support | 🟢 完成 | || || |
| TCP: Ringbuffer support Pkg Xmit | 🟢 完成 | || || |

### Multiplayer 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Multiplayer component API | 🟢 完成 | || || |
| Local Prediction | 🟢 完成 | || || |
| Server Side Rollback | 🟢 完成 | || || |
| Play in Editor Mode | 🟡 活跃 | || || |
| Hosting/Joining a Game | 🟢 完成 | || || |
| Network property support | 🟢 完成 | || || |
| RPC support | 🟢 完成 | || || |
| Network Input support | 🟡 活跃 | || || |
| ScriptBind support | 🟡 活跃 | || || |
| Netbound entity support [NetBindComponent] | 🟢 完成 | || || |
| Entity replication support | 🟢 完成 | || || |
| Network Prefab Spawning | 🟡 活跃 | || || |:w
| Networked Animation | ❌ 未安排 | || || |
| Network Audio Support | ❌ 未安排 | || || |
| Network Simulation (Physics) | 🟢 完成 | || || |
| Quality of Service | 🟡 活跃 | || || |
| Debugging Tools | 🟡 活跃 | || || |
| Metrics | 🟡 活跃 | || || |

### AWS Cloud Services 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| HTTPS Support | 🟢 完成 | || || |
| Restful API Support | 🟡 活跃 | || || |
| AWS C++ SDK Support | 🟢 完成 | || || |
| Client Side Ident & Auth | 🟡 活跃 | || || |
| Runtime Metrics | 🟡 活跃 | || || |
| Amazon GameLift Support | 🟡 活跃 | || || |

### Microsoft Azure Cloud Services 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Core services | ❌ 未安排 | || || |

## SIG-Platform 

### Platform Abstraction Layer 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| PAL CMake | 🟡 活跃 | 🟡 部分 | || ||
| PAL Tools/Editor/AP | 🟡 活跃 | 🟡 部分 | || ||

### Platform Configure (Engine Centric) 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Windows | 🟢 完成 | 🟢 完成 | || ||
| Mac | 🟢 完成 | 🟢 完成 | || ||
| Android | 🟢 完成 | 🟢 完成 | || ||
| Linux | 🟢 完成 | 🟢 完成 | || ||

### Platform Build (Engine Centric) 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Windows | 🟢 完成 | 🟢 完成 | || ||
| Mac | 🟢 完成 | 🟢 完成 | || ||
| Android | 🟢 完成 | 🟢 完成 | || ||
| Linux | 🟢 完成 | 🟢 完成 | || ||


### Platform Configure (Project Centric) 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Windows | 🟡 活跃 | 🟡 部分 | || ||
| Mac | 🟡 活跃 | 🟡 部分 | || ||
| Android | 🟡 活跃 | 🟡 部分 | || ||
| Linux | 🟡 活跃 | 🟡 部分 | || ||

### Platform Build (Project Centric) 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Windows | 🟡 活跃 | 🟡 部分 | || ||
| Mac | 🟡 活跃 | 🟡 部分 | || ||
| Android | 🟡 活跃 | 🟡 部分 | || ||
| Linux | 🟡 活跃 | 🟡 部分 | || ||

### O3DE Object Externalization 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Project | 🟢 完成 | 🟢 完成 | || ||
| Gem | 🟢 完成 | 🟢 完成 | || ||
| Template | 🟢 完成 | 🟢 完成 | || ||
| Restricted | 🟡 活跃 | 🟡 部分 | || ||
| Repo | 🟡 活跃 | 🔵 设计中 | || ||

### Language/Localization 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Editor | 🟢 完成 | 🟢 完成 | || ||
| Runtime | 🟡 活跃 | 🟡 部分 | || ||

### Packaging 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Windows | 🟢 完成 | 🟢 完成 | || ||
| Mac | 🟢 完成 | 🟢 完成 | || ||
| Android | 🟢 完成 | 🟢 完成 | || ||
| Linux | 🟢 完成 | 🟢 完成 | || ||

## SIG-Testing 

### AutomatedReview 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Early Warning | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟢 最优化 | |
| CTest | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟢 最优化 | |
| GoogleTest | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟢 最优化 | |
| GoogleBenchmark | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟢 最优化 | |
| PyTest | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟢 最优化 | |
| O3DE EditorTest | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟢 最优化 | |
| Test Metrics | 🟢 完成 | 🟢 完成 | 🟢 完成 | 🟢 文档 | 🟢 最优化 | |

## SIG-UI-UX 

### UI Components 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Breadcrumb navigation | 🟢 完成 | 🟢 完成 | || ||
| Browse Edit | 🟢 完成 | 🟢 完成 | || ||
| Button | 🟢 完成 | 🟢 完成 | || ||
| Card | 🟠 已计划 | 🟡 部分 | || ||
| Checkbox | 🟢 完成 | 🟢 完成 | || ||
| Combobox | 🟢 完成 | 🟢 完成 | || ||
| Context menu | 🟢 完成 | 🟢 完成 | || ||
| Filtered search | 🟠 已计划 | 🟡 部分 | || ||
| Line edit | 🟢 完成 | 🟢 完成 | || ||
| Progress indicators | 🟢 完成 | 🟢 完成 | || ||
| Radio button | 🟢 完成 | 🟢 完成 | || ||
| Reflected property editor | 🟠 已计划 | 🟡 部分 | || ||
| Scrollbar | 🟢 完成 | 🟢 完成 | || ||
| Slider | 🟢 完成 | 🟢 完成 | || ||
| Spinbox | 🟢 完成 | 🟢 完成 | || ||
| Styled dock | 🟢 完成 | 🟢 完成 | || ||
| Tab | 🟠 已计划 | 🟡 部分 | || ||
| Toggle switch | 🟢 完成 | 🟢 完成 | || ||
| Tree view | 🟢 完成 | 🟢 完成 | || ||
| Array | ❌ 未安排 | ❌ 无 | || ||
| Table view | 🟢 完成 | 🟢 完成 | || ||

### UX 模式 

| 模块 | 特性 | 正常使用 | 内容 | 代码/API | 性能 | 平台 |
| :-- | :--- | :--- | :--- | :--- | :--- | :--- |
| Component Card | 🟡 活跃 | 🟡 部分 | || ||
| Error handling | 🟠 已计划 | 🟠 最小 | || ||
| Hotkey management | 🔵 待办 | 🟠 最小 | || ||
| UI/UX Responsiveness standard | 🔵 待办 | ❌ 无 | || ||
| Viewport interaction | 🔵 待办 | 🔵 设计中 | || ||
