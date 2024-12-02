---
title: 从设置注册表发出控制台命令
linkTitle: 发出控制台命令
description: 了解如何使用 Open 3D Engine （O3DE） 中的设置注册表运行控制台命令
weight: 500
---

[AZ::Console](/docs/user-guide/programming/az-console/) 允许您通过在设置注册表中的特定键下设置 JSON 值来启动 Console 命令。控制台挂接到设置注册表通知系统以及 JSON 补丁报告系统，以检测以下对象下设置注册表项的更改：

* `/O3DE/Autoexec/ConsoleCommands` - 此对象下的控制台命令将合并到 `bootstrap.game.<config>.setreg` 并可在 Launcher 应用程序中运行。
* `/Amazon/AzCore/Runtime/ConsoleCommands` - 此对象下的控制台命令*不会*合并到  `bootstrap.game.<config>.setreg` ，并且不会在 Launcher 应用程序中运行。

{{< note >}}
在 Launcher 应用程序中，设置注册表文件只能可靠地加载 [`bootstrap.game.<config>.setreg`](https://github.com/o3de/o3de/blob/6b62d1131116c074831902cb6e8d30271d673288/Code/Framework/AzGameFramework/AzGameFramework/Application/GameApplication.cpp#L90-L99)  文件。
{{< /note >}}

## 从文件运行 Console 命令

您可以使用`AZ::IConsole::ExecuteConfigFile` 函数从文件运行控制台命令。该函数可以从以下文件类型加载 Console 命令：

* Windows INI Style 文件 (`.cfg`)
* JSON Merge Patch 文件 (`.setreg`) - 可以创作为普通 JSON 文件
* JSON Patch 文件 (`.setregpatch`)

### 包含控制台命令的配置文件(`.cfg`)

以下示例演示了向 Windows 样式的`.cfg`文件添加命令：

**user.cfg**

```ini
testInit = 3
testInit 3
testBool true
testChar Q
testUInt64 18446744073709551615
testFloat 1.0
testDouble 2
testString Stable
ConsoleSettingsRegistryFixture.testClassFunc Foo Bar Baz

LoadLevel path/to/level.spawnable
bg_ConnectToAssetProcessor false
```

### 包含控制台命令的 Settings Registry 文件 (`.setreg`)

“/O3DE/Autoexec/ConsoleCommands”对象下的设置将添加到聚合中。`bootstrap.game.<config>.setreg` 则由 **Asset Processor** 处理`user.setreg` 文件时由设置注册表生成器创建。不会添加`/Amazon/AzCore/Runtime/ConsoleCommands`设置，因为它们在 Asset Processor 设置中是 [排除](https://github.com/o3de/o3de/blob/e878b06166dc4953b8c6c79b745375a1db7c341f/Registry/setregbuilder.assetprocessor.setreg#L22)。

**user.setreg**

```json
{
    "Amazon": {
        "AzCore": {
            "Runtime": {
                "ConsoleCommands": {
                    "testInit": 3,
                    "testBool": true,
                    "testChar": "Q",
                    "testUInt64": 18446744073709551615,
                    "testFloat": 1.0,
                    "testDouble": 2,
                    "testString": "Stable",
                    "ConsoleSettingsRegistryFixture.testClassFunc": "Argument1 Argument2 Argument3"
                }
            }
        }
    },
    "O3DE": {
        "Autoexec": {
            "ConsoleCommands": {
                "LoadLevel": "path/to/level.spawnable",
                "bg_ConnectToAssetProcessor": false
            }
        }
    }
}
```

### 设置注册表 带有控制台命令的补丁文件(`.setregpatch`)

以下示例演示了如何向`.setregpatch` 文件添加命令：

**user.setregpatch**

```json
[
    { "op": "add", "path": "/O3DE/Autoexec/ConsoleCommands/testInit", "value": 3 },
    { "op": "add", "path": "/O3DE/Autoexec/ConsoleCommands/testBool", "value": true },
    { "op": "add", "path": "/O3DE/Autoexec/ConsoleCommands/testChar", "value": "Q" },
    { "op": "add", "path": "/O3DE/Autoexec/ConsoleCommands/testUInt64", "value": 18446744073709551615 },
    { "op": "add", "path": "/O3DE/Autoexec/ConsoleCommands/testFloat", "value": 1.0 },
    { "op": "add", "path": "/O3DE/Autoexec/ConsoleCommands/testDouble", "value": 2 },
    { "op": "add", "path": "/O3DE/Autoexec/ConsoleCommands/testString", "value": "Stable" },
    { "op": "add", "path": "/O3DE/Autoexec/ConsoleCommands/ConsoleSettingsRegistryFixture.testClassFunc", "value": "Foo Bar Baz" },
    { "op": "add", "path": "/O3DE/Autoexec/ConsoleCommands/LoadLevel", "value": "levels/levelname/levelname.spawnable" },
    { "op": "add", "path": "/O3DE/Autoexec/ConsoleCommands/bg_ConnectToAssetProcessor", "value": true }
]
```

## 从配置或设置注册表文件运行 Console 命令

您可以通过在 Console 中使用单个 `ExecuteConfigFile`函数来运行配置文件和设置注册表文件。

```c++
auto console = AZ::Interface<AZ::IConsole>::Get();
console.ExecuteConfigFile(<path to [user.cfg|user.setreg|user.setregpatch]>);
```

特定配置或 Settings Registry 文件中的控制台命令按照它们在这些文件中出现的顺序从上到下运行。

以下是使用 JSON Merge Patch 语法运行三个 Console 命令的示例：

```json
{
    "O3DE": {
        "Autoexec": {
            "ConsoleCommands": {
                "testInit": 3,
                "testBool": true,
                "testChar": "Q"
            }
        }
    }
}
```

上述命令按照它们在 JSON 文件（testInit → testBool → testChar）的顺序运行。

## 通过更新 Settings Registry 中的值来运行控制台命令

您还可以通过修改“/O3DE/Autoexec/ConsoleCommands”JSON 对象下的设置注册表中的键来运行控制台命令。以下示例修改控制台变量 （CVar）：

**CVar 修改**

```c++
//! The AZ::IConsole::ConsoleAutoexecCommandKey variable is set to the Settings Registry Console commands root key:
//! "/O3DE/Autoexec/ConsoleCommands"
using FixedValueString = AZ::SettingsRegistryInterface::FixedValueString;
constexpr auto tScaleCVar = FixedValueString(AZ::IConsole::ConsoleRootCommandKey) + "/t_scale";
auto SettingsRegistry = AZ::SettingsRegistry::Get();
settingsRegistry->Set(tScaleCVar, "0.5");
```

## 调用 Console 命令时的排序

在[ComponentApplication::Create](https://github.com/o3de/o3de/blob/6d5a045386e20bc0d587007a65cf32f5b33baadd/Code/Framework/AzCore/AzCore/Component/ComponentApplication.cpp#L615-L622)中加载或激活任何 Gem 模块之前，`ComponentApplication` 会先加载 `.setreg` 和 `.setregpatch` 文件。ComponentApplication 还支持在加载 Gem 模块之后但在激活之前在资产缓存中运行`user.cfg`。您可以在 [ComponentApplication::CreateCommon](https://github.com/o3de/o3de/blob/6d5a045386e20bc0d587007a65cf32f5b33baadd/Code/Framework/AzCore/AzCore/Component/ComponentApplication.cpp#L679-L689)中看到这一点。

任何无法立即运行的 Console 命令都将添加到 [deferred 控制台命令队列](https://github.com/o3de/o3de/blob/6d5a045386e20bc0d587007a65cf32f5b33baadd/Code/Framework/AzCore/AzCore/Console/Console.cpp#L612-L628) 中。当 Gem 最终加载时，任何延迟的 Console 命令都会尝试 [再次调度](https://github.com/o3de/o3de/blob/6d5a045386e20bc0d587007a65cf32f5b33baadd/Code/Framework/AzCore/AzCore/Module/Module.h#L113)。成功的命令将从队列中删除，而失败的命令将保留在 [queue](https://github.com/o3de/o3de/blob/6d5a045386e20bc0d587007a65cf32f5b33baadd/Code/Framework/AzCore/AzCore/Console/Console.cpp#L177-L186) 中。

这可保证即使稍后加载定义 Console 命令的 Gem 时，仍会调用 Console 命令。

{{< note >}}
如果 Console 命令立即运行，则延迟的 Console 命令可能不会按与它们应有的顺序相同的顺序运行。例如，如果`.cfg` 文件包含以下控制台命令：

```ini
immediateCommand1 42
deferredCommand2 35
immediateCommand3 28
```

由于`deferredCommand2`无法立即运行，因此它会在所有立即命令（包括`immediateCommand3`）之后运行。
{{< /note >}}

### 控制台命令生命周期

虽然与设置注册表能够运行 Console 命令没有直接关系，但以下信息描述了应用程序何时可以针对其他应用程序生命周期事件运行控制台命令。

1. 创建 [Settings Registry](https://github.com/o3de/o3de/blob/7d69221a92f1eb830573ade15cb327b5ede6346c/Code/Framework/AzCore/AzCore/Component/ComponentApplication.cpp#LL466C16-L466C16).
1. 创建 [AZ Console](https://github.com/o3de/o3de/blob/7d69221a92f1eb830573ade15cb327b5ede6346c/Code/Framework/AzCore/AzCore/Component/ComponentApplication.cpp#L466C16-L475).
1. 创建 [Archive FileIO](https://github.com/o3de/o3de/blob/bc24a8fefb5438f4782f34caa0f9ecb3504d04c3/Code/Framework/AzFramework/AzFramework/Application/Application.cpp#L106-L137)
1. 将所有设置注册表文件(`.setreg`/`.setregpatch`)合并到 [注册表](https://github.com/o3de/o3de/blob/7d69221a92f1eb830573ade15cb327b5ede6346c/Code/Framework/AzCore/AzCore/Component/ComponentApplication.cpp#L762C1-L788)并尝试调用受监控的 [设置注册表控制台命令](https://github.com/o3de/o3de/blob/7d69221a92f1eb830573ade15cb327b5ede6346c/Code/Framework/AzCore/AzCore/Console/Console.cpp#L527-L605)下的任何控制台命令钥匙。任何无法运行的命令都将添加到 AZ 控制台 [deferred command queue](https://github.com/o3de/o3de/blob/7d69221a92f1eb830573ade15cb327b5ede6346c/Code/Framework/AzCore/AzCore/Console/Console.cpp#L103C8-L125)。
1. 加载动态模块（Gem 模块）。发送 `GemsLoaded` 生命周期事件。对于每个加载的 Gem，延迟的命令会尝试 [execute](https://github.com/o3de/o3de/blob/7d69221a92f1eb830573ade15cb327b5ede6346c/Code/Framework/AzCore/AzCore/Module/Module.h#L99-L114)。任何成功的命令都将从延迟的命令队列中 [removed](https://github.com/o3de/o3de/blob/bc24a8fefb5438f4782f34caa0f9ecb3504d04c3/Code/Framework/AzCore/AzCore/Console/Console.cpp#L196-L203)。
1. 尝试从 Projects 资源缓存文件夹（通常为`<project-root>/Cache/<platform>`）中运行 [user.cfg](https://github.com/o3de/o3de/blob/7d69221a92f1eb830573ade15cb327b5ede6346c/Code/Framework/AzCore/AzCore/Component/ComponentApplication.cpp#L843-L849) 中的控制台命令。
1. 运行在 [命令行](https://github.com/o3de/o3de/blob/7d69221a92f1eb830573ade15cb327b5ede6346c/Code/Framework/AzCore/AzCore/Component/ComponentApplication.cpp#L851-L852) 上指定的控制台命令。使用命令选项表示法`-<console command name> <args>`） 指定。例如，`-loadlevel <levelname>`。
1. 为 [所有活动 Gems](https://github.com/o3de/o3de/blob/bc24a8fefb5438f4782f34caa0f9ecb3504d04c3/Code/Framework/AzFramework/AzFramework/Application/Application.cpp#L199-L205) 设置 FileIO 别名`@alias@`
1. 激活 [Gems](https://github.com/o3de/o3de/blob/bc24a8fefb5438f4782f34caa0f9ecb3504d04c3/Code/Framework/AzCore/AzCore/Module/ModuleManager.cpp#L633-L783)中的系统组件。
