---
linkTitle: 序列化上下文
title: O3DE中的序列化上下文
description: 在 Open 3D Engine (O3DE) 中使用序列化上下文，为 C++ 对象或 O3DE 类型提供持久性。
weight: 100
---

您可以使用**Open 3D Engine (O3DE)** 序列化上下文为C++对象或任何O3DE类型提供持久性。 `Code/Framework/AzCore/AzCore/Serialization/SerializeContext.*`中定义了`SerializeContext`类。要实现这一点，请声明`AzTypeInfo`或使用`AZ_RTTI`（运行时类型信息），如下例所示：

```cpp
class SerializedObject
{
public:
    AZ_RTTI(SerializedObject, "");
    static void Reflect(AZ::ReflectContext* context)
    {
        SerializeContext* serializeContext = azrtti_cast<SerializeContext*>(reflection);
        if (serializeContext)
        {
            serializeContext->Class<SerializedObject>()
                ;
        }
    }
};
```

您还可以通过创建 `AZ_TYPE_INFO` 特殊化来反映本地类型和 [POD 结构](https://en.wikipedia.org/wiki/C++_classes#POD-structs) ，以便进行序列化，如以下代码示例所示：

```cpp
AZ_TYPE_INFO_SPECIALIZE(AZStd::chrono::system_clock::time_point, "{5C48FD59-7267-405D-9C06-1EA31379FE82}");
AZ_TYPE_INFO_SPECIALIZE(float, "{EA2C3E90-AFBE-44d4-A90D-FAAF79BAF93D}");
```

## 字段

要将文本字符串与地址关联到序列化对象的字段，请使用 `Field` 函数，如下例所示。您可以使用构建器模式序列化多个字段。

```cpp
serializedContext->Class<SerializedObject>()
    ->Field("myIntField", &SerializedObject::myIntField)
    ->Field("myFloatField", &SerializedObject::myFloatField)
;
```

## 序列化器

序列化器是提供自定义数据格式的有用方法。如果想在写入或读取对象之前对其进行自定义处理，可以覆盖 O3DE 的默认序列化器。

要覆盖默认序列化器，请实现 `AZ::SerializeContext::IDataSerializer` 接口。使用该接口可覆盖数据流转化为持久化格式时的处理方式。您还可以使用该接口来确定反射对象被序列化（读取或写入）时发生的操作。

`AZ::Uuid`类`(Code/Framework/AzCore/AzCore/Math/MathScriptHelpers.h)`提供了一个自定义序列化器的良好示例。要保存 UUID 值，代码会将其直接写入数据流。这部分代码简单明了。

```cpp
/// Store the class data into a binary buffer.
size_t Uuid::Save(const void* classPtr, IO::GenericStream& stream, bool)
{
    const Uuid* uuidPtr = reinterpret_cast<const Uuid*>(classPtr);
    return static_cast<size_t>(stream.Write(16, reinterpret_cast<const void*>(uuidPtr->data)));
}
```

加载 UUID 也很简单，但代码会进行一些错误检查，以确保数据按预期加载：

```cpp
/// Load the class data from a stream.
bool Uuid::Load(void* classPtr, IO::GenericStream& stream, unsigned int /*version*/, bool)
{
    if (stream.GetLength() < 16)
    {
        return false;
    }
    Uuid* uuidPtr = reinterpret_cast<Uuid*>(classPtr);
    if (stream.Read(16, reinterpret_cast<void*>(&uuidPtr->data)) == 16)
    {
        return true;
    }
    return false;
}
```

自定义序列化器具有在二进制和文本格式之间转换数据的功能。通过将数据转换为文本格式，可以将其存储到 `.xml` 或 `.json` 文件中。

下面的 `DataToText` 函数从输入流中读取 UUID 的二进制值。该函数将二进制值转换为 `AZStd::string`，然后将其写入输出流。

```cpp
size_t Uuid::DataToText(IO::GenericStream& in, IO::GenericStream& out, bool)
{
    if (in.GetLength() < 16)
    {
        return 0;
    }
    Uuid value;
    void* dataPtr = reinterpret_cast<void*>(&value.data);
    in.Read(16, dataPtr);
    char str[128];
    value.ToString(str, 128);
    AZStd::string outText = str;
    return static_cast<size_t>(out.Write(outText.size(), outText.data()));
}
```

下面的 `TextToData` 函数将文本输入字符串转换为二进制 UUID 格式，然后将二进制数据写入数据流。

```cpp
/// Convert text data to binary to support the loading of legacy formats.
// Respect the text version if the text->binary format has changed!
size_t Uuid::TextToData(const char* text, unsigned int, IO::GenericStream& stream, bool)
{
    Uuid uuid = Uuid::CreateString(text);
    stream.Seek(0, IO::GenericStream::ST_SEEK_BEGIN);
    return static_cast<size_t>(stream.Write(16, uuid.data));
}
```

## 数据容器

要为无法通过 `SerializeContext::Reflect` 函数直接反映的模板和类型创建自定义序列化，可以使用数据容器。

要创建数据容器，请实现 `AZ::SerializeContext::IDataContainer` 接口。您可以使用该接口为一个类或类模板提供序列化，并让用户选择要序列化的元素。之所以能做到这一点，是因为 `IDataContainer` 允许用户覆盖一个 `EnumElements` 函数。`EnumElements`"函数决定序列化类中哪些元素是枚举元素，因此能够被序列化。

### 模板

数据容器是在序列化上下文中添加模板支持的最佳方式。以下模板有一个[metaclass](https://en.wikipedia.org/wiki/Metaclass)，它实现了 `IDataContainer`接口并将模板序列化。

```cpp
AZStd::vector<T>
AZStd::basic_string<T>
AZStd::unique_ptr<T>
```

### 非模板类型

您可以使用 `IDataContainer` 接口序列化非模板类型，如 `AZStd::any`。这是因为序列化的元素类型取决于存储在`AZStd::any`对象中的类型。

**稳定元素**
如果在容器中添加或移除其他元素时，元素的指针不会发生变化，那么这些元素就被认为是稳定的。O3DE 对稳定元素的实现与[ISO/IEC 14882:2017(E)](https://www.iso.org/standard/68564.html)标准第 26 节中记录的[C++17](https://en.wikipedia.org/wiki/C++17)迭代器失效规则相对应。`AZStd::vector` 等类型中的元素并不稳定，因为它们是以连续序列存储的。当删除一个不在向量末尾的元素时，内存中该元素之后的所有元素都必须向左移动，以保持序列连续。从容器中移除稳定元素不会影响容器中的其他元素。可以使用 `IsStableElements` 函数来确定容器元素的状态。如果容器中的元素不稳定，则必须枚举这些元素才能对其进行序列化。

下面的代码示例展示了如何为存储同质元素动态序列的容器设置序列化。

```cpp
template<class T, bool IsStableIterators>
class AZStdBasicContainer
    : public SerializeContext::IDataContainer
{
public:
    typedef typename T::value_type ValueType;
    typedef typename AZStd::remove_pointer<typename T::value_type>::type ValueClass;
    ///... Functions implementing the IDataContainer interface
};
```

`SerializeContext::ClassElement` 是一个结构体，用于唯一标识一个类的序列化元素。它包括以下字段：

+ `TypeId` -- 用于在 `SerializeContext` 中查找 `ClassData` 中数据的 ID。
+ `Name`, `NameCrc` -- 元素序列化时使用的名称和 CRC。
+ 元素特定的序列化属性。

要查找数据容器支持的 `SerializeContext::ClassElement` 名称，请重载 `GetElement` 函数，如下例所示。

```cpp
// Returns the class element by looking up the CRC value of the element.
// Returns null if the element with the specified name can't be found.
const SerializeContext::ClassElement* GetElement(u32 elementNameCrc) const override
{
    if (elementNameCrc == m_classElement.m_nameCrc)
    {
        return &m_classElement;
    }
    return nullptr;
}
// The following GetElement method uses the supplied DataElement object to lookup the ClassElement with the supplied parameter. Returns true if it finds a ClassElement.
bool GetElement(SerializeContext::ClassElement& classElement, const SerializeContext::DataElement& dataElement) const override
{
    if (dataElement.m_nameCrc == m_classElement.m_nameCrc)
    {
        classElement = m_classElement;
        return true;
    }
    return false;
}
```

下面的示例展示了如何覆盖 `EnumElement` 方法，以指定要枚举的元素。通过枚举这些元素，可以保存它们。

```cpp
/// Enumerate elements in the array.
/// The ElementCB callback enumerates the children of the elements in the array.
/// By invoking the callback on an element, the enumeration continues down the path for that element.
void EnumElements(void* instance, const ElementCB& cb) override
{
    T* arrayPtr = reinterpret_cast<T*>(instance);
    typename T::iterator it = arrayPtr->begin();
    typename T::iterator end = arrayPtr->end();
    for (; it != end; ++it)
    {
        ValueType* valuePtr = &*it;
        if (!cb(valuePtr, m_classElement.m_typeId, m_classElement.m_genericClassInfo ? m_classElement.m_genericClassInfo->GetClassData() : nullptr, &m_classElement))
        {
            break;
        }
    }
}
```

要在**O3DE 编辑器**和反射属性编辑器中编辑模板，请覆盖以下代码中的约束函数：

```cpp
// The following code defines the characteristics of the container that is serialized.
// The editing facilities use this information to determine how to edit the elements within the container.

/// Return the number of elements in the container.
size_t  Size(void* instance) const override
{
    const T* arrayPtr = reinterpret_cast<const T*>(instance);
    return arrayPtr->size();
}
/// Return the capacity of the container. Return 0 for objects without fixed capacity.
size_t Capacity(void* instance) const override
{
    (void)instance;
    return 0;
}

/// Return true if the element pointers do not change when the element is added to or removed from the container. If false, you MUST enumerate all elements.
bool    IsStableElements() const override           { return IsStableIterators; }

/// Return true if the container has a fixed size; otherwise false.
bool    IsFixedSize() const override                { return false; }

/// Return true if the container has a fixed capacity; otherwise false.
bool    IsFixedCapacity() const override            { return false; }

/// Return true if the container is a smart pointer.
bool    IsSmartPointer() const override             { return false; }

/// Return true if the elements can be retrieved by index.
bool    CanAccessElementsByIndex() const override   { return false; }
```

{{< note >}}
+ 当 `IsFixedSize` 和 `IsFixedCapacity` 为 false 时，属性编辑器中的加号 (+) 和减号 (-) 按钮可用于从数据容器中添加和删除元素。
+ 当 `IsSmartPointer` 为 false 时，当元素被添加到数据容器时，数据容器不会创建 `SmartPointer` 类型的实例。
+ 当 `CanAccessElementsByIndex` 为 false 时，序列化系统会检查是否为新元素分配内存。对于固定大小的容器，如 `AZStd::array`、`AZStd::pair` 和 `AZStd::tuple`，`CanAccessElementsByIndex` 为 true，因为这些容器已经为其元素分配了内存。
{{< /note >}}

要将元素加载到模板类实例中，请重载 `ReserveElement`、`StoreElement` 和 `RemoveElements` 函数，如下例所示。

```cpp
/// Use the reserve element function.
/// The reserve element function allows creation of the element on the data container instance.
/// The following code serializes an element and returns an address to the reserved element.
void*   ReserveElement(void* instance, const SerializeContext::ClassElement* classElement) override
{
    (void)classElement;
    T* arrayPtr = reinterpret_cast<T*>(instance);
    arrayPtr->push_back();
    return &arrayPtr->back();
}
/// Use the GetElementByIndex function to get an element's address by its index.
// Call this function before the element is loaded.
void*   GetElementByIndex(void* instance, const SerializeContext::ClassElement* classElement, size_t index) override
{
    (void)instance;
    (void)classElement;
    (void)index;
    return nullptr;
}
/// Use the store element function.
void    StoreElement(void* instance, void* element) override
{
    (void)instance;
    (void)element;
    // Do nothing; you have already pushed the element.
    // However, you can assert and check if the element belongs to the container.
}
/// Remove the element from the container.
/// This also deletes the memory associated with the element.
bool    RemoveElement(void* instance, const void* element, SerializeContext* deletePointerDataContext) override
{
    T* arrayPtr = reinterpret_cast<T*>(instance);
    for (typename T::iterator it = arrayPtr->begin(); it != arrayPtr->end(); ++it)
    {
        void* arrayElement = &(*it);
        if (arrayElement == element)
        {
            if (deletePointerDataContext)
            {
                DeletePointerData(deletePointerDataContext, &m_classElement, arrayElement);
            }
            arrayPtr->erase(it);
            return true;
        }
    }
    return false;
}
/// Remove elements (remove an array of elements) whether the container is stable or not. Stability can be tested by IsStableElements.
size_t  RemoveElements(void* instance, const void** elements, size_t numElements, SerializeContext* deletePointerDataContext) override
{
    if (numElements == 0)
    {
        return 0;
    }
    size_t numRemoved = 0;
    // Handle the case when the container does not have stable elements.
    if (!IsStableIterators)
    {
        // If the elements are in order, you can remove all of them from the container.
        // Otherwise, they must be sorted again locally (not done in this example).
        // Or, ask the user to pass the elements in order and remove the first N possible in order.
        for (size_t i = 1; i < numElements; ++i)
        {
            if (elements[i - 1] >= elements[i])
            {
                AZ_TracePrintf("Serialization", "RemoveElements for AZStd::vector will perform optimally when the elements (addresses) are sorted in accending order!");
                numElements = i;
            }
        }
        // Traverse the vector in reverse order, and then addresses of elements that should not change.
        for (int i = static_cast<int>(numElements); i >= 0; --i)
        {
            if (RemoveElement(instance, elements[i], deletePointerDataContext))
            {
                ++numRemoved;
            }
        }
    }
    else
    {
        for (size_t i = 0; i < numElements; ++i)
        {
            if (RemoveElement(instance, elements[i], deletePointerDataContext))
            {
                ++numRemoved;
            }
        }
    }
    return numRemoved;
}
/// Clear elements in the instance.
void    ClearElements(void* instance, SerializeContext* deletePointerDataContext) override
{
    T* arrayPtr = reinterpret_cast<T*>(instance);
    if (deletePointerDataContext)
    {
        for (typename T::iterator it = arrayPtr->begin(); it != arrayPtr->end(); ++it)
        {
            DeletePointerData(deletePointerDataContext, &m_classElement, &(*it));
        }
    }
    arrayPtr->clear();
}
```

## 使用数据容器序列化模板类

定义数据容器后，就可以用它来序列化特定类型。例如，要为模板化的 `AZStd::vector<T>` 设置序列化，就必须序列化 `AZStd::vector` 的 `SerializeGenericTypeInfo<T>` 。要创建类数据结构，请使用下面的 `Create<ContainerType>` 函数：

```cpp
SerializeContext::ClassData::Create<ContainerType>("AZStd::vector", GetSpecializedTypeId(), Internal::NullFactory::GetInstance(), nullptr, &m_containerStorage);
```

`Create<ContainerType>` 函数参数的说明见下表。
1
| 参数 | 说明 |
| --- | --- |
| "AZStd::vector" | 以 JSON 或 XML 数据流形式指定类的用户友好名称。 |
| GetSpecializedTypeId() | 创建一个 ID，以实现不同类型的序列化。例如，整数的 `AZStd::vector`可以序列化为不同于浮点数的 `AZStd::vector`的类型。唯一 ID 是由模板类型 `AZStd::vector` 与所含类型 `T` 聚合而成。 |
| Internal::NullFactory::GetInstance() | NullFactory 用于防止堆内存被用于创建 `AZStd::vector`。要加载指针类型的 `AZStd::vector`元素，请将其改为`Serialize::InstanceFactory<AZStd::vector<T,A>>`。 |
| nullptr  | 这是序列化器参数。由于序列化是通过数据容器进行的，因此该参数为 nullptr。 |
| &m_containerStorage | `m_containerStorage` 结构是一个 `AZStdBasicContainer` ，ClassData 使用它来序列化 `AZStd::vector`元素数组。|

以下代码示例使用 `Create<ContainerType>` 函数为模板化的 `AZStd::vector<T>` 设置序列化。

```cpp
/// Generic serialization example for AZStd::vector.
template<class T, class A>
struct SerializeGenericTypeInfo< AZStd::vector<T, A> >
{
    typedef typename AZStd::vector<T, A> ContainerType;
    class GenericClassInfoVector
        : public GenericClassInfo
    {
    public:
        AZ_TYPE_INFO(GenericClassInfoVector, "{2BADE35A-6F1B-4698-B2BC-3373D010020C}");
        GenericClassInfoVector()
        {
            // The following code creates the ClassData structure that specifies how an element is serialized.
            m_classData = SerializeContext::ClassData::Create<ContainerType>("AZStd::vector", GetSpecializedTypeId(), Internal::NullFactory::GetInstance(), nullptr, &m_containerStorage);
        }
        SerializeContext::ClassData* GetClassData() override
        {
            return &m_classData;
        }
        size_t GetNumTemplatedArguments() override
        {
            return 1;
        }
        const Uuid& GetTemplatedTypeId(size_t element) override
        {
            (void)element;
            return SerializeGenericTypeInfo<T>::GetClassTypeId();
        }
        const Uuid& GetSpecializedTypeId() const override
        {
            return azrtti_typeid<ContainerType>();
        }
        const Uuid& GetGenericTypeId() const override
        {
            return TYPEINFO_Uuid();
        }
        void Reflect(SerializeContext* serializeContext)
        {
            if (serializeContext)
            {
                serializeContext->RegisterGenericClassInfo(GetSpecializedTypeId(), this, &AnyTypeInfoConcept<ContainerType>::CreateAny);
                if (GenericClassInfo* containerGenericClassInfo = m_containerStorage.m_classElement.m_genericClassInfo)
                {
                    containerGenericClassInfo->Reflect(serializeContext);
                }
            }
        }
        static GenericClassInfoVector* Instance()
        {
            static GenericClassInfoVector s_instance;
            return &s_instance;
        }
        Internal::AZStdBasicContainer<ContainerType, false> m_containerStorage;
        SerializeContext::ClassData m_classData;
    };
    static GenericClassInfo* GetGenericInfo()
    {
        return GenericClassInfoVector::Instance();
    }
    static const Uuid& GetClassTypeId()
    {
        return GenericClassInfoVector::Instance()->m_classData.m_typeId;
    }
};
```

## 事件

要在读取或写入序列化数据之前或之后处理数据，可以编写序列化事件处理程序。例如，通过处理序列化事件，可以针对序列化的数据执行运行时初始化。

要创建序列化事件处理程序，请按以下示例实现 `AZ::SerializeContext::IEventHandler` 接口。

该示例使用了一个事件处理程序，以在 `SceneData` 实例序列化后更新 `SceneData` 类中的映射容器。

```cpp
class SceneDataEventHandler : public AZ::SerializeContext::IEventHandler
{
public:
    /// Rebuild the endpoint map.
    void OnWriteEnd(void* classPtr) override
    {
        auto* sceneData = reinterpret_cast<SceneData*>(classPtr);
        BuildEndpointMap((*sceneData));
    }
};

// Next add the event handler to the reflection of the class that needs to perform additional data processing.

if (AZ::SerializeContext* serializeContext = azrtti_cast<AZ::SerializeContext*>(context))
{
    serializeContext->Class<SceneData>()
          ->EventHandler<SceneDataEventHandler>()
          ;
}
```

## 数据覆盖

在序列化过程中，您可以使用序列化上下文从外部来源提供数据。这些外部数据源称为**数据覆盖**。

要创建数据叠加，您需要实现一个[**事件总线（EBus）**](/docs/user-guide/programming/messaging/ebus/) ，通过它对数据进行序列化。下面的示例代码实现了数据叠加功能的单元测试：

```cpp
struct DataOverlayTestStruct
{
    AZ_TYPE_INFO(DataOverlayTestStruct, "{AD843B4D-0D08-4CE0-99F9-7E4E1EAD5984}");
        AZ_CLASS_ALLOCATOR(DataOverlayTestStruct, AZ::SystemAllocator, 0);
        DataOverlayTestStruct()
        : m_int(0)
        , m_ptr(nullptr) {}
    int                     m_int;
    AZStd::vector<int>      m_intVector;
    DataOverlayTestStruct*  m_ptr;
};
```

`DataOverlayTestStruct` 包含序列化时要反射的数据字段：

```cpp
serializeContext.Class<DataOverlayTestStruct>()
                  ->Field("int", &DataOverlayTestStruct::m_int)
                  ->Field("intVector", &DataOverlayTestStruct::m_intVector)
                  ->Field("pointer", &DataOverlayTestStruct::m_ptr);
```

接下来，实现数据覆盖提供程序。该提供程序代表覆盖到序列化数据中的数据源。

下面的代码展示了一个数据覆盖提供程序的示例：

```cpp
class DataOverlayProviderExample
    : public DataOverlayProviderBus::Handler
{
public:
    static DataOverlayProviderId    GetProviderId() { return AZ_CRC("DataOverlayProviderExample", 0x60dafdbd); }
    static u32                      GetIntToken() { return AZ_CRC("int_data", 0xd74868f3); }
    static u32                      GetVectorToken() { return AZ_CRC("vector_data", 0x0aca20c0); }
    static u32                      GetPointerToken() { return AZ_CRC("pointer_data", 0xa46a746e); }
    DataOverlayProviderExample()
    {
        m_ptrData.m_int = 5;
        m_ptrData.m_intVector.push_back(1);
        m_ptrData.m_ptr = nullptr;
        m_data.m_int = 3;
        m_data.m_intVector.push_back(10);
        m_data.m_intVector.push_back(20);
        m_data.m_intVector.push_back(30);
        m_data.m_ptr = &m_ptrData;
    }
    void FillOverlayData(DataOverlayTarget* dest, const DataOverlayToken& dataToken) override
    {
        if (*reinterpret_cast<const u32*>(dataToken.m_dataUri.data()) == GetIntToken())
        {
            dest->SetData(m_data.m_int);
        }
        else if (*reinterpret_cast<const u32*>(dataToken.m_dataUri.data()) == GetVectorToken())
        {
            dest->SetData(m_data.m_intVector);
        }
        else if (*reinterpret_cast<const u32*>(dataToken.m_dataUri.data()) == GetPointerToken())
        {
            dest->SetData(*m_data.m_ptr);
        }
    }
    DataOverlayTestStruct   m_data;
    DataOverlayTestStruct   m_ptrData;
};
```

`DataOverlayProviderExample`使用 `Crc32` ID来反射`DataOverlayTestStruct`源数据字段。然后，该示例实现了 `DataOverlayProviderBus::Handler``FillOverlayData` 函数。`FillOverlayData`函数是实际数据叠加的地方。`DataOverlayToken`保存被序列化字段的 ID。如果 ID 与要覆盖的字段匹配，则可以使用 `DataOverlayTarget` 设置数据。

