---
title: "How-To-Use-The-PerformanceCollector-API-To-Instrument-Key-Performance-Metrics"
description: ""
toc: false
---

The `PerformanceCollector` API was introduced in this PR: https://github.com/o3de/o3de/pull/12551.  

The first customer of the API is the `RPISystemComponent` [RPISystemComponent Initializing The PerformanceCollector](https://github.com/o3de/o3de/blob/aa529d712e69d5b94058d2b23a00685151d64b86/Gems/Atom/RPI/Code/Source/RPI.Private/RPISystemComponent.cpp#L189).  

The Unit Test is available at: [PerformanceCollectorTests.cpp](https://github.com/o3de/o3de/blob/development/Code/Framework/AzCore/Tests/Debug/PerformanceCollectorTests.cpp)

## How To Use The PerformanceCollector API
### 1. Make an instance of the PerformanceCollector:
```cpp
#include <AzCore/Debug/PerformanceCollector.h>

class MyClass {
    AZStd::unique_ptr<AZ::Debug::PerformanceCollector> m_performanceCollector;
}
```
### 2. Define in advance the Key Performance Metrics.
In this hypothetical example we'll collect two metrics:
> 2.1. `RunAI Time`: Measures the duration of a function named `MyClass::RunAI()`.  
> 2.2. `EnemySearchUpdate Time`: Measures the time spent between calls of a function called `MyClass::OnEnemySearchUpdated()`.
```cpp
#include <AzCore/Debug/PerformanceCollector.h>

class MyClass {
    void RunAI();
    void OnEnemySearchUpdated()();

    static constexpr string_view RunAITime = "RunAI Time";
    static constexpr string_view EnemySearchUpdateTime = "EnemySearchUpdate Time";
    AZStd::unique_ptr<AZ::Debug::PerformanceCollector> m_performanceCollector;
}
```

### 3. Initialize The PerformanceCollector.
```cpp
MyClass::MyClass()
{
    auto onBatchCompleteCallback = [](AZ::u32 pendingBatches) {
        AZ_TracePrintf("AISystem", "Completed a performance batch, still %u batches are pending.\n", pendingBatches);
    };

    auto performanceMetrics = AZStd::to_array<AZStd::string_view>({
        RunAITime ,
        EnemySearchUpdateTime,
    });
    m_performanceCollector = AZStd::make_unique<AZ::Debug::PerformanceCollector>(
                "AI", performanceMetrics, onBatchCompleteCallback);
}
```
### 4. Collecting The Metrics.
```cpp
// In this example we are measuring the time spent, in microseconds, inside the RunAI()
// function.
void MyClass::RunAI()
{
    AZ::Debug::ScopeDuration duration(m_performanceCollector.get(), RunAITime );
    ... Many lines of code ...
}

// In this example we are measuring the time spent "In Between" calls of OnEnemySearchUpdated().
// In other words, this doesn't measure the duration of OnEnemySearchUpdated(), instead, it is assumed
// that this function is periodically called, and we want to measure how long it takes in between calls
// of this function.
void MyClass::OnEnemySearchUpdated()
{
    m_performanceCollector->RecordPeriodicEvent(EnemySearchUpdateTime );
}
```
### 5. The PerformanceCollector Must Be Ticked Each Frame.
```cpp
void MyClass::OnTick()
{
    m_performanceCollector->FrameTick();
}
```
### 6. About The Collected Data
The PerformanceCollector will create a JSON File, in the Google Trace format. The amount of data that will be output depends on the value of `PerformanceCollector::m_dataLogType`.

