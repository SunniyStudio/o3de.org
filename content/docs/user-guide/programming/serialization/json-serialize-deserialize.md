---
linkTitle: 序列化和反序列化
title: 序列化和反序列化JSON对象
description: 了解如何将对象从 Open 3D Engine (O3DE) 序列化为 JSON，以及如何通过反序列化将其加载回来。
weight: 300
---

在类[注册了序列化上下文](/docs/user-guide/programming/serialization/register-objects/)后，就可以序列化和反序列化该类的对象。使用`AZ::JsonSerialization::Store()`函数可将对象序列化为 JSON，使用`AZ::JsonSerialization::Load()`可从 JSON 反序列化。

本文包括这些方法的参考、使用序列化和反序列化的示例，以及如何解释 JSON 序列化器的结果代码。有关如何序列化特定类型的信息，请参阅 [JSON 序列化 O3DE 数据类型](/docs/user-guide/programming/serialization/json-data-types)。

## 序列化 

序列化为 JSON 是通过 `static AZ::JsonSerialization::Store()` 方法完成的。该方法有多个重载，具体取决于希望如何序列化对象以及序列化时可用的信息。默认情况下，使用全局序列化上下文。

### `AZ::JsonSerialization::Store()` 重载 

`template<typename T> static AZ::JsonSerializationResult::ResultCode AZ::JsonSerialization::Store(rapidjson::Value& output, rapidjson::Document::AllocatorType& allocator, const T& object, AZ::JsonSerializerSettings settings = AZ::JsonSerializerSettings{});`
+ `output` - 要写入的 RapidJSON 文档或值。通过提供适当的值，可以在 JSON 文档的任意位置对对象进行序列化。
+ `allocator` - RapidJSON 使用的内存分配器。
+  `object` - 要序列化的对象。该对象的类必须在提供的序列化上下文中注册。

   序列化时，会通过默认构造函数（如果可能）创建第二个 `T` 类型的对象，以提供默认值。
+ `settings` - 如何处理序列化的配置。如果未提供，则使用默认设置。

`template<typename T> static AZ::JsonSerializationResult::ResultCode AZ::JsonSerialization::Store(rapidjson::Value& output, rapidjson::Document::AllocatorType& allocator, const T& object, const T& defaultObject, AZ::JsonSerializerSettings settings = AZ::JsonSerializerSettings{});`
+ `output` - 要写入的 RapidJSON 文档或值。通过提供适当的值，可以在 JSON 文档的任意位置对对象进行序列化。
+ `allocator` - RapidJSON 使用的内存分配器。
+  `object` - 要序列化的对象。该对象的类必须在提供的序列化上下文中注册。
+ `defaultObject` - 在序列化过程中提供默认值的对象。如果 `object` 的任何成员的值与 `defaultObject` 不匹配，则保证会被序列化。
+ `settings` - 如何处理序列化的配置。如果未提供，则使用默认设置，但默认值将存储在输出中。

`static AZ::JsonSerializationResult::ResultCode AZ::JsonSerialization::Store(rapidjson::Value& output, rapidjson::Document::AllocatorType& allocator, const void* object, const void* defaultObject, const AZ::Uuid& objectType, AZ::JsonSerializerSettings settings = AZ::JsonSerializerSettings{});`
+ `output` - 要写入的 RapidJSON 文档或值。通过提供适当的值，可以在 JSON 文档的任意位置对对象进行序列化。
+ `allocator` - RapidJSON 使用的内存分配器。
+  `object` - 要序列化的匿名数据对象。
+ `defaultObject` - 在序列化过程中提供默认值的对象。如果 `object` 的任何成员的值与 `defaultObject` 不匹配，则保证会被序列化。如果传递了一个空指针作为默认对象，则可能会在序列化过程中创建一个临时默认对象。
+ `objectType` - 在 O3DE 运行时注册的 UUID，代表所提供的 `object`的类。该 UUID 所代表的类必须在所提供的序列化上下文中注册。
+ `settings` - 如何处理序列化的配置。如果未提供，则使用默认设置，但如果 `defaultObject` 不为空，则默认值将存储在输出中。

