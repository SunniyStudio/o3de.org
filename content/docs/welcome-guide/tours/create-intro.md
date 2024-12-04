---
linkTitle: 使用 O3DE 创建
title: 使用 Open 3D Engine 进行创作
description: 了解技术艺术家、设计师或程序员可以使用 Open 3D Engine 进行创作的不同方式。
weight: 200
toc: true
---

在本指南的前面，我们介绍了 [关键概念](../key-concepts) 以及如何 [设置 **Open 3D Engine （O3DE）**](../setup) 的 Ban S T本主题概述了如何使用 O3DE 进行创作，首先介绍所有创作者都应牢记的一些概念，然后重点介绍不同创意角色感兴趣的领域。

## 模块化工具和功能

如果您在学习时牢记一个关键的 O3DE 设计理念，那么最好理解 O3DE 中的开发：*模块化*。构成 O3DE 的软件包、许多创意工具，甚至 O3DE 中的可视化脚本系统都使用模块化概念为您的创意愿景提供构建块。

### Gems，O3DE 的模块

O3DE、其系统及其环境构建为称为 *Gem*的 C++ 模块集合。Gem 是提供工具和功能的打包代码和资产。例如，Atom Gem 是一个包含多个较小 Gem 的 Gem，这些 Gem 提供 Atom 库、接口和工具，例如 **Material Editor**。Atom Gem 提供了实时渲染您的创作所需的一切。O3DE 的 Gem 几乎可以包含任何内容，从用于游戏状态和模拟的运行时系统、创意工具、用于分析、调试和项目管理的实用程序，甚至是模型、动画和纹理等资产。

根据项目设计和工作流程的组合选择合适的 Gem 将使您的开发过程保持专注，同样可以简化您的项目管理。

O3DE 的核心创意工具 **O3DE 编辑器** 的所有功能均由 Gems 提供。O3DE 在安装过程中附带了数十个 Gem，您可以从第三方获取其他 Gem，也可以自己编写新 Gem。在构建项目时，您添加的 Gem 的功能将组合在一起，以创建项目的运行时系统。您可以获取或创建仅包含所需功能的 Gem - 甚至可以独立于您的项目维护其代码！这意味着，例如，系统开发人员可以在 Gem 中开发 AI 功能，并将他们的 AI Gem 提供给创意团队。更新 AI Gem 后，设计人员可以在其项目中添加或更新 Gem，将中断降至最低，并立即使用 AI Gem 提供的新功能。

采用模块化的概念并创建自己的 Gem 有很多好处。通过创建自己的 Gem，您可以轻松地在项目之间重复使用功能和资源，与其他创建者共享您的作品，甚至允许其他贡献者扩展和改进 Gem 的功能。

### 组件、实体的模块

您会注意到这种模块化理念贯穿于 O3DE 的整个工具和功能中。例如，考虑 O3DE 中的 *实体* 概念。实体可以在 O3DE 的组件实体系统下表示项目中的几乎任何内容。通过提供实体组件，您可以开始塑造实体的功能和效用。这些组件是指定实体行为和属性的模块。

您可能有一个想要查看并与之交互的实体，该实体使用以下所有组件：

* 用于定义其位置的默认 **Transform** 组件
* 一个 **Mesh** 组件，用于定义其视觉几何体
* **NVIDIA PhysX** 组件，用于定义碰撞特性和其他方面，以实现逼真的刚体模拟
* 一个 **Input** 组件，用于引用输入事件绑定定义
* 一个 **Script** 组件，用于自动化某种行为，或处理来自播放器的输入事件

...还有很多！

您创建的其他实体将不可见，并提供运行时实用程序和功能。这些实体的存在可能是为了实现触发器、生成环境效果或引用使用工具创建的资产，例如您在 UI 编辑器中创建的 UI。

## 使用 O3DE 编辑器

O3DE Editor 是 O3DE 创意工具的中心枢纽。在 O3DE 中组装项目的主要部分围绕使用 O3DE 编辑器执行以下操作：

* 放置和分组实体
* 将组件添加到实体
* 配置组件的属性
* 使用与组件关联的工具

您可能使用的与特定组件关联的工具包括：

