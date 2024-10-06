---
description: ' 列出 AZ 框架应用程序初始化时发生的步骤。 '
title: AZ 引导过程
---

AZ 框架应用程序会根据应用程序描述符文件中列出的动态库和`CreateStaticModules()`函数引用的静态库初始化模块。

当 `AzFramework::Application` 启动时，会发生以下事件：

1.  可执行文件启动。

1.  初始化 `AzFramework::Application` 类。它接收应用程序描述符文件的路径和一个函数指针，该函数将从静态库中创建 `AZ::Modules`。

1.  应用程序自启动时，只需读取应用程序描述符文件即可。

1.  读取应用程序描述符文件是为了获取内存分配器设置和要加载的动态链接库列表。O3DE 还不能从文件中读取系统实体。

1.  O3DE 会关闭已启动的系统，根据刚刚加载的设置进行配置，然后重新启动这些系统。

1.  加载每个动态链接库。

1.  运行每个动态链接库的 `InitializeDynamicModule()`函数，将 DLL 附加到全局`AZ::Environment`中。

1.  每个静态库的 `AZ::Module` 实例都是使用步骤 2 中传入的函数指针创建的。

1.  每个动态库的 `AZ::Module` 实例都是通过其 `CreateModuleClass()` 函数创建的。

1.  每个 AZ 模块的 `RegisterComponentDescriptors()`函数都会被调用。现在，应用程序知道如何序列化库中定义的任何组件了。

1.  再次读取应用程序描述符文件，以提取系统实体及其组件和设置。

1.  每个 AZ 模块的 `GetRequiredSystemComponents()`函数都会被调用。如果系统实体中缺少任何组件，则会添加这些组件。

1.  系统实体被激活，其所有系统组件按适当顺序被激活。

至此，初始化完成，游戏开始运行。
