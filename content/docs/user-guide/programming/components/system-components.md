---
linktitle: 系统组件
title: O3DE中的系统组件
description: 了解如何在 Open 3D Engine 中创建系统组件。
weight: 600
---

系统组件类似于**Open 3D Engine (O3DE)** 组件实体框架中的其他组件。不过，它们不是创建游戏实体行为，而是控制引擎本身的行为。系统组件是游戏引擎的一级元素，在初始化过程的早期就被包含在深层次中。系统组件注册到Gem的主[`Az::Module`](/docs/api/frameworks/azcore/class_a_z_1_1_module.html) 类上，在Gem加载时激活，并在Gem卸载时停用。


与任何 O3DE [组件](/docs/user-guide/programming/components/create-component/)一样，系统组件可以提供服务，也可以依赖或需要其他系统组件的服务。O3DE 提供了对引擎初始化和系统依赖顺序的精确控制。

在创建系统组件时，请遵循[组件创建的最佳实践](/docs/user-guide/programming/components/entity-system-pg-components-ebuses-best-practices)。例如，系统组件应使用：

* [EBus 事件](/docs/user-guide/programming/messaging/ebus) 暴露它们的接口。
* [反射上下文](/docs/user-guide/programming/components/reflection/) 序列化和编辑设置。
* [AZ::Component](/docs/api/frameworks/azcore/class_a_z_1_1_component.html) 类激活或禁用系统组件。

{{< important >}}
与游戏组件一样，系统组件通常也提供请求和通知总线。不过，由于系统组件是全局系统，因此它们不应像游戏组件那样为其总线指定 ID。游戏开发人员应该能够调用系统的 EBuses，而无需处理或了解包含所有系统组件的系统实体。
{{< /important >}}

以提供系统组件的 Gem 为例，O3DE 所附带的 `HttpRequestor` Gem 有一个系统组件的实现，在整个主题中都有引用。该 Gem 源位于 O3DE 源中的 `Gems/HttpRequestor/Code`。

## 在 Gem 中创建系统组件

O3DE 可以通过 Gem 和 AZ 模块创建自定义系统组件。Gem是[AZ模块](/docs/user-guide/programming/gems/overview)的特殊化。大多数 O3DE 游戏都在一个或多个 Gem 中组织游戏代码。除了用于游戏实体的运行时和编辑器组件外，这些 Gem 还可以包含与游戏引擎集成的系统组件。

创建系统组件作为 Gem 的一部分时，请遵循以下要求：

* Gem 的 `GetRequiredSystemComponents()` 函数必须返回系统组件。
* 将 `<GemName>Bus.h` 文件放在 `Code/Include/<GemName>` 目录下。
* 组件源文件放在`Code/Source`目录下。

## 使组件成为系统组件

为组件创建代码后，将其添加到项目的系统实体中，使其成为系统组件。在应用程序启动时，使用 `GetRequiredSystemComponents()` 函数将组件添加到项目的系统实体中。

以下示例来自 `HttpRequestorModule.cpp`。

   ```cpp
   #include "HttpRequestorSystemComponent.h"
   #include <AzCore/Module/Module.h>

   namespace HttpRequestor
   {
       class HttpRequestorModule
           : public AZ::Module
       {
       public:
           AZ_RTTI(HttpRequestorModule, "{FD411E40-AF83-4F6B-A5A3-F59AB71150BF}", AZ::Module);

           HttpRequestorModule()
               : AZ::Module()
           {
               // Push results of [MyComponent]::CreateDescriptor() into m_descriptors here.
               m_descriptors.insert(m_descriptors.end(), {
                   HttpRequestorSystemComponent::CreateDescriptor(),
               });
           }

           /**
            * Add required SystemComponents to the SystemEntity.
            */
           AZ::ComponentTypeList GetRequiredSystemComponents() const override
           {
               return AZ::ComponentTypeList{
                   azrtti_typeid<HttpRequestorSystemComponent>(),
               };
           }
       };
   }
   ...
   ```