* **Script Canvas**，O3DE 用于创建脚本的可视化编辑器，然后从 **Script Canvas** 组件引用脚本
* **Animation Editor**为角色添加动画效果，然后从 **Anim Graph** 组件中引用
* **Asset Editor**创建将原始玩家输入（如击键）绑定到事件的输入绑定，然后从 **Input** 组件引用
* **Audio Controls Editor**设置映射到音频引擎控件的声音效果，然后从 **Audio Trigger** 和 **Audio Switch** 组件中引用

...以及更多。

某些工具可以直接从其关联的组件中打开。其他选项要求您从 O3DE Editor 的 **工具** 菜单中打开它们。O3DE 的模块化特性意味着您可以通过在 Project Manager 中启用 Gem 来添加其他资产、组件和工具。O3DE 附带一个 Gem 库，其中可以包含新代码和/或新资产！查看 Project Manager 以查看 O3DE 中可用的 Gem 的完整列表。

您将在下一个主题 [O3DE Editor 导览](./editor-tour) 中获得 O3DE Editor 的快速概述和导航教程。

## 开发角色

根据您作为艺术家、设计师、工程师的角色以及您所从事的项目的范围，您可能无法遇到 O3DE 提供的所有工具和技术。在下一节中，我们将根据您的角色，了解您可能希望关注哪些工具和功能。

### 艺术家

虽然作为艺术家，您的大部分工作可能涉及使用 O3DE 之外的工具，但 O3DE 提供的工具将您的所有资产整合到世界和参与者中，供人们可视化、交互和体验。您可能会与设计师密切合作，或者自己兼任设计师。艺术家和设计师使用的工具和功能之间存在一些重叠。

例如，让我们考虑在项目中设置 actor 的工作流程。

1. 在 O3DE 之外，您可以为角色创建角色模型、材质、纹理和装备。

1. O3DE 的 **Asset Processor** 会自动将源资产处理为特定于平台的运行时资产，确保新的或修改的文件尽快在 O3DE 中准备好使用。如果需要，您可以使用 **Scene Settings** 来修改 Asset Processor 生成运行时资源的方式。

1. 在 Animation Editor 中导入角色文件并创建运动集，以指定角色所需的运动。

1. 接下来，使用节点 （包括混合树、事件和状态） 创建动画图形，以便为角色创建复杂的动画行为。

1. 当您构建并预览了动画并准备好在运行时环境中试用它们时，您可以切换到 O3DE 编辑器。

1. 在 O3DE 编辑器中，创建或打开现有测试关卡。

1. 要查看动画角色，您需要一个具有以下特点的实体：

    * 一个 **Actor** 组件，用于使用 Animation Editor 中的角色文件和链接到角色资源的材质创建可控制角色。
    * 一个 **AnimGraph** 组件，用于使用您在 Animation Editor 中创建的动画图形和运动集资源。

1.  要控制关卡中的角色，您可能需要与设计师或程序员合作，添加 **Input** 组件、**PhysX** 组件和脚本组件，以便您可以在项目的特定环境中测试角色的所有动画。

我们建议您通过浏览以下 O3DE 工具和技术集来开始学习路径，然后专注于适用于您需求的工具和技术：

*  [O3DE 编辑器](/docs/user-guide/editor/)
*  [资产管道](/docs/user-guide/assets/pipeline/)
*  [组件参考](/docs/user-guide/components/reference/)
*  [Animation Editor](/docs/user-guide/visualization/animation/animation-editor/)
*  [Scene Settings 工具](/docs/user-guide/assets/scene-settings/)
*  [Gem 库](/docs/user-guide/gems/)
*  [过场动画和 Track View 编辑器](/docs/user-guide/visualization/cinematics/)
*  [着色器和材质](/docs/atom-guide/look-dev/)


### 设计师

O3DE Editor 是设计人员的重要核心工具。在这里，您可以创建关卡，使用实体填充关卡，并将组件分配给这些实体。它还提供对重要工具的访问，例如，UI 编辑器（用于 UI 设计人员）、音频控件编辑器（用于声音设计师）和 Script Canvas（用于所有将在 O3DE 中使用可视化脚本系统的设计人员）。

以下是您可能如何开始：

1. 首次启动 O3DE Editor 时，您可以创建一个新关卡。

