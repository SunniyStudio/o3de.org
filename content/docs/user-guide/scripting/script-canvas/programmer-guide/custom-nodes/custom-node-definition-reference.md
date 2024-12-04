---
linktitle: 节点定义参考
title: Script Canvas 节点定义参考
description: 有关定义使用 autogen 创建 Script Canvas 节点的节点定义文件时可用的所有 XML 元素的参考指南。
weight: 250
---

_Script Canvas 节点定义文件_是[AzAutoGen](/docs/user-guide/programming/autogen/)  用于生成必要的 C++ 代码的 XML 文件，用于注册节点、定义其拓扑并减少原本必要的“样板”代码量。本参考指南涵盖了创建 Script Canvas 节点所需的所有可用 XML 标签和属性。

---

### ScriptCanvas

__ScriptCanvas__ 标签是节点的顶级描述。它用于生成代表节点的C++类，并配置其序列化和编辑器属性。

| __属性__   |  __必须__  | __说明__ | __示例__ |
| ---------- | ------------| ------------| ------------|
| __Include__   | Required |  Script Canvas 节点的名称 | `Include="Include/ScriptCanvas/Internal/Nodeables/BaseTimer.h"` |
| __xmlns:xsi__ | Recommended | 指示架构中使用的元素和数据类型来自 `"http://www.w3.org/2001/XMLSchema"`命名空间。 它还指定来自它的元素和数据类型应以`xsi:`为前缀。 | `xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"` |

----

### Class

__Class__ 标记用于定义节点在 O3DE 中的外观。其中许多属性控制着节点的序列化。

| __属性__   |  __必须__  | __说明__                                                                                     | __示例__ |
| ---------- | ------------|--------------------------------------------------------------------------------------------|------------|
| __Name__   | Required | Script Canvas 节点的名称                                                                        | `Name="My Node"` |
| __QualifiedName__  | Required | 完全限定名称，包括所有命名空间。例如 `ScriptCanvas::Nodes::MyNode`.                                          | `QualifiedName="ScriptCanvas::Nodes::MyNode"` |
| __Description__ | Recommended | 描述节点及其功能，这在节点上显示为工具提示                                                                      | `Description="An example node"` |
| __Category__ | Recommended | Category 定义此节点在 Node Palette 上的显示位置。您可以通过使用 '/' 字符分隔来嵌套类别。                                 | `Category="Math/Linear Algebra"` |
| __PreferredClassName__ | Optional | 在某些情况下，可能需要在 Script Canvas Editor 中覆盖节点的类名称。此字段不会更改节点的功能，只会更改__EditContext__中使用的类名。        | `PreferredClassName="A Different Name"`|
| __Uuid__ | Optional | 允许您提供特定的通用标识符，这通常是生成的，但在某些情况下，可能需要提供特定的 UUID。                                              | `Uuid="{EE36A690-7C33-445F-B9E8-BD045D6ACC1D}"` |
| __Icon__ | Unused | 当前未使用，将提供一个路径，用于显示与节点关联的自定义图标                                                              | `Icon="Icons/ScriptCanvas/Checkpoint.png"` |
| __Version__ | Optional | 与 O3DE 中的任何序列化类一样，节点可以提供版本，这创建了一种机制来处理可能使数据无效的代码更改                                         | `Version="2"` |
| __VersionConverter__ | Optional | 如果节点的地形发生变化，可以使用版本转换器来提供迁移机制                                                               | `VersionConverter="ScriptCanvas::ForEachVersionConverter"` |
| __EventHandler__ | Optional | 由序列化系统使用，它有时可用于捕获序列化事件                                                                     | `EventHandler="SerializeContextOnWriteEndHandler&lt;EBusEventHandler&gt;"` |
| __Deprecated__ | Optional | 将此节点标记为已弃用，已弃用的节点将不会显示在节点调色板中，但仍可能显示在现有 Script Canvas 图形中                                  | `Deprecated="This node has been deprecated"`|
| __DeprecationUUID__ | Optional | 指定后，此 UUID 用于将此已弃用的节点替换为提供的 UUID 的节点，这允许自动更新图形                                             | `DeprecationUUID="{D3629902-02E9-AE59-0424-F366D342B433}"` |
| __Base__ | Optional | 在某些情况下，可能需要指定基类，例如，当节点派生自其他节点时                                                             | `Base="ScriptCanvas::Nodes::Internal::BaseTimerNode"` |
| __GraphEntryPoint__ | Optional | 启用后，Script Canvas 可以使用此节点作为开始运行的入口点。                                                       | `GraphEntryPoint="True"` |
| __EditAttributes__ | Optional | 在节点的序列化中提供 __EditContext__ 属性。使用 `@`  字符分隔键/值。用分号(`;`)分隔多个属性。                              | `EditAttributes="AZ::Script::Attributes::ExcludeFrom@AZ::Script::Attributes::ExcludeFlags::All;` `ScriptCanvas::Attributes::Node::TitlePaletteOverride@StringNodeTitlePalette"` |
| __DynamicSlotOrdering__ | Optional | 当插槽的顺序在编辑时可能更改时，请启用此选项。默认情况下，此选项处于禁用状态。| `DynamicSlotOrdering="True"` |
----

