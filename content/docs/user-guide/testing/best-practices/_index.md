---
linkTitle: 最佳实践
title: 测试自动化最佳实践
description: 在 Open 3D Engine （O3DE） 中自动执行测试的建议。
toc: true
weight: 600
---

本页提供了有关有多少独立开发人员可以通过自动化测试有效维护 **Open 3D Engine （O3DE）** 的建议。该建议适用于使用 O3DE 编写自己的自动化的团队，以及为 O3DE 做出贡献的任何人。本文档并非测试建议的详尽来源。相反，它为投资测试自动化提供了基线定义和启发式方法。每个概念仅作为简要介绍;您可以在其他来源中找到许多关于测试最佳实践的深入文章。

## 应避免的做法

### I/O

文件访问、设备输入和网络通信是缓慢的操作，可能会产生复杂的软件和硬件依赖关系。执行 I/O 的测试经常会遇到争用条件，例如并行争用。首选配置 I/O 接口的模拟实现，并使用内存中缓冲区来模拟实际的输入和输出行为。

### Sleep

不要在测试中依赖`this_thread::sleep` 或 `time.sleep`。如果您注意到拉取请求中出现休眠状态，请立即保持怀疑。一些简单的事实：

* 依赖睡眠的测试自然会很慢;将控制权交给底层 OS 计划程序并返回控制权是一个缓慢的操作。
  * 延迟取决于特定设备，并且仍会随其物理和数字硬件状态而波动。
* 将 duration 传递给 sleep 函数请求线程应暂停的 *最小* 时间，而不是线程恢复的确切时间。

{{< caution >}}
避免在任何代码中调用 sleep;它几乎从来都不是正确的工具！
{{< /caution >}}

What are the alternatives?

* 如果您需要等待长时间运行的操作，请考虑使用`AZStd::binary_sempahore` 或 `AZStd::condition_variable`（为简单起见，首选信号量） 来阻止，直到长时间运行的操作完成。
* 如果您需要测试一些与时间相关的功能和提前时间，请公开以编程方式提前时间的功能：
  * 它避免了对 OS 性能计数器的依赖。
  * 它允许在测试时进行更准确的模拟控制。
* 如果您需要 Python 在满足条件之前阻止，请尝试`ly_test_tools.environment.waiter.wait_for(boolean_function)`.
* 如果您需要在 O3DE 编辑器中同步 Python 和 C++ 代码，请首选`azlmbr.legacy.general.idle_wait_frames` 和 `idle_wait_seconds`.

### 非确定性

测试必须尽量减少随机行为。测试失败应证明功能中存在明显的确定性缺陷。特征应该能够隔离其不确定性的来源，允许测试绕过或控制随机性。

当生产代码依赖于随机性或其他非确定性行为时：

* 公开一个接口来设置随机生成器的种子或提供不同的生成器。
* 公开以编程方式完成步骤的功能，例如时间步长。
* 在确定性和非确定性代码之间创建接口
  * 使用接口，测试可以配置 mock 依赖项并仅验证确定性代码

测试应具有特定的期望，这些期望在测试执行之前声明。测试绝不应动态设置期望值，例如从生产代码或网络调用中获取“正确”的值。测试不应使用随机性。

{{< note >}}
当您不知道要检查哪些边界时，随机搜索不同值的 *Fuzz Tests* 可能是一个有用的设计工具。但是，由于这些测试不具有特异性且效率低下，因此请避免检查它们。使用 Fuzz Testing 作为编写特定确定性测试的先决条件。
{{< /note >}}

### 特定于平台的测试

O3DE 中的大多数代码和测试都应该编写为跨平台的。特定于平台的生产代码应使用平台抽象层 （PAL） 模式。然后，测试应依赖其生产代码来使用 PAL 在不同环境中提供相同的功能。

必须以特定于平台的行为为目标的测试应在该依赖项不可用时有条件地禁用自身。测试 ***should AVOID*** 根据其环境执行条件步骤，因为当“同一测试”在不同计算机上自动表现不同时，这会使“同一测试”更难调查。

### 否定断言

