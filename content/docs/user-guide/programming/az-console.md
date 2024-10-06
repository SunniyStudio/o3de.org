---
description:  使用 AZ::Console 为您的 O3DE 游戏设置控制台变量和函数。
title: AZ::Console
---

`AZ::Console` 类提供了一组用于定义变量和映射函数的宏，您可以用它们与游戏中的变量和进程进行交互。使用该类中定义的宏为游戏设置控制台变量（cvars）和函数（cfuncs），然后通过 O3DE 控制台访问它们。

`AZ::Console`在以下头文件中定义：`%INSTALL-ROOT%\dev\Code\Framework\AzCore\AzCore\Console\IConsole.h`

`AZ::Console`的功能：

+ 在发布版本中锁定 cvars 和 cfuncs 的基本访问保护和反作弊机制。
+ 默认支持多种 C++ 类型，包括 bool（布尔）、stdint（所有类型）、浮点数、双倍、向量和四元数，以及 enums（枚举）。
+ 灵活、可扩展的类型支持。您可以添加对新 cvar 类型的支持，而无需直接修改控制台代码。

![The AZ::Console](/images/user-guide/programming/az-console-1.png)

**主题**
- [控制台变量 (cvars)](#console-variables-cvars)
- [控制台函数 (cfuncs)](#console-functors-cfuncs)
- [可选标签](#optional-flags)
- [添加新的控制台变量类型](#adding-support-for-new-console-variable-types)

## 控制台变量 (cvars) 

使用 `IConsole.h:` 中的两个宏之一声明 cvar：

```
AZ_CVAR(_TYPE, _NAME, _INIT, _CALLBACK, _FLAGS, _DESC) //Standard cvar macro, provides no external linkage.
```

```
AZ_CVAR_EXTERNABLE(_TYPE, _NAME, _INIT, _CALLBACK, _FLAGS, _DESC) //Cvar macro that creates a console variable with external linkage.
```

参数：
+ **\_TYPE**: cvar的基本类型。
+ **\_NAME**: cvar的名称。
+ **\_INIT**: 分配给cvar的初始值。
+ **\_CALLBACK**: cvar更改值时调用的回调函数。

{{< note >}}
  这些宏不能保证回调在特定线程上运行。回调处理程序的实现者有责任确保线程安全。
{{< /note >}}

+ **\_FLAGS**: 一个或多个用于改变行为的 `AZ::Console::FunctorFlags`。使用逻辑AND \(`&&`\) 和  OR \(`||`\) 运算符来组合标志。如果没有要设置的标志，请使用 `FunctorFlags::None`。
+ **\_DESC**: 字符串，用于显示 cvar 的简短说明。

要在代码中声明新的 cvar，请包含 `IConsole.h` 头文件。然后使用其中一个 cvar 宏\(如 `AZ_CVAR`\) 在自己的代码 (.cpp) 文件中声明新的控制台变量。

{{< note >}}
AZ\_CVAR 和 AZ\_CVAR\_EXTERNABLE 变量只能在 C++ 代码 (.cpp) 文件中声明。而 AZ\_CVAR\_EXTERNED 变量则可以在 C++ 代码 (.cpp) 或头文件 (.h) 中声明。
{{< /note >}}

下面是几个例子。

```
AZ_CVAR(int32_t, cl_GameServiceRefreshTimeMs, 1000, nullptr, FunctorFlags::None, "Controls the auto-refresh delay for all gameService data, time in milliseconds");
AZ_CVAR(bool,    cl_QuitOnHubDisconnect,     false, nullptr, FunctorFlags::None, "If enabled, the client executable will terminate on disconnect");
```

```
void OnConsoleResUpdate(const int32_t& a_NewWidth)
{
    // Run update for new value
}

AZ_CVAR(int32_t, sv_ConsoleWidth, 160, OnConsoleResUpdate, FunctorFlags::ReadOnly, "The width of the server console window");
```

```
AZ_CVAR_EXTERNABLE(uint16_t, net_ServerRateMs, 33, nullptr, FunctorFlags::ReadOnly, "Server tick rate to use for network relevent simulations");
```

可选择使用以下名称前缀来帮助组织 cvars 组：
+ **sv\_**: 仅用于服务器 cvars
+ **cl\_**: 仅适用于客户端 cvars
+ **bg\_** : 通用 cvars（客户端和服务器），"Both games"

这些前缀可以快速限制自动完成的范围，并在控制台中查看相关的 cvars 组。你也可以使用自己的前缀。

要将现有的控制台变量设置为外部变量（extern），请使用 `AZ_CVAR_EXTERNED` 宏：

```
AZ_CVAR_EXTERNED(_TYPE, _NAME)
```

确保 **\_TYPE** 和 **\_NAME** 参数与之前定义的 cvar 匹配。

## 控制台函数 (cfuncs) 

控制台函数允许您在控制台注册一个命令，该命令与特定类型或值无关。在 O3DE 中，它们纯粹是一种允许从 O3DE 游戏控制台直接调用方法的机制。

有两种类型的 cfuncs：一种用于调用类成员方法\(`AZ_CONSOLEFUNC`\)，另一种用于调用静态方法 \(`AZ_CONSOLEFREEFUNC`\)。

要声明类成员方法 cfunc，请使用 `IConsole.h` 中的 `AZ_CFUNC` 宏：

```
AZ_CONSOLEFUNC(_CLASS, _FUNCTION, _INSTANCE, _FLAGS, _DESC)
```

参数：
+ **\_CLASS**: 包含调用方法（函数）的类。
+ **\_FUNCTION**: 作为回调调用的方法。

    {{< note >}}
  这些宏不能保证回调在特定线程上运行。回调处理程序的实现者有责任确保线程安全。
{{< /note >}}

+ **\_INSTANCE**: 调用此方法的类的实例\(通常设置为当前实例的 `this`\)。
+ **\_FLAGS**: 一个或多个用于改变行为的`AZ::Console::FunctorFlags`。使用逻辑AND \(`&&`\) 和 OR \(`||`\)运算符来组合标志。如果没有要设置的标志，请使用 `FunctorFlags::None`。
+ **\_DESC**: 字符串，用于显示 cfunc 的简短说明。

cfunc 声明的一些示例：

```
class Example
{
public:
    Example() { AZ_CONSOLEFUNC(Example, Method, this, FunctorFlags::DontReplicate, "Executes the Method method on this Example instance, invoke in the console using Example.Method"); }
    void Method(const StringSet&) {}
};
```

要为静态方法（或其他非成员函数）声明 cfunc，请使用 `AZ_CONSOLEFREEFUNC` 宏：

```
AZ_CONSOLEFREEFUNC(_FUNCTION, _FLAGS, _DESC)
```

参数：
+ **\_FUNCTION**: 作为回调调用的静态方法。

    {{< note >}}
  这些宏不能保证回调在特定线程上运行。回调处理程序的实现者有责任确保线程安全。
{{< /note >}}

+ **\_FLAGS**: 一个或多个用于改变行为的 `AZ::Console::FunctorFlags`。使用逻辑 AND \(`&&`\) 和 OR \(`||`\) 运算符来组合标志。如果没有要设置的标志，请使用 `FunctorFlags::None`。
+ **\_DESC**: 字符串，用于显示 cfunc 的简短说明。

示例：

```
void ForceEnableMetrics(const StringSet&) {}
    AZ_CONSOLEFREEFREEFUNC(ForceEnableMetrics, FunctorFlags::Null, "If called, force enable metrics");
```

## 可选标签

`AZ::Console` 提供了一组标志，可以传递给 cvar 和 cfunc 声明，并指示如何处理它们：

```
enum class FunctorFlags
{
    Null           = 0        // 空标签
,   DontReplicate  = (1 << 0) // 不应复制（目前未使用）
,   ServerOnly     = (1 << 1) // 不应复制到客户端（当前未使用）
,   ReadOnly       = (1 << 2) // 不应在运行时调用
,   IsCheat        = (1 << 4) // 不应在自动完成控制台中显示
,   IsDeprecated   = (1 << 5) // 命令已被弃用，调用时会发出警告
,   NeedsReload    = (1 << 6) // 执行此命令后，应重新加载关卡
,   AllowClientSet = (1 << 7) // 允许客户端修改此 cvar，即使是在发行版中（这将改变所有已连接服务器和客户端的 cvar，启用此标记时要非常小心）（当前未使用）
};
```

## 添加对新控制台变量类型的支持 

要添加对新 cvar 类型的支持，需要覆盖两个模板方法，将自定义类型从空格分隔的字符串输入向量转换为空格分隔的字符串。

例如，将 `AZ::Vector3` 转换为字符串并返回值的重载方法是这样声明的：

```
namespace AZ
{
    // CVar compatibility
    namespace ConsoleTypeHelpers
    {
        template <>
        AZStd::string ValueToString<AZ::Vector3>(const AZ::Vector3& a_Value);

        template <>
        bool StringSetToValue<AZ::Vector3>(AZ::Vector3& a_OutValue, const StringSet& a_Arguments);
    }
}
```
