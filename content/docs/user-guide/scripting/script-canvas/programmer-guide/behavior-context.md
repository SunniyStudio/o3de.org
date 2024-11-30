---
linktitle: 从 Behavior Context 创建节点
title: 从 Behavior Context 创建 Script Canvas 节点
description: 了解 Script Canvas 与行为上下文之间的重要关系，以及如何在 Open 3D Engine （O3DE） 中使用脚本绑定创建新节点。
weight: 100
---

**Open 3D Engine （O3DE）** 中的 [行为上下文](/docs/user-guide/programming/components/reflection/behavior-context) 是一个反射系统，它公开了 C++ 类、方法、常量、数据类型和 O3DE 事件机制，为脚本环境提供了在运行时调用代码所需的绑定。**Script Canvas** 使用脚本绑定在 Node Palette 中自动创建新节点，以便在 Script Canvas 图形中使用。使用这些新节点，您可以调用 C++ 方法、获取和设置属性、检索常量、广播和处理事件，以及通过节点的数据引脚传递自定义数据类型。

简而言之，将行为上下文与 Script Canvas 结合使用来执行以下操作：

+ 使用 Script Canvas 节点调用 C++ 方法。
+ 从 Script Canvas 访问属性和常量。
+ 向 Script Canvas 公开 C++ 数据类型。
+ 通过 Script Canvas 节点发送和接收 AZ：：Event 和 EBus 事件。

在本主题中，您将了解 Script Canvas 如何使用行为上下文创建新节点并公开新数据类型以执行所描述的所有操作。有几个说明性示例，每个示例都包含使用行为上下文扩展 Script Canvas 时的提示和最佳实践。

## Script Canvas 架构

以下代码架构图显示了 Script Canvas 与 Open 3D Engine 中的行为上下文之间的关系。

![Script Canvas code architecture](/images/user-guide/scripting/script-canvas/behavior-context-code-architecture.png)

核心 Script Canvas 代码构建为静态库，该库链接到依赖 Gem 和 Script Canvas 编辑器 Gem。这允许运行时的代码占用量与运行 Script Canvas 图形所需的最小值一样小。它还允许 Script Canvas 编辑器 Gem 包含编写和开发 Script Canvas 图形所需的所有代码。

使用行为上下文时，无需编写任何特定于 Script Canvas 的代码。但是，在可视化脚本环境中，将代码反映到行为上下文的方式必须保持直观和实用，这一点很重要。

Script Canvas 和行为上下文架构的组合包括以下优势：

+ 对 AZ::Event 和 EBus 事件系统的支持使您的脚本能够使用解耦的、事件驱动的编程模式。
+ Script Canvas 可以使用通过任何 Gem 的行为上下文公开的功能，使任何 Gem 都能增强 Script Canvas。
+ 支持通过行为上下文反映 C++ 代码的 Gem，这意味着无需向 Script Canvas 添加 Gem 依赖项。

## 示例: 静态函数

{{< note >}}
请参阅 [自定义自由函数节点](/docs/user-guide/scripting/script-canvas/programmer-guide/custom-nodes/custom-free-function-nodes/)  了解开销较轻的方法。
{{< /note >}}

为了演示 C++ 代码如何成为 Script Canvas 节点，此示例使用行为上下文来反映一些简单的静态数学库函数。

我们从静态函数声明开始。以下函数返回角度的正弦和余弦。角度以弧度为单位：

```cpp
float Sin(float angle);
float Cos(float angle);
```

我们还需要一个定义这些函数的命名空间的类：

```cpp
class GlobalClass
{
public:
    AZ_TYPE_INFO(GlobalClass, "{47A07917-103F-41F5-A586-8D7C1C40A625}");
    AZ_CLASS_ALLOCATOR(GlobalClass, SystemAllocator, 0);

    GlobalClass() = default;
    ~GlobalClass() = default;
        
    static void Reflect(AZ::ReflectContext* context);
};
```

