---
linkTitle: 术语
title: O3DE文档术语和用法
description: Open 3D Engine （O3DE） 特定技术术语的使用规则。
toc: true
weight: 550
---

**Open 3D Engine （O3DE）** 有许多独特而专业的术语。本主题介绍这些术语以及文档贡献者应如何使用它们。

## 通用术语指南

* 始终将工具、Gem 和组件的正确名称大写。
* 使用 **粗体** 文本作为工具、Gem 或组件专有名称的第一个页面引用。
* 对工具、Gem 或组件专有名称的后续引用使用常规文本。
* 以扩展形式引入首字母缩略词，后跟括号中的首字母缩略词。例如，“console variable （cvar）”。
* 使用首字母缩略词进行后续的页面引用。
* 使用 O3DE 术语时保持一致。例如，使用 “level” 而不是 “scene”。

## O3DE 特定术语

下表定义了 O3DE 特定术语，并为文档贡献者提供了指导。在整个 O3DE 文档中一致使用这些术语和样式。

|术语 |变化 |解释和用法 |
|-|-|-|
| actor | *\<none\>* | 具有 **Actor** 组件的专用类型的实体。Actor 可以使用关键帧动画和/或能够接收输入。一般而言，在引用参与者实体类型时，请勿将此术语大写。引用命名组件时，请将术语 “Actor” 大写，并在第一个页面上引用时使用粗体文本。 |
| AMD TressFX | *\<none\>* | 用于创建模拟逼真的头发和毛发的一组工具和 SDK。AMD TressFX 是产品名称和商标。“AMD”一词的每个字母都必须大写，“TressFX”中的字母“T”、“F”和“X”必须大写。在提及 AMD 和 GPUOpen 项目提供的产品和相关技术时，请使用完整术语“AMD TressFX”。 |
| **Animation Editor** | *\<none\>* | 用于为 actor 实体创建 **Anim Graphs**、专用碰撞和物理模拟的工具。始终将 “Animation” 和 “Editor” 都大写，因为这是一个工具名称和专有名词。对第一个页面引用使用粗体文本。 |
| **Asset Browser** | *\<none\>* | 用于浏览 Asset Processor 监控的目录中的 O3DE 资产的工具。始终将 “Asset” 和 “Browser” 都大写，因为这是工具名称和专有名词。对第一个页面引用使用粗体文本。 |
| **Asset Editor** | *\<none\>* | 用于创建 O3DE 特定资产的工具，例如 `.physicsmaterials` and `.inputbingings`。始终将 “Asset” 和 “Editor” 都大写，因为这是工具名称和专有名词。对第一个页面引用使用粗体文本。 |
| **Asset Processor** | *\<none\>* | 一种可自动监控源资产并将其从各种格式转换为运行时资产的工具。始终将“Asset”和“Processor”都大写，因为这是工具名称和专有名词。对第一个页面引用使用粗体文本。 |
| **Anim Graph** | *\<none\>* | **Animation Editor** 中角色的动画行为的交互式图形。这是 “animation” 和 “graph” 的复合体。始终将 “Anim” 和 “Graph” 大写，因为它是工具名称和专有名词。对第一个页面引用使用粗体文本。 |
| animation graph | anim graph| Anim Graph 工具的产品资产 (`.animgraph`);一个流图，用于根据输入和其他事件切换和混合动画状态。在此上下文中，请勿将术语 “animation graph” 或 “anim graph” 大写。 |
| **Atom Renderer** | Atom | O3DE 的渲染器。始终将 “Atom” 和 “Renderer” 都大写，因为它是专有名词。对第一个页面引用使用粗体文本中的 “**Atom Renderer**”。在常规文本中使用 “Atom” 作为后续引用。 |
| canvas | | 用于脚本化逻辑、组件或 UI 小部件布局的容器和虚拟表面。画布由 **Script Canvas**、**Landscape Canvas**、**UI Editor** 和 O3DE 中的许多其他工具使用。由于这是一个重载的术语，因此在引用文件或资源时，应按画布的类型或扩展名引用画布。仅在上下文明显的教程中使用通用术语“canvas”，以引用放置节点的虚拟画布表面。在这种情况下，不要将术语 “canvas” 大写。 |
| component | *\<none\>* | 可以添加到实体以提供特定功能的元素。不要将术语 “component” 大写。将术语“组件”的使用限制在组件实体系统的上下文中。 |
| Component Entity System | CES | 一种用于创建具有行为的复杂对象的模型，其中提供特定功能的组件被添加到可重用的实体中。在提及 O3DE 对这一概念的实施时，请始终将“Component”、“Entity”和“System”以及首字母缩略词“CES”中的每个字母大写。使用粗体的“**组件实体系统 （CES）**”作为第一个页面引用。在常规文本中使用 “CES” 以供后续引用。 |
| **Console** | *\<none\>* | 用于设置控制台变量、输入命令和执行脚本的工具。当提到 O3DE 编辑器控制台时，请始终将“控制台”大写，因为这是一个工具名称和专有名词。对第一个页面引用使用粗体文本。 |
| **Console Variables** | *\<none\>* | 用于全局设置和管理控制台变量 （cvar） 的工具。按名称引用工具时，请始终将 “Console” 和 “Variables” 两样大写。这是一个专有名词。对第一个页面引用使用粗体文本。一般而言，在谈论 “console variables” 时，不要将术语大写。|
| console variable | cvar | 为 O3DE 环境和运行时可执行文件设置状态的变量的通用术语。控制台变量 （cvar） 可以指定渲染设置、设置路径、启用调试输出等。请勿将此术语大写。在引用一般控制台变量 （cvar） 或引用特定控制台变量 （`sys_maxfps` cvar） 时，以小写常规文本形式使用此首字母缩略词。 |
| **Event Bus** | EBus | O3DE 的系统，用于在组件和系统之间调度和处理消息。始终将 “Event” 和 “Bus” 都大写。在事件总线中，“E”和“B”始终大写。使用粗体的 “**Event Bus （Ebus）**” 作为第一个页面引用。在常规文本中使用 “EBus” 以供后续引用。使用 “EBuses” 表示复数形式。 |
| entity | *\<none\>* | 表示某个功能对象的组件集合。请勿将此术语大写。将术语“entity”的使用限制在组件实体系统的上下文中。 |
| **Entity Inspector** | *\<none\>* | 用于编辑实体的组件和属性的工具。始终将 “Entity” 和 “Inspector” 都大写，因为这是工具名称和专有名词。对第一个页面引用使用粗体文本。 |
| **Entity Outliner** | *\<none\>* | 用于组织和查找关卡中包含的实体的工具。始终将 “Entity” 和 “Outliner” 都大写，因为这是一个工具名称和专有名词。对第一个页面引用使用粗体文本。|
| Gem | *\<none\>* |  包含特定于 O3DE 的源代码和/或资产的包。“Gem” 是专有名词，必须大写。|
| host platform | *\<none\>* | 编译项目可执行文件的平台。在可能的情况下，在一般情况下引用用户开发平台时，请使用此术语，而不是引用特定的操作系统。例如，在 Windows 环境中，您的主机平台使用 Visual Studio 进行项目，并使用 x64 工具链来构建 O3DE。 |
| level | *\<none\>* | 一个包含实体、脚本、资源等的容器，表示项目的可流式传输、可播放部分。“关卡”不一定包含地形。请勿将此术语大写。根据使用其他内容创建应用程序的经验，贡献者可能倾向于将术语“scene”与“level”互换使用。仅使用术语 “level” 保持一致性。 |
| **Level Inspector** | *\<none\>* | 用于编辑关卡的组件和属性的工具。始终将 “Level” 和 “Inspector” 都大写，因为这是一个工具名称，也是一个专有名词。对第一个页面引用使用粗体文本。 |
| **Lua Editor** | *\<none\>* | Lua 脚本的编辑器。始终将 “Lua” 和 “Editor” 都大写，因为这是一个工具名称，也是一个专有名词。对第一个页面引用使用粗体文本。  |
| **Material Editor** | *\<none\>* | 用于创建材质并设置其属性的工具。始终将 “Material” 和 “Editor” 都大写，因为这是一个工具名称，也是一个专有名词。对第一个页面引用使用粗体文本。 |
| Module | *\<none\>* | Gem 的代码组件，引用最终编译的库或定义此库入口点的`Az::Module`类。如果存在混淆，请使用 Module 或 “Module library” 来引用库工件，使用 “Module instance” 或 “Module implementation” 来引用类。“Module” 是 Gem 系统上下文中的专有名词，必须大写。 |
| Node | *\<none\>* | 脚本元素 （如变量、常量或 API 函数） 的可视化表示形式。节点放置在画布中并连接到网络中，以便为实体创建脚本化功能。在引用特定节点或其实现时，请将此术语大写。当您引用节点的一般概念时，或在更高抽象级别上编写节点时，请使用小写术语 “node”。 |
| NVIDIA Blast | *\<none\>* | 用于创建模拟销毁的一组工具和 SDK。NVIDIA Blast 是产品名称和商标。“NVIDIA”一词的每个字母都必须大写，“Blast”一词必须大写。在提及 NVIDIA 提供的产品和相关技术时，请使用完整术语“NVIDIA Blast”。 |
| NVIDIA Cloth | *\<none\>* | 用于创建模拟织物和布料的一组工具和 SDK。NVIDIA Cloth 是产品名称和商标。单词“NVIDIA”的每个字母都必须大写，单词“Cloth”必须大写。在提及 NVIDIA 提供的产品和相关技术时，请使用完整术语“NVIDIA Cloth”。 |
| NVIDIA PhysX | *\<none\>* | 用于创建物理模拟的一组工具和 SDK。NVIDIA PhysX 是产品名称和商标。单词“NVIDIA”的每个字母都必须大写，单词“PhysX”中的字母“P”和字母“X”必须大写。在提及 NVIDIA 提供的产品和相关技术时，请使用完整术语“NVIDIA PhysX”。 |
| **O3DE CLI** | `o3de` Python script | 一个 Python 脚本，用于使用命令行界面 （CLI） 进行项目配置。始终将 “O3DE” 和 “CLI” 大写，因为它们是首字母缩略词。使用粗体的 “**O3DE CLI**” 或粗体的 “**O3DE CLI** （"`o3de`）”，并将 Python 脚本的名称格式化为内联代码，以供第一个页面上引用。如果您使用后者，请使用 “"`o3de` Python script” 作为后续参考。否则，请使用不带粗体的 “O3DE CLI”。 |
| **O3DE Editor** | Editor | O3DE 的内容创建环境。始终将 “O3DE” 和 “Editor” 都大写，因为这是工具名称和专有名词。使用粗体的 “**O3DE Editor**” 作为第一个页面引用。在常规文本中使用 “Editor” 以供后续引用。 |
| **Object Tree** | *\<none\>* | 活动 UI 对象的查看器。始终将 “Object” 和 “Tree” 都大写，因为这是一个工具名称和专有名词。对第一个页面引用使用粗体文本。 |
| Open 3D Engine | O3DE | 始终将 “O”、“D” 和 “E” 大写。使用粗体的 “Open 3D Engine （O3DE）” 作为第一个页面参考。在常规文本中使用 “O3DE” 以供后续引用。|
| project | *\<none\>* | 所有文件、首选项、资源、级别、脚本、代码等的命名容器和目录层次结构，这些文件将编译为目标平台的运行时可执行文件，或打包为 Gem。请勿将此术语大写。使用术语 “project” 而不是 “game”。|
| **Python Console** | *\<none\>* | 用于在 O3DE 编辑器中运行 Python 脚本的工具。始终将 “Python” 和 “Console” 都大写，因为这是一个工具名称，也是一个专有名词。对第一个页面引用使用粗体文本。 |
| **Python Scripts** | *\<none\>* | 用于 Python 脚本文件的浏览器应用程序。在提及工具时，请始终将 “Python” 和 “Scripts” 都大写，因为这是工具名称，也是专有名词。对第一个页面引用使用粗体文本。在一般情况下引用 Python 脚本文件时，请使用 “Python scripts” - 不要将 “Python” 加粗，也不要将 “scripts” 大写或加粗。 |
| prefab | *\<none\>* | 存储在可重用资产中的已配置实体的集合。Prefab使用“.prefab”文件扩展名。请勿将此术语大写。|
| **Script Canvas Editor** | *\<none\>* | 通用的可视化脚本工具。引用该工具时，请始终将 “Script Canvas” 和 “Editor” 都大写，因为这是工具名称和专有名词。对第一个页面引用使用粗体文本。 |
| spawnable | *\<none\>* | Prefab的运行时表示形式，仅包含运行时组件。可生成对象使用“.spawnable”文件扩展名。请勿将此术语大写。  |
| target platform | *\<none\>* | 运行已编译项目可执行文件的平台。与特定平台（如 Windows PC 或移动设备）相比，更喜欢使用此术语。例如，如果您的 O3DE 项目是为 Linux 构建的，即使在交叉编译器环境中也是如此，它将使用 Clang 编译器工具生成elf_x64二进制文件。 |
| **Track View** | *\<none\>* | O3DE 的电影编辑器。始终将 “Track” 和 “View” 都大写，因为这是一个工具名称，也是一个专有名词。对第一个页面引用使用粗体文本。 |
| viewport | *\<none\>* | O3DE 编辑器中的关卡视图。 |
| **UI Editor** | *\<none\>* | 用于构造运行时用户界面的工具。始终将 “UI” 和 “Editor” 都大写，因为这是工具名称和专有名词。对第一个页面引用使用粗体文本。 |
| **Viewport Camera Selector** | *\<none\>* | 用于选择活动 Perspective 摄像机的工具。始终将 “Viewport”、“Camera” 和 “Selector” 大写，因为这是工具名称和专有名词。对第一个页面引用使用粗体文本。 |

## 标准领域和行业术语

使用标准和通用的领域和行业术语。如果存在歧义，请使用上下文来确保读者理解其含义。

## 应避免的术语及其替代方案

* **master** -- 使用 “main”、“controller” 或其他表示对象的主要角色或行为的术语。
* **slave** -- 使用 “subordinate”、“secondary”、“ancillary” 或其他表示对象从属行为的术语。
* **whitelist** -- 使用 “allow list”、“approved list”、“include list” 或其他表示对象包容性范围的术语/短语。
* **blacklist** -- 使用 “allow list”、“approved list”、“exclude list” 或其他表示对象独占范围的术语/短语。
