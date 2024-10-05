---
linkTitle: 启动和屏幕管理类
title: 启动和屏幕管理类
description: 便于项目管理器启动和屏幕转换逻辑的类
weight: 100
---

{{< note >}}
本信息面向**项目管理器**工具的开发人员。如果您是使用项目管理器的用户，请参阅[项目管理器用户指南](/docs/user-guide/project-config/project-manager)。
{{< /note >}}

{{< note >}}
其中包含各种类的功能和作用的简要概述。如果有，目录中还会提供其他链接，包括扩展信息或 github 链接。所有文档反映的代码截至提交 [(b79bd3df1f)](https://github.com/o3de/o3de/tree/b79bd3df1fe5d4c2a639d3921a29bd0d95712f6c) 
{{< /note >}}

## 概述

| Class                                         | Source Code                                                                                                                                                                                                                     | Description                                                                                                                                       |
|-----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| main.cpp                                      | [cpp 文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/main.cpp)                                                                                                                               | 项目管理器的入口点。启动 [应用程序主循环](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/main.cpp#L28)。 |
| [Application](#application)                   | [头文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/Application.h) [cpp 文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/Application.cpp)                    | 为 Qt 框架、日志、Python Bindngs 和 O3DE 引擎注册提供核心启动代码和配置加载。为启动应用程序主循环的前期工作提供便利，并在拆卸时进行清理。                                                                 |                                                        |
| [ProjectManagerWindow](#projectmanagerwindow) | [头文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/ProjectManagerWindow.h)  [cpp 文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/ProjectManagerWindow.cpp) | 构建应用程序的 `QMainWindow` 和 `DownloadController`，并为项目经理准备所有相关的过渡屏幕。                                                                                   |                                                                      
| [ScreensCtrl](#screensctrl)                   | [头文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/ScreensCtrl.h) [cpp 文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp)                    | 所有项目管理器屏幕和图形用户界面小工具的容器结构。还包含主要的过渡代码，用于促进项目经理的业务逻辑。                                                                                                |                                                                 
| ScreenFactory                                 | [头文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/ScreenFactory.h) [cpp 文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/ScreenFactory.cpp)                | 用于将 `ProjectManagerScreen` 枚举路由到适当的 `ScreenWidget` 构造函数的 `ScreensCtrl` 的辅助类，调用该构造函数，并为项目经理的业务逻辑返回给定屏幕的实例。                                         |                                      
| ScreenDefs                                    | [头文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/ScreenDefs.h)                                                                                                                              | 包含 `ProjectManagerScreen` 枚举的定义，该枚举描述了项目管理器中所有可能的屏幕类型。它还为每个枚举定义了哈希函数和相应字符串的映射。                                                                    |
| [ScreenWidget](#screenwidget)                 | [头文件](https://github.com/o3de/o3de/blob/development/Code/Tools/ProjectManager/Source/ScreenWidget.h)                                                                                                                            | 项目管理器中所有屏幕的父类。它包含屏幕管理和转换所需的所有存根。 `ScreensCtrl` 是根据 `ScreenWidget` 定义的，因此所有转换逻辑都是多态的。                                                              |


### Application

继承自`AzFramework::Application`。

应用程序类的功能完全由 `main.cpp` 运行。

需要跟踪的两个主要函数是 `Init` 和 `Run`。在 `Application` 解构时会调用 [`TearDown`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/Application.cpp#L260-L272) 函数。

##### [`Application::Init`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/Application.cpp#L34)
* 设置各种 `QApplication` 和 `QCoreApplication` 属性和参数。
* 为系统日志注册组件，并调用 [`InitLog`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/Application.cpp#L152)。
* 创建新的 `QApplication` 实例，输入命令行参数。
* 设置 PythonBindings。
* 调用 [`RegisterEngine`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/Application.cpp#L181) ，设置项目管理器附带的 O3DE 引擎。
* 获取与 CommandLine 的连接，并解析 screen 和 projectPath 参数。
* 创建 ProjectManagerWindow。

##### [`Application::Run`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/Application.cpp#L274)
* 设置 Qt GUI 样式表。
* 设置窗口装饰和主窗口几何形状。
* 显示主窗口。
* 运行 QApplication 的主循环。

[Back to Overview](#overview)



### ProjectManagerWindow
它继承了 `QMainWindow` 的所有主要功能。

项目管理器所特有的所有逻辑都发生在构造函数中，概述如下：
* 获取引擎信息，用正确的引擎版本信息设置窗口标题。
* 实例化`DownloadController`。
* 实例化用于屏幕管理的 `ScreensCtrl`。
* 构建相关 `ProjectManagerScreen` 枚举的列表，并使用 `ScreensCtrl` 为项目管理器的图形用户界面构建每个屏幕部件。
* 将 `ScreensCtrl` 实例设置为窗口的中心部件。
* 读取传入的 `startScreen` 和 `projectPath` 参数。
    * 如果设置了 `startScreen`，则首先强制加载项目主屏幕，然后过渡到所需屏幕。如果未设置，则默认为项目屏幕。
    * 如果设置了 `projectPath`，则通知 `ScreensCtrl` 实例当前高亮显示的项目路径。

[返回概述](#overview)




### ScreensCtrl
ScreensCtrl 是一个中央部件，用于存储项目管理器的所有图形用户界面信息。它由以下数据结构组成：

* `m_screenMap` - 用于访问 “项目管理器 ”中的所有可用屏幕。
* `m_screenStack` - 在`QStackedWidget`中存储所有屏幕。
* `m_tabWidget` - 项目管理器的三个主要选项卡（项目、Gem和引擎）都存储在这里。而该选项卡部件则作为 `m_screenStack` 的第一个元素存储。
* `m_screenVisitOrder` - 使用此功能可以跟踪 ProjectManager 屏幕的遍历历史。

所有函数概述如下：

| 函数 | 说明 |
| - | - |
| [`ScreensCtrl::ScreensCtrl`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L19) | 设置主屏幕布局。`m_tabWidget` 是推送到 `m_screenStack` 的第一个部件。|
| [`ScreensCtrl::BuildScreens`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L39) | 对于每个请求的屏幕，调用 `ResetScreen`。由于该调用是在应用程序生命周期开始时调用的，因此会触发 `ResetScreen` 中的初始化逻辑。|
| [`ScreensCtrl::FindScreen`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L47) | 搜索请求的 `ProjectManagerScreen` 枚举的 `ScreenWidget` 指针，如果找到则返回。否则返回一个 `nullptr`。|
| [`ScreensCtrl::GetCurrentScreen`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L60) | 首先检查 `m_screenStack` 中的当前 widget 是否为 `m_tabWidget`。如果是，则返回 `m_tabWidget` 的当前标签页。否则，返回 `m_screenStack` 中的任何当前部件。|
| [`ScreensCtrl::ChangeToScreen`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L72) | 如果可以，我们将使用 `ForceChangeToScreen` 过渡到所需的屏幕。|
| [`ScreensCtrl::ForceChangeToScreen`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L85) | 在项目管理器中更改屏幕图形用户界面的驱动程序代码。更多信息请见 [下文](#screensctrlforcechangetoscreenhttpsgithubcomo3deo3deblobce9765f7fca8feed2eefaaea3764805679d7425fcodetoolsprojectmanagersourcescreensctrlcppl85)| 
| [`ScreensCtrl::GoToPreviousScreen`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L147) | 如果 `m_screenVisitOrder` 不为空，我们会通过弹出 `m_screenVisitOrder` 来调用 `ForceChangeToScreen` 。|
|[`ScreensCtrl::ResetScreen`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L158) | 删除旧屏幕并重新创建。更多信息请见 [下文](#screensctrlresetscreenhttpsgithubcomo3deo3deblobce9765f7fca8feed2eefaaea3764805679d7425fcodetoolsprojectmanagersourcescreensctrlcppl158)|
| [`ScreensCtrl::ResetAllScreens`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L208) | 为每个屏幕调用 `ResetScreen`。|
| [`ScreensCtrl::DeleteScreen`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L216) | 如果可以在 `m_screenMap` 中找到屏幕，我们将继续从 `m_tabWidget` 中删除该屏幕（如果它是一个标签页），或者从 `m_screenStack` 中删除该屏幕（否则）。之后，我们将从 `m_screenMap` 中删除该条目。|
| [`ScreensCtrl::DeleteAllScreens`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L243) | 为所有屏幕调用 `DeleteScreen`。|
| [`ScreensCtrl::TabChanged`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L251) | [Slot 函数](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/ScreensCtrl.h#L50)，在 `m_tabWidget` 更换标签页时调用。这只会导致当前标签页通过 `NotifyCurrentScreen` 自行刷新。|
| [`ScreensCtrl::GetScreenTabIndex`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L260) | 如果是标签页，则读取屏幕在 `m_tabWidget` 中的索引位置，否则返回 -1。|

#### 深入解释
###### [`ScreensCtrl::ForceChangeToScreen`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L85)
* 首先，我们在 `m_screenMap` 中搜索所需的屏幕。
* 如果我们能找到它，那么如果它已经是当前屏幕，我们就运行 `NotifyCurrentScreen` 来刷新它。
* 否则，我们将设置 `m_screenStack` 的当前窗口部件（如果所需的屏幕是主标签页之一，则设置 `m_tabWidget`）。
  - 如果我们跟踪的是以前的访问，我们还要确保更新 `m_screenVisitOrder`。
* 最后，我们运行 `NotifyCurrentScreen` 和 `GoToScreen` 为我们的到来做好准备。



###### [`ScreensCtrl::ResetScreen`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L158)
* 使用`DeleteScreen`删除旧屏幕。
* 使用[`ScreenFactory::BuildScreen`](https://github.com/o3de/o3de/blob/ce9765f7fca8feed2eefaaea3764805679d7425f/Code/Tools/ProjectManager/Source/ScreenFactory.cpp#L27)重新构建屏幕。
* 如果屏幕应该是一个 Tab。
  - 用 screen 更新 `m_tabWidget` 。
  - 更新当前 widget `m_tabWidget` 和 `m_screenStack`。
  - 如果要恢复屏幕，请调用 `NotifyCurrentScreen`。
* 否则，将屏幕添加到 `m_screenStack` 并设置当前 widget。
  - 为新创建的屏幕调用 `NotifyCurrentScreen`。
* 用指向新屏幕的指针更新 `m_screenMap` 。
* 为新界面的过渡函数连接插槽。

[Back to Overview](#overview)


### ScreenWidget
如果要检查头文件，它只会包含存根函数。在本节中，我们将解释每个函数的高层目的，这样就能更容易地识别子类在重写给定函数时要做什么。
| Function | Description |
| - | - |
| [`ScreenWidget::GetScreenEnum`](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/ScreenWidget.h#L33) | 每个 `ScreenWidget` 的子类都将使用此函数来定义当前激活的屏幕类型。 |
| [`ScreenWidget::IsReadyForNextScreen`](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/ScreenWidget.h#L37) | 用于验证给定的[屏幕是否已准备好进行转换](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L77)，或是否由于某种原因必须暂停（例如，正在执行不可中断的任务）。目前尚未使用。|
| [`ScreenWidget::IsTab`](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/ScreenWidget.h#L41) | 此函数声明给定屏幕是否是 `m_tabWidget` 数据结构中的标签页。在 [`ScreensCtrl::ForceChangeToScreen`](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L118) 和 [`ScreensCtrl::DeleteScreen`](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L223)中，该函数用于将标签页作为边缘情况处理。覆盖此函数的类有 [`GemCatalogScreen`](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/GemCatalog/GemCatalogScreen.cpp#L782)、[`ProjectsScreen`](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/ProjectsScreen.cpp#L431)、[`ProjectGemCatalogScreen`](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/ProjectGemCatalogScreen.cpp#L121) 和 [`EngineScreenCtrl`](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/EngineScreenCtrl.cpp#L68)。|
| [`ScreenWidget::GetTabText`](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/ScreenWidget.h#L45) | 如果子类已定义，则获取选项卡屏幕的相应字符串值。|
| [`ScreenWidget::ContainsScreen`](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/ScreenWidget.h#L50) | 项目管理器中存在一些屏幕，如 [`EngineScreenCtrl`](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/EngineScreenCtrl.cpp#L75)，其内部包含其他较小的屏幕。这可用于检查给定屏幕是否存在于父屏幕内部。|
| [`ScreenWidget::GoToScreen`](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/ScreenWidget.h#L54) | 定义一个过渡函数，由子类决定是否移动到某个屏幕。 |
| [`ScreenWidget::Init`](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/ScreenWidget.h#L57) | 此函数会在 [`ScreenFactory::BuildScreen`](https://github.com/o3de/o3de/blob/c33daa6fc7f2ce175fca1e8325b9feac0c0d4d4e/Code/Tools/ProjectManager/Source/ScreenFactory.cpp#L78)屏幕构造器运行后不久出现。其目的是设置任何无法在构造时完成的系统钩子。这方面的两个例子可以在 [`CreateAGemScreen`](https://github.com/o3de/o3de/blob/c33daa6fc7f2ce175fca1e8325b9feac0c0d4d4e/Code/Tools/ProjectManager/Source/CreateAGemScreen.cpp#L79-L93)和 [`EditAGemScreen`](https://github.com/o3de/o3de/blob/c33daa6fc7f2ce175fca1e8325b9feac0c0d4d4e/Code/Tools/ProjectManager/Source/EditAGemScreen.cpp#L48-L64)中找到，其原理可以在 [这里](https://github.com/o3de/o3de/pull/12732/files/a8af31a375ed97152b0c0ae785c85a538c918de0#r1014510710)中找到。|
| [`ScreenWidget::NotifyCurrentScreen`](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/ScreenWidget.h#L61) | 屏幕需要刷新时调用的函数。所有必要的刷新逻辑都应在此表达。 |

`ScreenWidget` 还定义了各种 Qt [`signals`](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/ScreenWidget.h#L65-L70) ，通过连接到 `ScreensCtrl::ResetScreen` 中定义的 Qt [`slots`](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/ScreensCtrl.cpp#L201-L205)，这些 Qt [`signals`](https://github.com/o3de/o3de/blob/99f713702e5e0a8949e38c7b92bf00682c2633ca/Code/Tools/ProjectManager/Source/ScreenWidget.h#L65-L70) 可用于促进转换。

[返回概述](#overview)
