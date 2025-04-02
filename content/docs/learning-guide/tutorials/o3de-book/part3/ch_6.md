---
linkTitle: 第6章 AZ::Interface是什么
title: 第6章 AZ::Interface是什么
description: 第6章 AZ::Interface是什么
---
# 第6章 AZ::Interface是什么

## 介绍
O3DE 中的大多数组件通信都使用 AZ::EBus 进行。它是引擎中最强大、最通用、功能最丰富的组件间通信系统。我甚至可以说，一旦你理解了 AZ::EBus，你就会明白如何使用这个引擎。但是，我将首先向您展示更简单的模型 - 本章中的 AZ::Interface，然后是下一章中的 AZ::Event。通过首先学习这两个，您将准备好了解 AZ::EBus。

{{<note>}}
您可以在 GitHub 上找到本章的代码和项目更改：
https://github.com/AMZN-Olex/O3DEBookCode2111/tree/ch06_az_interface
{{</note>}}

O3DE 中的 AZ::Interaface 是一个跨模块边界工作的花哨的单例。它允许您声明一个接口，然后在您的一个对象中实现它。但是，由于 AZ::Interface 本质上是一个单例，因此充当接口处理程序的组件必须是处理该接口的唯一组件。

{{<note>}}
您可以在 AZ::Inteface 在线找到文档：
https://www.o3de.org/docs/user-guide/programming/az-interface/
{{</note>}}

## Level 组件
这为我们带来了一个自然唯一的组件类型，它将作为处理 AZ::Interface： Level 组件的绝佳候选者。我们已经看到了可以添加到 Editor 中的实体的常规游戏组件。在 Entity Outliner 中，还有另一种类型的实体隐藏在显眼的位置。

图 6.1.访问 Level 实体及其组件

![](/images/learning-guide/tutorials/o3de-book/Part3/o3de_book_3_3.PNG)

如果单击 Level 标题，Entity Inspector 将显示 Level 实体，默认情况下，该实体上没有组件。

图 6.2.Entity Inspector 中的 Level 实体

![](/images/learning-guide/tutorials/o3de-book/Part3/o3de_book_3_5.PNG)

只有在其 Reflect 方法中标记为 Level 组件的组件才能添加到 Level 实体中。

```c++
ec->Class<MyLevelComponent>("My Level Component", "[Communicates using AZ::Interface]")
  ->ClassElement(AZ::Edit::ClassElements::EditorData, "")
  ->Attribute(AppearsInAddComponentMenu, AZ_CRC_CE("Level"))
```

例 6.1.具有 My Level Component 的 Level 实体

![](/images/learning-guide/tutorials/o3de-book/Part3/o3de_book_3_4.PNG)

{{<tip>}}
每当您发现 Editor 或 Asset Processor 在刚刚添加新组件后无法启动时，请检查您的新组件是否具有唯一的 guid。复制和粘贴新组件文件很容易，而忘记更新 GUID。在这种情况下，Visual Studio 中的调试日志将显示以下错误：
```
Component: Two different components have the same UUID
({FE4F2E82-8E03-48EC-A967-559705597040}), which is not allowed.
Change the UUID on one of them.
```
{{</tip>}}

## 定义 AZ::Interface
为了定义和使用您自己的 AZ::Interface，涉及三个步骤：
* 将纯虚拟类定义为公共头文件（如 MyInterface） 中的接口。
* 使用 AZ::Interface::Registrar 为接口分配处理程序<MyInterface>
* 使用 AZ::Interface<MyInterface>::Get()从另一个组件访问接口

图 6.3.用 AZ::Interface

![](/images/learning-guide/tutorials/o3de-book/Part3/o3de_book_3_6.PNG)

下面是接口定义。

例 6.2. C:\git\book\MyProject\Code\Include\MyProject\MyInterface.h

```c++
#pragma once
#include <AzCore/RTTI/RTTI.h>
namespace MyProject
{
  class MyInterface
  {
  public:
    AZ_RTTI(MyInterface, "{D477F807-6795-4A1B-A475-AFBCF0584A08}");
    virtual ~MyInterface() = default;
    virtual int GetMyInteger() = 0;
  };
}
```
  
下面是一个组件示例，该组件充当接口上放置的任何请求的处理程序

例 6.3. MyLevelComponent使用AZ::Interface

```c++
// An example of a level component with AZ::Interface
class MyLevelComponent
  : public AZ::Component
  , public AZ::Interface<MyInterface>::Registrar
```

AZ::Interface<MyInterface>::Registrar 将在构造 MyLevelComponent 时将类注册为接口的处理程序，并在销毁时注销。

