---
linkTitle: 第5章 FindComponent是什么
title: 第5章 FindComponent是什么
description: 第5章 FindComponent是什么
---
# 第5章 FindComponent是什么

## 介绍
组件之间最简单、最直接的通信形式是获取指向另一个组件的指针，然后直接调用其方法。使用指向另一个组件的原始指针，您可以获得最快的通信形式。这种方法有一些缺点，但很多时候是值得的。

它也是最容易理解的，这就是为什么我使用 FindComponent()开始讨论的原因。

{{<note>}}
您可以在 GitHub 上找到本章的代码和项目更改：
https://github.com/AMZN-Olex/O3DEBookCode2111/tree/ch05_find_component
{{</note>}}

## 示例
从 AZ::Component 派生的任何组件都可以调用以下内容：

```c++
GetEntity()->FindComponent<T>()
```

这将枚举实体的组件，直到找到给定类型 T 的组件。您可以在此处的引擎代码中找到它：

例 5.1.AzCore\Component\Entity.h 中的 FindComponent

```c++
//! Finds the first component of the requested component type.
//! @return A pointer to the first component of the requested type.
//! Returns a null pointer if a component of the type cannot be found.
template<class ComponentType>
inline ComponentType* FindComponent() const
```

在前面的章节中，我介绍了 Transform 组件。在本章中，我将向您展示如何使用 TransformComponent 的公共接口获取其指针并获取实体的世界平移（位置）。

例 5.2.在 Transform 组件上调用 GetWorldTM 的示例

```c++
AZ::Entity* e = GetEntity();
using namespace AzFramework;
TransformComponent* tc = e->FindComponent<TransformComponent>();
if (tc)
{
  AZ::Transform t = tc->GetWorldTM();
  AZ::Vector3 p = t.GetTranslation();
```

就这么简单，但这里有一些事情需要考虑。
* 我们正在引用 AzFramework 库中的类，因此我们需要更新生成脚本以为 MyProject.Static包含 AzFramework 库。

* 我们正在为项目增加链接成本，因为从现在开始构建 MyProject.Static 必须包括整个 AzFramework 静态库。随着时间的推移，这可能会增长到大量时间链接大型游戏项目。

{{<note>}}
AzFramework 库几乎无处不在，因此这不是问题，但一般思路适用。密切关注项目的链接时间。
{{</note>}}

* 我们添加了对另一个组件的依赖。这意味着我们必须考虑如果找不到组件该怎么办。如果我们在组件激活时放置此代码，例如在 MyFindComponent::Activate() 中，如果 TransformComponent 在 MyFindComponent 之后激活，会发生什么情况？该组件可能尚未准备好让我们进行通信。

例 5.3.FindComponent 示例

![](/images/learning-guide/tutorials/o3de-book/Part3/o3de_book_3_1.PNG)

{{<note>}}
Transform 组件的激活必须在激活 MyFindComponent 之前进行，我们的逻辑才能正常工作。
{{</note>}}

让我们来看看如何解决这些问题。

## 添加库引用
MyProject.Static 库需要对 AzFramework 库的引用。你可以在 C:\git\book\MyProject\Code\CMakeLists.txt 中声明它，这是我们游戏项目的构建文件。在这种情况下，我们只关心 MyProject.Static 库的定义，如下所示：

例 5.4.CMake 中 MyProject.Static 的定义

```c++
ly_add_target(
  NAME MyProject.Static STATIC
  NAMESPACE Gem
  FILES_CMAKE
    myproject_files.cmake
  INCLUDE_DIRECTORIES
    PUBLIC
      Include
  BUILD_DEPENDENCIES
    PRIVATE
      AZ::AzGameFramework
      Gem::Atom_AtomBridge.Static
)
```

MyProject.Static 包含的库列表位于 BUILD_DEPENDENCIES 下。它有两个部分：private和public。Private 部分告知构建系统仅为此构建目标包含哪些库。Public 部分指示生成系统包括生成目标以及包含 MyProject.Static 的任何目标的库。

目前，我们对这种差异不感兴趣，PRIVATE 部分将起作用。

```c++
BUILD_DEPENDENCIES
  PRIVATE
    AZ::AzFramework
```

你怎么知道要包含哪个库来访问 TransformComponent？为此，我们必须进行一些洞穴探险。我们将继续遍历源代码和构建文件，以找出 TransformComponent 所在的位置。一开始，我们可能只能通过 Editor 了解组件。

![](/images/learning-guide/tutorials/o3de-book/Part3/o3de_book_3_2.PNG)

该组件在 Entity Inspector 中称为 Transform。正如我在上一章中所展示的，组件通过其 Reflect 方法中的指定来声明其名称。如果搜索时间足够长，您可以找到以下声明：

例 5.5.AzToolsFramework\ToolsComponents\TransformComponent.cpp

```c++
TransformComponent::Reflect(...)
{
  ptrEdit->Class<TransformComponent>("Transform",
    "Controls the placement of the entity in the world in 3d")
```

{{<note>}}
在这里，我直接查看 engine 代码。您应该克隆引擎的副本（如果不为其他原因），则以供参考。
{{</note>}}

```c++
cd c:\git
git clone https://github.com/o3de/o3de
```

然后上述文件将位于 C:\git\o3de\Code\Framework\AzToolsFramework\AzToolsFramework\ToolsComponents\TransformComponent.cpp。

此反射属于 AzToolsFramework::Components::TransformComponent。它是组件的编辑器风格，而 AzFramework::TransformComponent 是游戏运行时将在运行时交互的游戏组件。

