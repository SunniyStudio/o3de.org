---
linkTitle: 入门
title: Test Automation 入门
description: Learn about the automated testing tools provided with Open 3D Engine (O3DE).了解 Open 3D Engine （O3DE） 提供的自动化测试工具。
toc: true
weight: 500
---

本指南介绍了 Open 3D Engine （O3DE） 中的自动化测试工具，并介绍了它们的基本用法。有关每个工具的更多详细信息，请访问其特定的文档页面，链接在下面的每个部分中。

## 概述

O3DE 使用 [CMake](https://cmake.org/cmake/help/latest/)，这是一个包含 [CTest](https://cmake.org/cmake/help/latest/manual/ctest.1.html)的构建系统。CTest 是一个通用的测试运行器工具，用于协调测试的执行和报告，O3DE 使用它来启动所有自动化测试。O3DE 项目还可以通过 CTest 注册自己的测试，以测试其独特的游戏代码或引擎扩展。通过在本地运行 CTest，开发人员可以在提交更改之前验证代码运行状况。

{{< important >}}
所有 CTest 测试都在 O3DE 自动审阅 （AR） 管道中执行，以帮助防止不良合并。提交新代码需要进行测试;对现有代码的更改不得导致回归。确保在向 O3DE 项目提交任何代码之前在本地运行测试。
{{< /important >}}

本主题的其余部分介绍如何向 CTest 注册测试以测试 O3DE 代码，以及如何为各种测试运行程序编写这些测试。

![CMake and CTest workflow](/images/user-guide/testing/getting-started/cmake_ctest_workflow.png)

## CTest

CTest 类似于 GoogleTest、PyTest、JUnit 或 NUnit 等其他测试框架;尽管它使用更高级别的通用接口。CTest 将每个测试注册为一串命令行参数，并报告调用这些参数是否成功（返回“0”）而没有挂起。CTest 还可以协调并行进程中运行的测试。与其他测试运行程序不同，CTest 通过与 OS shell 连接而与编程语言无关。然而，这意味着 CTest 中缺少一个值得注意的功能：其他测试框架通常提供较低级别的接口，该接口直接调用特定编程语言中的函数。为了提供此函数级执行，O3DE 使用 CTest 来调用较低级别的测试运行程序。

O3DE 提供包装代码，以帮助将 GoogleTest 和 PyTest 等框架的测试注册到 CTest 中。除了提供的 test-tools 库外，这些包装器还允许编写可在底层框架支持的任何操作系统上运行的测试。生成 XML 文件以跟踪未报告给 CTest 的低级结果，并保存工件，例如日志输出和故障转储。

![CTest calls other test runners](/images/user-guide/testing/getting-started/ctest_to_runners.png)

简而言之，O3DE 中的所有测试都使用 CTest 作为高级测试协调器。执行时，CTest 会调用其他较低级别的测试工具，然后报告它们的成功情况。

### 启动 CTest

CTest 希望其工作目录是 CMake 构建目录，因此请务必先在终端中导航到此目录。此 build 目录应与 CMake 配置的目录相同。然后，使用 '-C' 选择您已经构建的构建配置：

* **Windows**:
    ```cmd
    cd <local_path_to>\o3de\build\<build_folder>
    ctest -C <build_configuration>
    ```

* **Linux**:
    ```shell
    cd <local_path_to>/o3de/build/<build_folder>
    ctest -C <build_configuration>
    ```

{{< caution >}}
如果没有过滤器，该命令将运行每个已注册的测试，并可能导致数小时的测试执行！如果要停止 CTest，请选择终端并按 **Ctrl+C** 来发送中断信号。
{{< /caution >}}

CTest 还可以使用`-L`参数运行标记测试套件的子集。这些测试套件应包含在括号(`(..)`)并用`|`字符分隔它们的名称。以下示例演示了在 Windows 或 Linux 上运行`profile`构建的 Main 和 Smoke 套件的语法：

* **Windows**:
    ```cmd
    cd C:\github\o3de\build\windows
    ctest -C profile -L "(SUITE_smoke|SUITE_main)"
    ```

* **Linux**:
    ```shell
    cd user/github/o3de/build/linux
    ctest -C profile -L "(SUITE_smoke|SUITE_main)"
    ```

建议您在本地计算机上使用 Main 和 Smoke 套件验证测试。这些测试将针对`o3de`存储库打开的任何拉取请求 （PR） 执行，作为 PR 工作流程中自动审查管道的一部分。这两个套件都必须执行得相对较快，并且不能间歇性地失败，并且是证明您的更改不会破坏其他功能的简单方法。

运行 CTest 后，结果保存到 `.../<build_folder>/Testing/`。 如果您希望直接在终端中查看失败的完整输出，请添加标志`--output-on-failure`。

有关 CTest 用法的更多信息，请参阅其 [在线文档](https://cmake.org/cmake/help/latest/manual/ctest.1.html).

### 向 CTest 添加测试模块

CTest 注册测试代码的整个模块，这些模块通常包含同一功能的多个单独测试。要添加新测试，请完成一个先决条件和三个步骤：

1. **先决条件**：为测试将针对的生产代码添加构建目标。
1. 为需要编译的测试添加构建目标（Python 不需要）。
1. 在`CMakeLists.txt`中注册您的测试模块。
1. 将单个测试函数添加到测试模块中。

具体步骤因不同类型的测试而异，将在后面的部分中讨论。

## GoogleTest

对于许多测试，使用与测试目标的生产代码相同的语言编写代码会更容易。由于 O3DE 的大部分都使用 C++，因此它的大部分测试也使用 C++。O3DE 的 C++ 测试通常是针对特定低级功能的小型 [单元级]https://softwaretestingfundamentals.com/unit-testing/) 测试，运行速度极快。O3DE 使用 [GoogleTest](https://github.com/google/googletest/blob/main/docs/index.md) 以及名为 [AzTest](/docs/user-guide/testing/aztest/aztest/) 的实用程序。AzTest 的一部分是名为`AzTestRunner`的执行包装器。CTest 调用`AzTestRunner`来加载 C++ 测试库和目标库，然后执行任何加载的 GoogleTest 测试。

O3DE 功能的 C++ 产品代码内置到库中，稍后加载到应用程序中。在为此代码编写 C++ 测试时，测试代码也应同样构建到其自己的单独库中。然后，这个 test-library 声明了对加载它所测试的 production 库的依赖性。构建单独的仅测试库的优点是保持生产二进制文件精简且易于发布，并且没有无关的```#if defined```块用于仅测试逻辑。这还可以确保测试以产品附带的完全相同的接口为目标。

### 注册新的 C++ 测试

在阅读后续步骤时，请参阅以下`CMakeLists.txt`示例：

```
# Preexisting module registration from completing the Prerequisite step
ly_add_target(
    NAME MyModuleName
    NAMESPACE Gem
    FILES_CMAKE
        mymodule_files.cmake
    INCLUDE_DIRECTORIES
        PRIVATE
            Source
        PUBLIC
            Include
    BUILD_DEPENDENCIES
        PRIVATE
            Legacy::CryCommon
    COMPONENT
        MyModuleComponent
)

# New test module registration from Step 1
# This configures CMake to also build a library called `MyModuleName.Tests.so` (.dll on Windows)
# The rest of CMake refers to this new module by its NAME ("MyModuleName.Tests")
if(PAL_TRAIT_BUILD_SUPPORTS_TESTS)  # only create these test modules if the target platform supports testing

    ly_add_target(
        NAME MyModuleName.Tests SHARED
        NAMESPACE Gem
        FILES_CMAKE
            mymodule_test_files.cmake
        INCLUDE_DIRECTORIES
            PRIVATE
                Tests
                Source
        BUILD_DEPENDENCIES
            PRIVATE
                AZ::AzTest
                Legacy::CryCommon
                Gem::MyModuleName
        COMPONENT
            MyModuleComponent
    )

    # Register new test module from Step 2
    ly_add_googletest(
        NAME MyModuleName.Tests
    )

endif()
```

#### 先决条件：添加生产构建目标

在配置测试之前，必须首先定义要测试的库。如果您的测试以现有功能为目标，则此步骤已完成，您只需找到正确的文件路径即可。Target 库在`CMakeLists.txt`中定义，该  通常与代码位于同一目录或父目录中。有关配置`CMakeLists.txt`文件的更多信息，请参阅 [构建部分](/docs/user-guide/build/)。请注意，您只需要定义测试的目标 production 库，而不是 production 库中的每个功能。您可以在完成生产代码之前配置和编写测试。

#### 步骤 1：添加测试生成目标

与生产构建目标类似，测试目标在`CMakeLists.txt`配置文件中定义库。首先，找到您在先决条件步骤中创建的`CMakeLists.txt`。 它应该位于类似于`o3de/.../<MyModule>/CMakeLists.txt`的路径中。

修改`CMakeLists.txt`文件，用`ly_add_target()` 定义你的新测试模块。与生产构建目标类似，最简单的方法是创建另一个 `.cmake` 文件，其中列出了用于编译测试库的 C++ 文件。

上面的示例使用了`o3de/.../<MyModule>/mymodule_test_files.cmake`，其内容类似于以下内容：

```
set(FILES
    test/MyModuleTestFile.cpp
    test/MyModuleMathTests.cpp
)
```

#### 第 2 步：注册测试模块

在`CMakeLists.txt`中，使用帮助程序函数向 CTest 注册模块`ly_add_googletest()`。

{{< important >}}
GoogleTest 模块应避免使用`TEST_SERIAL`标志，因为这会阻止测试与其他测试模块并行高效执行。如果测试存在依赖关系，导致它们无法并行执行，请在[O3DE Discord](https://{{< links/o3de-discord >}}) 频道 sig-testing 中与测试特别兴趣小组展开讨论！
{{< /important >}}

要验证所有内容都已正确设置，请从 CMake CLI** 或 CMake GUI** 运行 CMake configure 命令（在 [配置和构建](/docs/user-guide/build/configure-and-build/) 部分中描述）。这将注册您刚刚添加的所有内容，如果有任何配置错误，则会发出错误。

#### 第 3 步：编写新测试

现在，您已经配置了 CMake 以创建测试库并将其注册到 CTest，您可以编写新的测试了。要简化模块结构，请在 `o3de/.../<MyModule>/tests/` 中创建新的测试文件。

测试是使用标准 [GoogleTest](https://github.com/google/googletest/blob/main/docs/index.md) 语法编写的，这有助于您编写小型函数来测试代码。要从 GoogleTest 中提取所有内容以及一些方便的工具，请将以下语句添加到您的 C++ 测试文件中：

```cpp
#include <AzTest/AzTest.h>
```

为了使测试函数一目了然，我们建议使用`UnitOfWork_StateUnderTest_ExpectedBehavior` 的 [Osherove 命名约定](https://osherove.com/blog/2005/4/3/naming-standards-for-unit-tests.html)。这在读取包含许多单个测试用例失败的报告时很有帮助。考虑此模式的一种方法是将测试总结为`WhatIsExecuted_UniqueSetupStep_MostImportantVerification`。以便可以根据名称理解测试失败，而无需总是调查测试中的代码。如果您难以总结测试，这可能表明测试过于复杂。尝试将复杂的测试分解为多个较小的测试。请注意，虽然 GoogleTest 文档建议在测试名称中 [不使用任何下划线](http://google.github.io/googletest/faq.html#why-should-test-suite-names-and-test-names-not-contain-underscore)，但只要 test 和 fixture 名称不以下划线(`_`)开头或结尾，测试就可以正常运行。

C++ 测试结构的简短示例：

```cpp
// The first parameter is a test fixture, which provides shared setup to multiple tests
// The second parameter is the test name
TEST_F(Matrix4x4Tests, MatrixMultiply_InverseMatrix_ReturnIdentityMatrix)
{
    // (Call the functions under test here.)
    
    ASSERT_TRUE(someErrorState);
    EXPECT_TRUE(someResult);
    EXPECT_FALSE(secondaryProperty);
}
```

有关使用 C++ 编写测试的更多信息，请参见[使用AzTest](/docs/user-guide/testing/aztest/aztest/).

## GoogleBenchmark

对于小段 C++ 代码的性能基准测试，O3DE 使用 [GoogleBenchmark](https://github.com/google/benchmark/blob/main/docs/index.md)。GoogleBenchmark 与 GoogleTest 类似，但测试和基准测试之间的主要区别在于 _failure_ 的定义。在大多数测试中，通过/失败状态直接评估为布尔状态，从而创建成功和失败的目标报告。相反，基准测试会创建一个主观性能指标。定期记录这些指标以帮助检测随时间和代码更改的趋势时，这些指标最有价值。基准测试期间唯一的目标失败发生在代码无法运行或崩溃时。

要配置 GoogleBenchmark 库，请使用 [上述 GoogleTest 库的步骤](#googletest)，但以下情况除外：

* 将代码中的 include 语句更改为 `#include <benchmark/benchmark.h>`.
* 在`CMakeLists.txt`中使用以下 CMake 辅助函数：
   ```
   ly_add_googlebenchmark(
       NAME MyModule.Benchmarks
   )
   ```

## PyTest

有些测试更容易用脚本语言编写，对于这些测试，O3DE 更喜欢使用带有 [PyTest](https://docs.pytest.org/)库的 Python。这些测试的范围通常位于 [集成级别](https://softwaretestingfundamentals.com/integration-testing/) 或更高级别。这些测试有助于验证系统的正确性，类似于最终用户体验软件的方式。尽管有积极的一面，但大范围测试通常很慢，并且提供的故障信息不太具体。这些测试的数量必须受到限制。尽可能使用快速的单元范围测试来验证功能，并且只编写一些更广泛的集成或系统范围的测试。仅当被测库是用 Python 编写的时，才应在 PyTest 中编写单元测试。要对 C++ 代码进行快速单元范围测试，请使用 GoogleTest 而不是 PyTest。

虽然 Python 等解释型语言的性能可能较低，但测试不应在 Python 代码中执行大量计算。相反，测试应该通过向事件发送信号，然后验证响应来协调工作流程。为测试所针对的代码保留繁重的操作，并在 Python 测试代码中执行简单检查。

### 多个 Python 实例

在执行期间，面向 O3DE 的测试可以创建多个单独的 Python 解释器实例，每个实例加载不同的脚本。最常见的是两个实例：_external interpreter_ 和 _Editor interpreter_。本节的其余部分有助于确定您的测试应该在哪个环境中运行，以及如何在那里执行它。

#### 外部解释器中的测试

外部解释器在任何活动的 O3DE 应用程序之外运行。这与运行`python/python.sh` 或 `python\python.cmd`时启动的解释器相同，最适合用于涉及以下内容的测试：

* 启动应用程序并发送外部信号的通用测试，通常带有[LyTestTools](/docs/user-guide/testing/lytesttools/).
* 监控应用程序崩溃。

要在外部解释器中以 PyTest 为目标，默认情况下，您的测试文件名应以`test_`开头。

#### Editor 解释器中的测试

**O3DE 编辑器** 在内部管理 Python 解释器，并通过 Python 绑定库公开特定于编辑器的功能。虽然此环境不等同于启动`python/python.*`，但它使用相同版本的 Python 解释器。Editor 解释器最适合用于涉及以下内容的测试：

* 在 Editor 中定位特定功能，使用[EditorPythonBindings and EditorTest](/docs/user-guide/testing/parallel-pattern/).
* 依赖外部崩溃处理（Editor 崩溃将导致测试脚本崩溃）。

要与 Editor 解释器集成，请创建一个使用 `EditorPythonBindings` 的测试。这些测试**不得**位于以`test_` 或 `tests_`开头的文件中，以避免意外注册为失败的测试。

{{< note >}}
PyTest 不在 Editor 解释器中使用，因此 PyTest 功能不适用于在 Editor 解释器中运行的测试。在设计这些测试时，避免依赖 PyTest 夹具。

EditorTest 仍然使用 PyTest 来管理测试，并额外处理外部崩溃监控、批处理和并行性。如果此工具不能满足您的需求，请提出 [功能请求](https://github.com/o3de/o3de.org/issues/new/choose)或在 [O3DE Discord](https://{{< links/o3de-discord >}})频道 sig-testing 中与测试特别兴趣小组展开讨论！
{{< /note >}}

### 注册新的 Python 测试

注册基于 Python 的测试比注册 C++ 测试更简单。但是，它仍然要求您定义要测试的 C++ 库，您可能在设计集成测试之前已经完成了该库。以下步骤假定已定义 production 库。您可以在 [Build](/docs/user-guide/build/) 部分阅读有关定义生产代码的更多信息。

#### 第 1 步：注册 PyTest 目标

找到定义您正在测试的系统的`CMakeLists.txt`。该文件应位于类似于`o3de/.../<MyModule>/CMakeLists.txt`的路径中。根据测试的不同，将测试注册到其他位置的`CMakeLists.txt`文件可能更合适。原因包括：

* 子模块的测试应存在于其模块的子目录中。
* 跨多个功能集成的广泛测试应存在于功能的父目录中。
* 依赖于游戏项目中的代码或资产的测试必须存在于该游戏项目中。
  * 这避免了禁用项目会破坏仍在注册的测试的情况。

当您找到合适的`CMakeLists.txt`来注册测试时，请添加类似于以下内容的行：

```
ly_add_pytest(
    NAME myPythonTest 
    PATH ${CMAKE_CURRENT_LIST_DIR}/Tests/test_MyFile.py
    TEST_SERIAL # many larger-than-unit tests can interfere with one another in parallel
    RUNTIME_DEPENDENCIES # helps the test build-and-run from an IDE
        Legacy::Editor
        AZ::AssetProcessor
        AutomatedTesting.Assets
)
```

要验证所有内容都已正确设置，请从 CMake CLI** 或 CMake GUI** 运行 CMake configure 命令（在 [配置和构建](/docs/user-guide/build/configure-and-build/) 部分中描述）。这将注册您刚刚添加的所有内容，如果有任何配置错误，则会发出错误。

#### 第 2 步：编写新的 Python 测试

基于 Python 的测试使用 [Python 3](https://docs.python.org/3/)代码以及 [PyTest](https://docs.pytest.org/)。根据测试关注的功能，测试可能包括 O3DE 提供的两个自动化库之一：

* [EditorPythonBindings](/docs/user-guide/testing/parallel-pattern/) 用于 O3DE 编辑器功能的内部测试。
  * 这些测试始终需要`TEST_SERIAL` CTest 标志。
* [LyTestTools](/docs/user-guide/testing/lytesttools/) 模块进行外部测试。
  * 这些测试通常使用`TEST_SERIAL`标志，除非它们不会产生副作用并禁用繁重的辅助功能，例如渲染。