在类的静态 '`Reflect`' 方法中，我们使用行为上下文来反映 '`GlobalClass`' 并绑定作为类一部分的静态 '`Sin`' 和 '`Cos`' 方法。在此示例中，函数被配置为名为“`Globals`”的组的一部分。该组用作新节点上的副标题，以及节点将显示在 Script Canvas 的 Node Palette 中的类别。

```cpp
static void GlobalClass::Reflect(AZ::ReflectContext* context)
{
    if (auto behaviorContext = azrtti_cast<AZ::BehaviorContext*>(context))
    {
        behaviorContext->Class<GlobalClass>("Globals")
            ->Method("Sin", &Sin)
            ->Method("Cos", &Cos);
    }
}
```

要完成该示例，必须从系统组件的 Reflect 函数中调用 `GlobalClass::Reflect` 。

编译代码后，它可用作 Script Canvas 中的新节点：

![Sin function available as a Script Canvas node](/images/user-guide/scripting/script-canvas/behavior-context-sin-function.png)

但是，可以进行一些可用性改进来改善其在 Script Canvas 中的外观：

+ 为类提供顶级类别 '`My Extensions`'，而不是默认的 '`Other`'。
+ 为输入引脚提供用户友好的参数名称“Radians”。
+ 当用户将鼠标悬停在 `Radians` 参数上时提供工具提示。

这一切都可以通过更改 '`Reflect`' 函数中的代码来实现：

```cpp
        behaviorContext->Class<GlobalClass>("Globals")
          ->Attribute(AZ::Script::Attributes::Category, "My Extensions")
          ->Method("Sin", &Sin, {{{"Radians", "The value in radians"}}})
          ->Method("Cos", &Cos, {{{"Radians", "The value in radians"}}});
```

结果包含一些对这个新节点的用户有用的分类和参数信息：

![](/images/user-guide/scripting/script-canvas/behavior-context-my-extensions-nodes.png)
![](/images/user-guide/scripting/script-canvas/behavior-context-sin-node-with-tooltip.png)

## 示例：反射事件总线

将 EBus 绑定到行为上下文的能力使脚本编写成为驱动和模块化的脚本。将事件总线反映到行为上下文的两个主要用例是 _event handlers_ 和 _events_。

事件通常在代码中定义为请求总线的一部分，并且通常由某些代码系统（如组件）处理。事件处理程序通常定义为通知总线的一部分。

在此示例中，我们将了解基本的 **Light** 组件。该示例显示了其行为上下文反射如何转换为 Script Canvas 节点。

在这个 Light 组件中，用户可以通过设置颜色、强度和半径等参数来配置光线。Light （光源） 组件也可以打开或关闭，您可以在这些事件发生时做出响应。与实体的 Light 组件的通信通过两个事件总线完成：'`LightComponentRequestBus`' 和 '`LightComponentNotificationBus`'。

### 请求总线

请求总线是可以发送 _events_ 的事件总线。可以将事件视为供系统或对象处理的请求。组件可以将其事件方法反映到行为上下文，以使其可用于脚本环境，例如 Script Canvas。

以下是 Light 组件中的一些 C++ 事件方法：

```cpp
// Turn light on. Returns true if the light was successfully turned on.
bool TurnOnLight();

// Turn light off. Returns true if the light was successfully turned off.
bool TurnOffLight();

// Toggle light state.
void ToggleLight();
```

这些事件是 '`LightComponentRequestBus`' 的一部分。它们的行为由 '`LightComponent`' 实现。

```cpp
bool LightComponent::TurnOnLight()
{
    bool success = m_light.TurnOn();
    if (success)
    {
        LightComponentNotificationBus::Event(GetEntityId(), &LightComponentNotifications::LightTurnedOn);
    }
    return success;
}

bool LightComponent::TurnOffLight()
{
    bool success = m_light.TurnOff();
    if (success)
    {
        LightComponentNotificationBus::Event(GetEntityId(), &LightComponentNotifications::LightTurnedOff);
    }
    return success;
}

void LightComponent::ToggleLight()
{
    if (m_light.IsOn())
    {
        TurnOffLight();
    }
    else
    {
        TurnOnLight();
    }
}
```