### `AZ::JsonSerializerSettings` 

通过将 `AZ::JsonSerialization::Store()` 实例设置为 `settings` 参数，可以控制 `AZ::JsonSerializerSettings` 方法的行为。

`bool m_keepDefaults`
如果 `true`，则在序列化时将默认值写入 JSON 值。
**默认值**： `false`

`AZ::SerializeContext* m_serializeContext`
序列化上下文，用于查询有关如何序列化所提供对象的信息。
**默认值**： 从`ComponentApplicationBus`事件总线获取的全局 seralization 上下文。

`AZ::JsonSerializationResult::JsonIssueCallback m_reporting`
当序列化过程中遇到错误时调用的回调方法。该函数无法访问正在序列化的对象或正在写入的 JSON 值，但可用于更改结果代码。
**默认值**： 默认问题报告器，用于记录序列化过程中遇到的警告和错误。

`AZ::JsonRegistrationContext* m_registrationContext`
JSON 注册上下文。有关如何使用自定义 JSON 上下文的示例，请参阅源代码。
**默认值**： 从事件总线获取的全局注册上下文。

**序列化示例**
下面是一个简短的示例，演示如何序列化一个简单的类。有关在序列化上下文中注册类的细节略去不提。

```
class SerializableClass
{
    double  m_var1;
    int     m_var2;
}

// ... Register with serialization context

SerializableClass instance;
instance.m_var1 = 42.0;
instance.m_var2 = 88;

rapidjson::Document document;
AZ::JsonSerializationResult::ResultCode result = AZ::JsonSerialization::Store(document, document.GetAllocator(), instance);
if (result.GetProcessing() == AZ::JsonSerializationResult::Processing::Halted)
{
    AZ_Warning("Serialization", false,
        "Unable to fully serialize SerializableClass to json because %s.",
        result.ToString().c_str());
}

rapidjson::StringBuffer buffer;
rapidjson::Writer<decltype(buffer)> writer(buffer);
document.Accept(writer);
AZ_TracePrintf("Serialization", "SerializableClass as Json:\n%s", buffer.GetString());
```

## 反序列化 

通过 `static AZ::JsonSerialization::Load()` 方法将 JSON 反序列化为对象。该方法有两个重载，一个是在有反序列化对象类型的实例时使用，另一个是使用 `void*` 和 RTTI 信息。

### `AZ::JsonSerialization::Load()` 重载 

`template<typename T> static AZ::JsonSerializationResult::ResultCode AZ::JsonSerialization::Load(T& object, const rapidjson::Value& root, AZ::JsonDeserializerSettings settings = AZ::JsonDeserializerSettings{});`
+ `object` - 要将数据加载到的对象。
+ `root` - 要反序列化的 JSON 树的根。这通常是一个完整的 JSON 文档，但也可以是能正确反序列化为 `T` 类型的任何 JSON 值。
+ `settings` - 配置如何处理反序列化。

 `static AZ::JsonSerializationResult::ResultCode AZ::JsonSerialization::Load(void* object, const AZ::Uuid& objectType, const rapidjson::Value& root, AZ::JsonDeserializerSettings settings = AZ::JsonDeserializerSettings{});`
+ `object` - 指向作为对象分配的内存的指针，该对象与为 `objectType` 注册的类型相匹配。
+ `objectType` - 在 O3DE 运行时注册的 UUID，代表所提供的 `object`的类。该 UUID 所代表的类必须在所提供的序列化上下文中注册。
+ `root` - 要反序列化的 JSON 树的根。这通常是一个完整的 JSON 文档，但也可以是任何能正确反序列化为 `objectType` 类型的 JSON 值。
+ `settings` - 配置如何处理反序列化。

### `AZ::JsonDeserializerSettings` 

通过将 `AZ::JsonDeserializerSettings` 实例设置为 `settings` 参数，可以控制 `AZ::JsonSerialization::Load()` 方法的行为。

