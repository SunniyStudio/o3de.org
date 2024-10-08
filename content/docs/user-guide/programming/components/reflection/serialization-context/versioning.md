---
linkTitle: 版本化序列化数据
title: 版本化组件序列化
description: 使用Open 3D Engine (O3DE)组件序列化版本系统来验证和更新组件的序列化数据。
---

随着需求、代码和数据表示的变化，您可能需要修改数据反射的实现。然而，序列化数据的更改可能会导致不兼容。为了管理兼容性，可以为序列化数据结构分配一个版本号增量。采用这种方法，可以在序列化过程中执行验证，确保读取的数据与反射系统指定的格式相匹配。我们建议你在反射字段发生变化时增加序列化数据的版本号。

以下代码展示了如何为序列化上下文指定版本号。

```cpp
serializeContext->Class<SerializedStruct>()
    ->Version(1)
```

要成功地将序列化数据转换为更新的版本，需要精心策划。

## 版本转换器

版本变更可能会产生不兼容性，需要将数据从一种格式转换为另一种格式。为了解决这个问题，你可以实现一个版本转换器，“当场 ”重新格式化数据，以保持数据的兼容性。例如，如果更改了数据类型或容器（例如，`AZStd::vector`变成了`AZStd::unordered_map`），就可能需要版本转换器。

请使用上一节提到的 `Version` 函数来指定版本转换器，如下例所示。

```cpp
serializeContext->Class<EditorEntitySortComponent, EditorComponentBase>()
    ->Version(2, &SerializationConverter)
```

版本转换器直接对序列化数据进行操作。

为方便创建版本转换器，**Open 3D Engine (O3DE)** 提供了辅助函数和示例，如以下所示：

* 要定位要操作的特定元素，可以使用 `AZ::Utils::FindDescendantElements` 辅助函数。
* 要访问序列化数据并对其进行操作，可使用`DataElementNode`类(`Code/Framework/AzCore/AzCore/Serialization/SerializeContext.h`)中的公共函数。
* 有关版本转换器示例，请参阅 `Code/Framework/AzCore/Tests/Serialization.cpp`文件中的 AZ core 序列化单元测试。

替换容器的版本转换操作可能遵循这种常见模式：

1. 比较序列化数据的版本号和当前版本。如果版本不匹配，请执行以下步骤。

1. 通过元素的 `Crc32` 键找到要转换的元素。

1. 创建一个容器来存储更新的元素。

1. 用现有数据填充新容器。

1. 从根数据中删除旧元素

1. 使用相同的 `Crc32` 键将新容器添加为根数据中的新元素。

此操作完成后，数据将以新格式存在。再次序列化数据时，数据将以最新格式存储。

下面的代码显示了一个数据转换示例：

```cpp
if (rootElement.GetVersion() <= 1)
{
    // This line of code:
    //  using Events = AZStd::vector<EBusEventEntry>;
    //  is changed to this:
    //  using EventMap = AZStd::unordered_map<AZ::Crc32, EBusEventEntry>;
    auto ebusEventEntryElements = AZ::Utils::FindDescendantElements(serializeContext, rootElement, AZStd::vector<AZ::Crc32>{AZ_CRC("m_events", 0x191405b4), AZ_CRC("element", 0x41405e39)});
    EBusEventHandler::EventMap eventMap;
    for (AZ::SerializeContext::DataElementNode* ebusEventEntryElement : ebusEventEntryElements)
    {
        EBusEventEntry eventEntry;
        if (!ebusEventEntryElement->GetDataHierarchy(serializeContext, eventEntry))
        {
            return false;
        }
        AZ::Crc32 key = AZ::Crc32(eventEntry.m_eventName.c_str());
        AZ_Assert(eventMap.find(key) == eventMap.end(), "Duplicated event found while converting EBusEventHandler from version 1 to 2.");
        eventMap[key] = eventEntry;
    }
    // Remove the previous Events element.
    rootElement.RemoveElementByName(AZ_CRC("m_events", 0x191405b4));
    // Replace it with the new EventMap element.
    if (rootElement.AddElementWithData(serializeContext, "m_eventMap", eventMap) == -1)
    {
        return false;
    }
    return true;
}
```

