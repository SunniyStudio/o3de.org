---
linktitle: 故障排除
title: Open 3D Engine中的构建故障排除
description: 解决 O3DE 编译失败问题的技巧、窍门和建议。
weight: 400
---

本指南将帮助您识别并解决在**Open 3D Engine (O3DE)** 构建系统中可能遇到的一些常见问题。
请注意，您可能会遇到与您的项目或联编无关的情况。本参考资料仅针对最常见的构建问题，这些问题不会受到已知错误的影响，也不容易解决。如果您在这里找不到您的问题，请尝试搜索[我们的论坛](https://github.com/o3de/o3de/discussions) 或在[O3DE Discord](https://{{< links/o3de-discord >}})中询问。

如果您认为您的构建问题是由于 O3DE 中的错误造成的，请检查[现有错误报告](https://github.com/o3de/o3de/issues) 和[提交问题](https://github.com/o3de/o3de/issues/new/choose)，如果可以的话！

查找错误日志或内存转储？有关位置，请参阅 [Open 3D Engine日志文件](/docs/user-guide/appendix/log-files)。

## 生成文件的C2027错误

**问题:** MSVC [C2027 编译器错误](https://docs.microsoft.com/cpp/error-messages/compiler-errors-1/compiler-error-c2027) 是由于尝试编译引用缺失类型的文件造成的。这个问题通常是由代码生成工具创建的空文件引起的，最常见的情况是在构建
`AzQtFramework` 库时创建的空文件所致。

**补救措施：**

1. 确保有足够的磁盘空间来构建所选目标。如果没有足够的磁盘空间来写入文件，代码生成器将生成空文件。
1. 删除包含 CMake 缓存信息的目录。
1. 从构建文件夹中删除包含自动生成源代码的文件夹。执行以下操作之一：
1. 确保有足够的磁盘空间来构建所选目标。如果磁盘空间不足，代码生成器将生成空文件。
1. 删除包含 CMake 缓存信息的目录。
1. 从构建文件夹中删除包含自动生成源代码的文件夹。执行以下操作之一：

   * 删除构建目录中所有名称中包含 `_autogen` 的文件夹。
   * 删除 CMake 配置和生成时使用的构建目录，然后重新配置并重新生成。

## CMake 在配置过程中搜索错误的 MSVC

**问题:** CMake 工具报告缺少 MSVC 编译器。这会产生类似以下的警告：

```cmd
CMake Error at CMakeLists.txt:15 (project):
  The CMAKE_C_COMPILER:
 
    C:/Program Files (x86)/Microsoft Visual Studio/2019/Professional/VC/Tools/MSVC/14.24.28314/bin/Hostx64/x64/cl.exe
 
  is not a full path to an existing compiler tool.
```

当 Visual Studio 更新或修改时，CMake 缓存中的信息指向之前安装的编译器时，就会出现这种情况。

**补救措施：** 这个问题最常见于 Visual Studio 更新后，O3DE 项目文件没有再生。
CMake 系统在配置时使用 `CMAKE_C_COMPILER` 和 `CMAKE_CXX_COMPILER` 值设置 C 和 C++ 编译器的路径。
这些值保存在 CMake 缓存中。清除缓存并重新配置，方法如下：

* 删除 CMake 生成目录中的 `CMakeCache.txt` 文件。
* 完全删除 CMake 生成目录。

清理缓存后，在 CMake 配置阶段应能检测到正确的编译器。

## CMake查找`Findo3de.cmake`失败 

**问题：** CMake 工具报告找不到由 “o3de ”提供的软件包配置文件。由此产生的警告类似于：

```cmd
CMake Error at CMakeLists.txt:10 (find_package): 

By not providing "Findo3de.cmake" in CMAKE_MODULE_PATH this project has 
asked CMake to find a package configuration file provided by "o3de", but 
CMake did not find one. 

Could not find a package configuration file provided by "o3de" with any of 
the following names: 

o3deConfig.cmake 

o3de-config.cmake

```

**补救措施：** 此问题通常是由配置错误的 `o3de_manifest.json` 文件引起的，该文件有一个已不存在的 O3DE 版本的条目，或者有丢失或损坏的文件。 如果您安装了 O3DE，然后删除了引擎文件夹，而不是使用 “添加或删除程序 ”卸载，就会出现这种情况。 请执行以下操作：
1. 关闭已打开的项目管理器 (`o3de.exe`)。
1. 在文本编辑器中打开 `<user>/.o3de/o3de_manifest.json`。
1. 删除"engines" 和 "engines_path" 列表中已删除或路径已不存在的引擎条目。
1. 确保"engines" 和 "engines_path"中的最后一行不以逗号结束。
1. 保存并关闭文件。
1. 重启项目管理器 (`o3de.exe`)。
1. 如果在`Projects`页面没有看到您的项目，请重新添加。

当 Project Manager 打开时，它会自动重新注册 O3DE 引擎，并用正确的信息更新 `o3de_manifest.json`。


## 软件包目录检测失败

**问题：** 在配置过程中，软件包目录检测不正确，CMake 配置任务会报告类似以下的错误：

```cmd
cmake -B build/windows -S . -G "Visual Studio 16" -DLY_3RDPARTY_PATH="C:\o3de\3rdParty\"
-- Selecting Windows SDK version 10.0.18362.0 to target Windows 10.0.17763.
-- Using Windows target SDK 10.0.18362.0
CMake Error at cmake/3rdParty.cmake:19 (file):
  file FILE([TO_CMAKE_PATH|TO_NATIVE_PATH] path result) must be called with
  exactly three arguments.
Call Stack (most recent call first):
  CMakeLists.txt:43 (include)CMake Error at cmake/3rdParty.cmake:21 (if):
  if given arguments:    "NOT" "EXISTS" "C:/o3de/3rdParty\"  Unknown arguments specified
Call Stack (most recent call first):
  CMakeLists.txt:43 (include)-- Configuring incomplete, errors occurred!
```

**在 Windows 系统中，当传递给 CMake 的 `LY_3RDPARTY_PATH` 值以 `\` 字符结尾时，就会出现此问题。请执行以下操作之一：

* 更改数值，删除尾部的 `/`。
* 更改 `LY_3RDPARTY_PATH` 的格式，使用与平台无关的 `/` 路径分隔符。

##构建时间长

**问题：** 构建时间比预期的要长。

**补救措施：** 有几个步骤可以解决构建时间过长的问题。
1. 使用可测量耗时的构建命令，以便在排除故障时比较构建时间。 在 Windows 上可以使用 PowerShell `measure-command` 来测量构建时间，在 Linux 上可以使用 `time`。
{{< tabs name="CMake 构建计时示例" >}}
{{% tab name="Windows" %}}

```pwsh
measure-command { cmake --build build/windows --target Editor AutomatedTesting.GameLauncher --config profile | Out-Default}
```

{{% /tab %}}
{{% tab name="Linux" %}}

```shell
time cmake --build build/linux --target Editor AutomatedTesting.GameLauncher --config profile 
```

{{% /tab %}}
{{< /tabs >}}
1. 确保只构建正在使用的目标。 例如，选择适当的目标（如 `Editor`）进行编译，而不是选择 `ALL_BUILD`目标，因为后者会编译所有内容，耗时更长。
   请记住，第一次编译将花费最长的时间，因为必须编译所选目标和依赖项的所有代码，但随后的编译应该会快很多（几分钟或几秒钟），尤其是在编写游戏代码时。
1. 在开发过程中，如果不需要 `debug` 编译配置中所有未优化的调试信息，请使用 `profile` 编译配置。
1. 通过配置选项 `-DLY_UNITY_BUILD=ON`，确保使用 unity 联编（默认开启）。
1. 使用选项 `-DLY_DISABLE_TEST_MODULES=ON` 进行配置后，如果不对引擎进行修改，就不要构建单元测试。
1. 使用 [O3DE CLI 或项目管理器](/docs/user-guide/project-config/add-remove-gems/) 停用项目不需要的Gem。
1. 请参阅下面的 [配置编译器特定设置的故障排除步骤](#building-causes-computer-to-freeze-or-reboot)，在 CPU 核心数量较多的硬件上，这可能是必要的。

## 编译导致计算机冻结、锁定或重启

**问题：** 在某些计算机上，尤其是CPU内核数较多的计算机上，跨内核编译的方式可能导致资源匮乏，甚至冻结。 此外，编译过程中也可能出现内存不足的情况，导致资源短缺和重启等问题。

**补救措施：** 可能的话，调整构建设置，分散每个项目允许使用的最大并行进程数和最大 CL 处理器数。

{{< tabs name="平台专用设置" >}}
{{% tab name="Windows" %}}

对于 Windows 上的 Visual Studio，命令可能如下所示：将最大 CPU 数量设置为 16，并将每个项目的 CL 处理器数量限制为 32。
```pwsh
cmake --build build/windows --target Editor --config profile -- /m:16 /p:CL_MPCount=32
```
如果在 Visual Studio IDE 中构建或调试，请在 “工具”->“选项”->“项目和解决方案”->“构建和运行 ”选项页面设置并行项目构建的最大次数：

![Visual Studio Maximum Number of Parallel Build Options](/images/user-guide/build/visual-studio-options-build-and-run.jpg)

在 “工具”->“选项”->“项目和解决方案”->“VC++ 项目设置 ”选项页面设置最大并发 C++ 编译次数：

![Visual Studio Maximum Concurrent C++ Compilations](/images/user-guide/build/visual-studio-options-vc++-project-settings.jpg)

尝试使用不同的值，以找到最适合您硬件的设置。
在 Windows 系统中，您可以在 PowerShell 终端中使用以下命令测量不同设置下的编译时间：

```pwsh
measure-command { cmake --build build/windows --target Editor --config profile -- /m:8 /p:CL_MPCount=8 | Out-Default}
```
示例输出：
```cmd
Days              : 0
Hours             : 0
Minutes           : 15
Seconds           : 56
Milliseconds      : 701
Ticks             : 9567014354
TotalDays         : 0.0110729332800926
TotalHours        : 0.265750398722222
TotalMinutes      : 15.9450239233333
TotalSeconds      : 956.7014354
TotalMilliseconds : 956701.4354
```

{{% /tab %}}
{{% tab name="Linux" %}}

使用[`-j` 或 `--parallel` cmake 选项](https://cmake.org/cmake/help/latest/manual/cmake.1.html#cmdoption-cmake-build-j) 来限制构建时使用的并行作业数量。

例如，在一台 12 核 32 GB 的机器上，如果编译进程被杀死，则可能需要将并行编译作业的数量减少到 6 个。

```bash
cmake --build <build-dir> -j 6 --config profile --target MyProject.GameLauncher -- <generator specific options>
```

{{% /tab %}}
{{< /tabs >}}
