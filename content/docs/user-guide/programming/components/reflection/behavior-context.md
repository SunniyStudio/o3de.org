---
linkTitle: 行为上下文
title: O3DE中的行为上下文
description: 在Open 3D Engine (O3DE)中使用行为上下文，使脚本系统（如 Script Canvas 和 Lua）可以访问运行时代码。
weight: 300
---

行为上下文可让 **Open 3D Engine (O3DE)** 脚本系统（如 Script Canvas 或 Lua）访问运行时代码。它提供了脚本绑定，可调用运行时 C++ 方法、读取常量、写入属性以及创建和处理 **Event Bus (Ebus)** 事件。你可以拥有多个专门用于不同目的的行为上下文，还可以取消对行为上下文的反射以实现重载。

使用行为上下文可绑定以下 C++ 结构以编写脚本：

+ [类](#classes)
+ [方法](#methods)
+ [属性](#properties)
+ [常量](#constants)
+ [枚举](#enums)

此外，行为上下文还支持 O3DE [EBus](/docs/user-guide/programming/messaging/ebus) 和 [AZ::Event](/docs/user-guide/programming/messaging/az-event)事件系统：

+ [EBus](#ebus)
  + [事件](#events)
  + [事件处理程序](#event-handlers)

## 类

行为上下文中的类反射了 C++ 类或结构体。您可以为类提供一个可选名称。如果不提供名称，则使用 `AzTypeInfo` 中的类名。该名称在作用域中必须是唯一的。由于系统使用 `AzRTTI` 来构建类的层次结构，因此如果您想反射基类的功能，可以使用 RTTI。

绑定到行为上下文的类将成为可在脚本环境中实例化的对象。要反射一个类，必须将所反射的类型作为模板参数提供给类函数。例如：

```cpp
if (AZ::BehaviorContext* behaviorContext = azrtti_cast<AZ::BehaviorContext*>(context))
{
    behaviorContext->Class<MyClass>();
}
```

如果适用，还应指定基类：

```cpp
if (AZ::BehaviorContext* behaviorContext = azrtti_cast<AZ::BehaviorContext*>(context))
{
    behaviorContext->Class<MyClass, TheBaseClass>();
}
```

### 类构建器函数

要为类绑定提供额外配置，可以在行为上下文 `Class` 函数之后链入类构建器函数。以下是可用的构建器函数：

| 构建器功能 | 说明|
| --- | --- |
| **Allocator** | 允许你为你的类提供一个自定义的分配器/去分配器，以覆盖任何现有的分配模式。如果不提供自定义分配器，则使用 `aznew`(`AZ_CLASS_ALLOCATOR`)。 |
| **Constant** | 只读属性。有关此构建函数的更多信息，请参阅 [常量](#constants) 部分。 |
| **Constructor** | 允许您枚举要反射的类构造函数。您必须将所有构造函数参数作为模板参数传递。|
| **Enum** | 只读的 `int` 属性。有关此构造函数的更多信息，请参阅 [枚举](#enums) 章节。 |
| **Method** | 反射 C++ 类方法。也可以反射全局方法。有关此构建函数的更多信息，请参阅 [方法](#methods) 章节。 |
| **Property** | 反射类数据。也可反射全局属性。有关此构建函数的更多信息，请参阅 [properties](#properties)。 |
| **UserData** | 允许您提供指向用户数据的指针。您为该类实现的所有回调（如自定义分配器）都可以访问该指针。 |
| **Wrapping <br> WrappingMember** | 向行为上下文表明该类是另一个类的包装器。这在反射智能指针和字符串包装器时非常有用。 |

#### 类构建器函数的 C++ 示例

```cpp
// Custom allocator and deallocator.
void* ScriptClassAllocate(void* userData);
void ScriptClassFree(void* obj, void* userData);

// Reflect the custom allocator and deallocator for use with ScriptClass.
behaviorContext->Class<ScriptClass>()
    ->Allocator(&ScriptClassAllocate, &ScriptClassFree);

// Reflect two constructors for BoxShapeConfig.
behaviorContext->Class<BoxShapeConfig>()
    ->Constructor()
    ->Constructor<AZ::Vector3&>()
    ->Property("Dimensions", BehaviorValueProperty(&BoxShapeConfig::m_dimensions));

// Allow BehaviorEntity to be passed to functions expecting AZ::Entity*.
behaviorContext->Class<BehaviorEntity>("Entity")
    ->WrappingMember<AZ::Entity*>(&BehaviorEntity::GetRawEntityPtr);

// Reflect a method, two properties, a constant, and an enum for ScriptClass.
behaviorContext->Class<ScriptClass>()
    ->Method("MemberFunc0", &ScriptClass::MemberFunc0)
    ->Property("data", &ScriptClass::GetData, &ScriptClass::SetData)
    ->Property("data1", BehaviorValueProperty(&ScriptClass::m_data1))
    ->Constant("EPSILON", BehaviorConstant(0.001f))
    ->Enum<ScriptClass::SC_ET_VALUE2>("SC_ET_VALUE2");
```

有关这些函数的更多信息，请参阅 O3DE`AzCore` API 参考 中的 [ClassBuilder](/docs/api/frameworks/azcore/struct_a_z_1_1_behavior_context_1_1_class_builder.html) 。

### 属性

除构建器函数外，您还可以使用以下属性来装饰一个类：

| 属性 | 说明 | 类型 | 值 |
| --- | --- | --- | --- |
| **Category** | 编辑器用于对列表中的对象进行分类。要嵌套类别，可以使用斜线 (`/`) 分隔符。例如： <br> `Attribute(AZ::Script::Attributes::Category, "Gameplay/Triggers")` | `string` | |
| **ClassNameOverride** | 为脚本反射提供不同于行为上下文名称的自定义名称。 | `string` | |
| **ConstructibleFromNil** | 指定在提供 nil 时是否默认构造类。 | `bool` | `true`, <br> `false` |
| **ConstructorOverride** | 提供一个自定义构造函数，在通过 Lua 脚本创建时调用。 | function pointer | |
| **Deprecated** | 将反射的类、方法、EBus 或属性标记为已废弃。 | `bool` | `true`, <br> `false` |
| **ExcludeFrom** | 从编辑器列表、自文档、预览构建或上述所有选项中隐藏对象。这个可选标记主要用于不打算通过脚本访问的内部对象。 | `AZ::Script::Attributes::ExcludeFlags` | `List`, <br> `Documentation`, <br> `Preview`, <br> `All` |
| **Ignore** | 指定是否在反射过程中忽略元素。 | `bool` | `true`, <br> `false` |
| **Storage** | 指定反射对象内存存储空间的所有者。所有者可以是脚本系统（`ScriptOwn`）、本地运行时代码（`RuntimeOwn`）或脚本系统的虚拟机（`Value`）。 | `AZ::Script::Attributes::StorageType` | `ScriptOwn`, <br> `RuntimeOwn`, <br> `Value` |
| **ToolTip** | 编辑器用来在工具提示中显示附加信息。 | `string` | |

#### 属性的C++示例

```cpp
behaviorContext->Class<AreaBlenderConfig>()
    ->Attribute(AZ::Script::Attributes::Category, "Vegetation");

behaviorContext->Class<BlastFamilyComponent>()
    ->Attribute(AZ::Script::Attributes::Scope, AZ::Script::Attributes::ScopeFlags::Common);

behaviorContext->Class<AZ::GameplayNotificationId>("GameplayNotificationId")
    ->Attribute(AZ::Script::Attributes::Deprecated, true)
    ->Constructor<AZ::EntityId, AZ::Crc32>()
        ->Attribute(AZ::Script::Attributes::Storage, AZ::Script::Attributes::StorageType::Value)
        ->Attribute(AZ::Script::Attributes::ConstructorOverride, &GameplayEventIdNonIntrusiveConstructor);

behaviorContext->Class<MathUtils>("MathUtils")
    ->Method("ConvertTransformToEulerRadians", &AZ::ConvertTransformToEulerRadians)
        ->Attribute(AZ::Script::Attributes::ExcludeFrom, AZ::Script::Attributes::ExcludeFlags::All);
```

有关脚本属性的完整列表，请参阅 O3DE 源中的 [`Code/Framework/AzCore/AzCore/Script/ScriptContextAttributes.h`](https://github.com/o3de/o3de/blob/main/Code/Framework/AzCore/AzCore/Script/ScriptContextAttributes.h)。

### Lua 用法示例

用于编写脚本的类将成为可以在 Lua 中实例化的对象：

```lua
local myObj = MyClass()
```

要在 Lua 中使用非默认构造函数，首先要将构造函数反射到行为上下文中：

```cpp
behaviorContext->Class<MyClass>("MyClass")
    ->Constructor<int>();
```

然后就可以在 Lua 中实例化了：

```lua
local myClass = MyClass(10)
```

## 方法

使用行为上下文 `Method` 函数来反射 C++ 全局方法和类方法。每个方法的作用域都必须有一个唯一的名称。

您可以将方法作为自由函数或类的一部分来反射：

```cpp
// This method is reflected as a free function:
behaviorContext->Method("AZTestAssert", &AZTestAssert);

// This method is reflected as a part of a class:
behaviorContext->Class<MyMath>("MyMath")
    ->Method("Cos", &cosf);
```

为编写脚本而反射的类方法可通过反射的类进行访问：

```lua
-- Method from a class:
local math = MyMath()
local result = math:Cos(3.14)

-- Free method:
AZTestAssert(ScriptClass ~= nil)
```

要为方法提供默认值，请使用 `AZ::BehaviorDefaultValuePtr`：

```cpp
int globalMethod(int a)
{
    return a + 3;
}

behaviorContext->Method("GlobalMethod", &globalMethod, AZ::BehaviorDefaultValuePtr(aznew AZ::BehaviorDefaultValue(255)));
```

在绑定方法时，为提高可用性并方便文档记录，请提供描述方法参数的字符串：

```cpp
// Given this method:
bool BoundsCheck(float value, float minBounds, float maxBounds)
{
    return value >= minBounds && value < maxBounds;
}

// Bind the given method to the behavior context with friendly argument names.
behaviorContext->Method("BoundsCheck", &BehaviorTestClass::BoundsCheck,
      {{{"Value", "Value which will be checked to be within the two bounds arguments."},
        {"Minimum Bound Value", "Check if value is greater than or equal to this minimum value.", AZ::BehaviorDefaultValuePtr(aznew AZ::BehaviorDefaultValue(static_cast<float>(0.0)))},
        {"Maximum Bound Value", "Check if value is less than this maximum value.", AZ::BehaviorDefaultValuePtr(aznew AZ::BehaviorDefaultValue(static_cast<float>(1.0)))}}}
    );
```

这种方法在 Script Canvas 中特别有用，用户可以理解他们需要提供的参数的含义。

## 属性

使用行为上下文属性向脚本公开全局和类数据。每个属性的作用域都必须有一个唯一的名称。可以使用 getter 和 setter 方法查询和设置属性值。如果不为某个属性提供 getter 方法，则该属性只能写入。如果不提供 setter 方法，则该属性只能读取。

你可以使用全局函数、成员函数或 lambda 表达式作为属性的获取器和设置器。

为方便起见，O3DE 提供了以 lambda 表达式实现 getter 和 setter 函数的宏。使用 `BehaviorValueProperty(&value)`可以同时实现一个属性的getter和setter方法。或者，可以使用 `BehaviorValueGetter` 和 `BehaviorValueSetter` 单独实现获取器和设置器函数。下表包含每个宏的使用示例：

| 操作 | 宏 | 实例 |
| --- | --- | --- |
| Getter | BehaviorValueGetter |  <pre>->Property("ReadOnlyFlag", BehaviorValueGetter(&MyClass::m_readOnlyFlag), nullptr)</pre> |
| Setter | BehaviorValueSetter |  <pre>->Property("WriteOnlyFlag", nullptr, BehaviorValueSetter(&MyClass::m_writeOnlyFlag))</pre> |
| Both | BehaviorValueProperty |  <pre>->Property("ReadWriteFlag", BehaviorValueProperty(&MyClass::m_readWriteFlag))</pre> |

下面的示例将类成员数据 `m_upperDistanceInMeters` 作为读写属性 `UpperDistanceInMeters` 公开：

```cpp
behaviorContext->Class<SurfaceTagDistance>()
    ->Attribute(AZ::Script::Attributes::Category, "Vegetation")
    ->Property("UpperDistanceInMeters", BehaviorValueProperty(&SurfaceTagDistance::m_upperDistanceInMeters));
```

要执行比简单获取或设置值更复杂的操作，你可以实现自己的获取器和设置器，而不是使用属性宏：

```cpp
behaviorContext->Property("SpawnerType", &Descriptor::GetSpawnerType, &Descriptor::SetSpawnerType);

AZ::TypeId Descriptor::GetSpawnerType() const
{
    // Do stuff, then...
    return m_spawnerType;
}

void Descriptor::SetSpawnerType(const AZ::TypeId& spawnerType)
{
    m_spawnerType = spawnerType;
    SpawnerTypeChanged();
}
```

## 常量

常量在行为上下文中以只读 [属性](#properties) 的形式实现。为了简化常量的反映，`BehaviorContext` 类提供了两个辅助函数： `Constant` 和 `ConstantProperty`。这些函数有助于定义获取函数，使脚本能够读取常量的值。请注意，若要为反映的常量关联附加属性，必须使用 `ConstantProperty` 函数。

### `Constant` 函数

使用 `Constant` 辅助函数为行为上下文中的 C++ 常量定义获取器。您可以使用行为上下文宏 `BehaviorConstant` 为您实现 lambda 获取器。

为了方便起见，你可以将对 `Constant` 和许多其他行为上下文函数的调用串联起来：

```cpp
behaviorContext->Constant("SystemEntityId", BehaviorConstant(SystemEntityId))
               ->Constant("PI", BehaviorConstant(3.14f));
```

要将常量与类关联，可在 `BehaviorContext::Class` 函数返回的 `ClassBuilder` 对象上调用该常量：

```cpp
behaviorContext->Class<AxisWrapper>("AxisType")
    ->Constant("XPositive", BehaviorConstant(AZ::Transform::Axis::XPositive))
    ->Constant("XNegative", BehaviorConstant(AZ::Transform::Axis::XNegative))
    ->Constant("YPositive", BehaviorConstant(AZ::Transform::Axis::YPositive))
    ->Constant("YNegative", BehaviorConstant(AZ::Transform::Axis::YNegative))
    ->Constant("ZPositive", BehaviorConstant(AZ::Transform::Axis::ZPositive))
    ->Constant("ZNegative", BehaviorConstant(AZ::Transform::Axis::ZNegative));
```

与名称相关联的类就像是常量的命名空间。例如，要在 Lua 中获取上例中 `XPositive` 的值，可以使用 `AxisType.XPositive`。

如果没有为关联类指定名称，则不需要名称空间前缀。

### `ConstantProperty` 函数

`ConstantProperty`函数与`Constant`函数类似，可帮助您在行为上下文中定义常量的获取器。此外，由于该函数返回一个 `GlobalPropertyBuilder` 对象，因此您可以用它将其他属性与所反映的常量关联起来：

```cpp
behaviorContext->ConstantProperty("DefaultMaterialAssignment", BehaviorConstant(DefaultMaterialAssignment))
    ->Attribute(AZ::Script::Attributes::Scope, AZ::Script::Attributes::ScopeFlags::Common)
    ->Attribute(AZ::Script::Attributes::Category, "render")
    ->Attribute(AZ::Script::Attributes::Module, "render");
```

## 枚举

与常量类似，C++ 枚举也在行为上下文中以只读 [属性](#properties) 的形式实现。为了简化枚举的反映，`BehaviorContext` 类提供了两个辅助函数： `Enum` 和 `EnumProperty`。这些函数有助于定义获取函数，使脚本能够读取枚举的值。请注意，要将附加属性与反映的枚举关联起来，必须使用 `EnumProperty` 函数。

### `Enum` 函数

使用 `Enum` 辅助函数在行为上下文中定义 C++ 枚举的获取器。由于每个枚举值本身都是一个属性，要在行为上下文中反映整个枚举，必须反映其每个值。

为方便起见，您可以连锁调用 `Enum` 和许多其他行为上下文函数：

```cpp
enum class MyTypes
{
    One = 1,
    Two = 2,
};

behaviorContext->Enum<aznumeric_cast<int>(MyTypes::One)>("MyTypes_One")
    ->Enum<aznumeric_cast<int>(MyTypes::Two)>("MyTypes_Two");
```

以这种方式反映枚举值时，请注意给每个值取一个唯一的名称，因为在行为上下文中，每个属性都必须有一个唯一的名称。

要将枚举与类关联，请在 `BehaviorContext::Class` 函数返回的 `ClassBuilder` 对象上调用它：

```cpp
behaviorContext->Class<PhotometricValue>("PhotometricUnit")
    ->Enum<static_cast<int>(PhotometricUnit::Candela>("Candela")
    ->Enum<static_cast<int>(PhotometricUnit::Lumen>("Lumen")
    ->Enum<static_cast<int>(PhotometricUnit::Lux>("Lux")
    ->Enum<static_cast<int>(PhotometricUnit::Unknown>("Unknown");
```

您用名称反映的关联类就像常量的命名空间。例如，要在 Lua 中获取上例中的 `Lumen` 值，您可以使用 `PhotometricUnit.Lumen`。

如果没有为相关类指定名称，则不需要名称空间前缀。

### `EnumProperty` 函数

`EnumProperty`函数与`Enum`函数类似，可帮助您在行为上下文中定义枚举的获取器。此外，由于该函数返回一个 `GlobalPropertyBuilder` 对象，因此您可以用它将附加属性与所反映的枚举关联起来：

```cpp
behaviorContext->EnumProperty<static_cast<int>(FrameCaptureResult::None)>("FrameCaptureResult_None")
    ->Attribute(AZ::Script::Attributes::Scope, AZ::Script::Attributes::ScopeFlags::Automation)
    ->Attribute(AZ::Script::Attributes::Module, "atom");
```

### `AZ_ENUM`工具宏

O3DE `AzCore`提供了一个名为`AZ_ENUM_DEFINE_REFLECT_UTILITIES`的实用程序宏。该宏与 `AZ_ENUM` 宏配合使用，可生成能反映枚举所有值的实用程序函数，因此无需单独反映每个值。这对于在开发过程中可能会更改的枚举或包含大量值的枚举非常有用。

要使用这个宏，您必须使用 `AZ_ENUM`宏，包括 `AZ_ENUM`、`AZ_ENUM_WITH_UNDERLYING_TYPE`、`AZ_ENUM_CLASS` 或 `AZ_ENUM_CLASS_WITH_UNDERLYING_TYPE`，来定义枚举。您还必须在希望反映枚举的源代码中包含 `<AzCore/Preprocessor/EnumReflectUtils.h>` 。

```cpp
// Define the enum and its values.
AZ_ENUM_CLASS(TestEnum, Walk, Run, Fly);

// Generate the utility functions for reflection.
AZ_ENUM_DEFINE_REFLECT_UTILITIES(TestEnum)

// Call the generated reflect function from within your component's Reflect function.
TestEnumReflect(*behaviorContext);
```

有关如何使用这些宏的更多信息，请查看`Code\Framework\AzCore\Tests\EnumTests.cpp`中的测试代码或使用这些宏的 Gem，如 `Gems\Atom\RHI\Code\Source\RHI.Reflect\RenderStates.cpp` 或 `Gems\Atom\RHI\Code\Include\Atom\RHI.Reflect\RenderStates.h`中的测试代码。

## EBus

为使脚本能够发送和接收事件，请将 EBus 事件和事件处理程序绑定到行为上下文。

### 事件

EBus 提供了一种机制，可将事件广播给所有处理程序，或将事件直接发送给特定 ID 连接的处理程序。当你绑定一个事件时，O3DE会根据你的EBus配置自动反射`Broadcast`, `Event`, `QueueBroadcast`, 和 `QueueEvent`。

```cpp
behaviorContext.EBus<TestBus>("TestBus")->
    Handler<TestBusHandler>()->
    Event("SetSum1", &TestBus::Events::SetSum1)->
    Event("GetSum1", &TestBus::Events::GetSum1)->
;
```

编译后，您可以在 Script Canvas 中使用事件，并在 Lua 中调用它们：

```lua
local result = TestBus.Broadcast.GetSum1(1)
```

### 事件处理程序

事件处理程序反映了一个类，您必须实现该类才能将 EBus 的消息转发给行为上下文方法。要实现事件处理程序，必须创建一个能监控指定 EBus 并将消息转发到行为上下文的类。

之所以需要这样做，是因为行为上下文无法保证每条消息都有处理程序。如果消息期望一个结果，则必须提供一个默认结果，以防行为上下文用户不处理该消息。请记住，系统会根据行为上下文的需要创建尽可能多的处理程序。处理程序还可以在不同的线程中执行。因此，应避免为变化的值提供静态存储。例如：

1. 给定以下 EBus：

    ```cpp
    class TestBusMessages
        : public AZ::EBusTraits
    {
    public:
        virtual void    SetSum1(int) = 0;
        virtual int     GetSum1(int) = 0;
    };

    using TestBus = AZ::EBus<TestBusMessages>;
    ```

1. 实现事件处理程序绑定：

    ```cpp
    class TestBusHandler
        : public TestBus::Handler
        , public AZ::BehaviorEBusHandler
    {
    public:
        AZ_EBUS_BEHAVIOR_BINDER(TestBusHandler, "{CD26E702-6F40-4FF9-816D-4DCB652D97DF}", AZ::SystemAllocator,
                SetSum1,
                GetSum1);
            void SetSum1(int d1) override
            {
                Call(FN_SetSum1, d1);
            }
            int GetSum1(int d1) override
            {
                int result = 0;
                CallResult(result, FN_GetSum1, d1);
                return result;
            }
        };
    ```

   该处理程序将 C++ EBus 接口与 Lua 等脚本语言绑定在一起。

1. 告诉行为上下文反射事件处理程序可用：

    ```cpp
    behaviorContext.EBus<TestBus>("TestBus")->
        Handler<TestBusHandler>()->
        Event("SetSum1", &TestBus::Events::SetSum1)->
        Event("GetSum1", &TestBus::Events::GetSum1)->
    ;
    ```

1. 可选择提供 EBus 处理程序的实现：

   ```cpp
   MyBusHandlerMetaTable1 = {
       SetSum1 = function(self, _1)
           -- custom handler code can go here!
           TestAssert(_1 == 1)
       end,
       GetSum1 = function(self, _1)
           -- custom handler code can go here!
           return _1
       end,
   }
   handler = TestBus.Connect(MyBusHandlerMetaTable1)
   ```

