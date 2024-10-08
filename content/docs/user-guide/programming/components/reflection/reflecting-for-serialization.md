---
title: 反射组件以便序列化和编辑
description: 学习如何用 C++ 反射Open 3D Engine (O3DE) 组件。
weight: 50
---

**Open 3D Engine (O3DE)** 组件使用 AZ 反射来描述它们序列化的数据，以及内容创建者如何与它们交互。

下面的示例反射了一个用于序列化和编辑的组件：

```cpp
class MyComponent
    : public AZ::Component
{
    // ... AZ_COMPONENT, Activate(), Deactivate(), etc, ...

    static void Reflect(AZ::ReflectContext* context);

    enum class SomeEnum
    {
        EnumValue1,
        EnumValue2,
    }
    float m_someFloatField;
    AZStd::string m_someStringField;
    SomeEnum m_someEnumField;
    AZStd::vector<SomeClassThatSomeoneHasReflected> m_things;

    int m_runtimeStateNoSerialize;
}

void MyComponent::Reflect(AZ::ReflectContext* context)
{
    AZ::SerializeContext* serializeContext = azrtti_cast<AZ::SerializeContext*>(context);
    if (serializeContext)
    {
        // Reflect the class fields that you want to serialize.
        // In this example, m_runtimeStateNoSerialize is not reflected for serialization.
        // Base classes with serialized data should be listed as additional template
        // arguments to the Class< T, ... >() function.
        serializeContext->Class<MyComponent, AZ::Component>()
            ->Version(1)
            ->Field("SomeFloat", &MyComponent::m_someFloatField)
            ->Field("SomeString", &MyComponent::m_someStringField)
            ->Field("Things", &MyComponent::m_things)
            ->Field("SomeEnum", &MyComponent::m_someEnumField)
            ;

        AZ::EditContext* editContext = serializeContext->GetEditContext();
        if (editContext)
        {
            editContext->Class<MyComponent>("My Component", "The World's Most Clever Component")
                ->ClassElement(AZ::Edit::ClassElements::EditorData, "")
                      ->Attribute(AZ::Edit::Attributes::AppearsInAddComponentMenu, AZ_CRC("Game"))
                ->DataElement(AZ::Edit::UIHandlers::Default, &MyComponent::m_someFloatField, "Some Float", "This is a float that means X.")
                ->DataElement(AZ::Edit::UIHandlers::Default, &MyComponent::m_someStringField, "Some String", "This is a string that means Y.")
                ->DataElement(AZ::Edit::UIHandlers::ComboBox, &MyComponent::m_someEnumField, "Choose an Enum", "Pick an option among a set of enum values.")
                    ->EnumAttribute(MyComponent::SomeEnum::EnumValue1, "Value 1")
                    ->EnumAttribute(MyComponent::SomeEnum::EnumValue2, "Value 2")
                ->DataElement(AZ::Edit::UIHandlers::Default, &MyComponent::m_things, "Bunch of Things", "A list of things for doing Z.")
            ;
        }
    }
}
```

上例为 `MyComponent`添加了五个数据成员。前四个数据成员将被序列化。最后一个数据成员不会被序列化，因为它只包含运行时状态。这是典型的情况；组件通常包含一些被序列化的成员和一些未被序列化的成员。

