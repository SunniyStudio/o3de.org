---
linktitle: 自定义可节点
title: 在 Script Canvas 中创建自定义nodeable节点
description: 了解如何在 Open 3D Engine （O3DE） 中创建自定义 Script Canvas nodeable节点。
weight: 100
---

本主题将指导您逐步完成如何创建自定义 Script Canvas nodeable节点。

您将看到整个 O3DE 源代码中使用的术语 _nodeable_。
nodeable 可以引用由于 AzAutoGen 处理而出现在 Node Palette 中的节点，
以及编译后的 Script Canvas 图形可以调用 C++ 函数的机制。

## 第 1 步：向 Gem 添加对自定义可节点节点的支持
{{< note >}}
首次创建自定义可nodeable节点时，只需执行一次此步骤。
{{< /note >}}

在 Gem 的`Code/CMakeLists.txt`中，为`AUTOGEN_RULES`添加一个部分，并将`Gem::ScriptCanvas`声明为构建依赖项。

此部分的确切位置将因 Gem 的配置方式而异。
但是，我们建议您的 Gem 定义一个`STATIC`库，以使代码可用于运行时和编辑器项目。

例如，以下是 StartingPointInput Gem 的“`Code/CMakeLists.txt`的部分定义，该定义支持 Script Canvas 自定义节点，并进行了以下所需更改：
1. `Gem::ScriptCanvas`必须在`STATIC`库的`BUILD_DEPENDENCIES`中
1. 在`STATIC`库下为自定义免费函数添加 `AUTOGEN_RULES` 部分
   ```cmake
   AUTOGEN_RULES
       *.ScriptCanvasNodeable.xml,ScriptCanvasNodeable_Header.jinja,$path/$fileprefix.generated.h
       *.ScriptCanvasNodeable.xml,ScriptCanvasNodeable_Source.jinja,$path/$fileprefix.generated.cpp
       *.ScriptCanvasNodeable.xml,ScriptCanvasNodeableRegistry_Header.jinja,AutoGenNodeableRegistry.generated.h
       *.ScriptCanvasNodeable.xml,ScriptCanvasNodeableRegistry_Source.jinja,AutoGenNodeableRegistry.generated.cpp
   ```
1. `STATIC`库必须直接在 Gem 运行时模块的`BUILD_DEPENDENCIES`中声明（并且它应该作为编辑器模块构建依赖项层次结构的一部分包含在内）
1. `StartingPointInput.Static`包含两个 .cmake 文件列表。
   * 我们包括了在`startingpointinput_files.cmake`中设置的通用文件和平台特定文件。
   * 我们包括 AzAutoGen ScriptCanvas 免费功能所需的模板，这些模板在 `startingpointinput_autogen_files.cmake`中设置（我们建议将此文件单独保存以明确范围）

   `startingpointinput_autogen_files.cmake`的示例内容：
   ```cmake
   set(FILES
       ...
       ${LY_ROOT_FOLDER}/Gems/ScriptCanvas/Code/Include/ScriptCanvas/AutoGen/ScriptCanvasNodeable_Header.jinja
       ${LY_ROOT_FOLDER}/Gems/ScriptCanvas/Code/Include/ScriptCanvas/AutoGen/ScriptCanvasNodeable_Source.jinja
       ${LY_ROOT_FOLDER}/Gems/ScriptCanvas/Code/Include/ScriptCanvas/AutoGen/ScriptCanvasNodeableRegistry_Header.jinja
       ${LY_ROOT_FOLDER}/Gems/ScriptCanvas/Code/Include/ScriptCanvas/AutoGen/ScriptCanvasNodeableRegistry_Source.jinja
   )
   ```

   如果您出于自己的目的创建自定义模板，则 autogen 模板列表可能会有所不同。
   例如，如果您要扩展 Script Canvas 以执行超出其“开箱即用”功能的功能，则可以拥有自己的模板集，以您定义的语法生成代码。
   有关更多信息，请参阅[AzAutoGen](/docs/user-guide/programming/autogen/)上的文档。

