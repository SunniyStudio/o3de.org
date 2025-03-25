---
linkTitle: 第4章 编写自己的组件
title: 第4章 编写自己的组件
description: 第4章 编写自己的组件
---
# 第4章 编写自己的组件

官方参考如何创建 O3DE 游戏项目：
https://www.o3de.org/docs/user-guide/project-config/project-manage

## Introduction to Game Project Files
We created a new project in Chapter 2, Creating a New Project. In this chapter we are going to take a look at the files involves and add a new component to the project.

{{<note>}}
In later chapters, we will re-visit the structure of a game project in O3DE once we go over more concepts, such as EBus, gems, prefabs, etc. For now, I am going to cover just the basics to get us started writing new components.
{{</note>}}

This chapter covers the following topics:
* Basic project structure.
* Adding a new C++ component.
* Finding the new C++ component in the Editor

## Basic Structure of a New Project

{{<note>}}
The source code for this project can be found on GitHub at:
https://github.com/AMZN-Olex/O3DEBookCode2111/tree/ch04_creating_components
{{</note>}}

Previously we created a new project and placed it at C:\git\book\MyProject. We generated the build folder at C:\git\book\build that contains the Visual Studio solution MyProject.sln.

There are a lot of projects and sub-folders in here but we are going to focus on the essentials.
* ZERO_CHECK is a special CMake target that rebuilds Visual Studio from any changes to CMake build files. When we add a new component we will need to build this project and re-load the solution with the updates.
* Editor is the target that runs O3DE Editor with our project. It does not actually build any of the Editor
code. The Editor binary is provided by the O3DE installation in C:\O3DE\21.11.2.
* MyProject.Static is the static library for adding new components.

Figure 4.1. MyProject solution in Visual Studio

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_23.PNG)

MyProject is the dynamic library that links MyProject.Static and provides the component information to the Editor and game launcher. In other words, MyProject.Static defines the components and MyProject registers them for your project.

At the moment the only component we have in the project is the default system component.
* C:\git\book\MyProject\Code\Source\MyProjectSystemComponent.h
* C:\git\book\MyProject\Code\Source\MyProjectSystemComponent.cpp

What are the elements of MyProject.Static library?
* Include/MyProject contains various public interfaces. I will cover various options available in O3DE in later chapters when we tackle communication between components, starting with Chapter 5, What is FindComponent?
* Source contains source and header files of the components and any other classes and objects you might write.
* CMakeLists.txt is the build script where MyProject.Static is defined.
* enabled_gems.cmake deals with the gem system that we will tackle in Chapter 12, What is a Gem?
* myproject_files.cmake defines the list of source and other files to be included in the build and  in Visual Studio solution

Figure 4.2. MyProject.Static library

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_24.PNG)

Generally, any new component you would write would be placed under MyProject\Code\Source,  either directly in Source folder or under a sub-folder of your choice.

If you were to search for references to MyProjectSystemComponent under C:\git\book \MyProject, you would find two more files where it is referenced.

### myproject_files.cmake
*_files.cmake are O3DE project files that list the files to be included and compiled for a project.

Example 4.1. MyProject\Code\myproject_files.cmake
```shell
set(FILES
  Include/MyProject/MyProjectBus.h
  Source/MyProjectSystemComponent.cpp
  Source/MyProjectSystemComponent.h
  enabled_gems.cmake
)
```

{{<tip>}}
You can also include files other than the source code, such as various configuration files, shaders files, if you wish to have easier access them inside Visual Studio. The build system will exclude them from compilation.
{{</tip>}}

You can see references to source and header files of MyProjectSystemComponent. File paths must be local to the project's Code folder: C:\git\book\MyProject\Code.

{{<note>}}
In order to add new files to the build, you must reference them in this file.
{{</note>}}

{{<note>}}
O3DE uses CMake as its build system. This chapter will only cover the essentials of CMake in order to add a new component. Each following chapter will introduce just enough of CMake knowledge to accomplish each task. As you progress through the chapters you will acquire all the necessary basics to feel comfortable with O3DE's use of CMake.
{{</note>}}

### MyProjectModule.cpp
And the other reference to MyProjectSystemComponent is in the module file. A module source file is the root file of a project that declares the project and, most importantly for this chapter, registers all the components.
```c++
MyProjectModule()
  : AZ::Module()
{
  // Push results of [MyComponent]::CreateDescriptor()
  // into m_descriptors here.
  m_descriptors.insert(m_descriptors.end(), {
    MyProjectSystemComponent::CreateDescriptor(),
  });
}
```
Any components added to m_descriptors in the code snippet above would be available for use in your project and, if properly configured, in the Editor. Here is the entire module file for reference.

Example 4.2. A default module file for a new project, MyProjectModule.cpp

