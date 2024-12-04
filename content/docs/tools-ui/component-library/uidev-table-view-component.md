---
linktitle: 表视图
title: O3DE UI 表视图组件
description: 了解如何使用 O3DE UI 表视图组件在 O3DE 工具和 Gem 中显示结构化数据列。
toc: true
---

使用 **table view** 组件以表格格式显示多列结构化数据。默认情况下，此组件使用可排序列和“斑马条带化”（其中行的背景色交替）来帮助您创建易于阅读、可扫描和可排序的数据表示。

![component table view example](/images/tools-ui/component-table-view-example.png)

{{< note >}}
`AzQtComponents::TableView` 实际上派生自`QTreeView`，而不是`QTableView`，以提供对行大小的更多自定义。
{{< /note >}}

## Basic table view

![component table view basic](/images/tools-ui/component-table-view-basic.png)

创建简单的日志记录表视图。

{{< note >}}
如果表格视图与 [树视图](/docs/tools-ui/component-library/uidev-tree-view-component) 结合使用，则可能需要使用 setAlternatingRowColors（false） 函数在其中一个小部件中关闭斑马纹条带化。
{{< /note >}}

### 示例

```cpp
#include <AzQtComponents/Components/Widgets/TableView.h>
#include <AzToolsFramework/UI/Logging/LogTableModel.h>
#include <AzToolsFramework/UI/Logging/LogLine.h>
#include <QDateTime>

// Create a log table model for this example.
auto logModel = new AzToolsFramework::Logging::LogTableModel(this);

// Create the table view.
auto tableView = new AzQtComponents::TableView(parent);

// Set the model for the table.
tableView->setModel(logModel);

// Add a few lines of sample data.
logModel->AppendLine(
    AzToolsFramework::Logging::LogLine(
        "An informative message for debugging purposes.",
        "Window",
        AzToolsFramework::Logging::LogLine::TYPE_MESSAGE,
        QDateTime::currentMSecsSinceEpoch()));

logModel->AppendLine(
    AzToolsFramework::Logging::LogLine(
        "A warning message for things that may not have gone as expected.",
        "Window",
        AzToolsFramework::Logging::LogLine::TYPE_WARNING,
        QDateTime::currentMSecsSinceEpoch()));

logModel->AppendLine(
    AzToolsFramework::Logging::LogLine(
        "Critical error message, something went wrong.",
        "Window",
        AzToolsFramework::Logging::LogLine::TYPE_ERROR,
        QDateTime::currentMSecsSinceEpoch()));
```

## C++ API 参考

有关 **table view** API 的详细信息，请参阅 [O3DE UI 扩展 C++ API 参考](/docs/api/frameworks/azqtcomponents/namespace_az_qt_components.html) 中的以下主题:
+  [AzQtComponents::TableView](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_table_view.html)

相关的 Qt 文档包括以下主题：
+  [QTreeView Class](https://doc.qt.io/qt-5/qtreeview.html)
+  [QAbstractListModel Class](https://doc.qt.io/qt-5/qabstractlistmodel.html)