`bool m_clearContainers`
如果为 `true`，则在开始反序列化之前会清除要反序列化到的目标 `object` 的容器成员。此选项对具有固定布局的类无效。
**默认值**： `false`

`AZ::SerializeContext* m_serializeContext`
序列化上下文，用于查询有关如何反序列化为对象的信息。
**默认值**： 从 `ComponentApplicationBus` 事件总线获取的全局 序列化 上下文。

`AZ::JsonSerializationResult::JsonIssueCallback m_reporting`
在反序列化过程中遇到错误时调用的回调方法。该函数无法访问正在序列化的对象或正在写入的 JSON 值，但可用于更改结果代码。
**默认值**： 默认问题报告器，用于记录反序列化过程中遇到的警告和错误。

`AZ::JsonRegistrationContext* m_registrationContext`
JSON 注册上下文。有关如何使用自定义 JSON 上下文的示例，请参阅源代码。
**默认值**： 从事件总线获取的全局注册上下文。

**反序列化示例**
下面是一个演示如何反序列化的简短示例。有关如何将 JSON 数据加载到内存中以及如何注册序列化上下文的细节就省略了。

```
rapidjson::Document document;
// ...read json document from file.

SerializableClass instance;
AZ::JsonSerializationResult::ResultCode result = AZ::JsonSerialization::Load(instance, document);
if (result.GetProcessing() == AZ::JsonSerializationResult::Processing::Halted)
{
    AZ_Warning("Deserialization", false,
        "Unable to fully deserialize SerializableClass from json because %s.",
        result.ToString().c_str());
}
```

## 结果代码 

 JSON 序列化器使用`AZ::JsonSerializationResult::ResultCode`类型来报告对象的错误、警告以及成功序列化和反序列化。结果代码分为三个部分：任务、处理结果和最终结果。这些值可分别通过 `GetTask()`、`GetProcessing()` 和 `GetOutcome()`方法获得。检查结果时，应首先从处理开始，查看任务是否成功完成或是否遇到错误。对于非`Success`值，任务会指出在处理过程中的哪个环节出错，而结果则反映了失败发生的原因。

要将序列化结果写入字符串，请使用 `AZ::JsonSerializationResult::ResultCode::AppendToString()` 和 `AZ::JsonSerializationResult::ResultCode::ToString()`, 或 `AZ::JsonSerializationResult::ResultCode::ToOSString()` 方法。

**处理结果**

`Completed`
处理已成功完成。

`Altered`
处理在遇到错误后完成。进行了错误恢复，因此输入和输出不一定一致。

`PartialAlter`
在遇到多个错误后完成了处理，并通过多次修改进行了错误恢复。

`Halted`
处理失败，无法完成。这表示出现了无法恢复的错误或其他严重故障。

**Tasks**

`RetrieveInfo`
从系统中检索信息，例如查询序列化上下文。

`CreateDefault`
创建默认实例，用于在处理过程中提供默认值。

`Convert`
数值之间的类型转换。

`Clear`
清除字段或数值。

`ReadField`
从 JSON 中读取字段以写入对象值。

`WriteValue`
将对象值写入 JSON 字段。

`Merge`
将两个 JSON 值或文档合并在一起。

`CreatePatch`
创建补丁，将一个 JSON 值转换为另一个 JSON 值。

**Outcomes**

`Success`
任务已成功完成。

`Skipped`
任务跳过了一个字段或值。

`PartialSkip`
任务在处理 JSON 对象或数组时跳过一个或多个字段。

`DefaultsUsed`
仅使用默认值完成任务。

`PartialDefaults`
使用某些值的默认值完成任务。

`Unavailable`
任务试图使用空间，而空间并不可用。

`Unsupported`
在执行任务时，要求进行不支持的操作。

`TypeMismatch`
源和目标不相关，无法进行类型转换。

`TestFailed`
针对某个值的测试检查失败。

`Missing`
缺少必填字段或值。

`Invalid`
字段或元素包含无效值。

`Unknown`
任务期间遇到未知信息。

`Catastrophic`
任务期间发生无法识别或未知的灾难性错误。