要使这些事件可用于脚本编写，必须将其方法反映到行为上下文中。这是在 '`LightComponent::Reflect`' 中完成的。

```cpp
if (AZ::BehaviorContext* behaviorContext = azrtti_cast<AZ::BehaviorContext*>(context))
{
    behaviorContext->EBus<LightComponentRequestBus>("Light", "LightComponentRequestBus")
        ->Attribute(AZ::Script::Attributes::Category, "Rendering")
        ->Event("TurnOn", &LightComponentRequestBus::Events::TurnOnLight)
        ->Event("TurnOff", &LightComponentRequestBus::Events::TurnOffLight)
        ->Event("Toggle", &LightComponentRequestBus::Events::ToggleLight);
}
```

当 Script Canvas 检查行为上下文时，它会查找这些事件并自动生成相应的节点。

![Light component nodes in Script Canvas](/images/user-guide/scripting/script-canvas/behavior-context-ebus-request-light-nodes.png)

#### 事件总线事件和 EntityId

事件总线与实体的组件进行通信。为此，它需要一个地址。所有组件事件总线都派生自“`AZ::ComponentBus`”，该 ID 可通过“`AZ::EntityId`”类型的 ID 进行寻址。因此，组件事件总线中的所有节点都有一个 **EntityID** 的数据 pin。**EntityID** 引脚中存在“`Self`”是指拥有 Script Canvas 图形的实体的“`AZ::EntityID`”。但是，此 ID 可以分配给其他实体，甚至可以更改为无效的 ID。

![Self EntityID data pin](/images/user-guide/scripting/script-canvas/behavior-context-ebus-request-entityID.png)

#### 工具提示

Script Canvas 节点应包含每个参数的有用工具提示。例如，Light 组件可能有一个带有 '`state`' 参数的 '`SetLightState`' 事件：

```cpp
// Set the light state to on or off.
void SetLightState(State state);
```

您应该在行为上下文反射中添加工具提示来描述参数。在此示例中，当用户将鼠标悬停在 **SetState** 节点上的 **State** 数据引脚上时，工具提示将显示“`1=On， 0=Off`”。

```cpp
    behaviorContext->EBus<LightComponentRequestBus>("Light", "LightComponentRequestBus")
        ->Event("SetState", &LightComponentRequestBus::Events::SetLightState, {{{"State", "1=On, 0=Off"}}});
```

### 通知总线

通知总线是支持使用事件处理程序的事件总线。组件中的事件处理程序可以响应发送到组件的事件。您可以将事件处理程序反映到行为上下文，以使其可用于脚本环境，例如 Script Canvas。

我们的 Light 组件示例在“`LightComponentNotificationBus`”上使用以下 C++ 事件处理程序方法处理来自请求总线的“`TurnOn`”、“`TurnOff`”和“`Toggle`”事件：

```cpp
class LightComponentNotifications
      : public AZ::ComponentBus
{
public:

    // Sent when the light is turned on.
    virtual void LightTurnedOn() {}

    // Sent when the light is turned off.
    virtual void LightTurnedOff() {}
};

using LightComponentNotificationBus = AZ::EBus <LightComponentNotifications>;
```

要在 C++ 事件总线和脚本系统之间创建脚本绑定，您必须实施事件总线处理程序。

在下面的代码中，'`BehaviorLightComponentNotificationBusHandler`' 处理程序使用两个事件处理程序 '`LightTurnedOn`' 和 '`LightTurnedOff`' 建立脚本绑定。

```cpp
class BehaviorLightComponentNotificationBusHandler : public LightComponentNotificationBus::Handler, public AZ::BehaviorEBusHandler
{
public:
    AZ_EBUS_BEHAVIOR_BINDER(BehaviorLightComponentNotificationBusHandler, "{969C5B17-10D1-41DB-8123-6664FA64B4E9}", AZ::SystemAllocator,
        LightTurnedOn, LightTurnedOff);

    // Sent when the light is turned on.
    void LightTurnedOn() override
    {
        Call(FN_LightTurnedOn);
    }

    // Sent when the light is turned off.
    void LightTurnedOff() override
    {
        Call(FN_LightTurnedOff);
    }
};
```

