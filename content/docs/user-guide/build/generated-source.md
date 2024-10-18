---
linktitle: 构建生成的源文件
title: 使用 AzAutoGen 生成源文件
description: 了解如何将 AzAutoGen 集成到 Open 3D Engine (O3DE)，以便在使用 CMake 构建目标时生成源文件。
weight: 150
---

对于使用大量模板代码的复杂系统来说，在可能的情况下执行某种轻量级自动化来生成源文件是非常有用的。为了在构建过程中生成文件，**Open 3D Engine (O3DE)** 通过 CMake 命令使用**AzAutoGen**工具。O3DE 将输出的源代码存储在 CMake 编译目录中，并提取代码在目标中进行编译。

要使用 AzAutoGen，必须提供 Jinja 模板和 XML 或 JSON 数据文件，以便 AzAutoGen 生成输出文件。这些文件的内容取决于你使用 AzAutoGen 的预期目的。有关 AzAutoGen 如何工作以及如何编写模板和生成数据输入的完整说明，请参阅 [使用 AzAutoGen 从模板自动生成源代码](/docs/user-guide/programming/autogen/)。


## 与 O3DE 目标构建集成

当 CMake 编译 O3DE 目标时，会调用 AzAutoGen。要调用 AzAutoGen 并为项目或 Gem 生成输出源文件，请完成以下步骤：

