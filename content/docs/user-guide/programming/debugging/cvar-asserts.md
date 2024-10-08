---
title: 使用 sys_asserts 控制台变量 (CVAR)
linktitle: CVAR 断言
description: 了解如何在 Open 3D Engine (O3DE) 中使用断言来调试游戏代码。
---

使用 `sys_asserts` [控制台变量 (CVAR)](/docs/user-guide/appendix/cvars/)在 **Open 3D Engine (O3DE)** 中管理断言通知。下表列出了可能的值及其含义。

| 值 | 说明 |
| --- | --- |
| 0 | 忽略断言条件检查。不评估断言表达式。在所有这些值中，该选项的性能最好。 |
| 1 | 如果断言和断言调用堆栈可用，它们将被记录并打印到控制台或终端。这是默认值。 |
| 2 | 如果断言和断言调用堆栈可用，它们将被记录并打印到控制台或终端。该值会显示一个对话框，其中包含忽略当前断言、忽略所有断言或断开断言的选项。 |

## 示例输出

`sys_asserts=1` 会产生类似下面的输出结果：

```
(System) - Trace::Assert
<path>\Particle.cpp(1289): (68792) 'void __cdecl CParticle::Update(const struct SParticleUpdateContext &,float,bool)'
(System) - <path>\particle.cpp (1290) : CParticle::Update
(System) - <path>\particlecontainer.cpp (777) : CParticleContainer::UpdateParticleStates
(System) - <path>\particlecontainer.cpp (731) : CParticleContainer::UpdateParticles
(System) - <path>\particleemitter.cpp (87) : <lambda_11fc931574fd38d67807576e751a0e04>::operator()
```

`sys_asserts=2` 会打开如下对话框：

![Assert dialog box](/images/user-guide/programming/debugging/debugging-using-asserts-1.png)

下表描述了 **Assert** 对话框的选项。


****

| 选项 | 说明 |
| --- | --- |
| Ignore | 忽略当前断言，继续运行应用程序。同一断言不再触发对话框显示。 |
| Ignore All | 防止当前断言和所有未来的断言显示对话框。为防止性能下降，仅在完成后才将调试信息打印到日志中。 |
| Break | 在断言处中断。如果已连接调试器，则创建一个断点，并在调试器中的断点处中断。如果未连接调试器，则停止应用程序。 |

## 在初始化时设置断言级别

要在引擎初始化时设置断言级别，请在项目的 `game.cfg` 文件中添加一个条目。下面的示例显示了 SamplesProject 的 `game.cfg` 文件。

```
sys_game_name = "SamplesProject"
sys_localization_folder = Localization
ca_useIMG_CAF = 0
collision_classes = "Ship=0,Shield=1,Asteroid=2"
r_DisplayInfo=3
sys_asserts=2
```

## 在运行时设置断言级别

您可以在运行时在控制台窗口中设置 `sys_asserts` 控制台变量。下图是一个示例。

![Setting the sys_asserts console variable at runtime.](/images/user-guide/programming/debugging/debugging-using-asserts-2.png)

## 设置移动设备的断言级别

调试移动平台时，可以使用基于 Windows 的通用远程控制台在应用程序的命令行窗口中设置断言级别。

![Using the Universal Remote Console to set the assert level for mobile platforms.](/images/user-guide/programming/debugging/debugging-using-asserts-3.png)

## **在源代码中设置断言** 

要在源代码中添加断言，请使用 `AZ_Assert` 宏。

```
AZ_Assert(m_useCount >= 0, "AssetData has been deleted")
```

更多信息，请参阅 [跟踪](/docs/user-guide/programming/debugging/tracing/)。