注意：指定 C++ 代码时，必须使用 HTML 实体代码。例如，在 __EventHandler__ 的情况下，<EBusEventHandler>指定`SerializeContextOnWriteEndHandler&lt;EBusEventHandler&gt;`， 而不是C++代码`SerializeContextOnWriteEndHandler<EBusEventHandler>`。


### 示例

```xml
<ScriptCanvas Include="Include/ScriptCanvas/Libraries/Core/Start.h" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <Class Name="Start"
        QualifiedName="ScriptCanvas::Nodes::Core::Start"
        PreferredClassName="On Graph Start"
        Uuid="{F200B22A-5903-483A-BF63-5241BC03632B}"
        Category="Timing"
        Version="2"
        GraphEntryPoint="True"
        Description="Starts executing the graph when the entity that owns the graph is fully activated.">
    </Class>
</ScriptCanvas>
```


## 执行槽元素

### Input

__Input__ 标签用于在节点上配置执行入口槽。从视觉上看，这些参数将位于 Script Canvas 编辑器中节点的左侧。

### 支持的属性

| __属性__   |  __必须__  | __说明__   | __示例__ |
| ---------- | ------------| ------------|-------------------------------|
| __Name__ | Required | 此输入槽的名称 | `Name="My Node"`              |
| __Description__ | Optional | 描述此输入槽，它在编辑器中显示为工具提示 | `Description="Useful node"`   |
| __DisplayGroup__ | Optional | 允许对插槽进行可视化分组 | `DisplayGroup="Tests"`        |
| __Output__ | Optional | 允许提供相应的输出执行槽 | `Output="Done"`               |
| __Latent__| Optional |这仅在提供 __Output__ 属性时适用，它表示 __Output__ 槽可能不会立即执行，并且可能需要多个帧 | `Output="Done" Latent="true"` |
----

### Output

__Output__ 标签用于在节点上配置执行出口槽。从视觉上看，这些参数将位于 Script Canvas 编辑器中节点的右侧。

### 支持的属性

| __属性__   |  __必须__  | __说明__   | __示例__ |
| ---------- | ------------| ------------| ------------| 
| __Name__ | Required | 此输入槽的名称 | `Name="My Node"` |
| __Description__ | Optional | 描述此输入槽，它在编辑器中显示为工具提示| `Description="Useful node"`|
| __DisplayGroup__ | Optional | 允许对插槽进行可视化分组 | `DisplayGroup="Tests"` |
| __Latent__| Optional | 仅当提供了 __Input__ 标记的 __Output__ 属性时，这才适用。它表示 __Output__ 槽可能不会立即执行，并且可能需要多个帧。 | `Output="Done" Latent="true"` |
----

### 示例