{{< note >}}
如果需要在转换失败时发出警告或错误（例如，用于资产构建），请使用 `AZ_Warning` 或 `AZ_Error` 宏。
{{< /note >}}

## 升级类构建器

切片数据补丁对组件数据结构的版本控制提出了独特的挑战。数据补丁无法通过版本转换器升级，因为它们不包含组件类的所有信息。在不升级包含部分组件数据的数据补丁的情况下更改组件的序列化，可能会导致崩溃、切片数据损坏或切片文件无效，无法加载或操作，必须从头开始重建。

在大多数情况下，解决办法是在使用版本转换器的同时使用 `NameChange` 和 `TypeChange` 类构建器。这将使序列化器更新数据补丁，并应用基本的类型更改和字段名称更改。您可以将这些构建器串联起来，以便在多个版本变更时进行升级。您也可以编写这些构建器来完全跳过版本。

### 类创建器语法

名称更改类构建器需要输入和输出版本，然后是输入序列化名称和新的输出名称。

```cpp
NameChange(InputVersion, OutputVersion, "OldFieldName", "NewFieldName")
```

类型更改类构建器需要输入和输出数据类型作为模板参数，然后是相关字段名称、输入和输出版本以及转换函数。

```cpp
TypeChange<InputType, OutputType>("FieldName", InputVersion, OutputVersion, Function<OutputType(InputType)>)
```

### NameChange 类创建器示例

在下面的示例中，我们使用 `NameChange` 类创建器将一个字段的序列化名称从组件序列化版本 `4` 中的 `“MyData”` 更改为版本 `5` 中的  `"Data"` 。

```cpp
serializeContext->Class<ExampleClass>()
    ->Version(5)
    ->Field("Data", &ExampleClass::m_data)
    ->NameChange(4, 5, "MyData", "Data");
```

在将类反映到序列化上下文时，还可以更改结构体或类成员的序列化名称。在下面的示例中，我们使用 `NameChange` 类创建器将名称从版本 `4` 中的 `“MyStructData”` 更改为版本 `5` 中的 `“StructData”。

```cpp
class ExampleClass
{
    ...
    DataStruct m_data;
};

serializeContext->Class<ExampleClass>()
    ->Version(5)
    ->Field("StructData", &ExampleClass::m_data)
    ->NameChange(4, 5, "MyStructData", "StructData");
```

### TypeChange 类创建器示例

在下面的示例中，类成员 `m_data` 已从版本 `4` 中的 `int` 变为版本 `5` 中的 `float`。我们在序列化上下文中添加了一个 `TypeChange` 类创建器，这样任何包含序列化字段名称 `“MyData”` 的数据修补程序都会使用新的数据类型。

```cpp
// Serialization Context for Version 4:
class ExampleClass
{
    ...
    int m_data;
    reflect(...)
    {
        serializeContext->Class<ExampleClass>()
        ->Version(4)
        ->Field("MyData", &ExampleClass::m_data);
    }
};

// Serialization Context for Version 5:
class ExampleClass
{
    ...
    float m_data;
    reflect(...)
    {
        serializeContext->Class<ExampleClass>()
        ->Version(5)
        ->Field("MyData", &ExampleClass::m_data)
        ->TypeChange<int, float>("MyData", 4, 5, [](int in)->float { return (float)in; });
    }
};
```

您还可以处理嵌套值变化。在下面的示例中，字段 `m_data` 嵌套在版本 `5` 中新的 `MyData` 结构中。我们使用 `TypeChange` 类创建器来指示序列化器将简单的 `int` 数据类型转换为更复杂的 `MyData` 类型。

```cpp
// Serialization Context for Version 4:
class ExampleClass
{
    ...
    int m_data;
    reflect(...)
    {
        serializeContext->Class<ExampleClass>()
        ->Version(4)
        ->Field("MyData", &ExampleClass::m_data);
    }
};

// Serialization Context for Version 5:
struct MyData
{
    int m_data;
};

