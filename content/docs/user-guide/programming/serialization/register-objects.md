---
linkTitle: 注册对象
title: 注册对象以序列化
description: 了解如何在 Open 3D Engine (O3DE) 中为 JSON 或 XML 序列化注册对象。
weight: 100
---

在**Open 3D Engine (O3DE)**中，序列化是通过在**序列化上下文**中注册类来完成的。该上下文获取关于所提供类的信息，并使用反射机制来确定要发射哪些类成员及其类型。序列化是通过 `AZ::SerializeContext`类来控制的，该类在`AZCore/Serialization/SerializeContext.h`中声明，是 `AzCore` 库的一部分。

序列化需要访问`AZ::ReflectContext`实例，您可以通过 AzCore 反射系统将该实例安全地转换为 `AZ::SerializeContext`对象。在 O3DE 中有一个全局管理的序列化上下文，您可以通过 `AZComponentApplicationBus`获取。

```
AZ::SerializeContext* serializeContext = nullptr;
AZ::ComponentApplicationBus::BroadcastResult(serializeContext, &AZ::ComponentApplicationBus::Events::GetSerializeContext);
```

{{< caution >}}
使用全局序列化上下文时，只能在`Reflect` 函数调用中为序列化注册对象。在该函数之外注册可能会导致竞赛条件。如果需要在其他时间注册序列化，请使用自定义序列化上下文。
{{< /caution >}}

## 注册类

类是通过`AZ::SerializeContext::Class<T>()`方法在序列化上下文中注册的，使用`T`类型来确定要注册的类。要进行序列化，类**必须**是使用`AZ_TYPE_INFO_SPECIALIZE()`宏注册的`AzTypeInfo`的特化，或使用`AZ_RTTI`宏设置了 RTTI 信息。`AZ::SerializeContext::Class<T>()`方法会返回一个`AZ::SerializeContext::ClassBuilder`对象，用于存储类的版本和字段信息。

### `AZ::SerializeContext::ClassBuilder` 

`Version(unsigned int version, VersionConverter converter = nullptr)`
设置序列化的版本信息。
+  `version` - 类的版本。每当类的内部结构发生变化时，就应更新版本。
+  `converter` - 转换函数，用于将类的旧版本转换为所提供的 `version` 版本。

`template<class ClassType, class FieldType> Field(const char* name, FieldType ClassType::* address, AZStd::initializer_list<AttributePair> attrbuteIds = {})`
标记存储类中的一个字段。
+ `name` - 存储字段的名称。同一类别的字段名必须是唯一的。不要求与成员名称匹配。
+ `address` - 要存储的字段地址，可以是指向成员的指针，也可以是从 `ClassType` 实例开始的偏移量。如果使用成员指针，则会推断出所有类型信息。
+ `attributeIds` - 将其他属性对象与该字段关联。

`template<class ClassType, class BaseType, class FieldType> FieldFromBase(const char* name, FieldType BaseType:* address)`
从基类成员创建字段。如果要序列化基类成员，而不需要注册和序列化整个基类，则可以使用此方法将序列化后的类与其基类解耦。
+ `name` - 存储字段的名称。同一类别的字段名必须是唯一的。不要求与成员名称匹配。
+ `address` - 要存储的字段地址，可以是指向成员的指针，也可以是从 `BaseType` 实例开始的偏移量。如果使用成员指针，则会推断出所有类型信息。

**示例 为序列化注册一个类**
下面是 O3DE 资产处理器代码中的一个示例，演示了如何为序列化注册一个类。

```
if (AZ::SerializeContext* serializeContext = azrtti_cast<AZ::SerializeContext*>(context))
{
    serializeContext->Class<AssetBuilderDesc>()
        ->Version(2)
        ->Field("Flags", &AssetBuilderDesc::m_flags)
        ->Field("Name", &AssetBuilderDesc::m_name)
        ->Field("Patterns", &AssetBuilderDesc::m_patterns)
        ->Field("BusId", &AssetBuilderDesc::m_busId)
        ->Field("Version", &AssetBuilderDesc::m_version)
        ->Field("AnalysisFingerprint", &AssetBuilderDesc::m_analysisFingerprint);
}
```

## 注册枚举

枚举是使用`AZ::SerializeContext::Enum<T>()`方法在序列化上下文中注册的，使用`T`类型来决定注册哪个枚举。为了进行序列化，枚举必须***是使用`AZ_TYPE_INFO_SPECIALIZE()`宏的`AzTypeInfo`的特化。 `AZ::SerializeContext::Enum<T>()` 方法会返回一个`AZ::SerializeContext::EnumBuilder`对象，用于存储枚举的版本和值信息。

### `AZ::SerializeContext::EnumBuilder` 

`Version(unsigned int version, VersionConverter converter = nullptr)`
设置序列化的版本信息。
+  `version` - 枚举的版本。与类不同，每次更新枚举定义时，它都不需要更改，主要用于转换目的。
+  `converter` - 转换函数，用于将枚举的旧版本转换为所提供的 `version` 版本。

`template<class EnumType> Value(const char* name, EnumType value)`
标记一个枚举值，作为枚举信息的一部分进行序列化。
+ `name` - 存储值的名称。同一枚举的字段名必须是唯一的。不要求与内部值名称匹配。
+ `value` - 枚举的相关存储值。如果这是一个与枚举相关联的值，则会推断出所有类型信息。

{{< important >}}
如果要序列化带有枚举类型的类中的成员，则**必须**在序列化器中注册该枚举。
{{< /important >}}

**示例 为序列化注册一个枚举**
下面的示例展示了一个枚举声明示例，以及一个可用于在序列化系统中注册枚举的简短函数。

```
enum ExampleEnum : uint8_t
{
    BaseValue   = 0,
    Flag1       = 0x1,
    Flag2       = 0x2,
    Flag3       = 0x4,
    Flag4       = 0x8
}

// AzTypeInfo must be set from within the AZ namespace
namespace AZ
{
    AZ_TYPE_INFO_SPECIALIZE(ExampleEnum, "{7ebef8a5-b40d-4a9a-8511-162da1dc02f0}");
}

void registerEnum(AZ::SerializeContext* context)
{
    if (AZ::SerializeContext* serializeContext = azrtti_cast<AZ::SerializeContext*>(context))
    {
        context->Enum<ExampleEnum>()
            ->Value("Base", ExampleEnum::BaseValue)
            ->Value("Flag1", ExampleEnum::Flag1)
            ->Value("Flag2", ExampleEnum::Flag2)
            ->Value("Flag3", ExampleEnum::Flag3)
            ->Value("Flag4", ExampleEnum::Flag4)
    }
}
```
