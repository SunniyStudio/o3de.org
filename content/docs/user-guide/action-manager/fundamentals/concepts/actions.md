---
title: 动作
linkTitle: 动作
description: 动作是动作管理器（Action Manager）的一个元素，允许用户注册并触发 Open 3D Engine (O3DE) 中的编辑器行为。
weight: 101
---

[动作管理器](/docs/user-guide/action-manager)系统提供了注册动作和相关框架的接口，以便与**Open 3D Engine (O3DE)** 编辑器进行交互。

## 动作上下文

动作上下文是一个拥有动作集合的对象。

动作上下文由以下部分组成

* 一个字符串**标识符**，以便在 API 调用中引用；
* 一个**名称**字符串，这是一个可在用户界面中显示的人读描述；


### 注册

可以通过以下 API 调用注册新的动作上下文：

```
AzToolsFramework::ActionContextProperties contextProperties;
contextProperties.m_name = "Test Action Context";

actionManagerInterface->RegisterActionContext(
    "o3de.context.identifier", 
    contextProperties
);
    
```

### 关联部件

动作上下文的目的是将其所拥有的一组动作绑定到编辑器中的一个部件上，这样就可以根据用户与编辑器中的哪一部分进行交互，通过键盘快捷键访问这些部件。
更多信息，请参阅 [快捷键](./hotkeys)页面。


## 动作

动作是可重复使用的编辑器交互的抽象。创建时无法访问，但一旦添加到菜单或工具栏，或为其分配了热键，用户就可以访问。

动作包括

* 一个字符串**标识符**，以便在应用程序接口调用中引用；
* 一个字符串**上下文标识符**，用于定义拥有该动作的动作上下文；
* 一个函数形式的行为，每当触发动作时都会被调用。


### 注册

可通过以下 API 调用注册动作：

```
actionManagerInterface->RegisterAction(
    "o3de.context.identifier",
    "o3de.action.identifier",
    {},
    []
    {
        /* Trigger Behavior */
    }
);
```

动作可以以 lambda 或函数引用的形式提供。

附加描述符和装饰器可作为动作属性添加：

* 一个**name**字符串，简明扼要地描述动作的效果，用于菜单或工具栏等用户界面；
* 一个**description**字符串，可用于向用户提供附加信息，通常在用户界面中悬停动作时显示在工具提示中；
* 一个**category**字符串，用于对用户界面中的类似字符串进行分类；
* 指向 svg 图像的**iconPat**，该图像将用于在菜单和工具栏中表示动作。它必须是 qrc 资源文件中定义的 svg 文件的 Qt 路径。

```
AzToolsFramework::ActionProperties actionProperties;
actionProperties.m_name = "Action Name";
actionProperties.m_description = "A longer description of what the action does.";
actionProperties.m_category = "Category";
actionProperties.m_iconPath = ":Path/to/icon.svg";

actionManagerInterface->RegisterAction(
    "o3de.context.identifier",
    "o3de.action.identifier",
    actionProperties,
    []
    {
        /* Trigger Behavior */
    }
);
```


### 启用状态回调

动作的启用状态可以根据编辑器的状态进行更改；例如，某些动作只有在实体选择不为空时才会启用。

为此，可以在动作上安装启用状态回调，该回调将根据任意标准返回启用状态。

```
actionManagerInterface->InstallEnabledStateCallback(
    "o3de.action.identifier",
    []() -> bool
    {
        bool enabledState = true;
        return enabledState;
    }
);    
```

当安装启用状态回调时，会重新计算并自动设置动作的启用状态。
请注意，动作的状态不会定期更新，因此需要建立一种机制，以便在回调中使用的标准可能改变其结果时更新动作。更多信息请查看更新动作部分。

可以在一个动作上安装多个已启用的状态回调。在这种情况下，所有回调的结果都将作为 AND 链进行计算和解析，这意味着只有当所有回调都返回 true 时，动作才会启用。