接下来，您需要将通知总线反射到 Light 组件的 '`Reflect`' 方法中的行为上下文。作为此反射的一部分，您还指定 '`BehaviorLightComponentNotificationBusHandler`' 处理 Light 组件的事件。在请求总线的反射之后添加以下代码：

```cpp
if (AZ::BehaviorContext* behaviorContext = azrtti_cast<AZ::BehaviorContext*>(context))
{
    ...

    behaviorContext->EBus<LightComponentNotificationBus>("LightNotification", "LightComponentNotificationBus", "Notifications for the Light components")
        ->Attribute(AZ::Script::Attributes::Category, "Rendering")
        ->Handler<BehaviorLightComponentNotificationBusHandler>();
}
```

编译后，这些事件处理程序可从 **LightNotification** 节点提供给 Script Canvas。

![Light notification node](/images/user-guide/scripting/script-canvas/behavior-context-ebus-light-notification-node.png)

## 示例：数据类型

要使自定义数据类型可用作 Script Canvas 中的变量，您可以使用行为上下文的“`Class`”生成器来反映它。新类型还可以作为参数传递给函数和事件。

此示例使用 '`BoxShapeConfig`' 类作为示例。 这个类被定义在`Gems\LmbrCentral\Code\include\LmbrCentral\Shape\BoxShapeComponentBus.h`文件中，在`Gems\LmbrCentral\Code\Source\Shape\BoxShapeComponent.cpp`中反射。

数据类型必须反映到序列化上下文和行为上下文中。序列化上下文允许存储数据类型并从文件中读取，而行为上下文允许将其绑定到脚本系统。

```cpp
void BoxShapeConfig::Reflect(AZ::ReflectContext* context)
{
    if (auto serializeContext = azrtti_cast<AZ::SerializeContext*>(context))
    {
        serializeContext->Class<BoxShapeConfig, ShapeComponentConfig>()
            ->Version(2)
            ->Field("Dimensions", &BoxShapeConfig::m_dimensions);
     }

    if (auto behaviorContext = azrtti_cast<AZ::BehaviorContext*>(context))
    {
         behaviorContext->Class<BoxShapeConfig>()
            ->Constructor()
            ->Constructor<AZ::Vector3&>()
            ->Property("Dimensions", BehaviorValueProperty(&BoxShapeConfig::m_dimensions));
    }
}
```

结果变量节点：

![Get BoxShapeConfig variable node](/images/user-guide/scripting/script-canvas/behavior-context-data-types-boxshapeconfig.png)

## 最佳实践：在节点中显示事件总线事件参数名称

要正确显示事件总线事件的参数名称，请确保在将事件反映到行为上下文时指定自定义名称。

如果未指定参数的名称，则会为它们指定默认显示名称，如“`1`”、“`2`”或“`3`”，如下图所示：

![Default parameter names displayed](/images/user-guide/scripting/script-canvas/behavior-context-displaying-parameter-names-1.png)

以下代码生成了图像中的 event 节点：

```cpp
if (auto behaviorContext = azrtti_cast <AZ::BehaviorContext*>(reflectContext))
{
    behaviorContext->EBus<MyBus>("MyBus")
        // This is the category that appears in the Node Palette window.
        ->Attribute(AZ::Script::Attributes::Category, "Rendering")
        ->Event("SomeEvent", &MyBus::Events::SomeEvent);
}
```

相同代码的改进版本将参数名称 '`FirstParam`' 和 '`SecondParam`' 以及相应的工具提示文本添加到 '`Event`' 函数中：

