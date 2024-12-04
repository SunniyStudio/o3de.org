---
linktitle: 教程
title: Open 3D Engine (O3DE) 教程
description: 在分步指导教程的帮助下学习 Open 3D Engine （O3DE）。
weight: 200
toc: true
---

在分步指导教程的帮助下，了解 **Open 3D Engine （O3DE）** 的功能、工具和流程。

## 想要开始？

您是否已安装 O3DE，但不确定从哪里开始？然后 [尝试后续操作](/docs/learning-guide/samples/follow-along/)。这些简单的指南视频将引导您完成 Pong 等基本游戏的创建，让您熟悉 O3DE 界面和工具。

## 动画

|教程 |描述 |
| - | - |
| [数据驱动的根运动](animation/data-driven-root-motion) |了解数据驱动的根运动以及如何将其应用于角色。 |

## 人工智能

|教程 |描述 |
| - | - |
| [使用 Kythera AI 进行 2D 导航](ai/navigation-2d) | 创建一个人工智能 （AI） 代理，该代理执行基于地面的导航并遵循可视化脚本化的行为树。本教程使用 [Kythera AI Gem](/docs/user-guide/gems/reference/kythera-ai) 提供的广泛工具集。 |

## 资产

|教程 |描述 |
| - | - |
| [自定义网格资产处理](assets/mesh-assets) | 使用 Scene Settings （场景设置） 处理 O3DE 的网格资产。 |
| [自定义 Actor 资产处理](assets/actor-assets) | 使用 Scene Settings （场景设置） 处理 O3DE 的角色资产。 |
| [处理 PhysX 碰撞器资产](assets/physx-colliders) | 使用 Scene Settings （场景设置） 处理 O3DE 的 PhysX 碰撞器资产。 |

## 实体和Prefab

|教程 |描述 |
| - | - |
| [实体和Prefab基础知识](entities-and-prefabs/entity-and-prefab-basics) | 了解创建和修改实体和Prefab的基础知识。 |
| [覆盖Prefab](entities-and-prefabs/override-a-prefab) | 了解如何更改单个Prefab实例。 |
| [生成和取消生成Prefab](entities-and-prefabs/spawn-a-prefab.md) | 使用 **Script Canvas** 创建生成和取消生成Prefab的脚本。 |

## 环境

|教程 |描述 |
| - | - |
| [创建关卡](environments/create-a-level) | 为 O3DE 创建关卡。|
| [从图像创建地形](environments/create-terrain-from-images) | 学习使用图像创建地形。 |

## 扩展 O3DE 编辑器

通过创建自定义工具 Gem 来扩展 **O3DE Editor**。工具是在 Editor 中实现功能的可停靠构件或对话框窗口。您可以在 C++ 或 Python 中创建自定义工具。

|教程 |描述 |
| - | - |
| [在 C++ 中创建自定义工具 Gem](extend-the-editor/shape-example-cpp.md) | 通过创建用 C++ 编写的自定义工具 Gem 来扩展 Editor。了解如何使用 **CppToolGem** 模板，并使用 [Qt](https://wiki.qt.io/Main)、O3DE 工具 UI API 和其他 O3DE API 练习C++开发。 |
| [在 Python 中创建自定义工具 Gem](extend-the-editor/shape-example-py.md) | 通过创建用 Python 编写的自定义工具 Gem 来扩展 Editor。了解如何使用 **PythonToolGem** 模板，并使用 [Qt](https://wiki.qt.io/Main)、O3DE 工具 UI API 和其他 O3DE API 练习 Python 开发。|

## 输入和移动

了解如何使用键盘、鼠标和其他输入设备移动实体。

|教程 |描述 |
| - | - |
| [基于网格的移动](input-and-movement/grid-based-movement) | 了解如何从输入设备事件实现基于网格的移动。 |

## 多人游戏

|教程 |描述 |
| - | - |
| [您的第一个网络组件](multiplayer/first-multiplayer-component) | 在网络组件简介中使用 C++ 创建多人游戏组件。 |

## PhysX

使用 NVIDIA 的 PhysX 系统在 O3DE 中创建物理模拟。本节中的教程演示了如何使用 PhysX 在项目中添加动态物理模拟。

|主题 |描述 |
| - | - |
| [创建风力](physx/wind-provider) | 使用 **PhysX Force Region** 和 NVIDIA Cloth 模拟风力。 |

## PostFX

|教程 |描述 |
| - | - |
| [PostFX Shape Weight Modifier](postfx/use-postfx-shape-weight-modifier) | 使用 **PostFX Shape Weight Modifier** 组件修改 O3DE 中的曝光控制。此示例演示如何在运行时修改后处理效果 （PostFX）。 |

## 远程仓库

|教程 |描述 |
| - | - |
| [您的第一个远程仓库](remote-repositories/create-remote-repository) | 了解如何创建 **O3DE 远程存储库** 以与全世界共享您的项目、Gem 和模板。 |

## 渲染

|教程 |描述 |
| - | - |
| [创建 StandardPBR 材质](rendering/create-standardpbr-material) | 本教程将指导您使用 Material Editor 创建您的第一个 StandardPBR 材质。 |
| [材质类型和着色器](rendering/get-started-materialtypes-and-shaders) | 介绍 Atom 中的材质类型和着色器的初级教程。了解如何使用简单的 AZSL 着色器创建自定义材质类型。 |
| [植被弯曲的顶点变形教程](rendering/vegetation-bending-tutorial) | 使用 **Atom Renderer** 创建具有自定义顶点着色器的自定义材质类型，以实现植被弯曲。 |
