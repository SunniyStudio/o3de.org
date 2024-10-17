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


Each element of the set passed to `AUTOGEN_RULES` should follow the `<input>,<template>,<output>` format. Here, `<input>` is the name of the XML or JSON data input file, `<template>` is the name of the Jinja template file, and `<output>` is the name that you want the generated file to have. 

{{< important >}}
Data input files and Jinja template files must be listed in your project's set of files, which is located in a `*_files.cmake` file, inside a `set(FILES...)` command. For organization, we recommend creating a specific `*_autogen_files.cmake` file and placing Jinja template files there. 
{{< /important >}}

### Data input files

A data input filename can refer to one file explicitly or many files that match a pattern.  If you list a filename explicitly, then it must have a proper path that's absolute or relative to the `CMakeLists.txt` file. If a filename contains a pattern, then the expansion finds all data input files that match the filename.

<!-- TODO: Verify - can explicit filenames be absolute or relative to the CMakeLists.txt file?-->

The input filename supports the following matching operators:
  * `*` - Sequence of characters any length.
  * `?` - Single-character sequence.
  * `[<sequence>]` - Matches any characters in `<sequence>`.
  * `[!<sequence>]` - Matches any characters _not_ in `<sequence>`.

For information about authoring data input files, refer to [Authoring Jinja templates and data inputs](/docs/user-guide/programming/autogen/).

### Jinja Templates

A template filename must be an explicit path to a single `.jinja` file. The path can be absolute or relative to the `CMakeLists.txt` file. 

For information about authoring Jinja templates, refer to [Authoring Jinja templates and data inputs](/docs/user-guide/programming/autogen/).


### Output files

When AzAutoGen feeds data input files into the Jinja2 templates, it generates output files. You must specify the names of the output files you want to generate in the autogen rules. You can use the following *special values* when defining the output filenames:

* `$path`: The path to the final output destination, `${CMAKE_CURRENT_BINARY_DIR}/Azcg/Generated`.
* `$fileprefix`: The name of the current input file up to the first `.` token. This is equivalent to running the UNIX shell command `basename ${file%%.*}`.
* `$file`: The full name of the current input file. This is equivalent to running the UNIX shell command `basename $file`.

{{< important >}}
When generating multiple output files, the output name should _always_ begin with `$path`. Otherwise, AzAutoGen may place output files in an incorrect directory.
{{< /important >}}

### Example

The following example shows how autogen rules are formatted.

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

## Integrating with any target

Most of the time, you want to run AzAutoGen by passing autogen rules into `ly_add_target()`, as outlined in the [Integrating with an O3DE target build](#integrating-with-an-o3de-target-build) section earlier. 
For situations where a target is already defined and AzAutoGen needs to be invoked, use the `ly_add_autogen()` CMake command. This command associates a set of autogen rules, including build outputs, with an existing target. 
The `ly_add_autogen()` function is defined in [LyAutoGen.cmake](https://github.com/o3de/o3de/blob/development/cmake/LyAutoGen.cmake#L9-L15). It takes the following parameters:
```cmake
ly_add_autogen(
    NAME Name of the target to add the autogen step to
    OUTPUT_NAME (optional) Overrides the name of the output target. If not specified, the name will be used.
    INCLUDE_DIRECTORIES List of directories to use as include paths
    AUTOGEN_RULES Set of autogen rules that describe output generation and are passed to the AzAutoGen expansion system
    ALLFILES List of data input files contained by the target and used to generate source code
)
```

## Related topics

| Title | Description |
|-|-|
| [Automate Source Generation from Templates with AzAutoGen](/docs/user-guide/programming/autogen/) | How to generate new types of network packets for O3DE. |
| [Networking Auto-packets](/docs/user-guide/networking/aznetworking/autopackets/) | How to create new packet types for `AzNetworking` using AzAutoGen. |
| [Creating Custom Nodes in Script Canvas](/docs/user-guide/scripting/script-canvas/programmer-guide/custom-nodes/) | How to create custom nodes in Script Canvas using XML definitions and turn the nodes into code with AzAutoGen. |toGen. |
