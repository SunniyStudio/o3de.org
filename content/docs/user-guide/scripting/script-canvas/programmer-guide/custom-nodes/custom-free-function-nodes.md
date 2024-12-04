---
linktitle: 自定义自由函数节点
title: 在 Script Canvas 中创建自定义自由函数节点
description: 了解如何在 Open 3D Engine （O3DE） 中创建自定义 Script Canvas 自由函数节点。
weight: 200
---

## 指示
本主题将指导您如何逐步创建自定义 Script Canvas Free Function 节点。

{{< note >}}
[ScriptCanvasPhysics](https://github.com/o3de/o3de/tree/development/Gems/ScriptCanvasPhysics) Gem演示了在 ScriptCanvas 中创建自定义免费函数节点的成品 Gem。
在按照本教程进行操作时，您可以引用 ScriptCanvasPhysics Gem。
{{< /note >}}

### 第 1 步：向 Gem 添加对自定义免费函数节点的支持
{{< note >}}
首次创建自定义免费函数节点时，只需执行一次此步骤。
{{< /note >}}

在 Gem 的`Code/CMakeLists.txt`中，为`AUTOGEN_RULES`添加一个部分，并将`Gem::ScriptCanvas.Extensions`声明为构建依赖项。

此部分的确切位置将因 Gem 的配置方式而异。
但是，我们建议您的 Gem 定义一个`STATIC`库，以使代码可用于运行时和编辑器项目。

例如，以下是 Gem 的`Code/CMakeLists.txt`的部分定义，它支持 Script Canvas 自定义节点，并进行了以下必需的更改：
1. `Gem::ScriptCanvas.Extensions`必须声明为`STATIC`库的`BUILD_DEPENDENCIES`
1. 在`STATIC`库下为自定义免费函数添加`AUTOGEN_RULES`部分
   ```cmake
   AUTOGEN_RULES
       *.ScriptCanvasFunction.xml,ScriptCanvasFunctionRegistry_Header.jinja,AutoGenFunctionRegistry.generated.h
       *.ScriptCanvasFunction.xml,ScriptCanvasFunctionRegistry_Source.jinja,AutoGenFunctionRegistry.generated.cpp
   ```
1. `STATIC`库必须直接声明为 Gem 运行时模块的`BUILD_DEPENDENCIES`（并且它应该作为编辑器模块构建依赖项层次结构的一部分包含在内）
1. `MyGem.Static` 包括两个 .cmake 文件列表。
   * 我们包括了在 `mygem_files.cmake` 中设置的通用文件和平台特定文件。
   * 我们包括 AzAutoGen ScriptCanvas 免费功能所需的模板，这些模板在 `mygem_autogen_files.cmake` 中设置（我们建议将此文件单独保留以明确范围）

   `mygem_autogen_files.cmake`的示例内容：
   ```cmake
   set(FILES
       ${LY_ROOT_FOLDER}/Gems/ScriptCanvas/Code/Include/ScriptCanvas/AutoGen/ScriptCanvas_Macros.jinja
       ${LY_ROOT_FOLDER}/Gems/ScriptCanvas/Code/Include/ScriptCanvas/AutoGen/ScriptCanvasFunctionRegistry_Header.jinja
       ${LY_ROOT_FOLDER}/Gems/ScriptCanvas/Code/Include/ScriptCanvas/AutoGen/ScriptCanvasFunctionRegistry_Source.jinja
   )
   ```

   如果您出于自己的目的创建自定义模板，则 autogen 模板列表可能会有所不同。
   例如，如果您要扩展 Script Canvas 以执行超出其“开箱即用”功能的功能，则可以拥有自己的模板集，以您定义的语法生成代码。
   有关更多信息，请参阅 [AzAutoGen](/docs/user-guide/programming/autogen/) 上的文档。

```cmake
...

ly_add_target(
    NAME MyGem.Static STATIC
    NAMESPACE Gem
    FILES_CMAKE
        mygem_files.cmake
        mygem_autogen_files.cmake                                                                             # 4
    INCLUDE_DIRECTORIES
        PRIVATE
            Source
        PUBLIC
            Include
    BUILD_DEPENDENCIES
        PUBLIC
            AZ::AzCore
            AZ::AzFramework
            Gem::ScriptCanvas.Extensions                                                                      # 1
    AUTOGEN_RULES                                                                                             # 2
        *.ScriptCanvasFunction.xml,ScriptCanvasFunctionRegistry_Header.jinja,AutoGenFunctionRegistry.generated.h
        *.ScriptCanvasFunction.xml,ScriptCanvasFunctionRegistry_Source.jinja,AutoGenFunctionRegistry.generated.cpp
)

ly_add_target(
    NAME MyGem ${PAL_TRAIT_MONOLITHIC_DRIVEN_MODULE_TYPE}
    NAMESPACE Gem
    FILES_CMAKE
        mygem_shared_files.cmake
    INCLUDE_DIRECTORIES
        PRIVATE
            Source
        PUBLIC
            Include
    BUILD_DEPENDENCIES
        PRIVATE
            AZ::AzCore
            Gem::MyGem.Static                                                                                 # 3
)

...
```


### 第 2 步：创建用于代码生成的 XML 文件 {#create-an-xml-file}
通过创建包含以下内容的信息的 XML 文件来准备代码生成：
1. **(必须)** 函数的头文件。
1. **(建议)** 函数的命名空间，用于区分重复的函数名称。
1. **(可选)** 函数类别（如果未显示）将改用 `Global Methods`
1. **(必须)** 每个函数的函数名称。

AzAutoGen 使用此文件生成用于函数注册和反射的 C++ 代码。

例如， HelloWorldFunctions.ScriptCanvasFunction.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ScriptCanvas>
    <Library
        Include="HelloWorldFunctions.h"                   # 1
        Namespace="MyGem::HelloWorldFunctions"            # 2
        Category="Hello World">                           # 3

        <Function Name="HelloWorld"/>                     # 4
      
        <Function Name="HelloWorldTo"/>                   # 4
    </Library>
</ScriptCanvas>
```

### 步骤 3：创建函数源文件 {#create-the-function-source-files}
下一步是实现将由 Script Canvas 节点调用的 C++ 函数。

需要牢记两个要求：
1. 函数的命名空间应与 XML 文件中提供的 `Namespace` 匹配。
2. 函数名称应与 XML 文件中提供的特定函数名称匹配。（不支持函数重载）

例如， HelloWorldFunctions.h
```cpp
#pragma once
#include <AzCore/std/string/string.h>

namespace MyGem
{
    namespace HelloWorldFunctions
    {
        AZStd::string HelloWorld();
        
        AZStd::string HelloWorldTo(AZStd::string name);
    }
}
```

例如，HelloWorldFunctions.cpp
```cpp
#include "HelloWorldFunctions.h"

namespace MyGem
{
    namespace HelloWorldFunctions
    {
        AZStd::string HelloWorld()
        {
            return "Hello World!";
        }
        
        AZStd::string HelloWorldTo(AZStd::string name)
        {
            return "Hello World to " + name;
        }
    }
}
```

### 第 4 步：将源文件添加到 CMake {#add-source-files-to-cmake}
将 XML 和函数源文件添加到 Gem 的其中一个 .cmake 文件中，例如`mygem_files.cmake`。

```cmake
set(FILES
    ...
    Include/HelloWorldFunctions.h
    Include/HelloWorldFunctions.cpp
    Include/HelloWorldFunctions.ScriptCanvasFunction.xml
    ...
)
```

### 第 5 步：注册新节点 {#register-the-new-node}
{{< note >}}
首次创建自定义免费函数节点时，只需执行一次此步骤。
{{< /note >}}

最后一步是注册新节点。为此，您需要修改 Gem 的 [Gem 模块](/docs/user-guide/programming/gems/overview/) 或 [系统组件](/docs/user-guide/programming/components/system-components/)。

在 Gem 的模块或系统组件中，包含自动生成的注册表头文件，并使用经过清理的 Gem 目标名称调用`REGISTER_SCRIPTCANVAS_AUTOGEN_FUNCTION` 。
{{< note >}}
使用您在步骤 1 中 Gem 的`Code/CMakeLists.txt`中的`AUTOGEN_RULES`下声明的相同自动生成的注册表头文件。在我们的示例中，它是`AutoGenFunctionRegistry.generated.h`。
{{< /note >}}
{{< note >}}
经过净化的 Gem 目标名称应仅包含字母和数字。在我们的示例中，它是`MyGemStatic`，它指的是 `MyGem.Static` 目标。
{{< /note >}}
   
例如，在 `MyGemSystemComponent.cpp` 中

```cpp
#include <AutoGenFunctionRegistry.generated.h>

REGISTER_SCRIPTCANVAS_AUTOGEN_FUNCTION(MyGemStatic)
...
```

## 高级使用 ScriptCanvasFunction.xml
本主题探讨了我们在函数 XML 文件中支持的其他功能。

### 基本用法
例如，我们有 `Sum` 函数，这是非常基本的用法
```cpp
namespace MyGem
{
    namespace HelloWorldFunctions
    {
      int Sum(int a, int b);
    }
}
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<ScriptCanvas>
    <Library
        Include="HelloWorldFunctions.h"
        Namespace="MyGem::HelloWorldFunctions"
        Category="Hello World">

        <Function Name="Sum"/>
    </Library>
</ScriptCanvas>
```

基本节点将如下所示

![Basic Sum](/images/user-guide/scripting/script-canvas/basic-sum.PNG)

### 详细用法
对于相同的`Sum`函数，我们可以在 XML 中提供更多详细信息
```xml
<Function Name="Sum">
    <Parameter Name="InputA" DefaultValue="1" Description="The input A of sum function."/>
    <Parameter Name="InputB" DefaultValue="1" Description="The input B of sum function."/>
</Function>
```

详细节点将如下所示

![Verbose Sum](/images/user-guide/scripting/script-canvas/verbose-sum.PNG)

### 分支布尔函数结果
通常，node 只有一个`Out`执行槽。但是我们可以根据函数 result 在 node 上创建分支。
例如，我们有 `IsPositive` 函数
```cpp
bool IsPositive(int input);
```

基本用法
* **(必须)** 当函数结果类型为布尔值时，`Branch` 属性应指示为`Boolean`。
* **(可选)** 如果要包含 result，则 `BranchWithValue`属性应指示为 `True`，默认值为  `False`
```xml
<Function Name="IsPositive" Branch="Boolean" BranchWithValue="True"/>
```

![Basic IsPositive](/images/user-guide/scripting/script-canvas/basic-ispositive.PNG)

详细用法
```xml
<Function Name="IsPositive" Branch="Boolean" BranchWithValue="True">
    <Parameter Name="Input" DefaultValue="1" Description="The input of positive check function."/>
    <Out Name="Input is Positive" Description="The out slot for true branch"/>
    <Out Name="Input is not Positive" Description="The out slot for false branch"/>
</Function>
```

![Verbose IsPositive](/images/user-guide/scripting/script-canvas/verbose-ispositive.PNG)

### 分支非布尔函数结果
即使函数 result 不是布尔值，但它必须与辅助函数耦合，我们也可以创建 branch ，
它应该将非布尔结果作为输入并返回布尔结果。

例如，我们可以同时使用 `Sum` 和 `IsPositive` 函数

基本用法
* **（必需）** `Branch` 属性应使用辅助函数名称表示，在我们的示例中为`IsPositive`。
* **（可选）** 如果要包含 result，则`BranchWithValue` 属性应指示为`True`，默认为  `False`
```xml
<Function Name="IsPositive"/>

<Function Name="Sum" Branch="IsPositive" BranchWithValue="True"/>
```

![Basic Sum IsPositive](/images/user-guide/scripting/script-canvas/basic-sum-ispositive.PNG)

详细用法
```xml
<Function Name="Sum" Branch="IsPositive" BranchWithValue="True">
    <Parameter Name="Input A" DefaultValue="1" Description="The input A of sum function."/>
    <Parameter Name="Input B" DefaultValue="1" Description="The input B of sum function."/>
    <Out Name="Sum Is Positive" Description="The out slot for sum result is positive branch"/>
    <Out Name="Sum Is not Positive" Description="The out slot for sum result is non-Positive branch"/>
</Function>
```

![Verbose Sum IsPositive](/images/user-guide/scripting/script-canvas/verbose-sum-ispositive.PNG)

{{< note >}}
更多节点名称、提示框和分类自定义请参考 [文本替换](/docs/user-guide/scripting/script-canvas/editor-reference/text-replacement/)
{{< /note >}}

## 通用函数节点的迁移说明
本主题将指导您逐步完成从通用函数节点迁移到自定义免费函数节点的过程。

### 步骤 1：定位泛型函数节点用法
泛型函数节点已弃用。在未来版本中将删除支持。为了做好准备，我们建议您尽早规划迁移。删除泛型函数节点后，现有图形将中断。

在您的 Gem 中，通过搜索以下三个宏来查找通用函数节点代码：
```cpp
SCRIPT_CANVAS_GENERIC_FUNCTION_NODE(NODE_NAME, CATEGORY, UUID, DESCRIPTION, ...)
SCRIPT_CANVAS_GENERIC_FUNCTION_NODE_WITH_DEFAULTS(NODE_NAME, DEFAULT_FUNC, CATEGORY, UUID, DESCRIPTION, ...)
SCRIPT_CANVAS_GENERIC_FUNCTION_MULTI_RESULTS_NODE(NODE_NAME, CATEGORY, UUID, DESCRIPTION, ...)
```

{{< note >}}
迁移过程假定节点签名和行为保持不变。如果替换节点签名和行为不同，则应手动升级图形。
{{< /note >}}

### 第 2 步：创建替换自定义免费函数节点
有关如何创建自定义免费函数节点的详细说明，请参阅前面的主题。

例如，以下是我们为 `Math/Matrix3x3` 节点完成的迁移的一部分：
1. 替换 `SCRIPT_CANVAS_GENERIC_FUNCTION_NODE`的示例：
   ```cpp
   AZ_INLINE Data::Vector3Type GetRow(const Data::Matrix3x3Type& source, Data::NumberType row) { ... }
   SCRIPT_CANVAS_GENERIC_FUNCTION_NODE(GetRow, k_categoryName, "{C4E00343-3642-4B09-8CFA-2D2F1CA6D595}", "returns vector from matrix corresponding to the Row index", "Source", "Row");
   ```
   替换函数和ScriptCanvasFunction.xml 内容：
   ```cpp
   Data::Vector3Type GetRow(const Data::Matrix3x3Type& source, Data::NumberType row) { ... }
   ```
   ```xml
   <Function Name="GetRow">
       <Parameter Name="Source"/>
       <Parameter Name="Row"/>
   </Function>
   ```

1. 替换`SCRIPT_CANVAS_GENERIC_FUNCTION_NODE_WITH_DEFAULTS`的示例：
   ```cpp
   AZ_INLINE Data::BooleanType IsClose(const Data::Matrix3x3Type& lhs, const Data::Matrix3x3Type& rhs, const Data::NumberType tolerance) { ... }
   SCRIPT_CANVAS_GENERIC_FUNCTION_NODE_WITH_DEFAULTS(IsClose, MathNodeUtilities::DefaultToleranceSIMD<2>, k_categoryName, "{020C2517-F02F-4D7E-9FE9-B6E91E0D6D3F}", "returns true if each element of both Matrix are equal within some tolerance", "A", "B", "Tolerance");
   ```
   替换函数和ScriptCanvasFunction.xml 内容：
   ```cpp
   Data::BooleanType IsClose(const Data::Matrix3x3Type& lhs, const Data::Matrix3x3Type& rhs, const Data::NumberType tolerance) { ... }
   ```
   ```xml
   <Function Name="IsClose">
       <Parameter Name="A"/>
       <Parameter Name="B"/>
       <Parameter Name="Tolerance" DefaultValue="0.01"/>
   </Function>
   ```

1. 替换`SCRIPT_CANVAS_GENERIC_FUNCTION_MULTI_RESULTS_NODE`的示例：
   ```cpp
   AZ_INLINE std::tuple<Data::Vector3Type, Data::Vector3Type, Data::Vector3Type> GetRows(const Data::Matrix3x3Type& source) { ... }
   SCRIPT_CANVAS_GENERIC_FUNCTION_MULTI_RESULTS_NODE(GetRows, k_categoryName, "{DDF76F4C-0C79-4856-B577-7DBA092CE59B}", "returns all rows from matrix", "Source", "Row1", "Row2", "Row3");
   ```
   替换函数和ScriptCanvasFunction.xml 内容：
   ```cpp
   AZStd::tuple<Data::Vector3Type, Data::Vector3Type, Data::Vector3Type> GetRows(const Data::Matrix3x3Type& source) { ... }
   ```
   ```xml
   <Function Name="GetRows">
       <Parameter Name="Source"/>
   </Function>
   ```

### 步骤 3：更新泛型函数节点宏
使用从 ScriptCanvasFunction.xml 派生的经过净化的替换节点标识符，将每个通用函数节点宏替换为`SCRIPT_CANVAS_GENERIC_FUNCTION_REPLACEMENT` 宏。我们使用`Namespace` 和 `Function Name`来保证唯一性。

例如，在 [Matrix3x3.ScriptCanvasFunction.xml](https://github.com/o3de/o3de/blob/development/Gems/ScriptCanvas/Code/Include/ScriptCanvas/Libraries/Math/Matrix3x3.ScriptCanvasFunction.xml)中
* Namespace: `ScriptCanvas::Matrix3x3Functions`
* 函数名: `GetRow`, `GetRows`, `IsClose`
```xml
<ScriptCanvas>
    <Library
        Include="Include/ScriptCanvas/Libraries/Math/Matrix3x3.h"
        Namespace="ScriptCanvas::Matrix3x3Functions"
        Category="Math/Matrix3x3">
...
        <Function Name="GetRow">
        <Function Name="GetRows">
        <Function Name="IsClose">
...          
    <Library/>  
<ScriptCanvas/>
```
已清理的替换节点标识符为： `ScriptCanvas_Matrix3x3Functions_GetRow`, `ScriptCanvas_Matrix3x3Functions_GetRows` 和 `ScriptCanvas_Matrix3x3Functions_IsClose`
```cpp
...
SCRIPT_CANVAS_GENERIC_FUNCTION_REPLACEMENT(GetRow, k_categoryName, "{C4E00343-3642-4B09-8CFA-2D2F1CA6D595}", "ScriptCanvas_Matrix3x3Functions_GetRow");
SCRIPT_CANVAS_GENERIC_FUNCTION_REPLACEMENT(GetRows, k_categoryName, "{DDF76F4C-0C79-4856-B577-7DBA092CE59B}", "ScriptCanvas_Matrix3x3Functions_GetRows");
SCRIPT_CANVAS_GENERIC_FUNCTION_REPLACEMENT(IsClose, k_categoryName, "{020C2517-F02F-4D7E-9FE9-B6E91E0D6D3F}", "ScriptCanvas_Matrix3x3Functions_IsClose");
...
```

### 步骤 4：升级现已过期的现有图表
此时，系统已包含执行替换所需的所有信息。要升级图表，请执行以下操作之一：
1. 批处理 - 为方便起见，您可以使用 Script Canvas Editor 中 **Tools/Upgrade Graphs** 菜单中的版本资源管理器工具。您可以在工具中选择多个源图，并将它们全部升级到最新版本。该工具将以新格式保存它们，或者在升级失败时报告错误。
1. 单次处理 - 您可以在 Script Canvas Editor 中打开特定图形。图表将自动升级。保存图表以保持新格式。
