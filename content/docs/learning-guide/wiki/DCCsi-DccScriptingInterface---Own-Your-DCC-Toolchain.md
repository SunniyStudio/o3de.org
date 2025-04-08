---
title: "[DCCsi]-DccScriptingInterface---拥有您的 DCC 工具链"
description: ""
toc: false
---

本页概述了有关称为 DccScriptingInterface（又名 DCCsi）的 DCC 不可知 Python 框架（作为 Gem）的基本信息

DCCsi 位于类似于以下的位置：
* C:\Depot\o3de-engine\Gems\AtomLyIntegration\TechnicalArt\DccScriptingInterface

DCCsi 与 DCC 应用程序及其 Python API/SDK 的多个轻量级集成相关。

# 介绍

DCCsi 是一种独立于“数字内容创建”（DCC） 的解决方案，用于处理网格、材质和纹理等 O3DE Atom 资产。在 Atom 中，我们为技术艺术家提供了一个 Python 驱动的任意数据层，用于创建多点管道和工作流工具，这些工具在 O3DE 编辑器和多个 DCC 应用程序之间提供互作，以及用于独立或嵌入式工具和工具 （Qt\PySide2） 的框架。我们打算提供示例 DCC 模块模板，例如理论上是“Substance Builder”（用于材质的 API 和工具，它将使用 Substance Automation Toolkit 'SAT' python API 生成 Atom 材质）。

提供此框架可促进和协助为内容创建者创建自定义工具和管道流程。该框架旨在支持许多 DCC 工具及其源文件，如 Substance .sbsar（材质）和 Maya，用于网格/场景中间格式，如 FBX 和 glTF。

## 短
为什么命名为“DCC Scripting Interface”？ Atom 将他们的许多 API 称为“接口”，我们正在与 O3dE 中的其他计划合作，例如“Python 编辑器绑定”（编辑器脚本计划），以充实一些初始用例和原型设计，并使用 DDC 工具（如 Maya（用于建模）和 Substance（材质））和 Python 来构建成熟的材质创作工作流程的原型。所以 'DCC Tools' + 'Python Scripting' + 'Atom Interface'。

### 目标
我们打算以类似于以下的目标来开发 DCCsi：

* 面向技术美术师的开箱即用的共享开发环境，面向跨多个 DCC 工具使用 Python，并利用现有的 Python 生态系统进行技术艺术创作。
* 在 Python 驱动的 VFX 和游戏生态系统中使用 Blender、Maya 或 Substance 等 DCC 应用程序来“扩展 O3DE”，并为内容创作者和为他们服务的技术艺术家释放其潜力。
* 展示如何通过利用可扩展性构建自定义解决方案和工作流程来深化“DCC 工具集成”，例如“Substance Automation Toolkit”（用于处理 Substance 文件的基于 python 的 SDK 和 API）。
* 在整个工具链中对“PBR 渲染”和“端到端 Substance 工作流程”以及所见即所得的“同步外观开发”进行完整的材质和工作流程验证。

### 信条
* 互作性 - 设计模块以高效、直观地协同工作。
* 封装 - 根据模块的基本功能和与其他组件的接口来定义模块，以促进逻辑分层设计和易于维护。
* 可扩展性 - 将工具集设计为可通过新功能和新工具轻松扩展。各个部分应该有一个通用的通信机制，以允许新编写的工具干净、透明地插入到工具链中。

