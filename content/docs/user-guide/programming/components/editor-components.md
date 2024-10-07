---
linktitle: 编辑器组件
title: O3DE中的编辑器组件
description: 学习如何在 Open 3D Engine 中创建编辑器组件。
weight: 500
---

**Open 3D Engine (O3DE)** 中的某些组件有独立的 “编辑器 ”和 “运行时 ”版本。编辑器版本在编辑器中激活。运行时版本用于在游戏中或在编辑器中通过按**Ctrl+G**或点击视口下方的**AI/Physics**来运行关卡。O3DE 使用编辑器组件来保持特定于工具的代码和数据与更精简的运行时组件数据之间的分离。一般来说，运行时游戏组件不需要对应的编辑器。组件很少需要在编辑时完全激活。光线和网格组件是例外，因为它们在编辑时的行为必须与运行时相同。

运行时组件完全支持 `EditContext` 反射。编辑时是编辑器组件处于活动状态的唯一时间。在运行时，当 O3DE 处理一个关卡或动态切片时，它会使用运行时编辑器组件。使用运行时组件的 `EditContext` 通常足以提供丰富的编辑体验。

{{< important >}}
编辑器组件不是必需的。只有当以下情况之一为真时，才需要编辑器组件：

* 您的组件必须在**编辑时间**（指标准编辑模式）完全激活。运行时组件用于**AI/Physics**模式和游戏（**Ctrl+G**）。
* 您必须在组件中添加特殊工具功能，这要求您只能编译到编辑器二进制文件中。
* 您的组件只在编辑器中提供功能，而不输出运行时组件（例如，如果您的组件管理选择逻辑）。
{{< /important >}}

## 编辑器组件示例

下面的代码显示了一个编辑器组件示例。

```cpp
/* Include the following headers:
 * EditorComponentBase.h - the editor component base class. Derive from
 * this class to create a component to use in the editor that is the
 * counterpart of the version of the component that is used during runtime.
 * EntityDebugDisplayBus.h - provides a debug draw API to be used by components.
 * EditorVisibilityBus.h - controls whether an entity is shown or hidden in the editor
 * MyComponent.h - header file for the runtime version of the component.
*/
#include <AzToolsFramework/ToolsComponents/EditorComponentBase.h>
#include <AzToolsFramework/ToolsComponents/EditorVisibilityBus.h>
#include <AzFramework/Entity/EntityDebugDisplayBus.h>
#include <MyComponent.h>

class MyEditorComponent
      : public AzToolsFramework::Components::EditorComponentBase
      , private AzFramework::EntityDebugDisplayEventBus::Handler
{
public:
      AZ_EDITOR_COMPONENT(MyEditorComponent, "{5034A7F3-63DB-4298-83AA-915AB23EFEA0}");

      // Perform reflection for this component. The context parameter is the reflection context.
      static void Reflect(AZ::ReflectContext* context);

      // AZ::Component interface implementation.
      void Init() override;
      void Activate() override;
      void Deactivate() override;


      // AzFramework::EntityDebugDisplayEventBus implementation.
      void DisplayEntityViewport(const AzFramework::ViewportInfo& viewportInfo, AzFramework::DebugDisplayRequests& debugDisplay) override;

      // Optional functions for defining provided and dependent services.
      static void GetProvidedServices(AZ::ComponentDescriptor::DependencyArrayType& provided);
      static void GetDependentServices(AZ::ComponentDescriptor::DependencyArrayType& dependent);
      static void GetRequiredServices(AZ::ComponentDescriptor::DependencyArrayType& required);
      static void GetIncompatibleServices(AZ::ComponentDescriptor::DependencyArrayType& incompatible);

      // Faciliate the translation of an editor component into a runtime component.
      void BuildGameEntity(AZ::Entity* gameEntity) override;

};
```

## 编辑器组件和运行时组件的区别

编辑器组件的代码与运行时组件的代码类似。以下章节列出了主要区别。除了列出的区别外，可以认为编辑器组件代码与运行时组件代码相同。更多信息，请参阅 [创建组件](/docs/user-guide/programming/components/create-component/)。

### 基类

所有编辑器组件在其继承祖先的某个地方都包含`AzToolsFramework::Components::EditorComponentBase`类。如果组件必须显示编辑时的可视化效果，它必须是 `AzFramework::EntityDebugDisplayEventBus` 总线上的处理程序，如下例所示。

```cpp
#include <AzToolsFramework/ToolsComponents/EditorComponentBase.h>
#include <AzFramework/Entity/EntityDebugDisplayBus.h>
...
class MyComponent
      : public AzToolsFramework::Components::EditorComponentBase
      , private AzFramework::EntityDebugDisplayEventBus::Handler
```

### 宏

每个编辑器组件都必须在其类定义中指定`AZ_EDITOR_COMPONENT`宏。该宏包含两个参数：

1. 组件类型名称。

1. 唯一的 UUID。您可以使用任何 UUID 生成器来生成该值。Visual Studio 通过**工具**、**创建 GUID** 提供了这一功能。使用**注册表格式**设置，然后复制并粘贴生成的值。

`AZ_EDITOR_COMPONENT`宏的示例如下。

```cpp
AZ_EDITOR_COMPONENT(MyEditorComponent, "{5034A7F3-63DB-4298-83AA-915AB23EFEA0}");
```

{{< note >}}
一些 O3DE 编辑器组件指定 `AzToolsFramework::Components::EditorComponentBase` 为基类，但使用了 `AZ_COMPONENT` 而不是 `AZ_EDITOR_COMPONENT` 宏，如下例所示。
{{< /note >}}

```cpp
AZ_COMPONENT(EditorMannequinComponent, "{C5E08FE6-E1FC-4080-A053-2C65A667FE82}", AzToolsFramework::Components::EditorComponentBase);
```

### DisplayEntityViewport 方法

要在特定实体的视口中绘制调试视觉效果，请实现 `AzFramework::EntityDebugDisplayEventBus` 接口的 `DisplayEntityViewport` 方法。将此位置用于自定义原始编辑时可视化代码。

```cpp
#include <AzFramework/Entity/EntityDebugDisplayBus.h>
...
void DisplayEntityViewport(const AzFramework::ViewportInfo& viewportInfo, AzFramework::DebugDisplayRequests& debugDisplay) override;
```

| 参数 | 说明 |
| --- | --- |
| viewportInfo | 确定摄像机位置等信息。 |
| debugDisplay | 包含调试绘制或显示命令的界面。 |

### BuildGameEntity 方法

`EditorComponentBase.h` 中的 `BuildGameEntity` 方法有助于将编辑器组件转换为运行时组件。重载此方法如下。

```cpp
#include <AzToolsFramework/ToolsComponents/EditorComponentBase.h>
...
void BuildGameEntity(AZ::Entity* gameEntity) override;
```

`BuildGameEntity` 方法的典型实现会执行以下操作：

1. 根据指定的 `gameEntity` 的编辑器组件创建运行时组件。

1. 将编辑器组件中的配置数据复制到运行时组件中。

1. 将运行时组件添加到指定的`gameEntity`中。

此时，运行时组件会序列化`gameEntity`并重新加载，以创建一个新的、干净的运行时实体版本。
