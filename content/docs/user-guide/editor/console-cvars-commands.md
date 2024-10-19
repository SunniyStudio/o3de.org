---
linkTitle: 自定义控制台
title: 自定义O3DE控制台
description: 使用 Open 3D Engine 控制台自定义和创建自己的控制台变量命令。
weight: 550
---

控制台是一个用户界面系统，用于处理控制台命令和控制台变量。它还输出日志信息并存储输入和输出历史。

## 颜色编码

游戏控制台支持颜色编码，方法是使用带有前导 $ 字符的颜色指数 0...9。代码会隐藏在控制台输出的文本中。通过 `ILog` 接口发送的简单日志信息可用于向控制台发送文本。

```
This is normal $1one$2two$3three and so on
```

在前面的示例中，1 显示为红色，2 显示为绿色，3（以及其余文字）显示为蓝色。

## 控制台变量

控制台变量（cvars）提供了一种方便的方法来暴露变量，用户可以通过在运行时在控制台中输入或在启动应用程序前将其作为命令行参数传递来轻松修改这些变量。

### 注册新的控制台变量

对于基于整数或浮点数的控制台变量，建议使用 `IConsole::Register()` 函数将 C++ 变量公开为控制台变量。

要声明一个新的字符串控制台变量，请使用`IConsole::RegisterString()`函数。

### 从 C++ 访问控制台变量

控制台变量使用 `ICVar` 接口公开。要检索该接口，请使用 `IConsole::GetCVar()`函数。

读取控制台变量值的最有效方法是直接访问绑定到控制台变量代理的 C++ 变量。

## 添加新的控制台命令

使用新的控制台命令可以轻松扩展控制台。新的控制台命令可以在 C++ 中以静态函数的形式实现，该函数遵循 `ConsoleCommandFunc` 类型。该控制台命令的参数使用 `IConsoleCmdArgs` 接口传递。

以下代码显示了控制台命令的基本实现：

```
static void RequestLoadMod(IConsoleCmdArgs* pCmdArgs);

void RequestLoadMod(IConsoleCmdArgs* pCmdArgs)
{
  if (pCmdArgs->GetArgCount() == 2)
  {
	 const char* pName = pCmdArgs->GetArg(1);
	 // ...
  }
  else
  {
    // error
  }
}
```

以下代码将在控制台系统中注册该命令：

```
IConsole* pConsole = gEnv->pSystem->GetIConsole();
pConsole->AddCommand("g_loadMod", RequestLoadMod);
```

## 控制台变量组

控制台变量组提供了一种方便的方法，可同时对多个控制台变量应用预定义设置。

在代码库中，控制台变量通常被称为`CVarGroup`。控制台变量组可以修改其他控制台变量，从而建立更大的层次结构。

{{< caution >}}
作业中的循环未被检测到，可能导致崩溃。
{{< /caution >}}

### 注册新变量组

要注册新的变量组，请在 `GameSDK\config\CVarGroups`目录下添加一个新的`.cfg`文本文件。

`sys_spec_Particles.cfg`

```
[default]
; default of this CVarGroup
= 4
e_particles_lod=1
e_particles_max_emitter_draw_screen=64

[1]
e_particles_lod=0.75
e_particles_max_emitter_draw_screen=1

[2]
e_particles_max_emitter_draw_screen=4

[3]
e_particles_max_emitter_draw_screen=16
```

这将创建一个名为 `sys_spec_Particles` 的新控制台变量组，其行为类似于整数控制台变量。默认情况下，该变量的状态为 `4`（在示例中注释后的一行中设置）。

更改变量后，新状态将被应用。未在 `.cfg` 文件中指定的控制台变量不会被设置。所有控制台变量都必须是默认部分的一部分。如果违反此规则，将输出错误信息。

如果未在自定义部分中指定控制台变量，则会应用默认部分中指定的值。

### 控制台变量组文档

控制台变量组的文档会自动生成。

`sys_spec_Particles`

```
控制台变量组将设置应用于多个变量

sys_spec_Particles [1/2/3/4/x]:
 ... e_particles_lod = 0.75/1/1/1/1
 ... e_particles_max_screen_fill = 16/32/64/128/128
 ... e_particles_object_collisions = 0/1/1/1/1
 ... e_particles_quality = 1/2/3/4/4
 ... e_water_ocean_soft_particles = 0/1/1/1/1
 ... r_UseSoftParticles = 0/1/1/1/1
```

### 检查控制台变量组值是否代表其控制变量的状态

#### 从控制台

在控制台中，您可以输入控制台变量组名称并按 tab 键。如果未表示变量值，则将打印 `RealState` 的值。

```
sys_spec_Particles=2 [REQUIRE_NET_SYNC] RealState=3
sys_spec_Sound=1 [REQUIRE_NET_SYNC] RealState=CUSTOM
sys_spec_Texture=1 [REQUIRE_NET_SYNC]
```

通过调用控制台命令 `sys_RestoreSpec` 可以检查为什么 `sys_spec_` 变量不代表正确的状态。

#### 来自 C++ 代码

通过代码，您可以使用成员函数 `GetRealIVal()` 并将其返回值与 `ICVar` 中的 `GetIVal()` 结果进行比较。

## 延迟执行命令行控制台命令

与其他控制台命令相比，使用 + 前缀通过命令行传递的命令会存储在一个单独的列表中。

该列表允许应用程序将这些命令的执行分配到多个帧中，而不是一次性执行所有命令。

### 示例

请看下面的例子。

```
--- autotest.cfg --
hud_startPaused = "0"
wait_frames 100
screenshot autotestFrames
wait_seconds 5.0
screenshot autotestTime

-- console --
StarterGameLauncher.exe -devmode +map SinglePlayer +exec autotest +quit
```

在示例中，执行了以下操作：
- 加载SinglePlayer地图。
- 等待 100 帧。
- 截取名为 autotestFrames 的屏幕截图。
- 等待 5 秒。
- 截取名为 autotestTime 的截图。
- 退出应用程序。

### 详细信息

定义了两类命令：阻塞命令和正常命令。

在每一帧中，延迟命令列表作为一个 fifo 进行处理。只要遇到正常命令，就会消耗该列表中的元素。

当阻止程序从列表中被消耗并执行时，进程会延迟到下一帧。例如，`map`和`screenshot`等命令就是阻塞程序。

控制台命令（命令或变量）可以在声明时使用` VF_BLOCKFRAME` 标记标记为阻塞程序。

支持以下同步命令。


**可选标题**

|  命令  |  类型  |  说明  |
| --- | --- | --- |
| wait\_frames num: |  <int>  |  等待 **num** 帧后，重新开始执行列表。  |
| wait\_seconds sec: |  <float>  |  等待 *num* 秒后，重新开始执行列表。  |
