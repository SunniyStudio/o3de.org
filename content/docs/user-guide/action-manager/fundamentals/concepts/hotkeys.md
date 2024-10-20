---
linkTitle: 快捷键
title: 快捷键
description: 动作管理器快捷键 API的概述。
weight: 104
---

快捷键管理器系统提供了将快捷键与动作绑定的接口，以便在 Open 3D Engine (O3DE) 编辑器用户界面中通过键盘快捷键触发这些动作。


## 将部件分配给上下文

要确定从编辑器的哪个部分可以访问哪些快捷键，可以通过以下调用将部件分配到动作上下文：

```
QWidget* widget = new QWidget();

hotKeyManagerInterface->AssignWidgetToActionContext("o3de.context.identifier", widget);
```

每当用户输入键盘快捷键时，事件就会在编辑器中当前聚焦的部件上触发；如果该部件上没有定义快捷键，事件就会传递到该部件的父部件，并沿着部件层次向上传递，直到找到具有相应快捷键的动作或到达根部件为止。

可以将多个窗口部件分配给同一个动作上下文。如果将同一动作上下文中的多个动作设置为同一快捷键，则按下同一按键可触发多个快捷方式。

### 为动作设置快捷键

为动作设置快捷键只需定义哪种输入组合会触发该动作的行为，如下所示：

```
hotKeyManagerInterface->SetActionHotKey(
    "o3de.action.identifier",
    "Ctrl+N"
);
```