另外请注意，即使在计算回调后认为动作已启用，该动作也可能从动作上下文模式系统中停用。更多信息，请参阅 [动作上下文模式切换](/docs/user-guide/action-manager/fundamentals/concepts/actions/#action-context-mode-switching)部分。


### 可选中的动作

可将实现编辑器行为切换的动作注册为可选中动作。这些动作将利用 QAction 的 `checked`状态，在用户界面中向用户可视化底层功能的状态。

{{< note >}}
可复选动作的值可以查询和更新，但不能直接设置。使用可复选动作可视化底层设置，而不是存储或查询其值。
{{< /note >}}

要登记一个可核对的动作，需要一个单独的注册调用：

```
actionManagerInterface->RegisterCheckableAction(
    "o3de.context.identifier",
    "o3de.action.identifier",
    {},
    []
    {
        /* Trigger Behavior */
    },
    []() -> bool
    {
        /* Checked State Callback */
        bool checkedState = true;
        return checkedState;
    }
);
```
选中状态回调将在注册时自动调用，返回值将用于设置选中状态。要随时间刷新值，请参阅[更新动作](#updating-actions)部分。

可选中动作的状态会根据其所在的用户界面元素以不同的方式可视化：
- 如果添加到菜单中，选中的可复选动作会在动作名称左侧显示一个小复选标记；
- 如果添加到工具栏中，选中的可复选动作图标会被一个矩形包围，以表示其激活/选中状态。


### 可见性

`ActionProperties` 结构包括按动作粒度提供可见性的设置。

* `m_menuVisibility` 可用于确定是否在菜单中显示该动作；
* `m_toolBarVisibility` 可用于确定是否在工具栏中显示动作。

更多信息，请参阅 [动作可见性](/docs/user-guide/action-manager/fundamentals/architecture/visibility/)。


## 更新动作

一个动作有两个需要保持更新的状态：启用状态和选中状态，前者决定是否可以触发该动作，后者用于在用户界面中显示内部设置的状态。如前所述，这两种状态都由附加到动作的相应回调驱动；不过，为了优化性能，系统不会自动刷新动作的状态。相反，每当有可能触发其中任一状态发生变化的事件发生时，系统都会依靠其所属系统保持动作的最新状态。

为了简化动作，同时又能提供更多的定制解决方案，该架构提供了两种不同的方法来在需要时刷新动作。


### 动作更新器

由于动作管理器的架构支持模块化和可扩展性，因此它致力于让 Gems 可以轻松挂钩编辑器定义的事件，以驱动自定义动作的状态，而无需直接包含其定义或重复大量模板代码。

动作更新器就是为实现这一目标而设计的。动作更新器实质上是一个标签（更新器标识符）下的动作标识符列表。一个系统若要实现触发用户界面更改动作的事件，可通过以下 API 调用来定义动作更新器：

```
actionManagerInterface->RegisterActionUpdater("o3de.updater.identifier");
```

每当事件触发，系统就必须触发更新器：

```
actionManagerInterface->TriggerActionUpdater("o3de.updater.identifier");
```

设置完成后，任何系统都可以通过以下调用选择将其动作添加到更新程序中：

```
actionManagerInterface->AddActionToUpdater(
    "o3de.updater.identifier", "o3de.action.identifier");
```

这样，每当事件触发时，所有添加到更新器的动作都会被刷新。
可以根据需要在任意多个更新器中添加动作，没有任何限制。

{{< note >}}
注册更新程序的系统也必须在适当的时候触发更新程序。
{{< /note >}}


### 手动更新

在非常特殊的情况下，可能需要手动触发单个动作的更新。这并不是推荐的实现方式，因为它会妨碍可扩展性，但在特殊情况下，尤其是在处理遗留代码时，可能会有所帮助。

```
actionManagerInterface->UpdateAction("o3de.action.identifier");
```

## 动作上下文模式切换

O3DE 编辑器是一个复杂的工具，支持多个工作流程；有时需要批量启用或禁用一组动作，因为有些交互变得不支持了。

为了简化这一过程，动作管理器架构集成了一种称为 “动作上下文模式 ”的机制。

### 注册新模式

可以通过以下方式在动作上下文中注册新模式：

```
actionManagerInterface->RegisterActionContextMode(
    "o3de.context.identifier",
    "o3de.context.mode.identifier"
);
```

每个动作上下文都有自己的一套模式。


### 为模式添加动作

注册模式后，就可以为其分配动作。这与在更新程序中添加动作的原理类似。

```
actionManagerInterface->AssignModeToAction(
    "o3de.context.mode.identifier",
    actionIdentifier
);    
```

一个动作可以分配给任意数量的模式。要正确完成此动作，必须为指定的动作上下文注册模式。

未指定模式的动作（注册后的默认状态）将显示在所有模式中。如果一个动作只分配给与激活的动作上下文模式不同的模式，那么该动作将被停用，直到激活的模式被更改。

有关菜单和工具栏中停用动作的可见性的更多信息，请参阅 [动作可见性](/docs/user-guide/action-manager/fundamentals/architecture/visibility/) 部分。


### 切换模式

要更改当前激活的动作上下文模式，可调用此 API 函数。

```
actionManagerInterface->SetActiveActionContextMode(
    EditorIdentifiers::MainWindowActionContextIdentifier,
    modeIdentifier
);
```

当激活的动作上下文模式发生变化时，所有未分配给新模式的动作将被停用，然后所有分配给该模式的动作将被激活。如果一个动作同时被分配到先前的和新的活动模式，或者既没有被分配到这两种模式，那么它的激活状态将保持不变。


### 默认值

每个动作上下文都有一个默认的动作上下文模式，其标识符仅为 `default`。可以将动作分配给默认模式，以确保该动作在其他模式下被停用（除非它也被分配给新模式）。

默认情况下，动作不分配给任何模式。这意味着它们会出现在每个已注册的模式中。当模式被指定后，行为将切换为只出现在被指定的模式中。


##小部件动作

系统支持添加 Widget 动作，以便在界面中使用。

### 注册

注册 Widget 动作与注册普通动作非常相似，但有一些不同之处：

* Widget 动作无需分配给动作上下文。
* 不提供触发函数，而是提供一个工厂函数。只要需要部件的实例，系统就会使用它，从而允许在界面中显示多个部件副本。

```
AzToolsFramework::WidgetActionProperties widgetActionProperties;
widgetActionProperties.m_name = "Widget Name";
widgetActionProperties.m_category = "Category";

auto outcome = m_actionManagerInterface->RegisterWidgetAction(
    "o3de.widgetAction.identifier",
    widgetActionProperties,
    []() -> QWidget*
    {
        /* Widget Factory */
        return new PrefabEditVisualModeWidget();
    }
);
```

{{< note >}}
通过该工厂创建的 widget 将作为请求界面（如菜单或工具栏）的父节点，该界面将对其进行控制，并确保在不再需要时正确地将其删除。
{{< /note >}}