测试应该积极验证预期的结果，而不是试图确认 “没有不良副作用” 发生。如果不这样做，就会产生问题，因为与肯定验证的结果不同，所有潜在的错误结果的集合实际上是无限的。请考虑以下代码：

```cpp
int Math::FastSquare( const int number )
{
    return number + number; // bug!
}
```

```cpp
TEST(MathTests, FastSquare_Integer_Squared)
{
    //act
    int result = Math::FastSquare(10);
    //assert
    EXPECT_NE(result, 0);
    EXPECT_NE(result, -1);
    EXPECT_NE(result, 10);
}
```

上面的示例包含一个 bug，即被测代码实际上并不计算数字的平方。该测试正确验证了计算是否不等于几个已知不是 10 平方的值。但是，它从不尝试确定正确答案。因此，测试无法捕获 bug！

{{< note >}}
否定断言不同于否定测试用例。否定测试用例应设置一个错误场景，然后肯定地识别发出了一个正确的错误响应。否定 case 也应避免 negative asserts。
{{< /note >}}

当至少有一个肯定的断言时，包含否定的断言是可以的，但不应单独依赖它们。专注于确定预期的输出。

### 禁用和跳过测试

理想情况下，所有自动化测试失败都会提示立即修复损坏的功能，或者在基本期望发生变化时更新测试。但是，有时必须做出权衡，以中断功能才能启用其他功能。在已知功能已损坏的情况下，可以暂时 “跳过” 或禁用自动测试。

避免在看到失败时立即禁用测试。首先考虑失败会阻止什么操作，以及取消阻止有多重要。相比之下，所有贡献者都忽略了测试可能捕获的所有未来错误，从而进行了风险权衡。