***
# 当前范围
下面指导了 DCCsi Cross-DCC Python 脚本接口的开发方向（还有更多的研究要做）
## 痛点
* 适用于 Maya 等工具的传统 Lumberyard DCC 插件和环境是全局安装和设置的，因此切换环境甚至分支或更改每个项目的配置会给开发人员带来很多麻烦。
* 使用 Maya.env 是一种过时的环境管理和引导方法。 此方法实际上仅适用于用户在其本地计算机上修改 Maya 环境（对于专业生产环境，有更好的方法可以实现）。
* 硬编码环境/安装和使用 maya.env 会使分支和项目跳转变得繁琐且容易出错，并且不会为 DCC 工具提供共享/可分发的项目配置。
* Lumberyard 很少关注“技术艺术”的开发和进步，不是由 Python 驱动的，无法轻松访问 IDE 环境，以便在开发过程中编写工具和调试
* Lumberyard 没有通用或共享的基于 python 的框架，无法降低自定义工作流或管道脚本和自动化的进入门槛
* 传统 Lumberyard DCC 内容工作需要插件 （用于 .cgf 导出），提供的工具是预编译的并且基于 C++，它们没有明确的所有者，它们难以维护，并且没有保持最新状态。更高版本的 Lumberyard 不再需要它们 （已移至 FBX 管道），但为 O3DE 管道和 Editor 开发工作室工具和扩展的能力对许多客户仍然很有价值。
* 在 O3DE 之前，Lumberyard 技术艺术家无法轻松维护、自定义或扩展自己的域 （他们受到 C++ 和工程支持的瓶颈），DCCsi 使 TechArt 和 python 社区能够加入开发工作并自定义或扩展 O3DE。
* 多年来，我们采访过的许多技术美术师都希望有同样的想法，我们有证据表明许多游戏团队利用 Python 生态系统构建了自己的工作室工具，我们希望通过提供起点和有用的框架来简化这项工作。

## 范围内 （短期）
* 我们认为 DCCsi 的现状是一个基本的脚手架和“功能原型”
* DCCsi 将对主机系统、全局变量和现有工具/环境配置保持非破坏性。
* 能够禁用和绕过所有 Amazon 和/或 O3DE 自定义，返回原版 DCC。
* 提供与 dcc 工具无关的多点核心框架（共享纯 Python API 并支持多个 dcc 应用程序，因此是多点的）
* 为服务和工具提供通用的分支和项目无关配置（数据驱动配置）
* 使开发人员可以轻松地在分支或项目之间跳转 - 例如，Maya 可以多次运行和引导，具有不同的基于项目的配置。
* 按项目轻松配置（从框架运行工具以配置默认引导配置和启动器等）
* 要分发的项目共享“DCC 配置”（在项目数据源控制中提交），确保每个人都在本地项目环境环境（提供项目数据驱动的引导）中运行相同的每个项目和每个 dcc 配置。
* 能够在 O3DE 项目（环境）的上下文中引导多个 DCC，现在：Maya、Blender 和 Substance（后来的其他：如 Houdini、3dsMax 等）
* 构建到功能齐全的开发环境和脚本 API，用于工具、工作流和管道开发。支持 IDE 以及项目文件和解决方案，以及沙盒等开发功能。
* Wing IDE 集成，并致力于提供额外的 IDE 支持（示例），以便在 PyCharm 和 VScode 中进行开发，具有自动连接和远程调试功能。
* 符合 Python 3.x 标准，以应对即将到来的 2.7 生命周期终止截止日期
* 目前仅限 Windows（但我们应该以跨平台的方式编写代码。
* 托管在 GitHub 上（始终能够拉取最新的发行版）并推广社区驱动的扩展（我们希望这点亮了可扩展性的先导灯）

## 超出范围（帮助我们构建未来）
* 操作系统无关。在 Maya 或其他 DCC 工具可以安装的任何地方运行（仅限现有的原型窗口，我们需要社区支持才能使作系统不可知）
* 通过添加单元测试和测试覆盖率实现稳定性 - 下一阶段的开发应该转向测试驱动。
* 代码运行状况监控器（在线仪表板、上传崩溃报告和堆栈转储、用于日志记录的 cloudhandler 等）
* API 和功能版本控制以及适当的分支工作流程，允许功能与生产代码不同。
* 更多开箱即用的开发人员环境设置和 IDE 集成：Eclipse（pydev） 或其他。
* 支持更多 DCC 工具：Houdini、3dsMax 等（根据社区的优先级和帮助，这些工具将很快推出，并根据需要提供）

***

（更多内容即将推出）

![DCCsi_Diagram](https://github.com/HogJonny-AMZN/O3DE/blob/main/Images/DCCsi/dcc_scripting_interface_diagram.png)