```c++
namespace AzToolsFramework
{
  namespace Components
  {
    class TransformComponent
      : public EditorComponentBase
```

现在，只需知道，如果您正在调查的组件继承自 EditorComponentBase，那么它本质上是临时的编辑器组件，仅在设计时存在。在游戏时生成关卡时，这些编辑器组件将转换为游戏组件。

{{<tip>}}
实际上，您可以通过查看 BuildGameEntity 方法来了解创建的游戏组件
的 Transform 组件。
{{</tip>}}

```c++
void TransformComponent::BuildGameEntity(AZ::Entity* gameEntity)
{
  ...
  gameEntity->CreateComponent<AzFramework::TransformComponent>()
}
```

现在我们知道了在运行时将与哪个组件交互，我们可以找到包含它的库。对 TransformComponent.h 的引用位于 C:\git\o3de\Code\Framework\AzFramework\AzFramework\azframework_files.cmake 中。那么，这个文件列表是什么呢？AzFramework 静态库的生成脚本可以。

例 5.6.C:\git\o3de\Code\Framework\AzFramework\CMakeLists.txt

```c++
ly_add_target(
   NAME AzFramework STATIC
   NAMESPACE AZ
   FILES_CMAKE
    AzFramework/azframework_files.cmake
  ...
)
```

现在，我们可以使用其名称和命名空间 AZ::AzFramework 来链接此静态库。

例 5.7.针对 AZ::AzFramework 库的 MyProject.Static 链接

```c++
ly_add_target(
  NAME MyProject.Static STATIC
    NAMESPACE Gem
  ...
  BUILD_DEPENDENCIES
    PRIVATE
      AZ::AzFramework
  ...
)
```

## 声明组件之间的依赖关系
我们应该解决的下一个问题是摆脱由于组件的激活顺序而无法获得我们想要的组件的情况。

```c++
TransformComponent* tc = e->FindComponent<TransformComponent>();
```

我们可以确保找到我们正在寻找的组件，并在 TransformComponent 上声明我们的组件的游戏设计依赖项。组件可以声明它提供的服务和它需要的服务。例如，Transform 组件的编辑器版本声明了以下服务：

```c++
void TransformComponent::GetProvidedServices(AZ::ComponentDescriptor::DependencyArrayType& provided)
{
  provided.push_back(AZ_CRC("TransformService", 0x8ee22c50));
}
```

然后，我们的示例组件 MyFindComponent 将在静态 GetRequiredServices 方法中声明依赖项：

```c++
static void GetRequiredServices(AZ::ComponentDescriptor::DependencyArrayType& required)
{
  required.push_back(AZ_CRC_CE("TransformService"));
}
```

{{<note>}}
AZ_CRC 宏和 AZ_CRC_CE 宏都提供相同的功能。它们将字符串转换为经过哈希处理的整数。唯一的区别是 AZ_CRC_CE 在编译时计算该值。
{{</note>}}

当您在 Editor 中构建实体时，组件的保存方式是 TransformComponent 始终位于 MyFindComponent 之前。

{{<note>}}
这些依赖项和 Editor 存在一个问题，当您为实体上已有的组件添加或更改依赖项规则时，必须修改实体的组件才能重新应用规则。
{{</note>}}

## 小结

{{<note>}}
您可以在 GitHub 上找到本章的代码和项目更改：
https://github.com/AMZN-Olex/O3DEBookCode2111/tree/ch05_find_component
{{</note>}}

这是我们新组件的完整源代码。

例 5.8.MyFindComponent.h

```c++
#pragma once
#include <AzCore/Component/Component.h>
namespace MyProject
{
  // An example of the simplest O3DE component
  class MyFindComponent : public AZ::Component
  {
  public:
    AZ_COMPONENT(MyFindComponent, "{FE4F2E82-8E03-48EC-A967-559705597040}");
    static void GetRequiredServices(AZ::ComponentDescriptor::DependencyArrayType& required)
    {
      required.push_back(AZ_CRC_CE("TransformService"));
    }
    // AZ::Component overrides
    void Activate() override;
    void Deactivate() override {}
    // Provide runtime reflection, if any
    static void Reflect(AZ::ReflectContext* rc);
  };
}
```

例 5.9. MyFindComponent.cpp
```c++
#include "MyFindComponent.h"
#include <AzCore/Component/Entity.h>
#include <AzCore/Serialization/EditContext.h>
#include <AzFramework/Components/TransformComponent.h>
using namespace MyProject;
void MyFindComponent::Activate()
{
  AZ::Entity* e = GetEntity();
  using namespace AzFramework;
  TransformComponent* tc = e->FindComponent<TransformComponent>();
  if (tc)
  {
    AZ::Transform t = tc->GetWorldTM();
    AZ::Vector3 p = t.GetTranslation();
    AZ_Printf("MyFind", "activated at %f %f %f",
      p.GetX(), p.GetY(), p.GetZ() );
   }
}
void MyFindComponent::Reflect(AZ::ReflectContext* rc)
{
   auto sc = azrtti_cast<AZ::SerializeContext*>(rc);
   if (!sc) return;
   sc->Class<MyFindComponent, Component>()
    ->Version(1);
   AZ::EditContext* ec = sc->GetEditContext();
   if (!ec) return;
   using namespace AZ::Edit::Attributes;
   // reflection of this component for O3DE Editor
   ec->Class<MyFindComponent>("Find Component Example", "[Communicates using FindComponent]")
     ->ClassElement(AZ::Edit::ClassElements::EditorData, "")
     ->Attribute(AppearsInAddComponentMenu, AZ_CRC("Game"))
     ->Attribute(Category, "My Project");
}
```