```cmake
...
ly_add_target(
    NAME StartingPointInput.Static STATIC
    NAMESPACE Gem
    FILES_CMAKE
        startingpointinput_files.cmake
        startingpointinput_autogen_files.cmake                                                                # 4
    ...
    BUILD_DEPENDENCIES
        PUBLIC
            AZ::AzCore
            AZ::AzFramework
            CryCommon
            Gem::ScriptCanvas                                                                                 # 1
    AUTOGEN_RULES                                                                                             # 2
        ...
        *.ScriptCanvasNodeable.xml,ScriptCanvasNodeable_Header.jinja,$path/$fileprefix.generated.h
        *.ScriptCanvasNodeable.xml,ScriptCanvasNodeable_Source.jinja,$path/$fileprefix.generated.cpp
        *.ScriptCanvasNodeable.xml,ScriptCanvasNodeableRegistry_Header.jinja,AutoGenNodeableRegistry.generated.h
        *.ScriptCanvasNodeable.xml,ScriptCanvasNodeableRegistry_Source.jinja,AutoGenNodeableRegistry.generated.cpp
)

ly_add_target(
    NAME StartingPointInput ${PAL_TRAIT_MONOLITHIC_DRIVEN_MODULE_TYPE}
    NAMESPACE Gem
    FILES_CMAKE
        startingpointinput_shared_files.cmake
    ...
    BUILD_DEPENDENCIES
        PRIVATE
            AZ::AzFramework
            Gem::StartingPointInput.Static                                                                    # 3
)

...
```


## 第 2 步：创建用于代码生成的 XML 文件 {#create-an-xml-file}