class ExampleClass
{
    ...
    MyData m_data;
    reflect(...)
    {
        serializeContext->Class<ExampleClass>()
        ->Version(5)
        ->Field("MyData", &ExampleClass::m_data)
        ->TypeChange<int, MyData>("MyData", 4, 5, [](int in)->MyData { MyData out; out.m_data = in; return out; });
    }
};
```

### 高级类创建器示例

以下示例演示了类构建器更复杂的用法。

**示例： 一个版本中的多个升级**

类型更改优先于名称更改。您可以在同一版本升级中同时应用这两种更改，但类型更改会优先应用。因此，在同时更改类型和名称时，一定要在 `TypeChange` 中指定**上一个**字段的名称。

在下面的示例中， `TypeChange` 将类型从`float` 改为 `int`。紧随其后的 `NameChange` 将序列化名称从 `“FloatData”` 变为 `“IntData”`。

```cpp
// Serialization Context for Version 4:
class ExampleClass
{
    ...
    float m_data;
    static void Reflect(...)
    {
        serializeContext->Class<ExampleClass>()
            ->Version(4)
            ->Field("FloatData", &ExampleClass::m_data);
    }
};

// Serialization Context for Version 5:
class ExampleClass
{
    ...
    int m_data;
    static void Reflect(...)
    {
        serializeContext->Class<ExampleClass>()
            ->Version(5)
            ->Field("IntData", &ExampleClass::m_data)
            ->TypeChange<float, int>("FloatData", 4, 5, [](float in)->int { return (int)in; })
            ->NameChange(4, 5, "FloatData", "IntData");
    }
};
```

**举例说明： 版本跳过**

一个 `TypeChange` 可以跳过多个版本。除非中间类型更改包含可能丢失数据的转换，否则应避免跳过版本。

在下面使用 `ExampleClass` 的示例中，成员变量 `m_data` 从版本 `1` 中的 `float` 变为版本 `2` 中的 `int`。然后在版本 `3` 中，`m_data` 又变回了 `float`。我们使用多个 `TypeChange` 类构建器，以避免在将旧版本的覆盖升级到版本 `3` 时丢失浮点精度，同时仍能修复使用版本 `2` 的 `ExampleClass` 编写的数据补丁。

```cpp
// Version 1 of ExampleClass:
class ExampleClass
{
    ...
    float m_data;
    static void Reflect(...)
    {
        serializeContext->Class<ExampleClass>()
            ->Version(1)
            ->Field("Data", &ExampleClass::m_data);
    }
};

// Version 2 of ExampleClass:
class ExampleClass
{
    ...
    int m_data;
    static void Reflect(...)
    {
        serializeContext->Class<ExampleClass>()
            ->Version(2)
            ->Field("Data", &ExampleClass::m_data)
            ->TypeChange<float, int>("Data", 1, 2, [](float in)->int { return (int)in; });
    }
};

// Version 3 of ExampleClass:
class ExampleClass
{
    ...
    float m_data;
    static void Reflect(...)
    {
        serializeContext->Class<ExampleClass>()
            ->Version(3)
            ->Field("Data", &ExampleClass::m_data)
            ->TypeChange<float, int>("Data", 1, 2, [](float in)->int { return (int)in; })
            ->TypeChange<int, float>("Data", 2, 3, [](int in)->float { return (float)in; })
            ->TypeChange<float, float>("Data", 1, 3, [](float in)->float { return in; });
    }
};
```

我们强烈建议不要使用 `NameChange` 创建器跳过版本。这样做会给在跳过版本之间的同一字段上使用的 `TypeChange` 创建器带来问题，因为它们会尝试匹配序列化字段名。

## 废弃

序列化上下文还支持废弃先前反映的类名。要废弃一个类，请使用 `ClassDeprecate` 方法。类被废弃后，该类的任何实例都会在加载过程中被静默丢弃。

下面的示例展示了 `ClassDeprecate` 方法的使用。

```cpp
serializeContext->ClassDeprecate("DeprecatedClass", "{893CA46E-6D1A-4D27-94F7-09E26DE5AE4B}");
```
