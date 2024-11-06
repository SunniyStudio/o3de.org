---
linkTitle: Vegetation Instance System
title: Vegetation Instance System 组件
description: 管理和处理在 Open 3D Engine （O3DE） 中创建和销毁植被实例的请求。
---

Vegetation Instance System （植被实例系统） 组件管理和处理在整个环境中创建和销毁植被实例或对象的请求。

有关 Vegetation Instance System 组件如何在植被系统中工作的信息，请参阅本主题中的 [技术详情](#technical-details) 部分。

{{< note >}} 
这是一个系统组件，这意味着当您通过 Vegetation Gem 添加植被系统时，它已经存在。您可以通过 **Vegetation System Settings** 级别组件配置每个级别的植被实例属性。
{{< /note >}}

## 提供方

[Vegetation Gem](/docs/user-guide/gems/reference/environment/vegetation/)

## 源代码

[`Gems/Vegetation/Code/Source/InstanceSystemComponent.h`](https://github.com/o3de/o3de/blob/development/Gems/Vegetation/Code/Source/InstanceSystemComponent.h)


## 属性

| 属性| 说明 | 值 | 默认值 |
| --- | --- | --- | --- |
| **Max Instance Process Time Microseconds** | 用于处理实例生成和取消消失任务的每 tick 的微秒数。 | 0 - 33000 | 500 |
| **Max Instance Task Batch Size** | 可以一起批处理的单个实例生成和取消消失任务的数量。 | 1 - 2000 | 100 |

## InstanceSystemRequestBus

文件: [Gems/Vegetation/Code/Include/Vegetation/Ebuses/InstanceSystemRequestBus.h](https://github.com/o3de/o3de/blob/development/Gems/Vegetation/Code/Include/Vegetation/Ebuses/InstanceSystemRequestBus.h)

| 请求名称 |描述 |参数 |返回 |可编写脚本 |
| --- | --- | --- | --- | --- |
| `RegisterUniqueDescriptor` | 将 input 描述符与一组现有的已注册描述符中的条目进行比较。如果找到匹配的描述符，它将增加引用计数并返回指向该条目的指针。否则，通过复制 input descriptor 并返回它来添加新条目。 | `Vegetation::Descriptor&` | `Vegetation::DescriptorPtr` | No |
| `ReleaseUniqueDescriptor` | 减少唯一描述符的引用计数，如果没有更多引用，则将其删除。 | `Vegetation::DescriptorPtr` | None | No |
| `CreateInstance` | 使用植被实例数据对新实例的创建进行排队。 | `Vegetation::InstanceData&` | None | No  |
| `DestroyInstance` | 对与实例 ID 匹配的实例的销毁进行排队。 | `Vegetation::InstanceId` | None | No  |
| `DestroyAllInstances` | 对所有实例的销毁进行排队。 | None | None | No  |
| `Cleanup` | 销毁所有实例和任何其他资源。 | None | None | No  |


## 技术细节

Vegetation Instance System 组件负责管理单个植被实例的创建和销毁。
创建或删除植被实例的请求不会立即处理，因为实例和相关对象必须在主线程上进行管理。
相反，请求被排入任务批次中，这些任务在主线程中的每个时钟周期运行。
由于创建和删除多个实例的成本可能很高，并且会阻塞主线程，因此任务在每个时间片跨多个帧批量运行。
