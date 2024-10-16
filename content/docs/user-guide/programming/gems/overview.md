---
linktitle: 概述
title: Gem 模块系统
description: Open 3D Engine Gem 系统和模块加载器概述。
weight: 100
---

对于附带代码组件的Gem，与Gem相关的编译库称为**模块**。所有Gem模块，无论是否用于编辑器、运行时或专用服务器等工具应用程序，都以同样的方式接入系统。所有模块都有已知的入口点函数，这些函数应在派生自[`AZ::Module`](/docs/api/frameworks/azcore/class_a_z_1_1_module.html)的类上实现**并**声明为`#extern C`。

当 [`AzFramework::Application`](/docs/api/frameworks/azframework/class_az_framework_1_1_application.html)加载一个 Gem 模块时，它会使用[`AZ::ModuleManager`](/docs/api/frameworks/azcore/class_a_z_1_1_module_manager.html)来完成。当模块管理器收到加载模块的请求时，它会加载动态库并调用一系列指定的入口点。由于项目配置文件指定了项目使用的 Gems，因此通常情况下，加载会作为应用程序调用 `Start(...)` 方法的一部分自动完成。为了更精细地控制模块加载，开发者可以使用 O3DE 提供的 `ModuleManager` 实例，而不是让应用程序 [bootstrapping](../bootstrap) 进程来处理。使用手动方法允许开发人员在模块加载时设置自己的专门前置和后置条件，这些条件可能与 Gem 依赖关系无关。

## O3DE 如何加载和初始化Gem

![An image describing the relationships between classes and the load/initialization/cleanup/unload process for Gems in the Open 3D Engine.](/images/user-guide/programming/gems/gem-load-flow.svg)

### 模块入口点函数

所有模块都必须提供以下函数，声明时使用 `extern "C" AZ_DLL_EXPORT` 指定符：

* `void InitializeDynamicModule(void* env)` - 该函数在模块加载**后**立即调用，将一个指向 “环境实例 ”的指针传递给它，以便查询 EBus、事件处理程序和内存分配器等信息。
* `AZ::Module* CreateModuleClass()` - 创建代表模块的实际类的实例并返回。
* `void DestroyModuleClass(AZ::Module*)` - 销毁所提供的模块实例。
* `void UninitializeDynamicModule()` - 在卸载模块**之前**调用的函数。模块应清理不再需要但在 `InitializeDynamicModule()` 中设置的静态资源和环境信息。

{{< important >}}
虽然 `CreateModuleClass()` 和 `DestroyModuleClass(AZ::Module*)` 只被模块加载器调用一次（在加载后创建实例，并在卸载前销毁实例），但这并不是保证行为，因为最终用户可能会手动加载模块。如果您的模块需要单例行为，请务必自行执行，以防止开发人员意外实例化多个对象。
{{< /important >}}

这些模块入口点函数由 Gem 的 `AZ::Module` 类中调用的 `AZ_DECLARE_MODULE_CLASS(...)` 宏定义。

## AZ_DECLARE_MODULE_CLASS(...) 宏

由于许多模块会使用相同的模板代码来生成入口点代码，O3DE 提供了 `AZ_DECLARE_MODULE_CLASS(...)`宏，用于生成标准的入口点函数实现。该宏的语法是 `AZ_DECLARE_MODULE_CLASS(<UUID>,<ModuleClassName>)`。按照惯例，与Gem相关的模块所使用的 `<UUID>`应为 `Gem_<GemName>`。

`AZ_DECLARE_MODULE_CLASS(...)`宏定义在 `AZ::Module` 类中，该类位于引擎源代码的 `/Code/Framework/AzCore/AzCore/Module` 目录下。

## 示例模块类

下面的 C++ 类是最精简的 O3DE 模块： 它派生自 `AZ::Module`，声明必要的重载，并生成入口点函数。它不提供任何附加功能。请注意，这是一个**实现**，而不是一个**定义**。为了遵循良好的命名惯例，此模块实现属于名为 `Example` 的 Gem。

```cpp
class ExampleModule
    : public AZ::Module
{
    public:
    	AZ_RTTI(ExampleModule, "{CFC64EAF-7566-4D30-AAF4-A6FF19BF87DB}", AZ::Module);

        ExampleModule()
            : AZ::Module()
        {
	}
};

AZ_DECLARE_MODULE_CLASS(Gem_Example, Example)
```

## 单片构建Gem

模块系统也可在**单片**模式下运行。启用 [单片构建](/docs/user-guide/build/reference/#build-configuration) 后，动态模块将变为静态模块，并直接链接到应用程序可执行文件，而不是在启动时动态加载。这样，开发人员就可以发布单一（大型）服务器或客户端应用程序可执行文件。单体构建可优化启动时间，并避免性能问题或平台限制，这些问题或限制可能会阻止动态库的加载。
