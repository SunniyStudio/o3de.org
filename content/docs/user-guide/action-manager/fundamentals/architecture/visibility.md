---
title: 动作可见性
linktitle: 可见性
description: Open 3D Engine (O3DE) 动作管理器中动作的可见性属性概览。
weight: 204
---

动作的可见性属性决定了动作何时显示在用户界面中，并根据其激活状态和指定的动作上下文模式成为工具栏或菜单的一部分。
这些行为都是动态的，因为每当编辑器的状态发生变化、动作的启用状态被切换或当前激活的动作上下文模式发生变化时，都会重新评估这些行为。


##可见性设置值

动作的可见性属性可设置为三种值之一。


#### `AlwaysShow`

该动作始终显示为其指定的菜单或工具栏的一部分。
如果该动作的启用状态设置为假，或者该动作分配给了与当前活动模式不同的动作上下文模式，则该动作将显示为可见，但会以灰色显示，无法访问。


#### `OnlyInActiveMode`

该动作只有在分配给当前活动的动作上下文模式或无动作上下文模式时才会显示。
如果动作的启用状态设置为 “false”，则动作可见，但显示为灰色，无法访问。
如果该动作被分配到与当前活动模式不同的动作上下文模式，则该动作不会出现在菜单或工具栏中。


#### `HideWhenDisabled`

只有当动作被分配到当前激活的动作上下文模式或无动作上下文模式，且其启用状态设置为 “true ”时，动作才会显示。
如果动作的启用状态设置为假，或者动作被分配到与当前活动模式不同的动作上下文模式，则该动作不会显示在菜单或工具栏中。
该动作永远不会显示为灰色且无法访问。


## Default Values

可见性属性的默认值有所不同，以符合编辑器预期的用户体验：

- 菜单可见性默认设置为`HideWhenDisabled`。这样做的目的是让菜单只显示在当前情况下可用的动作，只有在当前情况下有明确的路径来满足启用该动作的要求时，或者如果这样做能提高工作流程的可发现性，动作才会显示为灰色。
- 工具栏的可见性默认设置为`OnlyInActiveMode`。由于它们始终显示，因此当用户与改变动作启用状态的元素（如实体选择）交互时，如果工具栏中的按钮和小部件会左右移动，就会很不协调。相反，当活动动作上下文模式发生变化时，工具栏中的动作也会发生巨大变化，这才是最有价值的。


## 为动作指定可见性属性

可见性属性可在动作注册时使用 `ActionProperties` 结构设置，具体如下：

```
AzToolsFramework::ActionProperties actionProperties;
actionProperties.m_menuVisibility = AzToolsFramework::ActionVisibility::AlwaysShow;
actionProperties.m_toolBarVisibility = AzToolsFramework::ActionVisibility::AlwaysShow;

m_actionManagerInterface->RegisterAction(
    "o3de.context.identifier",
    "o3de.action.identifier",
    actionProperties,
    []() {}
);
```