```c++
#include <AzCore/Memory/SystemAllocator.h>
#include <AzCore/Module/Module.h>
#include "MyProjectSystemComponent.h"
namespace MyProject
{
  class MyProjectModule
    : public AZ::Module
  {
  public:
    AZ_RTTI(MyProjectModule,
    "{4b67609b-b22c-4a71-9afe-3f9d10bcf5ac}", AZ::Module);
    AZ_CLASS_ALLOCATOR(MyProjectModule, AZ::SystemAllocator, 0);
    MyProjectModule()
      : AZ::Module()
    {
      m_descriptors.insert(m_descriptors.end(), {
        MyProjectSystemComponent::CreateDescriptor(),
      });
    }
    /**
    * Add required SystemComponents to the SystemEntity.
    */
     AZ::ComponentTypeList
     GetRequiredSystemComponents() const override
     {
       return AZ::ComponentTypeList{
        azrtti_typeid<MyProjectSystemComponent>(),
       };
     }
   };
}// namespace MyProject
AZ_DECLARE_MODULE_CLASS(Gem_MyProject, MyProject::MyProjectModule)
```

{{<note>}}
Note that GetRequiredSystemComponents() is a special method to register components that are marked as system components. System components are placed on a special system entity that is activated as soon as the engine starts and persists outside of game levels until shutdown.
{{</note>}}

## Creating Your Own Component

{{<note>}}
The official reference for creating a C++ components can be found at:
https://www.o3de.org/docs/user-guide/programming/components/create-component/
{{</note>}}

You cannot create your own custom AZ::Entity in O3DE but you can create your own custom component derived from AZ::Component. This chapter will show how to create a simple component that shows up in the Editor. We will create a dummy component called, MyComponent. In general, whenever you add a new component to a project, you have to do the following steps:
* Create its header file MyComponent.h at MyProject\Gem\Code\Source.
* Create its source file MyComponent.cpp in the same folder.
* Update myproject_files.cmake with references to the above two new files.
* Register MyComponent in MyProjectModule.cpp in MyProjectModule's constructor.
* Build the project.

I will now go through each step.

### MyComponent.h
The most basic and simplest component that does nothing aside from existing is as follows:

Example 4.3. The simplest component

```c++
#pragma once
#include <AzCore/Component/Component.h>

namespace MyProject
{
   // An example of the simplest O3DE component
   class MyComponent : public AZ::Component
   {
   public:
     AZ_COMPONENT(MyComponent,
      "{4b589f6b-79f3-47b6-b730-aad0871d5f8f}");
     // AZ::Component overrides
     void Activate() override {}
     void Deactivate() override {}
     // Provide runtime reflection, if any
     static void Reflect(AZ::ReflectContext* reflection);
   };
}
```

MyComponent must derive from AZ::Component. All O3DE game components must be derived from AZ::Component either directly or through another class that derives from it.

Macro AZ_COMPONENT. This macro marks the component type with a unique guid string. The first parameter is the class name. The second parameter is a unique identifier for this class in a guid form. You are responsible for creating a unique guid with curly brackets around it.

{{<note>}}
Visual Studio IDE has a built-in guid generator in the menu: Tools → Create GUID. You can also generate a guid using Visual Code plugins or even a PowerShell command:

```shell
PS C:\work\book\MyProject> (New-Guid).ToString("B")
{dcbbe9e2-5630-4e4b-ad7a-308c7abe5abf}
```
{{</note>}}

AZ::Component interface implementation. Activate() and Deactivate() are pure virtual methods from AZ::Component. They do not have to perform any work but they must be overridden. AZ::Component is defined in C:\O3DE\21.11.2\Code\Framework\AzCore\AzCore\Component\Component.h. We will discuss these methods later, for now they are not necessary to have MyComponent show up in the Editor.

Reflect() method: provides a runtime serialization of this component to the O3DE engine. In the next section, I will show the minimum work involved in reflecting a component to the Editor.

### MyComponent.cpp
This is where the description and the behavior of the component will be. However, for now we are going to only provide a simple stub.

Example 4.4. The simplest MyComponent.cpp

```c++
#include "MyComponent.h"
#include <AzCore/Serialization/EditContext.h>
using namespace MyProject;

void MyComponent::Reflect(AZ::ReflectContext* reflection)
{
  AZ_UNUSED(reflection); // TODO provide reflection
}
```

{{<tip>}}
AZ_UNUSED is macro that does nothing but pretends to use a parameter in order to silence some compiler warnings. In modern C++ you can also use [[maybe_unused]] on the parameter declaration.
{{</tip>}}

This is enough to compile the project. Once we have everything in place, we will return to Reflect method. It will be useful to try different implementation of Reflect and see the consequences. So let us finish the rest of the C++ code changes that we need.

### myproject_files.cmake
The file list for the project is located at C:\git\book\MyProject\Code\myproject_files.cmake. Since we only added one component, the following changes are enough:

```shell
set(FILES
  Include/MyProject/MyProjectBus.h
  Source/MyProjectSystemComponent.cpp
  Source/MyProjectSystemComponent.h
  enabled_gems.cmake
  Source/MyComponent.cpp # new
  Source/MyComponent.h # new
)
```

### MyProjectModule.cpp
And lastly, the project needs to register the new component by adding its descriptor to the project module.

```c++
...
#include "MyComponent.h"
...
MyProjectModule()
  : AZ::Module()
{
  m_descriptors.insert(m_descriptors.end(), {
    ...
    MyComponent::CreateDescriptor(),
  });
}
```