{{< note >}}
创建 XML 文件时要遵循的确切架构可在此处找到：[ScriptCanvasNodeable.xsd](https://github.com/o3de/o3de/blob/development/Templates/ScriptCanvasNode/Template/Source/AutoGen/ScriptCanvasNodeable.xsd)
{{< /note >}}
通过创建一个 XML 文件来准备代码生成，该文件包含有关节点类、输入引脚、输出引脚和关联的工具提示文本的信息。AzAutoGen 使用此文件生成节点类在实现节点功能时使用的 C++ 代码。

我们将使用以下 XML，该 XML 是从 **Input Handler** 节点的 O3DE 源复制的<!-- , 作为解释此文件重要部分的示例-->.

文件: [InputHandlerNodeable.ScriptCanvasNodeable.xml](https://github.com/o3de/o3de/blob/development/Gems/StartingPointInput/Code/Source/InputHandlerNodeable.ScriptCanvasNodeable.xml)

```xml
<ScriptCanvas Include="Source/InputHandlerNodeable.h" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <Class Name="InputHandlerNodeable"                                                                                  # 1
        Namespace="StartingPointInput"                                                                                  # 2
        QualifiedName="StartingPointInput::InputHandlerNodeable"                                                        # 3
        PreferredClassName="Input Handler"                                                                              # 4
        Category="Input"                                                                                                # 5
        Description="Handle processed input events found in input binding assets">

        <Output Name="Pressed" Description="Signaled when the input event begins." >                                    # 6
            <Parameter Name="value" Type="float"/>
        </Output>
        <Output Name="Held" Description="Signaled while the input event is active." >                                   # 6
            <Parameter Name="value" Type="float"/>
        </Output>
        <Output Name="Released" Description="Signaled when the input event ends." >                                     # 6
            <Parameter Name="value" Type="float"/>
        </Output>
        <Input Name="Connect Event" Description="Connect to input event name as defined in an input binding asset.">    # 7
            <Parameter Name="Event Name" Type="AZStd::string" Description="Event name as defined in an input binding asset. Example 'Fireball'."/>
        </Input>
    </Class>
</ScriptCanvas>
```

## 步骤 3：创建节点类文件 {#create-the-node-class-files}

下一步是实现将由 Script Canvas 节点调用的 C++ 函数。这些源文件引用节点自动生成的源，并使用 `ScriptCanvas::Nodeable` 类作为基类。

每个 Script Canvas 可节点头文件都需要三个关键部分：

1. 它必须派生自`ScriptCanvas::Nodeable`。
1. 它必须包含节点定义宏 `SCRIPTCANVAS_NODE`。
1. 它必须包含生成的标头。

**Input Handler** 节点的 `InputHandlerNodeable` 头文件中的以下代码片段演示了这些要求：

文件: [InputHandlerNodeable.h](https://github.com/o3de/o3de/blob/development/Gems/StartingPointInput/Code/Source/InputHandlerNodeable.h)

```cpp
#include <ScriptCanvas/Core/Nodeable.h>
#include <ScriptCanvas/Core/NodeableNode.h>
#include <ScriptCanvas/CodeGen/NodeableCodegen.h>
#include <StartingPointInput/InputEventNotificationBus.h>

#include <Source/InputHandlerNodeable.generated.h>                                                   // (3)

namespace StartingPointInput
{
    //////////////////////////////////////////////////////////////////////////
    /// Input handles raw input from any source and outputs Pressed, Held, and Released input events
    class InputHandlerNodeable
        : public ScriptCanvas::Nodeable                                                              // (1)
        , protected InputEventNotificationBus::Handler
    {
        SCRIPTCANVAS_NODE(InputHandlerNodeable)                                                      // (2)
        ...
    };
}
```

## 第 4 步：将源文件添加到 CMake {#add-source-files-to-cmake}

将 XML 和类源文件添加到 Gem 的 .cmake 文件之一。

例如，在 `InputHandlerNodeable` 中，我们必须添加以下行：

文件: [startingpointinput_files.cmake](https://github.com/o3de/o3de/blob/development/Gems/StartingPointInput/Code/startingpointinput_files.cmake)
```cmake
set(FILES
    ...
    Source/InputHandlerNodeable.h
    Source/InputHandlerNodeable.cpp
    Source/InputHandlerNodeable.ScriptCanvasNodeable.xml
    ...
)
```

## 第 5 步：注册新节点 {#register-the-new-node}
{{< note >}}
首次创建可节点节点时，只需执行一次此步骤。
{{< /note >}}

最后一步是注册新节点。为此，您需要修改 Gem 的 [Gem 模块](/docs/user-guide/programming/gems/overview/) 或 [系统组件](/docs/user-guide/programming/components/system-components/)。使用 O3DE 源中的 **StartingPointInput** Gem 作为参考：

在 Gem 的模块或系统组件中，包含自动生成的注册表头文件，并使用经过净化的 Gem 目标名称调用`REGISTER_SCRIPTCANVAS_AUTOGEN_NODEABLE`。
{{< note >}}
使用您在步骤 1 中 Gem 的`Code/CMakeLists.txt`中的`AUTOGEN_RULES` 下声明的相同自动生成的注册表头文件。在 **StartingPointInput** 示例中，它是`AutoGenNodeableRegistry.generated.h`。
{{< /note >}}
{{< note >}}
经过净化的 Gem 目标名称应仅包含字母和数字。在 **StartingPointInput** 示例中，它是 `StartingPointInputStatic`，它指的是`StartingPointInput.Static`目标。
{{< /note >}}

例如，在 [`StartingPointInputGem.cpp`](https://github.com/o3de/o3de/blob/development/Gems/StartingPointInput/Code/Source/StartingPointInputGem.cpp) 中：

```cpp
#include <AutoGenNodeableRegistry.generated.h>
...

REGISTER_SCRIPTCANVAS_AUTOGEN_NODEABLE(StartingPointInputStatic);
...
```

## 高级使用 ScriptCanvasNodeable.xml
本主题探讨了我们在可结块 XML 文件中支持的其他功能。

### 预设
预设是一种跨常见类型的可节点对象的所有 XML 标记预配置默认属性的方法。当前实施的预设如下所示。

#### 紧凑
您可以将 Compact 预设用于不使用执行槽的较小 Compact 样式节点。

例如，以下是 **+=** 节点的 XML 文件：

文件： [CompactAddNodeable.ScriptCanvasNodeable.xml](https://github.com/o3de/o3de/blob/development/Gems/ScriptCanvas/Code/Include/ScriptCanvas/Libraries/Compact/BasicOperators/CompactAddNodeable.ScriptCanvasNodeable.xml)
```xml
<?xml version="1.0" encoding="utf-8"?>

<ScriptCanvas Include="Include/ScriptCanvas/Libraries/Compact/BasicOperators/CompactAddNodeable.h" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			  xsi:noNamespaceSchemaLocation="../../../AutoGen/ScriptCanvasNodeable.xsd">
    <Class Name="CompactAddNodeable"
        QualifiedName="Nodeables::CompactAddNodeable"
        PreferredClassName="+="
        Category="Compact/Basic Operators"
        Namespace="ScriptCanvas"
        Description="Adds the first input number to the second input number"
        Preset="compact">

        <Input Name="In" OutputName="Out">
            <Parameter Name="a" Type="float"/>
            <Parameter Name="b" Type="float"/>
            <Return Name="Out" Type="float"/>
        </Input>

    </Class>
</ScriptCanvas>
```

### 基础节点和派生的可节点节点
如果您在多个可节点之间共享逻辑，则可以创建一个基本节点和多个派生节点。

以下示例使用 **Time Delay** 节点的 O3DE 源：

文件： [TimeDelayNodeable.ScriptCanvasNodeable.xml](https://github.com/o3de/o3de/blob/development/Gems/ScriptCanvas/Code/Include/ScriptCanvas/Libraries/Time/TimeDelayNodeable.ScriptCanvasNodeable.xml)
```xml
<ScriptCanvas Include="Include/ScriptCanvas/Libraries/Time/TimeDelayNodeable.h" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <Class Name="TimeDelayNodeable"
        QualifiedName="Nodeables::Time::TimeDelayNodeable"
        PreferredClassName="Time Delay"
        Base="Nodeables::Time::BaseTimer"    # Declare base class as Nodeables::Time::BaseTimer
        Category="Timing"
        Namespace="ScriptCanvas"
        Description="Delays all incoming execution for the specified number of ticks">

        <Input Name="Start" Description="">
            <Parameter Name="Delay" Type="Data::NumberType" DefaultValue="0.0" Description="The amount of time to delay before the Done is signalled."/>
        </Input>

        <Output Name="Done" Description="Signaled after waiting for the specified amount of times."/>

        <PropertyInterface Property="m_timeUnitsInterface" Name="Units" Type="Input" Description="Units to represent the time in."/>
    </Class>
</ScriptCanvas>
```

`TimeDelayNodeable`类实现了一个名为 `BaseTimer` 的基类。在下面的基类 XML 中，您可以看到基类定义了共享属性 “Units” 和 “TickOrder”：
文件：[BaseTimer.ScriptCanvasNodeable.xml](https://github.com/o3de/o3de/blob/development/Gems/ScriptCanvas/Code/Include/ScriptCanvas/Internal/Nodeables/BaseTimer.ScriptCanvasNodeable.xml)
```xml
<?xml version="1.0" encoding="utf-8"?>

<ScriptCanvas Include="Include/ScriptCanvas/Internal/Nodeables/BaseTimer.h" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <Class Name="BaseTimer"
        QualifiedName="ScriptCanvas::Nodeables::Time::BaseTimer"
        PreferredClassName="BaseTimer"
        Uuid="{64814C82-DAE5-9B04-B375-5E47D51ECD26}"
        BaseClass="True"    # Declare this nodeable as a base class, so it won't be reflected as a node
        Category="Timing"
        Description="Provides a basic interaciton layer for all time based nodes for users(handles swapping between ticks and seconds).">

        <Property Name="m_timeUnits" Type="int" DefaultValue="0" Serialize="true">
            <PropertyData Name="Units"
                Description="Units to represent the time in."
                Serialize="true"
                UIHandler="AZ::Edit::UIHandlers::ComboBox">
                <EditAttribute Key="AZ::Edit::Attributes::GenericValueList" Value="&amp;BaseTimer::GetTimeUnitList"/>
                <EditAttribute Key="AZ::Edit::Attributes::PostChangeNotify" Value="&amp;BaseTimer::OnTimeUnitsChanged"/>
            </PropertyData>
        </Property>

        <Property Name="m_tickOrder" Type="int" DefaultValue="static_cast&lt;int&gt;(AZ::TICK_DEFAULT)" Serialize="true">
            <PropertyData Name="TickOrder" Description="When the tick for this time update should be handled."/>
        </Property>
    </Class>
</ScriptCanvas>
```

{{< note >}}
更多节点名称、工具提示和分类自定义请参考 [文本替换](/docs/user-guide/scripting/script-canvas/editor-reference/text-replacement/)。
{{< /note >}}
