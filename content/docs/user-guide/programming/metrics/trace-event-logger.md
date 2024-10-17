---
linktitle: Google跟踪事件格式 
title: Google 跟踪事件格式指标
description: 介绍如何使用 Open 3D Engine (O3DE) 中的事件记录器 API 将指标创建、注册和记录为 JSON 格式，并可在 chromium about:tracing 窗口中查看。
weight: 100
---


## 学习 JSON 跟踪事件记录器

**Open 3D Engine (O3DE)** 中的跟踪事件记录器能够记录与谷歌[跟踪事件格式](https://docs.google.com/document/d/1CvAClvFfyA5R-PhYUmn5OOQtYMH4h6I0nSsKchNAySU/preview#)兼容的指标。
该实现是 Google 跟踪格式的一个有限子集，只实现了与 O3DE 当时需要相关的事件类型。

O3DE 目前支持以下事件集：

|事件|描述|阶段标记|
|---|---|---|
|Duration|提供了一种测量单个线程内持续时间的方法。 持续时间事件可以嵌套|B (begin), E(end)|
|Complete|将Duration 事件的开始和结束合并为一个事件。|X|
|Instant|与发生在某个时间点的事件相对应。它没有相关的持续时间。例如，内存不足事件可记录为 “即时 ”事件。"s" 作用域参数可提供事件的作用域。有效的作用域包括 “g”（全局）、“p”（进程）和 “t”（线程）。如果没有提供范围，则假定为线程范围事件。|i|
|Counter|提供了一种随时间跟踪多个值的方法。如果提供了 “id”，它就会与 “name ”字段组合成计数器名称。|C|
|Async|提供了一种跨多个线程测量异步操作的方法。 该事件可用于输出帧时间或网络 I/O 统计数据。 具有相同 “cat”（类别）和 “id ”字段值的事件被视为来自同一事件树。 "scope"参数是一个可选字符串，用于避免 id 冲突。指定后，“cat”、“id ”和 “scope ”将用于识别同一事件树中的事件。 |b (nestable start), n (nestable instant), e (nestable end)|


O3DE 中的 JSON 跟踪事件记录器目前不提供以下事件集：
|事件|描述|阶段标记|
|---|---|---|
|Flow|与异步事件类似，只是每个线程可以有一个与之相关的持续时间。|s (start), t (step), f (end)|
|Object|提供了一种在事件中构建复杂结构的方法。 通常用于跟踪对象实例在内存中创建（“N”）和销毁（“D”）的时间。 O “对象可用于将 ”快照 "数据与对象关联起来，快照数据可以是任何可存储在 JSON 对象中的数据。|N (created), O (snapshot), D (destroyed)|
|Metadata|允许将自定义数据与支持的字段之一（“process_name”、“process_labels”、“process_sort_index”、“thread_name”、“thread_sort_index”）关联。args "参数用于指定元数据。|M|
|Memory Dump|相当于全局操作系统内存或运行中的应用程序进程内存的内存转储。 "V" 阶段用于全局内存信息，如系统内存。"v" 阶段用于进程统计。|V (global), v (process)|
|Mark|(O3DE 不需要）在使用网络导航定时 API 时用于记录事件。|R|
|Clock Sync|(O3DE 不需要）用于在多个事件日志之间执行同步时钟同步。|c|
|Context|用于标记属于上下文的一系列事件。|,|

有关这些事件的更多信息，请参阅 Google 跟踪事件格式文档的 [事件描述](https://docs.google.com/document/d/1CvAClvFfyA5R-PhYUmn5OOQtYMH4h6I0nSsKchNAySU/preview#heading=h.uxpopqvbjezh)部分。

## 事件记录器 API 文档

事件记录器 API 可用接口的详细 doxygen 注释位于[Code/Framework/AzCore/AzCore/Metrics](https://github.com/o3de/o3de/tree/development/Code/Framework/AzCore/AzCore/Metrics)目录下的头文件 API 文件中。

在该目录中，要了解 API，需要检查以下最重要的文件：

|文件|说明|
|---|---|
|[IEventLogger.h](https://github.com/o3de/o3de/blob/development/Code/Framework/AzCore/AzCore/Metrics/IEventLogger.h)|提供了使用事件描述记录事件数据的接口。 本文件还描述了特定事件阶段类型的结构定义。|
|[IEventLoggerFactory.h](https://github.com/o3de/o3de/blob/development/Code/Framework/AzCore/AzCore/Metrics/IEventLoggerFactory.h)|为使用全局注册器注册事件记录器提供接口。这可用于跨共享库边界访问事件记录器，或访问事件记录器而无需显式存储。|
|[EventLoggerUtils.h](https://github.com/o3de/o3de/blob/development/Code/Framework/AzCore/AzCore/Metrics/EventLoggerUtils.h)|包含用于简化使用事件记录器 API 记录事件的实用功能。 这些函数既能从事件记录器工厂查询事件记录器，也能在一次调用中记录事件。 该文件还提供简化的 `AZ::Metrics::RecordEvent` 应用程序，可接受任何类型的受支持事件阶段类型。|

## 如何使用 JSON 跟踪事件记录器

本节将介绍如何注册创建事件工厂并将其注册到全局接口、创建事件记录器、提供事件记录器流以记录数据输出以及记录示例指标。最后，您将能在基于 Chromium 的浏览器中查看收集到的指标。

本节的完整源代码可在 [JsonTraceEventLogger 单元测试](https://github.com/o3de/o3de/blob/development/Code/Framework/AzCore/Tests/Metrics/JsonTraceEventLoggerTests.cpp)和 [EventLoggerUtils 单元测试](https://github.com/o3de/o3de/blob/development/Code/Framework/AzCore/Tests/Metrics/EventLoggerUtilsTests.cpp)中找到。

### 创建 EventLogger Factory 并将其注册为全局实例
```c++
//! The logger is created from a compile time hash of the "SampleEventLogger" string using the FNV-1a 64 bit algorithm
constexpr AZ::Metrics::EventLoggerId SampleLoggerId{ static_cast<AZ::u32>(AZStd::hash<AZStd::string_view>{}("SampleEventLogger")) };

//! This is a google test fixture class used for illustration purposes of recording using the Event Logger API
class JsonTraceEventLoggerTest
    : public ::UnitTest::ScopedAllocatorSetupFixture
{
public:
    JsonTraceEventLoggerTest()
    {
        // Create an event logger factory
        m_eventLoggerFactory = AZStd::make_unique<AZ::Metrics::EventLoggerFactoryImpl>();

        // Register it as a global instance
        AZ::Metrics::EventLoggerFactory::Register(m_eventLoggerFactory.get());

        //...
    }

protected:
    AZStd::string m_metricsOutput;
    AZStd::unique_ptr<AZ::Metrics::IEventLoggerFactory> m_eventLoggerFactory;
};
```

### 创建写入内存字符串的 JSON 跟踪事件记录器，并向事件记录器工厂注册
```c++
    JsonTraceEventLoggerTest()
    {
        //... Previous logic to register event factory

        // Create an byte container stream that allows event logger output to be logged in-memory
        auto metricsStream = AZStd::make_unique<AZ::IO::ByteContainerStream<AZStd::string>>(&m_metricsOutput);
        // Create the trace event logger that logs to the Google Trace Event format
        auto eventLogger = AZStd::make_unique<AZ::Metrics::JsonTraceEventLogger>(AZStd::move(metricsStream));

        // Register the Google Trace Event Logger with the Event Logger Factory
        auto registerOutcome = m_eventLoggerFactory->RegisterEventLogger(SampleLoggerId, AZStd::move(eventLogger));
        // Validate that the registration of the factory succeeded
        EXPECT_TRUE(registerOutcome);
    }
```

### 生成持续时间跟踪指标并将其记录到事件记录器中

以下代码块展示了如何使用事件记录器记录字符串和数字：

```c++
{
    constexpr AZStd::string_view eventString = "Hello world";
    constexpr AZ::s64 eventInt64 = -2;


    // Populate the "args" container to associate with each events "args" field
    AZ::Metrics::EventObjectStorage argsContainer;
    argsContainer.emplace_back("string", eventString);
    argsContainer.emplace_back("int64_t", eventInt64);

    {
        // Record Duration Begin and End Events
        AZ::Metrics::DurationArgs durationArgs;
        durationArgs.m_name = "Duration Event";
        durationArgs.m_cat = "Test";
        durationArgs.m_args = argsContainer;
        durationArgs.m_id = "0";
        auto resultOutcome = AZ::Metrics::RecordEvent(SampleLoggerId, AZ::Metrics::EventPhase::DurationBegin, durationArgs);
        EXPECT_TRUE(resultOutcome);

        // Update the string value to "GoodbyeWorld and the integer value to "400"
        argsContainer[0].m_value = "Goodbye world";
        argsContainer[1]= 400;

        // Re-update the durationArgs argument container to have the latetst argument values
        durationArgs.m_args = argsContainer;

        resultOutcome = AZ::Metrics::RecordEvent(SampleLoggerId, AZ::Metrics::EventPhase::DurationEnd, durationArgs);
        EXPECT_TRUE(resultOutcome);
    }
}
```

### 将指标字符串刷新到文本文件中

此时，度量值可记录到 `stdout`、通过网络发送或写入磁盘上的 JSON 文件。
示例将把度量值写入一个文件，文件名前缀为 “sample_metrics_”，当前时间格式采用[ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) 标准。文件名示例为 `sample_metrics_2023-01-01T120000.123456.json`。

```c++
{
    constexpr AZ::IO::OpenMode openMode = AZ::IO::OpenMode::ModeWrite;
    auto metricsFilepath = AZ::IO::FixedMaxPath(AZ::Utils::GetProjectUserPath()) / "metrics/sample_metrics_";
    //! Append a ISO 8601 timestamp to the metrics filename to make it unique across runs
    AZ::Date::Iso8601TimestampString currentUtcTimestamp;
    AZ::Date::GetFilenameCompatibleFormatNowWithMicroseconds(currentUtcTimestamp)
    metricsFilepath.Native() += currentUtcTimestamp;
    metricsFilepath.Native() += ".json";

    AZ::IO::SystemFileStream systemFileStream(metricsFilepath.c_str(), openMode);
    // The metrics were written to the ByteContainerStream which was backed by the @m_metricsOutput member
    systemFileStream.Write(m_metricsOutput.size(), m_metricsOutput.c_str());
}
```

### 事件指标 JSON 输出

下面的示例显示了使用多线程记录指标的情况，以说明线程 ID 与每个指标相关联。  
```json
[
  {"name":"Duration Event","id":"0","cat":"Test","ph":"B","ts":1664329933375019,"pid":31760,"tid":36036,"args":{"string":"Hello world","int64_t":-2}},
  {"name":"Duration Event","id":"0","cat":"Test","ph":"E","ts":1664329933375119,"pid":31760,"tid":36036,"args":{"string":"Goodbye world","int64_t":400}}
]
```


### 查看事件指标
使用基于 Chromium 的浏览器 `about:tracing` 页面或[catapult repo](https://google.github.io/trace-viewer/)提供的跟踪查看器，可以根据事件类型可视化记录的指标。

![about::tracing](/images/user-guide/metrics/about-tracing.png)