You can find this module file under MyProject build target in Visual Studio.

Figure 4.3. MyProjectModule.cpp in Visual Studio

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_25.PNG)


CreateDescriptor is a method of AZ::Component. For simple tasks, you do not need to worry
about what it does at this point. Just be aware that it provides a description of the component to the engine.

### Re-compile the Project
Now that we have gone over the basic C++ changes for a new empty component, let us discuss the workflow involved in updating the project.

{{<tip>}}
Close the Editor and the Asset Processor if they are running. Otherwise, the Asset Processor or the Editor might hold a lock on one of your project binaries. In such a case you will get a build error:
```shell
9>LINK : fatal error LNK1104: cannot open file
'C:\git\book\build\bin\profile\MyProject.dll'
```
{{</tip>}}

We have two ways of compiling the project on Windows: either Visual Studio solution or through the CMake command line interface.

### Building from Command Line
In some ways, this is the easiest method to compile the project. The benefit of building from a command line is that it allows you to use any editor you wish. Execute:

Example 4.5. Building the project from command line

```shell
cmake --build C:\git\book\build\ --config profile
```

{{<note>}}
"--build" is a switch that tells CMake where the binary folder is for our project. Early on we created it at C:\git\book\build. --config profile tells CMake to build profile flavor of our project.
{{</note>}}

{{<tip>}}
If you ever need to debug your components, instead of compiling the project in debug build, you can add the following lines instead and still use profile binaries.
{{</tip>}}

```cpp
#pragma optimize( "", off )
/* unoptimized code section */
#pragma optimize( "", on )
```

For more details see your compiler documentation, for example:
https://docs.microsoft.com/en-us/cpp/preprocessor/optimize?view=msvc-170

### Building from Visual Studio
A more user-friendly way is to use Visual Studio.

Our Visual Studio solution is located the build folder: C:\git\book\build\MyProject.sln. In this chapter we modified one of the build files, myproject_files.cmake, and as you compile the solution, you will see the following build log.

```shell
Build started...
1>------ Build started: Project: ZERO_CHECK
1>Checking Build System
1>CMake is re-running because ...
1> the file 'C:/git/book/MyProject/Code/myproject_files.cmake'
1> is newer than ...
```

{{<tip>}}
CMake detects changes based on file modification time. ZERO_CHECK should only run when one of the build files have changed in your project. If you are seeing it run on every build, then inspect the build log and see which file CMake thinks has changed since the last build.
{{</tip>}}

Once the build completes, Visual Studio will ask you if you want to reload the solution from disk, select Reload All.

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_26.PNG)

Reload VS solution

Here is how the project now looks like in Visual Studio with MyComponent files.

Figure 4.4. Visual Studio solution with MyComponent

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_27.PNG)

## Summary
If you were to re-launch the Editor, you would not find the component in Add Component menu in Entity Inspector. This is because MyComponent::Reflect() was left empty, therefore it did not tell the Editor about MyComponent. Let us change that to finish up this chapter.

{{<note>}}
Reflecting a component to the Editor is a design choice. Not all components need to be visible in the Editor. We will see various reasons for both sides in later chapters.
{{</note>}}

Example 4.6. MyComponent.cpp with Editor reflection

```c++
#include "MyComponent.h"
#include <AzCore/Serialization/EditContext.h>
using namespace MyProject;
void MyComponent::Reflect(AZ::ReflectContext* reflection)
{
  auto sc = azrtti_cast<AZ::SerializeContext*>(reflection);
  if (!sc) return;
  sc->Class<MyComponent, Component>()
    ->Version(1);
  AZ::EditContext* ec = sc->GetEditContext();
  if (!ec) return;
  using namespace AZ::Edit::Attributes;
  // reflection of this component for O3DE Editor
   ec->Class<MyComponent>("My Component", "[my description]")
     ->ClassElement(AZ::Edit::ClassElements::EditorData, "")
     ->Attribute(AppearsInAddComponentMenu, AZ_CRC("Game"))
     ->Attribute(Category, "My Project");
}
```

Reflection in O3DE is broken down into several parts. For now, it is enough to know that the above code describes the following:
* MyComponent is an empty component with no properties and its version is one (1).
```c++
sc->Class<MyComponent, Component>()
  ->Version(1);
```
* MyComponent is a component that is available for use in game (and in the Editor) with name "My Component" under the category "My Project".
```c++
ec->Class<MyComponent>("My Component", "[my description]")
  ->ClassElement(AZ::Edit::ClassElements::EditorData, "")
  ->Attribute(AppearsInAddComponentMenu, AZ_CRC("Game"))
  ->Attribute(Category, "My Project");
```

Now Add Component menu in the Editor will list this component.

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_28.PNG)

MyComponent in the Editor

And you can add it to any entity, for example to Root Entity of the object we built in Chapter 3, Introduction to Entities and Components.

![](/images/learning-guide/tutorials/o3de-book/Part2/o3de_book_2_29.PNG)

{{<note>}}
The source code for this project can be found on GitHub at:
https://github.com/AMZN-Olex/O3DEBookCode2111/tree/ch04_creating_components
{{</note>}}
