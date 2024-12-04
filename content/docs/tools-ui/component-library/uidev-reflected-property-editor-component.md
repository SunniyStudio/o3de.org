---
linktitle: 反射属性编辑器
title: O3DEUI 反射属性编辑器
description: 使用 O3DE UI 反射属性编辑器在 O3DE 工具和 Gem 中自动布局用户可编辑的属性。
toc: true
---

**反射的属性编辑器** 会自动为使用编辑上下文反映的用户可编辑属性布置控件。它经常用作内容小部件来填充[card](./uidev-card-widget)组件。

![component reflected property editor in card](/images/tools-ui/component-reflected-property-editor-in-card.png)

有关反射和编辑上下文的更多信息，请参阅 *O3DE 用户指南*中的 [反射组件以进行序列化和编辑](/docs/user-guide/programming/components/reflection/reflecting-for-serialization)。

## 卡片中的反射属性编辑器

以下代码演示如何将简单的 **反射属性编辑器** 添加到 **卡** 中，如本主题开头的图片中所示。

### 示例

```cpp
#include <AzToolsFramework/UI/PropertyEditor/ReflectedPropertyEditor.hxx>
#include <AzCore/Serialization/SerializeContext.h>
#include <AzCore/Serialization/EditContext.h>
#include <AzQtComponents/Components/Widgets/Card.h>

// Create a card widget and set its title and header icon.
AzQtComponents::Card* card = new AzQtComponents::Card(parent);
card->setTitle(QStringLiteral("Card"));
card->header()->setIcon(QIcon(QStringLiteral(":/Gallery/Grid-small.svg")));

// Create a reflected property editor.
auto cardPropertyEditor = aznew AzToolsFramework::ReflectedPropertyEditor(card);

// Add the reflected property editor to the card as a content widget.
card->setContentWidget(cardPropertyEditor);
```

## C++ API参考

有关 **card** API 的详细信息，请参阅 [O3DE UI 扩展 C++ API 参考](/docs/api/frameworks/azqtcomponents/namespace_az_qt_components.html) 中的以下主题：
+  [AzQtComponents::Card](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_card.html)
+  [AzQtComponents::CardHeader](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_card_header.html)
+  [AzQtComponents::CardNotification](/docs/api/frameworks/azqtcomponents/class_az_qt_components_1_1_card_notification.html)

## 相关链接

有关 **reflected property editor** 组件的其他信息，请参阅以下主题：
+  [Card Widget](./uidev-card-widget)