1. 通过在项目的 `CMakeLists.txt` 文件中向 `ly_add_target(...)` 命令传递一组 `AUTOGEN_RULES` 来定义自动生成规则。每条规则将一组数据输入文件映射到一个模板，并指定生成内容的输出文件名。

   有关定义自动生成规则的规范，请参阅本页的 [自动生成规则](#autogen-rules) 部分。

2. 将数据输入文件（`.xml` 和 `.json`）和模板（`.jinja`）添加到项目的所有文件集中，以便 CMake 能在构建时找到它们。
   要做到这一点:
    - 在项目的 `*_files.cmake` 文件中，将文件名传入 `set(FILE...)` 命令。
    - 在项目的 `CMakeLists.txt` 文件中，将 `*_files.cmake` 文件包含到 `FILE_CMAKE` 参数中。

编译时，CMake 会检测自动生成规则列表，推断生成文件集，并设置适当的编译时钩子，以便为项目运行 AzAutoGen。然后，CMake 会将输出目录放在 `${CMAKE_CURRENT_BINARY_DIR}/Azcg/Generated`。

### 示例

下面的示例展示了 AzNetworking 框架如何为系统所需的各类数据包生成源代码的片段。

Autogen 规则在 [`Code/Framework/AzNetworking/CMakeLists.txt`](https://github.com/o3de/o3de/blob/dd0978c59f1d01b39e006e6c3ba3baf6060136cf/Code/Framework/AzNetworking/CMakeLists.txt#L33-L36)中定义。它还包括使用 `FILES_CMAKE` 参数的 `aznetworking_files.cmake` 文件。
```cmake
ly_add_target(
    NAME AzNetworking STATIC
    NAMESPACE AZ
    FILES_CMAKE
        AzNetworking/aznetworking_files.cmake
    # ...
    AUTOGEN_RULES
        *.AutoPackets.xml,AutoPackets_Header.jinja,$path/$fileprefix.AutoPackets.h
        *.AutoPackets.xml,AutoPackets_Inline.jinja,$path/$fileprefix.AutoPackets.inl
        *.AutoPackets.xml,AutoPackets_Source.jinja,$path/$fileprefix.AutoPackets.cpp
)
```

数据输入文件和模板包含在 [Code\Framework\AzNetworking\AzNetworking\aznetworking_files.cmake](https://github.com/o3de/o3de/blob/dd0978c59f1d01b39e006e6c3ba3baf6060136cf/Code/Framework/AzNetworking/AzNetworking/aznetworking_files.cmake#L12-L17)中。
```cmake
set(FILES
    # ...
    AutoGen/AutoPacketDispatcher_Header.jinja
    AutoGen/AutoPacketDispatcher_Inline.jinja
    AutoGen/AutoPackets_Header.jinja
    AutoGen/AutoPackets_Inline.jinja
    AutoGen/AutoPackets_Source.jinja
    AutoGen/CorePackets.AutoPackets.xml
    # ...
)
```


## 自动生成规则

**自动生成规则**指示 CMake 使用 AzAutoGen 生成文件。
您可以通过在项目的 `CMakeLists.txt` 文件中向 `ly_add_target(...)` 命令传递一组 `AUTOGEN_RULES` 来定义自动生成规则。


传给 `AUTOGEN_RULES` 的每个元素都应遵循 `<input>,<template>,<output>` 格式。这里，`<input>` 是 XML 或 JSON 数据输入文件的名称，`<template>` 是 Jinja 模板文件的名称，`<output>` 是希望生成的文件的名称。

{{< important >}}
数据输入文件和Jinja模板文件必须列在项目的文件集中，文件集位于`set(FILES...)`命令内的`*_files.cmake`文件中。为便于组织，我们建议创建一个特定的 `*_autogen_files.cmake` 文件，并在其中放置金雅模板文件。
{{< /important >}}

### 数据输入文件

数据输入文件名可以明确指代一个文件，也可以指代与模式匹配的多个文件。 如果您明确列出一个文件名，那么它必须有一个正确的路径，该路径可以是绝对路径，也可以是相对于 `CMakeLists.txt` 文件的路径。如果文件名包含一个模式，则扩展会找到与文件名匹配的所有数据输入文件。

<!-- TODO: 验证 - 显式文件名可以是 CMakeLists.txt 文件的绝对文件名还是相对文件名？-->

输入文件名支持以下匹配运算符：
  * `*` - 任意长度的字符序列
  * `?` - 单字符序列
  * `[<sequence>]` - 匹配`<sequence>`中的任意字符。
  * `[!<sequence>]` - 匹配**不在**`<sequence>`中的任何字符。

有关编写数据输入文件的信息，请参阅 [编写 Jinja 模板和数据输入](/docs/user-guide/programming/autogen/)。

### Jinja 模板

模板文件名必须是指向单个 `.jinja` 文件的明确路径。路径可以是绝对路径，也可以是相对于 `CMakeLists.txt` 文件的路径。

有关编写 Jinja 模板的信息，请参阅 [编写 Jinja 模板和数据输入](/docs/user-guide/programming/autogen/)。


### 输出文件

当 AzAutoGen 将数据输入文件送入 Jinja2 模板时，它会生成输出文件。您必须在自动生成规则中指定要生成的输出文件名。在定义输出文件名时，可以使用以下**特殊值**：

* `$path`: 到最终输出目的地的路径，`${CMAKE_CURRENT_BINARY_DIR}/Azcg/Generated`。
* `$fileprefix`: 当前输入文件的名称，直至第一个 `.` 标记。这相当于运行 UNIX shell 命令 `basename ${file%%.*}`。
* `$file`: 当前输入文件的全名。这相当于运行 UNIX shell 命令 `basename $file`。

{{< important >}}
生成多个输出文件时，输出名称应**始终**以 `$path`开头。否则，AzAutoGen 可能会将输出文件放到错误的目录中。
{{< /important >}}

### 示例

下面的示例显示了自动生成规则的格式。

```
AUTOGEN_RULES
    # Test wildcarded expansion for xml
    *.AutoEnum.xml,AutoEnum_Header.jinja,$path/$fileprefix.AutoEnum.h
    *.AutoEnum.xml,AutoEnum_Source.jinja,$path/$fileprefix.AutoEnum.cpp
    
    # Test wildcarded expansion for json
    *.AutoStruct.json,AutoStruct_Header.jinja,$path/$fileprefix.AutoStruct.h
    
    # Test explicit data input file
    AzCore/AutoGen/WeaponTypes.AutoStruct.json,AutoStruct_Header.jinja,$path/$fileprefix2.AutoStruct.h
    
    # Test globbing data input files into a single output file
    *.AutoEnum.xml,AutoEnumRegistry_Header.jinja,$path/AutoEnumRegistry.h
```

## 与任何目标集成

大多数情况下，您希望通过向 `ly_add_target()` 传递自动生成规则来运行 AzAutoGen，如前面的[与 O3DE 目标构建集成](#integrating-with-an-o3de-target-build) 部分所述。
如果目标已定义且需要调用 AzAutoGen，请使用 `ly_add_autogen()` CMake 命令。该命令将一组自动生成规则（包括编译输出）与现有目标关联起来。
`ly_add_autogen()`函数在 [LyAutoGen.cmake](https://github.com/o3de/o3de/blob/development/cmake/LyAutoGen.cmake#L9-L15)中定义。它需要以下参数：
```cmake
ly_add_autogen(
    NAME Name of the target to add the autogen step to
    OUTPUT_NAME (optional) Overrides the name of the output target. If not specified, the name will be used.
    INCLUDE_DIRECTORIES List of directories to use as include paths
    AUTOGEN_RULES Set of autogen rules that describe output generation and are passed to the AzAutoGen expansion system
    ALLFILES List of data input files contained by the target and used to generate source code
)
```

## 相关主题

| 标题 | 说明 |
|-|-|
| [使用 AzAutoGen 从模板自动生成源代码](/docs/user-guide/programming/autogen/) | 如何为 O3DE 生成新类型的网络数据包。 |
| [网络自动数据包](/docs/user-guide/networking/aznetworking/autopackets/) | 如何使用 AzAutoGen 为 `AzNetworking` 创建新的数据包类型。 |
| [创建自定义Script Canvas节点](/docs/user-guide/scripting/script-canvas/programmer-guide/custom-nodes/) | 如何使用 XML 定义在 Script Canvas 中创建自定义节点，并使用 AzAutoGen 将节点转化为代码。|AzAutoGen。 |
