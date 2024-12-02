---
description: '使用 AzTest 框架为 Open 3D 引擎编写 C++ 单元测试。 '
title: 使用 AzTest
---

AzTest 旨在成为 Open 3D Engine 使用的测试框架的抽象。通过使用抽象而不是框架本身，O3DE 不会被锁定在框架发生变化时必须在整个系统中进行更改。AzTest 还提供了方便的函数和基本实现，使使用它的所有模块编写测试变得更加容易。

Google Test 和 Google Mock 框架可以通过 AzTest 完全访问。在自定义测试中包含  `AzTest/AzTest.h` 使开发人员可以访问 Google Test 和 Google Mock 所具有的所有内容。

## CTest

CTest 是一个测试执行工具，也是 CMake 的一部分。测试由 CTest 注册和启动，然后调用 AzTestRunner。AzTestRunner 有助于在库或可执行文件中启动 GoogleTest 测试模块。CTest 将通过 AzTestRunner 运行 dll，以便在注册后执行测试。有关在 O3DE 中使用 CTest 的更多信息，请参阅 [O3DE 测试载入](/docs/user-guide/testing/getting-started/).

## 测试 Hook

*AzTest.h* 提供了几个宏，可用于创建测试扫描程序可用于调用测试的钩子。下面列出了这些和使用示例。

### `AZ_UNIT_TEST_HOOK(...)`

`AZ_UNIT_TEST_HOOK(...)` 用于单元测试，无论您是否有要初始化的环境，都可以使用。

```cpp
#include <AzTest/AzTest.h>

AZ_UNIT_TEST_HOOK()  // Don't have any environments
```

以下是使用同一 hook 添加环境的方法。

```cpp
#include <AzTest/AzTest.h>

// Environments subclass from AZ::Test::ITestEnvironment
class ExampleTestEnvironment : public AZ::Test::ITestEnvironment
{
public:
    AZ_TEST_CLASS_ALLOCATOR(ExampleTestEnvironment);
    virtual ~ExampleTestEnvironment() {}

protected:
    // There are two pure-virtual functions to implement, setup and teardown
    void SetupEnvironment() override
    {
        // Setup code
    }

    void TeardownEnvironment() override
    {
        // Teardown code
    }

private:
    // Put members that need to be maintained throughout testing lifecycle here
    // Don't declare them in the setup/teardown functions!
}

// IMPORTANT! Declare your environment dynamically BEFORE using the macro
// The framework will perform the appropriate deletes when done
AZ_UNIT_TEST_HOOK(new ExampleTestEnvironment)
```

使用 `AZ_TEST_CLASS_ALLOCATOR(...)`宏，以正确对齐和管理为测试环境创建的内存，并防止与内存管理器发生冲突和错误。

```cpp
#include <AzTest/AzTest.h>
#include "SharedTestEnvironment.h"

// In this case we have a shared environment, so we only need specific setup here
class ModuleSpecificTestEnvironment : public AZ::Test::ITestEnvironment
{
    AZ_TEST_CLASS_ALLOCATOR(ModuleSpecificTestEnvironment);
    // See above for environment example
}

// This macro is variadic, so you can list the environments you want without declaring
// a container.
// IMPORTANT! Environments are setup in given order and torn down in reverse order,
// so if one environment needs to be setup before another, make sure it comes first
// in the list!
AZ_UNIT_TEST_HOOK(new SharedTestEnvironment, new ModuleSpecificTestEnvironment)
```

### 链接

1. [GoogleTest 文档](https://github.com/google/googletest)
1. [Official CTest 文档](https://cmake.org/cmake/help/latest/manual/ctest.1.html)
1. [O3DE Test Onboarding](/docs/user-guide/testing/getting-started)