```cpp
if (auto behaviorContext = azrtti_cast<AZ::BehaviorContext*>(reflectContext))
{
    behaviorContext->EBus<MyBus>("MyBus")
        // This is the category that appears in the Node Palette window.
        ->Attribute(AZ::Script::Attributes::Category, "Rendering")
        ->Event("SomeEvent", &MyBus::Events::SomeEvent, {{{"FirstParam" , "First Param Tooltip"}, { "SecondParam", "Second Param Tooltip"}}});
}
```

在 node palette （节点调色板） 窗口中，参数名称将按指定方式显示：

![Specified parameter names displayed](/images/user-guide/scripting/script-canvas/behavior-context-displaying-parameter-names-2.png)

### 替代语法

您还可以使用替代语法 '`AZ::BehaviorParameterOverrides`' 创建参数覆盖实例，然后再将其传递给 '`Event`' 函数。

```cpp
if (auto behaviorContext = azrtti_cast<AZ::BehaviorContext*>(reflectContext))
{
    AZ::BehaviorParameterOverrides someEventParam1 = {"FirstParam", "First Param Tooltip"};
    AZ::BehaviorParameterOverrides someEventParam2 = {"SecondParam", "Second Param Tooltip"};
    behaviorContext->EBus<MyBus>("MyBus")
        // This is the category that appears in the Node Palette window
        ->Attribute(AZ::Script::Attributes::Category, "Rendering")
        ->Event("SomeEvent", &MyBus::Events::SomeEvent, {{ someEventParam1, someEventParam2 }});
}
```

## 常见行为上下文问题

以下是使用 Script Canvas 和行为上下文进行编程时出现的一些常见问题。

### 反射的对象未显示在 Script Canvas 中

序列化上下文和行为上下文都使用相同的 '`Reflect`' 函数：

```cpp
Reflect(AZ::ReflectContext*)
```

一个常见的错误是没有将反射范围分开。例如，您可能会错误地将 '`BehaviorContext`' 反射放在 '`SerializeContext`' 范围内。下面的代码示例显示了问题和解决方案。

**Problem**

```cpp
void Example::Reflect(AZ::ReflectContext* context)
{
    if (AZ::SerializeContext* serializeContext = azrtti_cast<AZ::SerializeContext*>(context))
    {
        serializeContext->Class<Example>()
            ->Version(1);

        // Problem! BehaviorContext is inside the SerializeContext scope and will not get reflected.
        if (AZ::BehaviorContext* behaviorContext = azrtti_cast<AZ::BehaviorContext*>(context))
        {
            behaviorContext->Class<Example>()
                ->Method("IsValid", &Example::IsValid);
        }
    }
}
```

**解决方案**

```cpp
void Example::Reflect(AZ::ReflectContext* context)
{
    if (AZ::SerializeContext* serializeContext = azrtti_cast<AZ::SerializeContext*>(context))
    {
        serializeContext->Class<Example>()
            ->Version(1);
    }

    // Correct! Each context requires its own scope as this function is called multiple times (once per context)
    if (AZ::BehaviorContext* behaviorContext = azrtti_cast<AZ::BehaviorContext*>(context))
    {
        behaviorContext->Class<Example>()
            ->Method("IsValid", &Example::IsValid);
     }
}
```

### 事件总线处理程序未被调用

**问题**

您创建了一个事件总线处理程序，并将其正确地公开给行为上下文。但是，当您尝试在 Script Canvas 中接收事件时，它不会被触发。

**溶液**

事件总线处理程序必须先连接，然后才能接收事件。确保您的组件连接到总线，如以下示例所示：

```cpp
MyBus::BusConnect();
```

根据总线的类型，您可能必须指定要连接的 ID。有关更多信息，请参阅 [Open 3D Engine Event Bus （EBus） 系统](/docs/user-guide/programming/messaging/ebus/).

## 附加资料

要开始在 O3DE 中创建集成行为上下文的新组件，我们建议您阅读 [组件开发程序员指南](/docs/user-guide/programming/components/).

要更详细地了解行为上下文系统本身，请参阅[Behavior Context](/docs/user-guide/programming/components/reflection/behavior-context).