```xml
<Input Name="Start" Output="Started" Description="Use this to start this operation"/>
<Input Name="Stop " Output="Stopped" Latent="true" Description="Use this to stop this operation"/>

<Output Name="Out" Description="Called immediately after this node is invoked"/>
<Output Name="OperationComplete" Latent="true" Description="Called when this node's operation has finished"/>
```

## 数据槽元素

### 参数

可以在 __Input__ 标记中指定 __Parameter__ 标记，它用于定义节点的数据入口点。

### 支持的属性

| __属性__   |  __必须__  | __说明__   | __示例__ |
| ---------- | ------------| ------------| ------------| 
| __Name__ | Required | 此数据输入槽的名称 | `Name="Operand A"` |
| __Type__ | Required | 这是属性的 C++ 类型。它必须是向 O3DE 序列化上下文公开的类型。如果类型包含特殊字符，则必须使用 HTML 实体代码 (例如，更新`AZStd::vector<int>` 为 `AZStd::vector&lt;int&gt;`). | `Type="float"` |
| __Description__ | Optional | 描述此数据输入槽，将鼠标悬停在槽上时显示为工具提示 | `Description="Result of this mathematical operation"`|

### Examples

```xml
<Input Name="In">
    <Parameter Name="Value 1" Type="float" Description="First Value in the equation"/>
    <Parameter Name="Value 2" Type="float" Description="Second Value in the equation"/>
</Input>
```


### Property

