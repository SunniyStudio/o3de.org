---
linktitle: 执行类
title: Script Canvas 执行类
description: 了解如何使用 Script Canvas 执行类将 Script Canvas 功能嵌入到 Open 3D Engine （O3DE） 中。
weight: 300
---

Script Canvas 提供了一套用于执行编译图形的类，目的是为图形的执行引入尽可能少的执行开销。图形的执行特征是在编译时确定的，并且可能因图形而异。管理其执行的变体封装在几个类中，以便嵌入式程序员能够轻松执行和管理图形的生命周期，从而减少或消除堆分配和其他可能的性能成本。


![Script Canvas Execution Classes Diagram](/images/user-guide/scripting/script-canvas/script-canvas-execution-classes-uml.png)

## ExecutionState

“`ExecutionState`”是一个抽象类，它为编译的 Script Canvas 图形的实际运行时执行提供接口。编译后的图形资产将存储 trait，这些 trait 确定需要 '`ExecutionState`' 的哪个子类来执行它。图形是否定义了纯功能（它只定义了仅对其输入进行操作的方法），或者定义了在初始函数执行之后将持续的状态，由 '`ExecutionStateStorage`' 读取，并创建相应的 '`ExecutionState`' 子类。

要执行编译后的图形，必须使用有效的 '`RuntimeData`'、'`RuntimeDataOverrides`' 和可选的 '`ExecutionUserData`' 创建一个 '`ExecutionState`' 对象。构造完成后，用户必须在调用 '`Execute（）`' 之前调用 '`Initialize（）`'。最后，如果需要，用户可以在销毁 '`ExecutionState`' 之前调用 '`StopExecution（）`' 以立即停止执行所需的图形。并非所有这些方法都会对所有图形产生任何影响。例如，如果图形是 “On Graph Start” 节点之后的简单 “`Print`” 节点，则 '`Initialize（）`' 和 '`StopExecution（）`' 将是空操作。

由于 '`ExecutionState`' 希望在提供给它的良好运行时数据上运行，并且没有开销，因此它不会对不良数据执行安全检查。作为一个接近纯接口的抽象类，'`ExecutionState`' 对象引用但尽可能少地拥有它需要用户开始和维持执行的任何输入。

如果开发人员希望将 Script Canvas 运行时功能嵌入到引擎中当前不存在的某个位置，则只需使用正确配置的“`ExecutionState`”对象即可。他们可以按照自己的意愿完成此操作，但 O3DE 提供了一些便利类（考虑到性能）来简化工作。所有这些方便的类都由 Script Canvas“`RuntimeComponent`”和 Script Canvas“`Interpreter`”使用。'`RuntimeComponent`' 是在对提供正确的运行时数据进行非常严格的安全检查后运行的执行示例。“解释器”是对有效运行时数据进行主动、实时、安全检查的一个示例，Script Canvas 作者可能正在快速迭代这些数据。

以下是用于嵌入 '`ExecutionState`' 的便捷类的描述。

## ExecutionStateStorage

'`ExecutionStateStorage`' 提供了足够的静态存储空间，允许构造 '`ExecutionState`' 的任何子类，而无需额外的堆分配。它被设计为与 '`ExecutionStateHandler`' 一起使用。

## ExecutionStateHandler

“`ExecutionStateHandler`”为该存储拥有的“`ExecutionStateStorage`”和“`ExecutionState`”提供 RAII 语义。如果开发人员以尝试执行无效的 ScriptCanvas 运行时数据的方式将其嵌入 O3DE 中，则它还为违反安全检查的行为提供错误报告度量。在非发布版本中，这是通过预处理器宏 '`SC_RUNTIME_CHECKS_ENABLED`' 启用的。此宏启用基本的、最少的错误检查和早期返回，同时最大限度地减少运行时的分支。如果禁用宏，则错误将写入断言（如果在构建中启用了断言），并且分支为零。

## Executor

'`Executor`' 类托管运行 '`ExecutionState`' 对象所需的所有数据的存储。它还提供了在执行时进行安全检查的基本方法，允许开发人员在尝试执行之前检查它拥有的 '`ExecutionState`' 是否可执行。这是 '`RuntimeComponent`' 和 '`Interpreter`' 用来运行 Script Canvas 图形的类。
