---
linktitle: 痕迹导航
title: O3DE UI 痕迹导航组件
description: 使用 O3DE UI 痕迹导航组件作为导航工具，向用户显示他们沿着分层路径的位置。痕迹导航还允许用户跳转到路径中的其他点。
toc: true
---

使用 breadcrumbs 组件，使用户能够沿分层路径跟踪其位置。痕迹导航包括从主页到用户当前位置的路径。痕迹导航中的点是指目录或节点。用户可以通过选择其中一个痕迹导航来轻松移动路径中的位置。

有关“痕迹导航”概念的示例，请参阅 O3DE **动画编辑器**中的 **动画图形** 导航。

![component breadcrumbs navigation example](/images/tools-ui/component-breadcrumbs-navigation-example.gif)

在 **Anim Graph** 痕迹导航中，图表的名称显示为顶级导航。您打开的每个节点都显示为可单击的超链接。

## 面包屑小部件剖析

痕迹导航具有多个自定义选项。标准的水平布局包括以下功能：

![component breadcrumbs anatomy](/images/tools-ui/component-breadcrumbs-anatomy.png)

1.  **路径历史记录导航**

    （可选）使用户能够在其痕迹导航路径选择的历史记录中向后和向前导航。例如，如果您浏览到列表中的新路径或选择新路径，然后选择 **返回** 按钮，您将导航回上一个痕迹导航位置。

1.  **面包屑路径**

    显示从 root 到 tail 的完整路径。用户可以选择路径中的任何点来设置新路径。以前选择的路径会自动添加到导航历史记录中。

1.  **“浏览”按钮**

    （可选）从技术上讲，它不是面包屑的一部分。浏览按钮通常很有用，因为它使用户能够选择全新的路径，而不是选择当前路径中的其他点。在浏览按钮处理程序代码中将新路径推送到痕迹导航小部件。

1.  **Truncation 菜单**

    如果整个痕迹导航路径无法容纳在分配的空间内，则路径上不适合的点将堆叠在下拉菜单中。用户可以从截断菜单中选择这些点。

## 基本痕迹导航

![component breadcrumbs basic](/images/tools-ui/component-breadcrumbs-basic.png)

最简单的痕迹导航示例包括 **breadcrumbs** 小部件和一个可选的初始路径。您的代码可以通过连接到 `pathChanged`信号来响应路径更改。 或者，您可以调用 'pushPath' 来设置痕迹导航状态以匹配项目的当前状态。

将路径 QString 传递到`pushPath`时，使用正斜杠 ('/') 或反斜杠 ('\\\\') 作为路径分隔符。

### 示例

```cpp
#include <AzQtComponents/Components/Widgets/BreadCrumbs.h>

// Create a new breadcrumbs widget.
AzQtComponents::BreadCrumbs* breadCrumbs = new AzQtComponents::BreadCrumbs(parent);

// (Optional) Set the initial path.
QString initialPath = "C:/Documents/SubDirectory1/Subdirectory2/SubDirectory3";
breadCrumbs->pushPath(initialPath);

// Add the widget to a previously defined QHBoxLayout.
layout->addWidget(breadCrumbs);

// Listen for path changes.
connect(breadCrumbs, &AzQtComponents::BreadCrumbs::pathChanged, this, [](const QString& newPath) {
    // Handle path change as needed by your project.
});

// Add breadcrumbs to a UI layout as needed.
```

## 带有路径导航和浏览的痕迹导航

![component breadcrumbs navigation and browse](/images/tools-ui/component-breadcrumbs-navigation-and-browse.png)

在某些情况下，用户能够在其导航历史记录中来回导航非常有用。此外，您可能希望提供一个浏览按钮来选择新路径。

在以下示例中，`createBackForwardToolBar`提供向前和向后导航箭头，AzQtComponents 中的`NavigationButton::Browse`提供文件浏览器按钮。

### 示例

```cpp
#include <AzQtComponents/Components/Widgets/BreadCrumbs.h>
#include <QFileDialog>

// Create a new breadcrumbs widget.
AzQtComponents::BreadCrumbs* breadCrumbs = new AzQtComponents::BreadCrumbs(parent);

// (Optional) Set the initial path.
QString initialPath = "C:/Documents/SubDirectory1/Subdirectory2/SubDirectory3";
breadCrumbs->pushPath(initialPath);

// Create the browse button widget.
auto browseButton = breadCrumbs->createButton(AzQtComponents::NavigationButton::Browse);

// Add the widgets to a previously defined QHBoxLayout.
layout->addWidget(breadCrumbs->createBackForwardToolBar());
layout->addWidget(breadCrumbs->createSeparator());
layout->addWidget(breadCrumbs);
layout->addWidget(breadCrumbs->createSeparator());
layout->addWidget(browseButton);

// Update the breadcrumb path from the output of the browse button.
connect(browseButton, &QPushButton::pressed, breadCrumbs, [breadCrumbs] {
    QString newPath = QFileDialog::getExistingDirectory(breadCrumbs, "Select a new path");
    if (!newPath.isEmpty())
    {
        breadCrumbs->pushPath(newPath);
    }
});

// Listen for path changes.
connect(breadCrumbs, &AzQtComponents::BreadCrumbs::pathChanged, this, [](const QString& newPath) {
    // Handle path change as needed by your project.
});

// Add the breadcrumbs to a UI layout as needed.
```

## C++ API 参考

有关 **痕迹导航** API 的详细信息，请参阅 [O3DE UI 扩展 C++ API 参考](/docs/api/frameworks/azqtcomponents/namespace_az_qt_components.html) 中的以下主题：
+  [AzQtComponents::BreadCrumbs](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_bread_crumbs.html)
