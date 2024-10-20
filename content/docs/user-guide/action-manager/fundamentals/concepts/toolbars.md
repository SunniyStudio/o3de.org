---
linkTitle: 工具栏
title: 工具栏
description: 动作管理器工具栏 API的概述
weight: 103
---

工具栏管理系统提供了注册和扩展工具栏的接口，以便在开放式 3D 引擎 (O3DE) 编辑器用户界面中使用。


## 注册

新工具栏可通过提供其字符串标识符进行注册。

```
AzToolsFramework::ToolBarProperties toolBarProperties;
toolBarProperties.m_name = "ToolBar Name";

toolBarManagerInterface->RegisterToolBar(
    "o3de.toolBar.identifier",
    toolBarProperties
);
```

工具栏属性结构还可指定其他信息：

* 一个**name**字符串，用于在用户界面中显示人可读的描述（目前未使用）；


## 显示工具栏

工具栏注册后，可以检索其 `QToolBar*` 并在界面中显示。

```
QToolBar* toolBar = m_toolBarManagerInterface->GetToolBar("o3de.toolbar.identifier);

// Show the toolbar in widget
mainWindow->addToolBar(toolBar);
```

{{< note >}}
工具栏管理器系统会在工具栏添加动作或添加到工具栏的动作的启用状态发生变化时自动清除并重新填充工具栏。因此，`QToolBar*` 只能用于显示工具栏，而不能用于编辑其结构，因为任何更改都会在下次更新时丢失。
{{< /note >}}


## 添加动作

可以通过此 API 调用向工具栏添加动作：

```
int sortKey = 100;

toolBarManagerInterface->AddActionToToolBar(
    "o3de.toolBar.identifier",
    "o3de.action.identifier", 
    sortKey 
);
```

有关工具栏项排序的更多信息，请参阅 [排序键](/docs/user-guide/action-manager/fundamentals/architecture/sort-keys/)部分。


## 添加分隔符

要将工具栏划分为多个部分，可以添加分隔符。

```
int sortKey = 100;

toolBarManagerInterface->AddSeparatorToToolBar(
    "o3de.toolBar.identifier",
    sortKey 
);
```

有关工具栏项排序的更多信息，请参阅 [排序键](/docs/user-guide/action-manager/fundamentals/architecture/sort-keys/)部分。

如果按顺序一个接一个地添加多个分隔符，或在工具栏的开头或结尾添加多个分隔符，它们可能会被折叠为一个或根本不显示（默认 Qt 行为）。


## 添加带子菜单的动作

在添加动作时，可以为动作添加子菜单。菜单可通过动作图标旁边的向下箭头进入。

```
int sortKey = 100;

toolBarManagerInterface->AddActionWithSubMenuToToolBar(
    "o3de.toolBar.identifier",
    "o3de.action.identifier", 
    "o3de.subMenu.identifier", 
    sortKey 
);
```

有关工具栏项排序的更多信息，请参阅 [排序键](/docs/user-guide/action-manager/fundamentals/architecture/sort-keys/)部分。


## 添加小工具动作

还可以按以下方式在工具栏中添加小工具：

```
int sortKey = 100;

toolBarManagerInterface->AddWidgetToToolBar(
    "o3de.toolBar.identifier", 
    "o3de.widgetAction.identifier", 
    sortKey
);
```

有关工具栏项排序的更多信息，请参阅 [排序键](/docs/user-guide/action-manager/fundamentals/architecture/sort-keys/)部分。


# 工具栏区域

可以向工具栏管理器注册主窗口的工具栏区域，以便通过工具栏管理器 API 访问该区域。

```
// Retrieve an existing Main Window
QMainWindow* mainWindow;

toolBarManagerInterface->RegisterToolBarArea(
    "o3de.toolBarArea.identifier", 
    mainWindow, 
    Qt::ToolBarArea::TopToolBarArea
);
```

工具栏区域注册后，外部 Gems 可通过其字符串标识符向工具栏区域添加新工具栏。


### 添加工具栏

```
int sortKey = 100;

menuManagerInterface->AddMenuToMenuBar(
    "o3de.toolBarArea.identifier",
    "o3de.toolBar.identifier", 
    100
);
```

有关工具栏项排序的更多信息，请参阅 [排序键](/docs/user-guide/action-manager/fundamentals/architecture/sort-keys/)部分。
