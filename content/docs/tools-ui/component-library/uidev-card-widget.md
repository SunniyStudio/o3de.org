---
linktitle: 卡片
title: O3DE UI卡片小部件
description: 使用 O3DE UI 卡片小部件作为容器，将组件属性设置和操作组织在一起。
toc: true
---

使用卡片在高度交互、灵活的容器中显示信息。用户可以轻松地堆叠、重新排序和折叠这些容器。卡片中的所有内容都应仅与一个构思相关。卡片应易于扫描以获取相关且可操作的信息，并且主要用于显示组件或类的可编辑详细信息。

例如，卡片可能包括常用属性、操作按钮、高级设置和用于其他操作的上下文菜单：

![component card concept](/images/tools-ui/component-card-concept.png)

为了方便用户扫描卡片，请使内容布局保持一致。这包括对标题和内容使用相同的字体大小、样式和间距。与图像和图标的使用以及主要和次要操作（如果需要）保持一致。卡片标题是允许用户轻松扫描内容的关键，因此请确保标题高度可见。

## 卡片小部件剖析

卡片允许一定数量的自定义。卡片的基本布局包括以下功能：

![component card anatomy](/images/tools-ui/component-card-anatomy.png)

1.  **Expander** 和 **header bar**

    当用户单击左上角的箭头时，卡片会展开或折叠。

    如果卡片可以移动，则用户按住并拖动标题栏以重新定位卡片。

1.  **Card icon**

    （可选）卡片可以具有与其用途相关的独特图标。有关图标的完整列表，请参阅 [O3DE 组件图标](/docs/tools-ui/icon-assets/uidev-component-icons/).
   
    {{< note >}}
新组件需要两个图标：

+ 一个 16 x 16 的 SVG，带有用于透视窗口的背景框。
+ 用于编辑器中其他任何位置的 SVG，没有背景框。
{{< /note >}}

1.  **Card name**

    每张卡片都有自己的名称，与其用途相关。您可以将名称配置为从白色更改为橙色，以表示小组件中的内容已被修改。

1.  **Help icon**

    （可选）卡可以显示指向描述卡功能的文档的链接。

1.  **Context menu icon**

    （可选）当用户选择此图标时，将打开卡的上下文菜单。也可以通过用鼠标右键单击卡标题来打开上下文菜单。

1.  **Card content widget**

    此区域包含卡片的内容 Widget。大多数卡片都包含 [反射属性编辑器](./uidev-reflected-property-editor-component/)  组件，但卡片可以容纳任何小部件。

1.  **Advanced options**

    （可选）设置辅助内容 Widget 后，此处会显示其标签。当用户选择标签左侧的箭头时，Advanced options （高级选项） 菜单将展开或折叠。您可以自定义标签的文本。