1. 通过创建实体，开始在视区中填充关卡。在 O3DE 中，实体几乎可以是任何东西，从您看到的静态对象到您编写脚本的触发器，再到交互式对象和用户界面。

1. 您可以通过 Entity Inspector 工具将组件添加到实体中。例如，您可以添加 **White Box** 组件，以快速绘制 3D 代理几何体，以设计关卡的对象和障碍物。

1. 要为您的实体提供玩家控制，您需要一个输入绑定。使用 **Asset Editor** 工具，您可以将来自键盘、鼠标和游戏控制器的原始玩家输入绑定到您创建的事件。然后，您可以使用其中一个脚本工具侦听和响应这些事件。

1. Script Canvas 和 Lua 是 O3DE 中常用的脚本工具。Script Canvas 提供了一个基于节点的可视化脚本系统，用于编写运行时逻辑的脚本，而 Lua 提供了一个基于 Lua API 的更传统的脚本环境。将 **Script Canvas** 组件或 **Lua Script** 组件添加到实体。在其中一个脚本编辑器中开发脚本。然后，您可以将该脚本添加到组件中，以便在运行时控制该实体。脚本可以通过响应玩家输入事件来控制实体，在运行时生成动态实体，触发音频和视觉效果等等。

1. 当您使用实体填充您的世界时，您将了解到节省时间的一个好方法是使用Prefab系统。Prefab允许您将组件实体分组和嵌套在一起，将组另存为Prefab，然后在整个项目级别中创建该Prefab的多个实例。在Prefab的每个实例中，您都可以对该特定实例进行更改。

1. 当您准备好编写 UI 时，您可以使用 UI 编辑器，您可以在其中建立 UI 画布和布局以及编写界面脚本。

1. 当您准备好向项目添加声音时，您可以在 **Audio Controls Editor** 中建立音频事件和触发器。然后，您可以通过组件将此音频添加到实体中，并使用其中一个脚本系统编写其播放脚本。

我们建议您通过浏览以下 O3DE 工具和技术集来开始学习路径，然后专注于适用于您需求的工具和技术：

*  [O3DE 编辑器](/docs/user-guide/editor/)
*  [资产管道](/docs/user-guide/assets/pipeline/)
*  [组件参考](/docs/user-guide/components/reference/)
*  [Gem 库](/docs/user-guide/gems/)
*  [玩家输入 / Asset Editor](/docs/user-guide/interactivity/input/using-player-input/)
*  [Script Canvas](/docs/user-guide/scripting/script-canvas/)
*  [Lua Editor](/docs/user-guide/scripting/lua/)
*  [Audio Controls Editor](/docs/user-guide/interactivity/audio/)
*  [Animation Editor](/docs/user-guide/visualization/animation/animation-editor/)
*  [UI Editor](/docs/user-guide/interactivity/user-interface/editor/)

### 工程师

作为工程师，您可能需要学习如何为设计人员和艺术家提供支持，以及如何编写单个组件。然后，可以将这些新组件添加到 O3DE 编辑器中的实体中，以添加新功能。您可能还会学习如何开发新的 Script Canvas 节点。然后，设计人员可以在 **Script Canvas 编辑器** 中使用这些新节点来处理您创建的新事件，或更改新组件的属性。在更大范围内，当您需要在可以作为代码和资产的可共享容器分发的系统上工作时，您可以学习如何创建 Gem。

我们建议您的学习路径如下所示：

1. 按照介绍教程了解组件实体系统和现有组件库。

1. 浏览 Gem 库，查看与单个组件相比，他们可以添加的更大功能的示例。

1. 了解如何编写自己的组件和 Gem，您还将了解如何使用事件总线、O3DE 的事件总线和通用消息传递系统;AZ 模块，作为静态或动态库构建的 C++ 代码集合，等等。

以下是一组需要关注的基本 O3DE 工具和技术：
*  [O3DE 编辑器](/docs/user-guide/editor/)
*  [编程指南](/docs/user-guide/programming/)
*  [Gems](/docs/user-guide/gems/)
*  [组件参考](/docs/user-guide/components/reference/)
*  [Animation Editor](/docs/user-guide/visualization/animation/animation-editor/)
*  [Script Canvas](/docs/user-guide/scripting/script-canvas/)
*  [Lua Editor](/docs/user-guide/scripting/lua/)