每当 O3DE 提供的测试被禁用时，请 [create an issue](https://github.com/o3de/o3de/issues/new/choose)  来跟踪修复该功能并重新启用测试。

## 要记住的练习

### 疯狂检查

通过临时编辑生产代码，确保将新测试配置为运行和报告失败。有意以测试应检测到的方式中断代码，然后运行测试套件。如果未发生故障，请调查原因！

{{< caution >}}
在验证测试可以检测到故障后，立即恢复任何故意损坏的代码。
{{< /caution >}}

### 浮点断言

浮点数在表示完全相等时存在问题，因为两个浮点数可以表示“相同”值，但舍入误差略有不同。测试框架提供了特殊的断言，这些断言已经说明了许多常见问题：
* C++ 区分浮点数和双精度浮点数，因为它们的评估方式不同`EXPECT_FLOAT_EQ(val1, val2)` 和 `EXPECT_DOUBLE_EQ(val1, val2)`。
  * 还可以为 `EXPECT_NEAR(val1, val2, abs_error)` 提供自定义容错。
* Python 只有一个浮点基元，可以很容易地用  `assert val1 == pytest.approx(val2)` 或 `unittest.assertAlmostEqual(val1, val2)` 来检查
  * 与 C++ 代码交互的编辑器内 Python 测试可以调用`azlmbr.math.Math_IsClose(val1, val2, tolerance)`.

### 断言消息

默认情况下，测试断言会输出有关比较值的简短消息。添加人类可读的消息，以帮助他人快速了解未来的故障。断言消息还可以包含有关其他变量的信息，以帮助调试：

```cpp
TEST(WaterTests, LeakChecker_WaterAdded_DoesNotLeak)
{
    // (arrange and act omitted to focus on the assert below)
    EXPECT_LE(difference, 0) << "Unexpectedly leaked " << difference << " units of Water from " << target.m_waterContainers;
}
```

```python
def test_LeakChecker_WaterAdded_DoesNotLeak():
    # (arrange and act omitted to focus on the assert below)
    assert difference <= 0, f"Unexpectedly leaked {difference} units of Water from {target.water_container_dict}"
```

### 硬编码帮助

测试应清楚地声明它们希望验证的自己的 ground truth。为了使测试简单易读，最好直接使用硬编码值，而不是从其他文件或函数获取的值。以这种方式降低复杂性还可以减少意外创建错误假设的可能性。请考虑以下示例代码：

```cpp
int Math::FastSquare( const int number )
{
    return number + number; // bug!
}
```

```cpp
// This other file contains hard-coded versions of invalid data
enum PrecomputedSquares
{
    pcs_one = 2;
    pcs_two = 4;
    // ...
    pcs_ten = 20;
}
```

```cpp
int CalculateTestSquare( const int number)
{
    return number + number;
}

TEST(MathTests, FastSquare_Integer_Squared1)
{
    // Assumes the function is correct, thus only proves an identity property
    int tenSquared = Math::FastSquare(10);
    int result = Math::FastSquare(10);
    ASSERT_EQ(result, tenSquared);
}

TEST(MathTests, FastSquare_Integer_Squared2)
{
    // Relies on a duplicate implementation of the production code that contains
    // the same bug, thus only proves the production bug was not changed
    int tenSquared = CalculateTestSquare(10);
    int result = Math::FastSquare(10);
    ASSERT_EQ(result, tenSquared);
}

TEST(MathTests, FastSquare_Integer_Squared3)
{
    // Uses incorrectly computed squares from another file, thus only
    // proves the production bug was not changed
    int result = Math::FastSquare(10);
    ASSERT_EQ(result, pcs_ten);
}

TEST(MathTests, FastSquare_Integer_Squared4)
{
    // directly states that 10^2 = 20, which is incorrect but also *easy to notice*
    int tenSquared = 20;
    int result = FastSquare(10);
    ASSERT_EQ(result, tenSquared);
}

TEST(MathTests, FastSquare_Integer_Squared5)
{
    // tersely verifies 10^2 results in 20, which is incorrect but should be *easy to notice*
    ASSERT_EQ(FastSquare(10), 20);
}
```

在上述每个测试中，生产代码的错误数学都是可信的;所有测试都将通过！每个测试都以不同的方式编写错误，无法捕获 bug。第一个测试最终只检查 identity 属性。无论代码如何更改，它都只会验证代码返回相同的值。第二个测试复制了生产代码，随着代码以复杂的方式更改，这将成为一个令人讨厌的模式。第三个测试使用 static 值，但将其隐藏在另一个文件的枚举中。但是，由于第四个和第五个测试对预期进行硬编码，因此更容易看到错误。避免将假设埋在其他文件和函数中。

## O3DE 中自动化测试的大小

大小（或范围）是分类测试的更有用的方法之一。在自动执行测试时，最好编写许多小型测试，而不是编写较少的大型测试。较大的测试需要更多的初始编写工作，并且持续消耗更多的执行和调试时间。如果你可以通过较小的测试捕获问题，请编写较小的测试！

### 单元测试

最直接的自动化测试是单元测试。单元测试有一个非常简单的口号：**一个输入，一个调用，一个输出**

单元测试的目的是小、具体和快速。它们很容易运行，当它们失败时，往往很容易确定原因。这种易用性主要来自“一次调用”方面。不同的交互被视为附加测试，而不是同一测试中的不同步骤，具有更扩展的工作流程。将单元测试视为创建一个非常简单的机器人，该机器人可以快速报告某个特定行为是否中断。每个机器人都允许作者减少因担心设计问题而花费的脑力资源。它还记录和监控任何不熟悉该地区的人的担忧。这使开发人员能够专注于更改生产代码，同时相信他们的机器人将有效地帮助他们检测和调试引入的问题。在编写打算存在一周以上的代码时，请考虑单元测试。

编写单元测试时要遵循的一个很好的模式是 ***Arrange， Act， Assert***：

1. 设置环境状态 （Arrange）
2. 调用被测函数 （Act）
3. 验证预期的副作用 （Assert）

这种简单的结构导致了简短、重点突出的测试，易于理解。下面是一个 C++ 中的简单示例：

```cpp
TEST(VectorTests, PushBack_ContainsTwoItems_ThreeTotalItems)
{
    std::vector<int> testVector {1, 2};  // Arrange
    testVector.push_back(3);             // Act
    EXPECT_EQ(3, testVector.size());     // Assert
}
```

上面的测试验证将 1 个项目添加到已具有 2 个项目的向量是否会导致 Vector 报告它有三个项目。下面是 Python 中的类似示例：

```python
def test_ListAppend_ContainsTwoItems_ThreeTotalItems(self):
    testList = [1, 2]          # Arrange
    testList.append(3)         # Act
    assert 3 == len(testList)  # Assert
```

请注意，上述测试都不是包含在 O3DE 中的好测试，因为它们测试非 O3DE 库（STL 容器和 Python 内置）的行为。外部依赖项应该有自己的自动化测试，这些测试位于自己的项目中。在编写单元测试时，请关注 *您的* 代码的一个特定行为。以下是 [C++](https://github.com/o3de/o3de/blob/development/Code/Framework/AzCore/Tests/Geometry2DUtils.cpp) 和 [Python](https://github.com/o3de/o3de/blob/development/Tools/LyTestTools/tests/unit/test_codeowners_hint.py) 中 O3DE 单元测试的真实示例。这些测试需要几毫秒才能自动验证！

### 集成测试

集成测试是继单元测试之后的下一个级别。这些高阶测试侧重于多个组件和依赖项之间的交互。集成测试涵盖的工作流程略宽一些，尽管它们的范围呈指数级增长，并且对整个代码库的更改更加敏感。这意味着他们可以捕获许多 bug，这既是优点也是缺点。主要缺点来自这些测试的运行和调试速度。当测试链接多个调用和验证与不同的关注点交互时，任何测试失败都将提供更复杂的调试体验。

集成测试对于关键功能区域和检测单元测试无法捕获的故障非常有用。当代码接近主要功能里程碑时，添加或更新集成测试。当您识别发出许多 bug 的功能时，请考虑添加集成测试以更快地检测问题。如果您发现集成测试难以调查和调试，请考虑更改基础功能及其单元测试。

以下是 O3DE 在 [C++](https://github.com/o3de/o3de/blob/development/Gems/EMotionFX/Code/Tests/MotionEventTrackTests.cpp) 和 [Python](https://github.com/o3de/o3de/blob/development/AutomatedTesting/Gem/PythonTests/largeworlds/dyn_veg/EditorScripts/RotationModifierOverrides_InstancesRotateWithinRange.py) 中的集成测试的真实示例，每个测试都需要检查许多同级文件。这些测试需要一分钟的时间才能自动验证。

#### 减轻缺点

1. 为了减少总体调试时间，单元测试可以充当过滤器，在简单问题显示为令人困惑的集成测试失败之前将其捕获。
    * 首先调查单元测试失败。
    * 当发现集成测试失败时，请花点时间问问：“如何用单元测试来捕获这个问题？
2. 测试应始终关注某个特定功能的行为。
    * 从结构上讲，集成测试应尝试遵循 `Arrange, Act, Assert` 的简单模式，并避免采取许多不同的操作。
    * 虽然集成测试旨在涵盖多个依赖项，但缩小操作和断言的范围可以降低花费更多时间调试测试的风险，而不是修复根本问题的风险。
3. 减少集成测试所需的代码量。
    * 考虑重构生产接口，使其更易于编写（单元）测试。
    * 将高度相似的集成测试合并到一个测试中，但要小心复杂性权衡。
4. 批量测试，启动时间长。
    * 在提供给每个测试的 fixture 中共享一次启动成本，但需要权衡允许共享状态干扰其他测试。
    * 有关编辑器内 Python 测试，请参阅 [编辑器测试](https://www.o3de.org/docs/user-guide/testing/parallel-pattern).
5. 并行化几乎没有外部依赖性的小型测试。
    * 可以暴露并行争用的间歇性故障。
    * 默认情况下，单元测试库以并行方式执行。

### 系统测试

系统测试侧重于整个完全集成产品的高层次要求，并对技术质量进行整体评估。这些测试涵盖了系统之间功能的主要工作流程。这种扩大的范围大大放大了使集成测试缓慢且维护成本高昂的一切因素。此外，人类擅长评估这些工作流程，而机器人脚本通常并不理想。因此，O3DE 主要依靠人工来验证大多数高级功能需求。换句话说：请记住手动测试您提议的更改！

由于成本高昂，O3DE 中应该只有少量的自动化系统范围测试。系统测试还应避免详尽地验证较低级别的需求，因为特定的单元和集成测试可以更好地检测特定问题。单元测试和集成测试可以筛选已知的较低级别问题，自动系统测试是针对意外问题的最终最不具体的检查。

如果你打算编写一个针对许多系统的大型自动化测试，请联系[SIG-Testing on Discord](https://{{< links/o3de-discord >}})寻求设计支持！

### 验收测试

验收测试侧重于用户体验。O3DE 没有自动化验收测试，因为机器人不太适合模拟实际的人类用户。这是由于通用人工智能的极端复杂性和成本，用莫拉维茨悖论等观察结果总结道，“让计算机在智力测试或玩跳棋时表现出成人水平的性能相对容易，而在感知和移动性方面，要赋予它们一岁儿童的技能则困难或不可能。

O3DE 依靠贡献者和 SIG 来帮助验证他们的代码是否提供了良好的用户体验。随后，O3DE 依靠用户反馈来告知代码如何满足许多不同用户的需求。每当您发现代码可以改进的地方时，请 [create an issue](https://github.com/o3de/o3de/issues/new/choose) 或 [propose a pull request](/docs/contributing/)!

## O3DE 中的其他自动化测试类别

### 用户界面 （UI） 测试

{{< caution >}}
自动化 UI 测试容易出现错误失败和错误通过。这可能会导致“当任何人更改代码的任何部分时，他们必须更改所有测试”或“此测试从未捕获任何真正的 bug”的反模式。由于这些风险，只有在自动化较低复杂性的测试后，才开始投资于自动化 UI 测试。
{{< /caution >}}

虽然 O3DE 中有大量的 UI 代码，但我们建议自动测试避免以 UI 层为目标。UI 测试相对复杂且维护成本高昂。为 UI 编写验收样式的测试也很容易，其中机器人脚本难以提供有用的反馈。与手动测试 UI 相比，此类测试可能会快速消耗更多的人工调试工作。

依靠 UI 测试来验证功能往往很脆弱，并且可能会引发许多令人困惑的调试会话。在理想情况下，已经存在非 UI 集成测试，这些测试会全面测试功能的底层逻辑。然后，该层之上的任何 UI 测试都不负责测试系统，而只负责界面对简单交互的反应。只要有可能，最好先实现较低层的非 UI 自动化。

### 性能、负载和压力测试

性能测试针对流程、硬件配置或计算机队列的效率。此类测试侧重于监控吞吐量和检测断点。由于重点是记录指标，因此性能测试不同于功能测试（如单元测试）。功能测试寻求目标布尔值通过/失败状态。性能测试会记录主观指标，这些指标需要额外的分析，因为它们会随时间变化。

O3DE 的性能测试工具专注于在本地计算机上创建指标的两层：

1. [微基准测试](/docs/user-guide/testing/getting-started/#googlebenchmark) 与 GoogleBenchmark 一起使用，类似于单元测试。
2. [Profiling markers](/docs/user-guide/testing/profiler/)注释到引擎的不同系统中，类似于系统测试。

## 哪些测试最好？

{{< note >}}
不要被测试设计所麻痹！最有用的自动化测试通常是单元测试。现在编写一个小测试，并在迭代该功能时调整您的方法。如果测试中漏掉了 bug，请修改它们以便下次捕获它。
{{< /note >}}

对于哪些类型的测试对功能最重要，通常没有简单的答案。自动化测试非常有用，因为它们减少了编写工作代码的人力，可以快速检测功能何时回归到损坏状态。自动执行各种类型的许多测试有助于确保最高质量的产品，但所有代码都有自己的维护成本。我们建议根据测试的有效性和弹性来平衡测试：

* 单元测试应占绝大多数测试，它们是：
  * 最高效的写入、执行和维护。
  * 针对新引入的 bug 的自动防御的第一道防线。
  * 通常被称为 *测试金字塔* 的底部。
* 集成测试应占其余自动化测试的大部分。 请记住以下权衡：
  * 集成测试通常会捕获种类最多的 bug，但需要花费更多的精力来编写和维护。
  * 自动化 UI 测试应受到限制，并依赖人工验证 UI。大多数集成测试都可以在 UI 层的正下方编写，以验证系统功能。
* 系统测试应该考虑很少的测试。 编写系统测试时：
  * 只应包含最关键的工作流。
  * 系统测试的目标不应详尽无遗。
* 仅在确定特定需求时添加性能指标和其他专业测试。

## 您在哪里编写新测试？

低级测试（如 Unit Tests 和 Micro-Benchmarks）应位于名为`/Tests/`的文件夹中，与正在测试的代码相邻。特定于测试的依赖项（如 mock 接口）应位于类似的位置，例如`/Mocks/`。修改现有功能的代码时，请查找已存在的测试。

更高级别的测试具有更多的依赖项，并且应该位于其依赖项集的公共目录中。这通常是一个 [O3DE 项目](/docs/user-guide/project-config/).

有关编写和注册自动化测试的更多信息，请参阅 [测试快速入门指南](/docs/user-guide/testing/getting-started/).

## 测试名称

测试应该具有唯一、明显的名称，以帮助记录它们执行的操作。一个强烈推荐的模式是`<UnitOfWork>_<StateUnderTest>_<ExpectedBehavior>` 的 [Osherove Naming](http://osherove.com/blog/2005/4/3/naming-standards-for-unit-tests.html) 结构。这映射到 “Arrange， Act， Assert” 测试结构，并使测试名称一目了然：

1. UnitOfWork：要验证的关键交互。（法案）
   * 通常为单个操作命名，通常包括目标函数的名称。
   * 易于阅读，因为它是第一位的。
2. StateUnderTest：独特的配置步骤。（编曲）
   * 描述测试与类似测试中的“正常”条件不同的原因。
     * 当看起来没有什么特别之处时，请使用占位符，例如 'Default' 或 'HappyPath'，或者省略此术语。
3. ExpectedBehavior：测试的预期结果。（断言）
   * 通常描述所做出的断言类型或突出显示多个断言值中最重要的值。
   * 易于阅读，因为它排在最后。

考虑此模式的另一种方法是`<WhatIsTested>_<NotableConfigurationStep>_<ImportantAssertion>`

使用此命名方法有两个主要原因：

1. 当测试失败时，人类可以查看报告摘要并快速了解出了什么问题。
   * 如果测试 `MatrixDotProduct_SecondMatrixInvalidRows_InvalidDimensions` 开始失败，则有人可能会在浏览代码之前立即怀疑导致问题的原因。
2. 名称记录了测试的独特重要性。
   * 这样可以更轻松地评估当前测试的内容，然后创建不重复的新测试。

{{< caution >}}
虽然上面的模式建议使用下划线，但切勿以下划线开始或结束测试名称！这可能会导致[GoogleTest 创建无效的函数名称](http://google.github.io/googletest/faq.html#why-should-test-suite-names-and-test-names-not-contain-underscore) 或 [PyTest 未发现测试](https://docs.python.org/3/tutorial/classes.html#private-variables).
{{< /caution >}}

## 测试驱动开发 （TDD）

*测试驱动开发* （TDD） 是一种软件编写工作流程，可提示工程师迭代开发代码。红绿重构流程可以将诸如“下一步应该做什么”和“这完成了吗”等问题划分为可操作的任务。这有助于避免迷失在软件设计的歧义中。TDD 还有一个额外的好处，就是留下针对关键需求的测试。重复以下三个步骤有助于设计新功能：

1. 红色：为新功能编写新的失败（单元）测试。
   * 成功是什么样的？
   * 谁将使用此代码？他们将如何使用它？
2. 绿色：编写生产代码以使测试通过。
   * 最简单的有效代码是什么？
3. 重构：改进代码和测试。
   * 是否有明显的间隙、重复或不一致？
   * 是否真的实现了正确的功能？它能被更好地证明吗？
   * 退后一步，有没有更好的方法？

{{< note >}}
TDD 是一种帮助进行软件设计的小型有效工具。它最好与其他设计工具一起使用，例如 [SOLID design principles](https://en.wikipedia.org/wiki/SOLID)。TDD 有助于关注较低级别的问题，并促使人们提出更广泛的问题。但是，它还假设高级需求收集已经为任务提供了初始方向。
{{< /note >}}

TDD 的主要好处是编写易于使用和维护的代码，并进行测试以证明它在将来能够正常运行。如果您还没有使用 TDD，请在您的下一个功能中试用它！