| __属性__   |  __必须__  | __说明__   | __示例__ |
| ---------- | ------------| ------------| ------------| 
| __Name__ | Required | 属性的名称。这是将显示在节点上的槽中的名称，也是 C++ 中属性的名称。可以使用 [PropertyData](#propertydata) 覆盖该名称。 | `Name="m_timeUnit"` |
| __Type__ | Required | 这是属性的 C++ 类型。它必须是向 O3DE 序列化上下文公开的类型。如果类型包含特殊字符，则必须使用 HTML 实体代码 (例如，更新 `AZStd::vector<int>` 为 `AZStd::vector&lt;int&gt;`). | `Type="float"` |
| __DefaultValue__ | Optional | 允许您为属性提供默认值 | `DefaultValue="1000.0"`|
| __UIHandler__ | Optional | 允许用户在 Node Inspector 中覆盖属性的默认 UI 处理程序 | `UIHandler="AZ::Edit::UIHandlers::ComboBox"`|
| __DisplayGroup__ | Optional | 允许对插槽进行可视化分组| `DisplayGroup="Tests"` |
| __IsInput__ | [Grammar Only](#grammar) | 用于定义是否将此属性作为输入数据槽放置在节点上。此选项不适用于 Nodeables | `IsInput="True"` 或 `IsInput="False"` |
| __IsOutput__ | [Grammar Only](#grammar) | 用于定义是否将此属性作为输出数据槽放置在节点上。此选项仅适用于 Nodeables | `IsOutput="True"` 或 `IsOutput="False"` |


### PropertyData

可以为 __Property__ 的元素提供__PropertyData__，它提供了一种机制来覆盖类型反射的某些 __EditContext__ 参数。

| __属性__   |  __必须__  | __说明__   | __示例__ |
| ---------- | ------------| ------------| ------------| 
| __Name__ | Required | 属性的名称，这是节点上插槽的名称 | `Name="Time Unit"` |

### EditAttribute

可以在 __PropertyData__ 中提供 __EditAttribute__ 元素，它允许为属性提供 O3DE 序列化属性。

| __属性__   |  __必须__  | __说明__   | __示例__ |
| ---------- | ------------| ------------| ------------| 
| __Key__ | Required | 需要设置的属性名称 | `Key="AZ::Edit::Attributes::GenericValueList"` |
| __Value__ | Required | 要在属性上设置的值 | `Value="&amp;BaseTimer::GetTimeUnitList"` |

### 示例

```xml
<Property Name="m_timeUnits" Type="int" DefaultValue="0" Serialize="true">
    <PropertyData Name="Units" Description="Units to represent the time in." UIHandler="AZ::Edit::UIHandlers::ComboBox">
        <EditAttribute Key="AZ::Edit::Attributes::GenericValueList" Value="&amp;BaseTimer::GetTimeUnitList"/>
        <EditAttribute Key="AZ::Edit::Attributes::PostChangeNotify" Value="&amp;BaseTimer::OnTimeUnitsChanged"/>
    </PropertyData>
</Property>
```

----


----

### Parameter

__Parameter__ 由 [Input](#input) 和 [Output](#output)  标签使用。它们表示节点上的数据槽。

| __属性__   |  __必须__  | __说明__   | __示例__ |
| ---------- | ------------| ------------| ------------| 
| __Name__ | | | | 
| __Type__ | Required | 这是属性的 C++ 类型。它必须是向 O3DE 序列化上下文公开的类型。如果类型包含特殊字符，则必须使用 HTML 实体代码 (例如，更新`AZStd::vector<int>` 为 `AZStd::vector&lt;int&gt;`). | `Type="float"` |
| __Description__ | Optional | 将鼠标悬停在插槽上时显示为工具提示 | `Description="The radius of the circle"`|
| __DefaultValue__ | Optional | 允许您为属性提供默认值 | `DefaultValue="1000.0"`|
| __DisplayGroup__ | Optional | 允许对插槽进行可视化分组 | `DisplayGroup="Tests"` |
----

### Return

__Return__ 由[Input](#input) 标签使用，表示节点上的输出数据槽。您可以将其视为函数中的返回值，例如以下 C++ 函数`int Foo() { return 42; }`返回的值。

| __属性__   |  __必须__  | __说明__   | __示例__ |
| ---------- | ------------| ------------| ------------| 
| __Name__ | Required | 该名称将用作数据输出槽的名称 | `Name="Return Value"` | 
| __Type__ | Required | 这是属性的 C++ 类型。它必须是向 O3DE 序列化上下文公开的类型。如果类型包含特殊字符，则必须使用 HTML 实体代码 (例如，更新`AZStd::vector<int>` 为 `AZStd::vector&lt;int&gt;`). | `Type="float"` |
| __Description__ | Optional | 将鼠标悬停在插槽上时显示为工具提示| `Description="The radius of the circle"`|
| __DefaultValue__ | Optional | 允许您为属性提供默认值 | `DefaultValue="1000.0"`|
| __DisplayGroup__ | Optional | 允许对插槽进行可视化分组 | `DisplayGroup="Tests"` |
----

### Examples

```xml
<Input Name="Fibonacci" OutputName="Done">
    <Parameter Name="X" Type="int" Description="The value to calculate Fibonacci for"/>
    <Return Name="Result" Type="int"/>
</Input>
```

### Branch

| __属性__   |  __必须__  | __说明__   | __示例__ |
| ---------- | ------------| ------------|--------| 
| __Name__ | Required | 指定分支的名称，在 C++ 中，这将创建一个名称为 `Call<BranchName>` 的函数，其中 `<BranchName>` 是指定的名称。 |        |
----

### 示例

以下示例创建一个节点，该节点将返回`Success`和`Entity Spawn Ticket`或返回 `Failed`
```xml
<Input Name="Create Ticket" OutputName="Ticket Created">
    <Parameter Name="Prefab" Type="const AzFramework::Scripts::SpawnableScriptAssetRef&amp;" Description="Prefab source asset to spawn"/>
	<Branch Name="Success">
        <Return Name="Entity Spawn Ticket" Type="AzFramework::EntitySpawnTicket"/>
    </Branch>
    <Branch Name="Failed"/>
</Input>
```
上面的节点定义将为每个 C++ 分支生成相应的调用，其方式与为输出生成 C++ 函数的方式相同。在此示例中，它将生成 `CallSuccess` 和 `CallFailed`，您可以从节点的 C++ 实现中调用这些函数。

```cpp
void CreateSpawnTicketNodeable::CreateTicket(const AzFramework::Scripts::SpawnableScriptAssetRef& prefab)
{
    auto ticket = m_spawnableScriptMediator.CreateSpawnTicket(prefab);
    if (ticket.IsSuccess())
    {
        CallSuccess(ticket.GetValue());
    }
    else
    {
        AZLOG_ERROR("Unable to Create Spawn Ticket - A valid prefab was not provided");
        CallFailed();
    }
}
```
----

## 自由功能元素

[Script Canvas 自由功能节点](./custom-free-function-nodes) 将 C++ 函数直接公开到行为上下文中。这提供了一种快速简便的方法来创建可在 Script Canvas 中使用的函数库。

定义 Free Function 节点时，请使用以下元素：

### Function

| __属性__   |  __必须__  | __说明__   | __示例__ |
| ---------- | ------------| ------------| ------------| 
| __Name__ | Required | 函数的名称 | `Name="RandomColor"` |
| __Branch__ | Optional | 指定使用该函数计算分支条件 | `Branch="IsValidFindPosition"` |
| __BranchWithValue__ | Optional |  | `BranchWithValue="True"` |

注意： 函数可以使用 [Parameter](#parameter) 元素来指定数据输入。

```xml
<Function Name="RandomColor">
    <Parameter Name="MinValue" DefaultValue="Data::ColorType(0.0f, 0.0f, 0.0f, 1.0f)"/>
    <Parameter Name="MaxValue" DefaultValue="Data::ColorType(1.0f, 1.0f, 1.0f, 1.0f)"/>
</Function>

<Function Name="IsValidFindPosition"> <!-- Used by ContainsString to evaluate the condition to branch -->
    <Parameter Name="Position" Description="The string find position to check against"/>
</Function>

<Function Name="ContainsString" Branch="IsValidFindPosition" BranchWithValue="True">
    <Parameter Name="Source" Description="The string to search in."/>
    <Parameter Name="Pattern" Description="The substring to search for."/>
    <Parameter Name="Search From End" Description="Start the match checking from the end of a string."/>
    <Parameter Name="Case Sensitive" Description="Take into account the case of the string when searching."/>
</Function>

```

### Out

函数可以使用 __Out__ 元素指定每个 [Branch](#branch) 结果的名称。

__NOTE：__ 分支函数可能只有 2 个 __Out__ 元素，如果存在 2 个以上，编译时会显示错误。

| __属性__   |  __必须__  | __说明__   | __示例__ |
| ---------- | ------------| ------------| ------------| 
| __Name__ | Required | 函数的名称 | `<Function Name="Clamp">` |

```xml
<Function Name="BranchExample" Branch="IsPositive" BranchWithValue="True">
    <Out Name="Is Positive"/>
    <Out Name="Is Negative"/>
</Function>
```

### Library

使用 `Library` 元素创建函数集合。

| __属性__   |  __必须__  | __说明__   | __示例__ |
| ---------- | ------------| ------------| ------------| 
| __Include__ | Required | 将包含库函数声明的 C++ 包含文件的相对路径 | `Include="Include/ScriptCanvas/Libraries/Math/Color.h"` |
| __Namespace__ | Required | 在此库中保存 C++ 函数的命名空间 | `Namespace="ScriptCanvas::ColorFunctions"` |
| __Category__ | Recommended | Category 定义此节点在 Node Palette 中的显示位置。您可以通过使用 '/' 字符分隔来嵌套类别。 | `Category="Math/Color"` |


```xml
<Library Include="Include/ScriptCanvas/Libraries/Math/MathFunctions.h"
        Namespace="ScriptCanvas::MathRandoms"
        Category="Math/Random">

    <!-- Function Definitions -->

</Library>
```
----

## Grammar

语法节点是直接转换为后端执行格式（即 Lua）的节点。如果 Script Canvas Nodeable 格式足够，则无需创建新的语法节点。但是，在某些情况下，可能需要扩展 Script Canvas 语法。

语法节点和节点定义之间有许多共享元素和属性。本节将介绍差异。

### In

指定执行入口槽

| __属性__   |  __必须__  | __说明__   | __示例__ |
| ---------- | ------------| ------------| ------------| 
| __Name__ | Required | 执行槽的名称 | `<In Name="Start">` |
| __Description__ | Optional | 将鼠标悬停在插槽上时显示为工具提示 | `Description="The radius of the circle"`|

### Out

指定执行退出槽

| __属性__   |  __必须__  | __说明__   | __示例__ |
| ---------- | ------------| ------------| ------------| 
| __Name__ | Required | 执行槽的名称 | `<Out Name="Done">` |
| __Description__ | Optional | 将鼠标悬停在插槽上时显示为工具提示 | `Description="The radius of the circle"`|

### OutLatent

指定潜在执行退出槽，则 latent slot 可以从调用节点的那一刻起执行多个帧或 tick

| __属性__   |  __必须__  | __说明__   | __示例__ |
| ---------- | ------------| ------------| ------------| 
| __Name__ | Required | 执行槽的名称 | `<OutLatent Name="TimerDone">` |
| __Description__ | Optional | 将鼠标悬停在插槽上时显示为工具提示 | `Description="The specified time interval is complete"`|

### SerializedProperty

需要序列化的 C++ 成员变量

| __属性__   |  __必须__  | __说明__   | __示例__ |
| ---------- | ------------| ------------| ------------| 
| __Name__ | Required | 执行槽的名称 | `<SerializedProperty Name="m_numericPrecision"/>` |


### EditProperty

允许向现有__Property__或__SerializedProperty__提供 __EditContext__ 属性

| __属性__   |  __必须__  | __说明__   | __示例__ |
| ---------- | ------------| ------------| ------------| 
| __Name__ | Required | 用于替换字段名称的名称，仅限装饰 | `<SerializedProperty Name="m_numericPrecision"/>` |
| __FieldName__ | Required | __Property__ 或 __SerializedProperty__ 的名称 | `FieldName="m_numericPrecision"` |
| __Description__ | Optional | 将鼠标悬停在插槽上时显示为工具提示 | `Description="The radius of the circle"`|
| __UIHandler__ | Optional | 允许用户在 Node Inspector 中覆盖属性的默认 UI 处理程序 | `UIHandler="AZ::Edit::UIHandlers::ComboBox"`|
| __DisplayGroup__ | Optional | 允许对插槽进行可视化分组 | `DisplayGroup="Tests"` |

```xml
<EditProperty UiHandler="AZ::Edit::UIHandlers::Default" FieldName="m_numericPrecision" Name="Precision" Description="The precision with which to print any numeric values.">
    <EditAttribute Key="AZ::Edit::Attributes::Min" Value="0"/>
    <EditAttribute Key="AZ::Edit::Attributes::Max" Value="24"/>
</EditProperty>
```

### EditAttribute

__EditProperty__也可以使用 [EditAttribute](#editattribute) 数据。

### DynamicDataSlot

动态数据槽 是没有预定义类型的槽;类型在建立连接或创建变量引用时由 slot 获取。槽的类型可以是值、容器或支持 'Any' 类型的通用槽。

| __属性__   |  __必须__  | __说明__                                                                          | __示例__ |
| ---------- | ------------|---------------------------------------------------------------------------------| ------------| 
| __Name__ | Required | 数据槽的名称                                                                          | `Name="Format"` |
| __Description__ | Optional | 将鼠标悬停在插槽上时显示为工具提示                                                               | `Description="The radius of the circle"`|
| __ConnectionType__ | Required | `ScriptCanvas::ConnectionType::Input` 或 `ScriptCanvas::ConnectionType::Output` | `ConnectionType="ScriptCanvas::ConnectionType::Input"` |
| __DynamicType__ | Required | `Value`, `Container`, 或 `Any`                                                   | `DynamicType="ScriptCanvas::DynamicDataType::Container"` | 


```xml
<DynamicDataSlot Name="Source"
                 Description="The container to get the size of."
                 ConnectionType="ScriptCanvas::ConnectionType::Input"
                 DynamicType="ScriptCanvas::DynamicDataType::Container" />
```                         