在使用[更改通知回调](#change-notification-callbacks)等高级反射功能时，常见的情况是字段在序列化时被反射，但在编辑时不被反射。在这种情况下，组件可能会根据用户属性的变化进行复杂的内部计算。这些计算的结果必须序列化，但不能用于编辑。在这种情况下，应将字段反射到 `SerializeContext` 中，但不要在 `EditContext` 中添加条目。下面是一个示例：

```cpp
serializeContext->Class<MyComponent>()
    ->Version(1)
    ...
    ->Field("SomeFloat", &MyComponent::m_someFloatField)
    ->Field("MoreData", &MyComponent::m_moreData)
    ...
    ;

...

AZ::EditContext* editContext = serializeContext->GetEditContext();
if (editContext)
{
    editContext->Class<MyComponent>("My Component", "The World's Most Clever Component")
        ->ClassElement(AZ::Edit::ClassElements::EditorData, "")
            ->Attribute(AZ::Edit::Attributes::AppearsInAddComponentMenu, AZ_CRC("Game"))
        ->DataElement(AZ::Edit::UIHandlers::Default, &MyComponent::m_someFloatField, "Some Float", "This is a float that means X.")
            ->Attribute(AZ::Edit::Attributes::ChangeNotify, &MyComponent::CalculateMoreData)
        // m_moreData is not reflected for editing directly.
        ;
}
```

O3DE 提供了以下反射上下文：
* [Serialization context](serialization-context/) -- 包含用于序列化和构建对象的反射数据。
* [Edit context](edit-context) -- 包含反射数据，用于对象的可视化编辑，如 O3DE 编辑器。
* [Behavior context](behavior-context) -- 包含反射数据，用于在运行时操作来自 Lua、[Script Canvas](/docs/user-guide/scripting/script-canvas/)或其他外部资源的对象。

{{< note >}}
此主题仅包含 `SerializeContext` 和 `EditContext`。
{{< /note >}}

O3DE 的所有反射 API 操作都设计得非常简单，可由人工读取和写入，并且不强制依赖于代码生成。

组件的 `Reflect()` 函数会自动调用所有相关上下文。

下面的代码将提供的匿名上下文动态转换为序列化上下文，这就是组件如何辨别调用 `Reflect()` 的上下文类型的方法。

```cpp
AZ::SerializeContext* serializeContext = azrtti_cast<AZ::SerializeContext*>(context);
```

## 序列化

 反射类的序列化涉及 C++ 中的 [builder pattern](https://en.wikipedia.org/wiki/Builder_pattern) 样式标记，如下所示：

```cpp
serializeContext->Class<TestAsset>()
         ->Version(1)
         ->Field("SomeFloat", &MyComponent::m_someFloatField)
         ->Field("SomeString", &MyComponent::m_someStringField)
         ->Field("Things", &MyComponent::m_things)
         ->Field("SomeEnum", &MyComponent::m_someEnumField)
         ;
```

该示例指定 `m_someFloatField`、`m_someStringField`、`m_things` 和 `m_someEnumField` 都应与组件一起序列化。字段名称必须是唯一的，并且不面向用户。

{{< tip >}}
我们建议您保持字段名称的简洁性，以备将来使用。如果您的组件发生了重大变化，而您又想编写一个数据转换器来保持向后的数据兼容性，则必须直接引用字段名。
{{< /tip >}}

上例反映了两种原始类型--浮点型和字符串--以及某种结构的容器（向量）。AZ 反射、序列化和编辑原生支持多种类型：
+ 原始类型，包括整数（有符号和无符号，所有大小）、浮点数和字符串
+ 枚举
+ `AZStd`容器（扁平和关联），包括`AZStd::vector`、`AZStd::list`、`AZStd::map`、`AZStd::unordered_map`、`AZStd::set`、`AZStd::unordered_set`、`AZStd:pair`、`AZStd::bitset`、`AZStd::array`、固定的 C 风格数组及其他。
+ 指针，包括 `AZStd::smart_ptr`、 `AZStd::intrusive_ptr`，以及原始本地指针。
+ 任何已被反映的类或结构。

{{< note >}}
示例省略了 `SomeClassThatSomeoneHasReflected` 的反射代码。但是，您只需反射该类。之后，您可以自由地在其他类中反映该类的成员或容器。
{{< /note >}}

## 编辑

当您运行 O3DE 工具（如 O3DE 编辑器）时，会提供一个 `EditContext` 和一个 `SerializeContext` 。您可以使用这些上下文中的强大功能将字段暴露给内容创建者。

以下代码演示了基本的编辑上下文反射：

```cpp
AZ::EditContext* editContext = serializeContext->GetEditContext();
if (editContext)
{
    editContext->Class<TestAsset>("My Component", "The World's Most Clever Component")
        ->ClassElement(AZ::Edit::ClassElements::EditorData, "")
             ->Attribute(AZ::Edit::Attributes::AppearsInAddComponentMenu, AZ_CRC("Game"))
        ->DataElement(AZ::Edit::UIHandlers::Default, &MyComponent::m_someFloatField, "Some Float", "This is a float that means X.")
        ->DataElement(AZ::Edit::UIHandlers::Default, &MyComponent::m_someStringField, "Some String", "This is a string that means Y.")
        ->DataElement(AZ::Edit::UIHandlers::ComboBox, &MyComponent::m_someEnumField, "Choose an Enum", "Pick an option among a set of enum values.")
            ->EnumAttribute(MyComponent::SomeEnum::EnumValue1, "Value 1")
            ->EnumAttribute(MyComponent::SomeEnum::EnumValue2, "Value 2")
        ->DataElement(AZ::Edit::UIHandlers::Default, &MyComponent::m_things, "Bunch of Things", "A list of things for doing Z.")
    ;
}
```

虽然本示例演示的是最简单的用法，但如果将结构（包括组件）反映到编辑上下文中，还可以使用许多功能和选项。为了让内容创建者直接看到字段，本例提供了一个友好的名称和一个描述（工具提示）作为 `DataElement` 的第三和第四个参数。对于三个字段，`DataElement` 的第一个参数是默认用户界面处理程序 `AZ::Edit::UIHandlers::Default`。属性系统的架构支持添加任意数量的用户界面处理程序，每个处理程序对一个或多个字段类型有效。一个给定的类型可以有多个可用的处理程序，其中一个处理程序被指定为默认处理程序。例如，浮点默认使用 `SpinBox` 处理程序，但也可使用 `Slider`处理程序。

下面是一个将浮点绑定到滑块的示例：

```cpp
->DataElement(AZ::Edit::UIHandlers::Slider, &MyComponent::m_someFloatField, "Some Float", "This is a float that means X.")
      ->Attribute(AZ::Edit::Attributes::Min, 0.f)
      ->Attribute(AZ::Edit::Attributes::Max, 10.f)
      ->Attribute(AZ::Edit::Attributes::Step, 0.1f)
```

`AZ::Edit::UIHandlers::Slider` 用户界面处理程序需要`AZ::Edit::Attributes::Min`和`AZ::Edit::Attributes::Max`属性。您还可以为 `AZ::Edit::Attributes::Step`提供一个值。该示例提供的增量为 `0.1`。如果不提供 `AZ::Edit::Attributes::Step` 的值，则使用默认步长 `1.0`。

{{< note >}}
属性系统支持外部用户界面处理程序，因此您可以在自己的模块中实现自己的用户界面处理程序。您可以自定义字段的行为、使用的 `Qt` 控件以及观察的属性。
{{< /note >}}

## 属性

该示例还演示了属性的使用。属性是编辑上下文中的一种通用结构，它允许将文字或返回值的函数绑定到命名的属性上。用户界面处理程序可以检索这些数据，并用它们来驱动自己的功能。

属性值可以绑定到以下内容：

字面值
`Attribute(AZ::Edit::Attributes::Min, 0.f)`

静态或全局变量
`Attribute(AZ::Edit::Attributes::Min, &g_globalMin)`

成员变量
`Attribute(AZ::Edit::Attributes::Min, &MyComponent::m_min)`

静态或全局函数
`Attribute(AZ::Edit::Attributes::ChangeNotify, &SomeGlobalFunction)`

成员函数
`Attribute(AZ::Edit::Attributes::ChangeNotify, &MyComponent::SomeMemberFunction)`

## 更改通知回调

编辑上下文的另一个常用功能是绑定更改通知回调：

```cpp
->DataElement(AZ::Edit::UIHandlers::Default, &MyComponent::m_someStringField, "Some String", "This is a string that means Y.")
    ->Attribute(AZ::Edit::Attributes::ChangeNotify, &MyComponent::OnStringFieldChanged)
```

该示例绑定了一个成员函数，以便在该属性发生变化时调用，从而使组件能够执行其他逻辑。`AZ::Edit::Attributes::ChangeNotify` 属性也会寻找一个可选的返回值，告诉属性系统是否需要刷新其状态的各个方面。例如，如果您的更改回调修改了影响属性系统的其他内部数据，您可以请求刷新值。如果回调修改的数据需要重新评估属性（并重新调用任何绑定函数），则可以请求刷新属性和值。最后，如果您的回调执行的工作需要完全刷新（这并不常见），您可以刷新整个状态。

下面的示例使属性网格在通过属性网格修改 `m_someStringField` 时刷新值。`AZ::Edit::PropertyRefreshLevels::ValuesOnly` 会向属性网格发出信号，以便根据底层数据的更改更新图形用户界面。

```cpp
->DataElement(AZ::Edit::UIHandlers::Default, &MyComponent::m_someStringField, "Some String", "This is a string that means Y.")
    ->Attribute(AZ::Edit::Attributes::ChangeNotify, &MyComponent::OnStringFieldChanged)
...
AZ::u32 MyComponent::OnStringFieldChanged()
{
    m_someFloatField = 10.0f;

    // We've internally changed displayed data, so tell the property grid to refresh values (cheap).
    return AZ::Edit::PropertyRefreshLevels::ValuesOnly;
}
```

`AZ::Edit::PropertyRefreshLevels::ValuesOnly`是您可以使用的下列刷新模式之一：

+ `AttributesAndValues` -- 重新评估用户界面中显示的属性的属性并刷新其值。由于属性可以绑定到数据成员、成员函数、全局函数或静态变量，因此有时有必要要求属性网格重新评估这些属性。这样做可能包括重新调用绑定的函数。
+ `EntireTree` -- 刷新用户界面中显示的整个树。
+ `None` -- 指定不刷新用户界面中显示的属性。
+ `ValuesOnly` -- 只刷新用户界面中显示的属性值。属性网格会更新图形用户界面，以反映更改回调中可能发生的底层数据更改。

## ComboBox UIHandler

下面的示例更为复杂，它将字符串列表绑定为组合框的选项。字符串列表与字符串字段 **属性 A** 相连。假设您想用另一个 **属性 B** 的值修改属性 A 组合框中的可用选项。在这种情况下，您可以将组合框的 `AZ::Edit::Attributes::StringList` 属性绑定到一个计算并返回选项列表的成员函数。在属性 B 的`AZ::Edit::Attributes::ChangeNotify`属性中，您可以告诉系统重新评估属性，进而重新调用计算选项列表的函数。

```cpp
...

bool m_enableAdvancedOptions;
AZStd::string m_useOption;

...

->DataElement(AZ::Edit::UIHandlers::Default, &MyComponent::m_enableAdvancedOptions, "Enable Advanced Options", "If set, advanced options will be shown.")
    ->Attribute(AZ::Edit::Attributes::ChangeNotify, AZ::Edit::PropertyRefreshLevels::AttributesAndValues)
->DataElement(AZ::Edit::UIHandlers::ComboBox, &MyComponent::m_useOption, "Options", "Available options.")
    ->Attribute(AZ::Edit::Attributes::StringList, &MyComponent::GetEnabledOptions)
...

AZStd::vector<AZStd::string> MyComponent::GetEnabledOptions()
{
    AZStd::vector<AZStd::string> options;
    options.reserve(16);

    options.push_back("Basic option");
    options.push_back("Another basic option");

    if (m_enableAdvancedOptions)
    {
        options.push_back("Advanced option");
        options.push_back("Another advanced option");
    }

    return options;
}
```

还可以将字符串列表绑定到其他数据类型的值上。其中一个例子是通过使用用户友好的名称将组件的字段设置为程序员友好的值。为此，可将字段的 `DataElement` 上的 `GenericValueList` 属性设置为返回 `AZStd::vector<AZStd::pair<T, AZStd::string>>`的值。`AZStd::pair`中的第一个参数 `T` 表示程序员友好的类型和值，第二个参数表示用户友好的字符串：

```cpp
...

->Attribute(AZ::Edit::Attributes::GenericValueList, &MyComponent::GetEnabledOptions)

//...
AZStd::vector<AZStd::pair<AZ::Crc32, AZStd::string>> GetEnemyTypes()
{
    AZStd::vector<AZStd::pair<AZ::Crc32, AZStd::string>> options;
    options.reserve(16);
    options.push_back({AZ_CRC_CE("Basic"), AZStd::string("Basic option")});
    options.push_back({AZ_CRC_CE("Basic2"), AZStd::string("Another basic option")});

    if (m_enableAdvancedOptions)
    {
        options.push_back({AZ_CRC_CE("Advanced"), AZStd::string("Advanced option")});
        options.push_back({AZ_CRC_CE("Advanced2"), AZStd::string("Another advanced option")});
    }

    return options;
}
```

在 O3DE 编辑器中，组合框默认与以下字段数据类型配合使用：
+ 数值 (`int`, `float`, 等)
+ `AZStd::string`
+ 枚举 [反射](#editing) 到 `EditContext`。

要使用其他类型的组合框，请通过调用`AzToolsFramework::RegisterGenericComboBoxHandler<T>()`为所需的字段类型注册组合框处理程序，其中 `T` 为字段类型。由于该函数位于 `AzToolsFramework` 名称空间中，因此请在链接到编辑器的目标（如 Gem 的 `EditorSystemComponent::Activate` 函数）中进行调用。

```cpp
...

AZ::Uuid m_actorTypeId;

...

class Humanoid;
class Quadruped;

...

->DataElement(AZ::Edit::UIHandlers::ComboBox, &MyComponent::m_actorTypeId, "Actor Type", "The type of enemy to spawn.")
->Attribute(AZ::Edit::Attributes::GenericValueList, &MyComponent::GetActorTypes);

...

AZStd::vector<AZStd::pair<AZ::Uuid, AZStd::string>> GetEnemyTypes()
{
    return  
    { 
        {AZ::AzTypeInfo<Humanoid>::Uuid(),  AZStd::string("Silly Human")},
        {AZ::AzTypeInfo<Quadruped>::Uuid(), AZStd::string("Four Legs")};
    }
}

```