1.  **Notifications**

    （可选）[通知](#card-with-notification) 将在此处显示。可以将按钮或其他小部件添加到通知中，作为解决它们的一种方式。

1.  **Call to action region**

    （可选）补充操作应放在卡片底部。我们将其称为 Call to Action 区域。通常，行动动员由添加到内容 Widget 底部的按钮或卡片通知表示。避免在此处使用主按钮，因为它们应该保留给页面上的整体操作。相反，请在此处使用二级或三级按钮、链接按钮和图标按钮。

## 基础卡片

![component card basic](/images/tools-ui/component-card-basic.png)

最简单的卡片由以下组件组成：
+ 卡头
  + 展开/折叠图标
  +（可选）卡片图标
  + 卡名
  +（可选）上下文菜单（默认启用）
+ 卡片内容

### 示例

```cpp
#include <AzQtComponents/Components/Widgets/Card.h>
#include <AzQtComponents/Components/Widgets/CardHeader.h>

// Create the card.
AzQtComponents::Card* card = new AzQtComponents::Card(parent);

// Set the card icon.
card->header()->setIcon(QIcon(QStringLiteral(":/stylesheet/img/some_icon.svg")));

// Set the card title.
card->setTitle("Example Card");

// Disable the context menu (enabled by default).
card->header()->setHasContextMenu(false);

// Add the content widget.
card->setContentWidget(new QWidget());

// Add the card to a UI layout as needed.
```

## 带有上下文菜单和帮助图标的卡片

![component card context and help](/images/tools-ui/component-card-context-and-help.png)

在卡片上显示帮助图标，以将用户重定向到文档网页。

以下代码演示了如何设置帮助链接并启动上下文菜单。

### 示例

```cpp
// Set the card help icon.
card->header()->setHelpURL("https://o3de.org/docs/");

// Enable the context menu (enabled by default).
card->header()->setHasContextMenu(true);

// Some base code to add a context menu.
// contextMenuRequested is triggered both by clicking the context menu icon
// and by right clicking the Card header.
connect(ui->basicCard, &AzQtComponents::Card::contextMenuRequested, this, [](const QPoint& point) {
    QMenu basicMenu;
    basicMenu.addAction(new QAction("Some Action", nullptr));
    basicMenu.exec(point);
});
// Note: The menu can be created in advance and executed on contextMenuRequested.
```

## 包含次要内容的卡片

![component card secondary content](/images/tools-ui/component-card-secondary-content.png)

显示辅助内容 Widget。它的标题是可定制的。

### 示例

```cpp
// Set the secondary content title.
card->setSecondaryTitle("Advanced Options");

// Add the secondary content widget.
card->setSecondaryContentWidget(new QWidget());
```

## 包含已修改内容的卡片

![component card content modified](/images/tools-ui/component-card-content-modified.png)

将 Care 标题配置为在内容编辑、与父切片不同或尚未保存时更改颜色。我们建议在所有卡上启用此功能。

### 示例

```cpp
// Set the content modified state in response to content change.
card->header()->setContentModified(true);
```

## 禁用卡片

![component card disabled](/images/tools-ui/component-card-disabled.png)

完全禁用卡，包括标题栏图标和子小组件。

{{< note >}}
如果希望禁用卡片内容，但允许用户仍使用标题栏中的帮助按钮和上下文菜单，请使用 [Mock Disabled](#mock-disabled-card)状态。
{{< /note >}}

### 示例

```cpp
// Disable the card widget using a Qt function. All functionality of the card is disabled.
card->setEnabled(false);
```

## 模拟禁用的卡片

![component card mock disabled](/images/tools-ui/component-card-mock-disabled.png)

禁用卡上的主要和辅助小组件，但保持启用标题栏。以下卡功能仍然有效：
+ 展开/折叠
+ 帮助
+ 上下文菜单

### 示例

```cpp
// Display the card as disabled while retaining functionality in the header bar.
card->mockDisabledState(true);
```

## 具有警告状态的卡

![component card warning state](/images/tools-ui/component-card-warning-state.png)

在卡头上设置警告状态。

{{< note >}}
您可以单独设置禁用状态和警告状态。
{{< /note >}}

### 示例

```cpp
// Display a warning icon in the header bar to indicate a warning state.
card->header()->setWarning(true);
```

## 带通知的卡片

![component card notification](/images/tools-ui/component-card-notification.png)

添加通知以指示错误配置和其他错误。问题解决后删除通知。

使用以下命令自定义通知：
+ 在线留言
+（可选）操作按钮
+（可选）自定义小部件

{{< note >}}
除了按钮之外，您还可以使用`addFeature(QWidget*)` 将自定义窗口小部件添加到警告通知中。
{{< /note >}}

### 示例

```cpp
#include <AzQtComponents/Components/Widgets/CardNotification.h>

// Create a new notification with a message string.
AzQtComponents::CardNotification* notification =
    card->addNotification("A warning message indicating a conflict or problem.");

// (Optional) Add a custom button to the notification.
QPushButton* button = notification->addButtonFeature("Clear Warnings");

// Connect a behavior to the button.
connect(button, &QPushButton::clicked, ui->functionalCard, &AzQtComponents::Card::clearNotifications);
```

## C++ API 参考

有关 **card** API 的详细信息，请参阅 [O3DE UI 扩展 C++ API 参考](/docs/api/frameworks/azqtcomponents/namespace_az_qt_components.html) 中的以下主题：
+  [AzQtComponents::Card](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_card.html)
+  [AzQtComponents::CardHeader](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_card_header.html)
+  [AzQtComponents::CardNotification](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_card_notification.html)

## 相关链接

有关 **card** 组件的其他信息，请参阅以下主题：
+ [反射属性编辑器](./uidev-reflected-property-editor-component)