{{<tip>}}
您还可以使用 Register 和 Unregister 方法从界面手动注册和取消注册。

```c++
AZ::Interface<MyInterface>::Register(this);
// ...
AZ::Interface<MyInterface>::Unregister(this);
```
{{</tip>}}

下面是使用该接口的组件的示例。

例 6.4.在另一个组件中使用 AZ::Interface

```c++
void MyInterfaceComponent::Activate()
{
   if (MyInterface* myInterface = AZ::Interface<MyInterface>::Get())
   {
      AZ_Printf("Example", "%d", myInterface->GetMyInteger());
   }
}
```

## 小结
在本章中，我介绍了如何使用 AZ::Interface 作为跨模块边界工作的单例。 创建了两个组件。MyLevelComponent 实现了 MyInterface 并被分配给了关卡实体。MyInterfaceComponent 访问了 MyInterface。我们有两个组件与每个组件进行通信，尽管它们被分配到完全不同类型的实体上。

这是 MyLevelComponent 的源代码。

例 6.5. MyLevelComponent.h

```c++
#pragma once
#include <AzCore/Component/Component.h>
#include <AzCore/Interface/Interface.h>
#include <MyProject/MyInterface.h>
namespace MyProject
{
 // An example of a level component with AZ::Interface
 class MyLevelComponent
   : public AZ::Component
   , public AZ::Interface<MyInterface>::Registrar
 {
 public:
   AZ_COMPONENT(MyLevelComponent, "{69C64F9C-4C35-4612-9B3B-85FEBCC06FDB}");
   
   // AZ::Component overrides
   void Activate() override {}
   void Deactivate() override {}
   // Provide runtime reflection, if any
   static void Reflect(AZ::ReflectContext* rc);
   // MyInterface
   int GetMyInteger() override { return 42; }
 };
}
```

例 6.6. MyLevelComponent.cpp
```c++
#include "MyLevelComponent.h"
#include <AzCore/Serialization/EditContext.h>
using namespace MyProject;
void MyLevelComponent::Reflect(AZ::ReflectContext* rc)
{
  auto sc = azrtti_cast<AZ::SerializeContext*>(rc);
  if (!sc) return;
  sc->Class<MyLevelComponent, Component>()
    ->Version(1);
  AZ::EditContext* ec = sc->GetEditContext();
  if (!ec) return;
  using namespace AZ::Edit::Attributes;
  // reflection of this component for O3DE Editor
  ec->Class<MyLevelComponent>("My Level Component", "[Communicates using AZ::Interface]")
    ->ClassElement(AZ::Edit::ClassElements::EditorData, "")
    ->Attribute(AppearsInAddComponentMenu, AZ_CRC_CE("Level"))
    ->Attribute(Category, "My Project");
}
```

这是 MyInterfaceComponent 的源代码

例 6.7. MyInterfaceComponent.h

```c++
#pragma once
#include <AzCore/Component/Component.h>
namespace MyProject
{
 // An example of using AZ::Interface
 class MyInterfaceComponent : public AZ::Component
 {
 public:
   AZ_COMPONENT(MyInterfaceComponent, "{F73AB7B7-4F19-42FD-9651-13167FD222A6}");
   
   // AZ::Component overrides
   void Activate() override;
   void Deactivate() override {}
   // Provide runtime reflection, if any
   static void Reflect(AZ::ReflectContext* rc);
 };
}
```

例 6.8. MyInterfaceComponent.cpp
```c++
#include "MyInterfaceComponent.h"
#include <AzCore/Serialization/EditContext.h>
#include <MyProject/MyInterface.h>
using namespace MyProject;
void MyInterfaceComponent::Activate()
{
  if (MyInterface* myInterface = AZ::Interface<MyInterface>::Get())
  {
    AZ_Printf("Example", "%d", myInterface->GetMyInteger());
  }
}
void MyInterfaceComponent::Reflect(AZ::ReflectContext* rc)
{
  auto sc = azrtti_cast<AZ::SerializeContext*>(rc);
  if (!sc) return;
  sc->Class<MyInterfaceComponent, Component>()
    ->Version(1);
  AZ::EditContext* ec = sc->GetEditContext();
  if (!ec) return;
  using namespace AZ::Edit::Attributes;
  // reflection of this component for O3DE Editor
  ec->Class<MyInterfaceComponent>("Using Interface Example", "[Communicates using AZ::Interface]")
    ->ClassElement(AZ::Edit::ClassElements::EditorData, "")
    ->Attribute(AppearsInAddComponentMenu, AZ_CRC("Game"))
    ->Attribute(Category, "My Project");
}
```
