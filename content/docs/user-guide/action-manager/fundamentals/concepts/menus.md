---
linkTitle: 菜单
title: 菜单
description: 动作管理器菜单 API的概述。
weight: 102
---

菜单管理系统提供注册和扩展菜单的接口，以便在开放式 3D 引擎 (O3DE) 编辑器用户界面中使用。


## 注册

新菜单可通过提供其字符串标识符进行注册。

菜单属性结构还可指定其他信息：

* 一个**name**字符串，它的目的是在用户界面中显示一个人类可读的描述；


```
AzToolsFramework::MenuProperties menuProperties;
menuProperties.m_name = "Menu Name";

menuManagerInterface->RegisterMenu(
    "o3de.menu.identifier",
    menuProperties
);
```


## 显示菜单

菜单注册后，可以在界面中显示。

```
// Display a menu at a specific screen position.
menuManagerInternalInterface->DisplayMenuAtScreenPosition("o3de.menu.identifier", screenPosition);

// Display a menu under the cursor.
menuManagerInternalInterface->DisplayMenuUnderCursor("o3de.menu.identifier");
```


## 添加动作

可以通过此 API 调用为菜单添加动作：

```
int sortKey = 100;

menuManagerInterface->AddActionToMenu(
    "o3de.menu.identifier",
    "o3de.action.identifier", 
    sortKey 
);
```

有关菜单项排序的更多信息，请参阅 [排序键](/docs/user-guide/action-manager/fundamentals/architecture/sort-keys/)部分。


## 添加分隔符

要将菜单划分为多个部分，可以添加分隔符。

```
int sortKey = 100;

menuManagerInterface->AddSeparatorToMenu(
    "o3de.menu.identifier",
    sortKey 
);
```

有关菜单项排序的更多信息，请参阅 [排序键](/docs/user-guide/action-manager/fundamentals/architecture/sort-keys/)部分。

如果按顺序一个接一个地添加多个分隔符，或者在菜单的开头或结尾添加多个分隔符，它们可能会被折叠为一个或根本不显示（Qt 默认行为）。


## 添加子菜单

可以通过以下 API 调用将现有菜单添加为子菜单。

```
int sortKey = 100;

menuManagerInterface->AddSubMenuToMenu(
    "o3de.menu.identifier",
    "o3de.subMenu.identifier",
    sortKey 
);
```

有关菜单项排序的更多信息，请参阅[排序键](/docs/user-guide/action-manager/fundamentals/architecture/sort-keys/)部分。

请注意，如果用户试图向自己添加菜单，或者如果该动作会导致创建循环依赖关系，从而使编辑器停滞，则函数调用将失败。


## 添加部件动作

小部件也可以按如下方式添加到菜单中：

```
int sortKey = 100;

menuManagerInterface->AddWidgetToMenu(
    "o3de.menu.identifier", 
    "o3de.widgetAction.identifier", 
    sortKey
);
```

有关菜单项排序的更多信息，请参阅 [排序键](/docs/user-guide/action-manager/fundamentals/architecture/sort-keys/)部分。


# 菜单栏

可以将主窗口的菜单栏区域注册到菜单管理器，这样就可以通过菜单管理器 API 访问该区域。

```
// Retrieve an existing Main Window
QMainWindow* mainWindow;

menuManagerInterface->RegisterMenuBar(
    "o3de.menuBar.identifier", 
    mainWindow
);
```

注册菜单栏后，外部 Gems 可通过字符串标识符向菜单栏添加新菜单。


## 添加菜单

向菜单栏添加菜单的方法与向菜单添加子菜单的方法相同。

```
int sortKey = 100;

menuManagerInterface->AddMenuToMenuBar(
    "o3de.menuBar.identifier",
    "o3de.menu.identifier", 
    100
);
```

有关菜单项排序的更多信息，请参阅 [排序键](/docs/user-guide/action-manager/fundamentals/architecture/sort-keys/)部分。
