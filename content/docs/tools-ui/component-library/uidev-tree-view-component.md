---
linktitle: 树视图
title: O3DE UI 树视图组件
description: 了解如何使用 O3DE UI 树视图组件在 O3DE 工具和 Gem 中为用户提供文件或列表导航界面。
toc: true
---

使用 **树视图** 组件，用户可以在 O3DE 中导航文件系统目录或分层数据列表。每个项 （如节点或分支） 都可以具有子项。可以展开项目以显示子项目。

**树视图** 组件通常用于以下场景：
+ 显示系统或预定义内容，例如检查器或系统设置页面中的设置。
+ 显示用户创建的内容，例如在 File Directory 或 Outliner 中。

来自 O3DE **资产浏览器**的示例：

![component tree view example](/images/tools-ui/component-tree-view-example.png)

## 使用指南

在使用树视图设计 UI 时，请遵循以下准则：

1. 当预计树状层次结构会很深时，例如文件目录中使用的树状视图，建议添加文件路径以帮助用户了解他们在路径中的位置。

## 基本树状视图

![component tree view basic](/images/tools-ui/component-tree-view-basic.png)

创建简单的树状视图，并支持显示分支线。

### 示例

```cpp
#include <AzQtComponents/Components/Widgets/TreeView.h>
#include <QTreeView>

// Create the tree view.
auto treeView = new QTreeView(parent);

// Set the model on the tree.
treeView->setModel(new MyModel);

// Set a BranchDelegate to support branch lines in your tree view.
treeView->setItemDelegate(new AzQtComponents::BranchDelegate());

// Show the connecting branch lines.
AzQtComponents::TreeView::setBranchLinesEnabled(treeView, true);
```

## C++ API 参考

有关 **树视图** API 的详细信息，请参阅 [O3DE UI 扩展 C++ API 参考](/docs/api/frameworks/azqtcomponents/namespace_az_qt_components.html) 中的以下主题:
+  [AzQtComponents::TreeView](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_tree_view.html)

相关的 Qt 文档包括以下主题：
+  [QTreeView Class](https://doc.qt.io/qt-5/qtreeview.html)
+  [QAbstractListModel Class](https://doc.qt.io/qt-5/qabstractlistmodel.html)
