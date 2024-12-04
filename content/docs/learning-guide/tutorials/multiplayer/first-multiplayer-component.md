---
linktitle: 您的第一个网络组件
title: 您的第一个网络组件
description: 在本教程中，您将学习如何使用 Multiplayer Gem 在 Open 3D Engine （O3DE） 中创建多人游戏组件。本教程使用 O3DE 多人游戏示例项目作为基本项目来帮助您入门。
---

按照本教程开始在 Open 3D Engine （O3DE） 中创建多人游戏组件。在本教程结束时，您将了解如何创建具有 network 属性和 remote 过程调用的新网络组件。

首先，构建并运行 O3DE MultiplayerSample 项目。有关如何构建和运行此项目的说明，请参阅 [MultiplayerSample Project README](https://github.com/o3de/o3de-multiplayersample#readme)。

## 网络组件

### 添加新组件

我们首先创建一个网络组件 - MyFirstNetworkComponent。它只会做一件事，那就是复制一个单调计数器。

由于我们已经为 [自动代码生成](/docs/user-guide/programming/autogen) （codegen） 设置了 MultiplayerSample，因此我们可以从复制一个现有的 codegen xml 文件开始。

1. 复制 `<o3de-multiplayersample>\Gem\Code\Source\AutoGen\SimplePlayerCameraComponent.AutoComponent.xml` 到 `<o3de-multiplayersample>\Gem\Code\Source\AutoGen\MyFirstNetworkComponent.AutoComponent.xml`

1. 修改 `<o3de-multiplayersample>\Gem\Code\Source\AutoGen\MyFirstNetworkComponent.AutoComponent.xml` 以包含以下内容：

    ```xml
    <Component Name="MyFirstNetworkComponent"
            Namespace="MultiplayerSample"
            OverrideComponent="true"
            OverrideController="false"
            OverrideInclude="Source/Components/MyFirstNetworkComponent.h"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    </Component>
    ```

1. 到 `<o3de-multiplayersample>\Gem\Code\multiplayersample_files.cmake` 并添加新的 XML 文件：

    ```
    set(FILES
    ...
        Source/AutoGen/MyFirstNetworkComponent.AutoComponent.xml
    ```

1. 为您的 O3DE 项目重新运行 CMake configure，或构建`MultiplayerSample.Static`。失败是意料之中的，因为我们没有在代码生成的文件之上编写代码。

    ```
    3>------ Build started: Project: MultiplayerSample.Static, Configuration: profile x64 ------
    3>Generating Azcg/Generated/Source/AutoGen/NetworkAnimationComponent.AutoComponent.h, Azcg/Generated/Source/AutoGen/NetworkCharacterComponent.AutoComponent.h, Azcg/Generated/Source/AutoGen/NetworkHitVolumesComponent.AutoComponent.h, Azcg/Generated/Source/AutoGen/NetworkPlayerSpawnerComponent.AutoComponent.h, Azcg/Generated/Source/AutoGen/NetworkRigidBodyComponent.AutoComponent.h, Azcg/Generated/Source/AutoGen/NetworkWeaponsComponent.AutoComponent.h, Azcg/Generated/Source/AutoGen/MyFirstNetworkComponent.AutoComponent.h, Azcg/Generated/Source/AutoGen/SimplePlayerCameraComponent.AutoComponent.h, Azcg/Generated/Source/AutoGen/WasdPlayerMovementComponent.AutoComponent.h, Azcg/Generated/Source/AutoGen/NetworkAnimationComponent.AutoComponent.cpp, Azcg/Generated/Source/AutoGen/NetworkCharacterComponent.AutoComponent.cpp, Azcg/Generated/Source/AutoGen/NetworkHitVolumesComponent.AutoComponent.cpp, Azcg/Generated/Source/AutoGen/NetworkPlayerSpawnerComponent.AutoComponent.cpp, Azcg/Generated/Source/AutoGen/NetworkRigidBodyComponent.AutoComponent.cpp, Azcg/Generated/Source/AutoGen/NetworkWeaponsComponent.AutoComponent.cpp, Azcg/Generated/Source/AutoGen/MyFirstNetworkComponent.AutoComponent.cpp, Azcg/Generated/Source/AutoGen/SimplePlayerCameraComponent.AutoComponent.cpp, Azcg/Generated/Source/AutoGen/WasdPlayerMovementComponent.AutoComponent.cpp, Azcg/Generated/Source/AutoGen/AutoComponentTypes.h, Azcg/Generated/Source/AutoGen/AutoComponentTypes.cpp
    3>Running AutoGen for MultiplayerSample.Static
    3>Generating D:\git\o3de\build\-86ba765d\Gem\Code\Azcg\Generated\Source\AutoGen\MyFirstNetworkComponent.AutoComponent.h with template D:/git/o3de/Gems/Multiplayer/Code/Source/AutoGen/AutoComponent_Header.jinja and inputs D:\git\o3de-multiplayersample\Gem\Code\Source\AutoGen\MyFirstNetworkComponent.AutoComponent.xml
    3>Generating D:\git\o3de\build\-86ba765d\Gem\Code\Azcg\Generated\Source\AutoGen\MyFirstNetworkComponent.AutoComponent.cpp with template D:/git/o3de/Gems/Multiplayer/Code/Source/AutoGen/AutoComponent_Source.jinja and inputs D:\git\o3de-multiplayersample\Gem\Code\Source\AutoGen\MyFirstNetworkComponent.AutoComponent.xml
    3>Generating D:\git\o3de\build\-86ba765d\Gem\Code\Azcg\Generated\Source\AutoGen\AutoComponentTypes.cpp with template D:/git/o3de/Gems/Multiplayer/Code/Source/AutoGen/AutoComponentTypes_Source.jinja and inputs D:\git\o3de-multiplayersample\Gem\Code\Source\AutoGen\NetworkAnimationComponent.AutoComponent.xml, D:\git\o3de-multiplayersample\Gem\Code\Source\AutoGen\NetworkCharacterComponent.AutoComponent.xml, D:\git\o3de-multiplayersample\Gem\Code\Source\AutoGen\NetworkHitVolumesComponent.AutoComponent.xml, D:\git\o3de-multiplayersample\Gem\Code\Source\AutoGen\NetworkPlayerSpawnerComponent.AutoComponent.xml, D:\git\o3de-multiplayersample\Gem\Code\Source\AutoGen\NetworkRigidBodyComponent.AutoComponent.xml, D:\git\o3de-multiplayersample\Gem\Code\Source\AutoGen\NetworkWeaponsComponent.AutoComponent.xml, D:\git\o3de-multiplayersample\Gem\Code\Source\AutoGen\MyFirstNetworkComponent.AutoComponent.xml, D:\git\o3de-multiplayersample\Gem\Code\Source\AutoGen\SimplePlayerCameraComponent.AutoComponent.xml, D:\git\o3de-multiplayersample\Gem\Code\Source\AutoGen\WasdPlayerMovementComponent.AutoComponent.xml
    3>Total Time 0:00:00.01
    3>MyFirstNetworkComponent.AutoComponent.cpp
    3>AutoComponentTypes.cpp
    3>D:\git\o3de\build\-86ba765d\Gem\Code\Azcg\Generated\Source\AutoGen\AutoComponentTypes.cpp(14,10): fatal error C1083: Cannot open include file: 'Source/Components/MyFirstNetworkComponent.h': No such file or directory
    3>#include <Source/Components/MyFirstNetworkComponent.h>
    3>         ^
    3>D:\git\o3de\build\-86ba765d\Gem\Code\Azcg\Generated\Source\AutoGen\MyFirstNetworkComponent.AutoComponent.cpp(20,10): fatal error C1083: Cannot open include file: 'Source/Components/MyFirstNetworkComponent.h': No such file or directory
    3>#include <Source/Components/MyFirstNetworkComponent.h>
    3>         ^
    3>Done building project "MultiplayerSample.Static.vcxproj" -- FAILED.
    ```

1. 请注意我们到目前为止得到的结果：

    ```
    3>Generating D:\git\o3de\build\-86ba765d\Gem\Code\Azcg\Generated\Source\AutoGen\MyFirstNetworkComponent.AutoComponent.h ...
    3>Generating D:\git\o3de\build\-86ba765d\Gem\Code\Azcg\Generated\Source\AutoGen\MyFirstNetworkComponent.AutoComponent.cpp ...
    ```

1. 打开头文件 `MyFirstNetworkComponent.AutoComponent.h`  并查找以下大型注释块：

    ```c++
    /*
    /// You may use the classes below as a basis for your new derived classes. Derived classes must be marked in MyFirstNetworkComponent.AutoComponent.xml
    /// Place in your .h
    #pragma once

    #include <Source/AutoGen/MyFirstNetworkComponent.AutoComponent.h>

    namespace MultiplayerSample
    {
        class MyFirstNetworkComponent
            : public MyFirstNetworkComponentBase
        {
        public:
            AZ_MULTIPLAYER_COMPONENT(MultiplayerSample::MyFirstNetworkComponent, s_myFirstNetworkComponentConcreteUuid, MultiplayerSample::MyFirstNetworkComponentBase);

            static void Reflect(AZ::ReflectContext* context);

            void OnInit() override;
            void OnActivate(Multiplayer::EntityIsMigrating entityIsMigrating) override;
            void OnDeactivate(Multiplayer::EntityIsMigrating entityIsMigrating) override;




        protected:

        };

    }
    /// Place in your .cpp
    #include <Source/Components/MyFirstNetworkComponent.h>

    namespace MultiplayerSample
    {
        void MyFirstNetworkComponent::Reflect(AZ::ReflectContext* context)
        {
            AZ::SerializeContext* serializeContext = azrtti_cast<AZ::SerializeContext*>(context);
            if (serializeContext)
            {
                serializeContext->Class<MyFirstNetworkComponent, MyFirstNetworkComponentBase>()
                    ->Version(1);
            }
            MyFirstNetworkComponentBase::Reflect(context);
        }

        void MyFirstNetworkComponent::OnInit()
        {
        }

        void MyFirstNetworkComponent::OnActivate([[maybe_unused]] Multiplayer::EntityIsMigrating entityIsMigrating)
        {
        }

        void MyFirstNetworkComponent::OnDeactivate([[maybe_unused]] Multiplayer::EntityIsMigrating entityIsMigrating)
        {
        }

    }
    */
    ```

1. 前一个块中的代码是我们的起始基数。将该代码复制粘贴到`<o3de-multiplayersample>\Gem\Code\Source\Components\MyFirstNetworkComponent.h` 和 `<o3de-multiplayersample>\Gem\Code\Source\Components\MyFirstNetworkComponent.cpp`.

1. 将这些文件添加到 `<o3de-multiplayersample>\Gem\Code\multiplayersample_files.cmake`:

    ```
    set(FILES
    ...
        Source/Components/MyFirstNetworkComponent.cpp
        Source/Components/MyFirstNetworkComponent.h
    ```

1. 现在，MultiplayerSample 项目应该可以编译，没有任何问题。打开 **O3DE 编辑器** 以在 Editor 中查看新组件。


    ![My First Network Component in the Editor](/images/learning-guide/tutorials/multiplayer/my_first_network_component_in_editor.png)

    Additionally, you can find the generated files and your XML file in Visual Studio:

    ![Generated Code in Visual Studio](/images/learning-guide/tutorials/multiplayer/visualstudio_generated_files_for_myfirstnetworkcomponent.png)

1. 此时，您有一个名为`MyFirstNetworkComponent`的多人组件。

### 添加网络属性

让我们添加一个网络属性，该属性会将运行时间从服务器复制到客户端。

1. 首先修改`MyFirstNetworkComponent.AutoComponent.xml`。以下是具有 network 属性的 codegen xml 文件：

    ```xml
    <?xml version="1.0"?>
    <Component Name="MyFirstNetworkComponent"
            Namespace="MultiplayerSample"
            OverrideComponent="true"
            OverrideController="false"
            OverrideInclude="Source/Components/MyFirstNetworkComponent.h"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <NetworkProperty Type="double"
                    Name="UpTime"
                    Init="0.0"
                    ReplicateFrom="Authority"
                    ReplicateTo="Client"
                    IsRewindable="true"
                    IsPredictable="true"
                    IsPublic="true"
                    Container="Object"
                    ExposeToEditor="false"
                    ExposeToScript="false"
                    GenerateEventBindings="false"
                    Description="Time since the start of the application" />
    </Component>
    ```

1. 重点介绍`NetworkProperty`的基本详细信息。这些是：

    ```xml
    <NetworkProperty Type="double"
                    Name="UpTime"
                    Init="0.0"
                    ReplicateFrom="Authority"
                    ReplicateTo="Client"
    ```

    用 C++ 表示，如下所示：

    ```c++
    double m_UpTime = 0.0; // replicate from Authority to Client
    ```

    {{<note>}}
    目前，我们可以将 '`Authority`' 视为多人游戏服务器。对`Client`的`Authority`是最常见的复制方向。
    {{</note>}}

1. 通过这些更改，`MyFirstNetworkComponent`可以通过客户端上的 GetUpTime（） 方法访问 `UpTime`。在授权服务器上，我们可以通过访问组件的控制器 - `MyFirstNetworkComponentController` 来设置`UpTime`的值，使用以下代码：

    ```c++
        void MyFirstNetworkComponent::OnTick( [[maybe_unused]] float deltaTime, AZ::ScriptTimePoint time )
        {
            if ( HasController() )
            {
                auto* controller = static_cast<MyFirstNetworkComponentController*>( GetController() );
                controller->ModifyUpTime() = time.GetSeconds();
                AZ_Printf( "MyFirstNetworkComponent", "server = %f", GetUpTime() );
            }
            else
            {
                AZ_Printf( "MyFirstNetworkComponent", "client = %f", GetUpTime() );
            }
        }
    ```

    {{<note>}}
控制器仅存在于授权服务器上，因此这有效地将执行拆分为仅服务器和仅客户端逻辑。
    {{</note>}}

1. 到目前为止，该组件的完整代码如下。头文件为`<o3de-multiplayersample>\Gem\Code\Source\Components\MyFirstNetworkComponent.h`:

    ```c++
    #pragma once

    #include <Source/AutoGen/MyFirstNetworkComponent.AutoComponent.h>
    #include <AzCore/Component/TickBus.h>

    namespace MultiplayerSample
    {
        class MyFirstNetworkComponent
            : public MyFirstNetworkComponentBase
            , public AZ::TickBus::Handler
        {
        public:
            AZ_MULTIPLAYER_COMPONENT(MultiplayerSample::MyFirstNetworkComponent, s_myFirstNetworkComponentConcreteUuid, MultiplayerSample::MyFirstNetworkComponentBase);

            static void Reflect(AZ::ReflectContext* context);

            void OnInit() override;
            void OnActivate(Multiplayer::EntityIsMigrating entityIsMigrating) override;
            void OnDeactivate(Multiplayer::EntityIsMigrating entityIsMigrating) override;

            void OnTick(float deltaTime, AZ::ScriptTimePoint time) override;
        };
    }
    ```

    源文件， `<o3de-multiplayersample>\Gem\Code\Source\Components\MyFirstNetworkComponent.cpp`:

    ```c++
    #include <Source/Components/MyFirstNetworkComponent.h>

    #include <AzCore/Serialization/SerializeContext.h>

    namespace MultiplayerSample
    {
        void MyFirstNetworkComponent::Reflect( AZ::ReflectContext* context )
        {
            AZ::SerializeContext* serializeContext = azrtti_cast<AZ::SerializeContext*>( context );
            if ( serializeContext )
            {
                serializeContext->Class<MyFirstNetworkComponent, MyFirstNetworkComponentBase>()
                    ->Version( 1 );
            }
            MyFirstNetworkComponentBase::Reflect( context );
        }

        void MyFirstNetworkComponent::OnInit()
        {
        }

        void MyFirstNetworkComponent::OnActivate( [[maybe_unused]] Multiplayer::EntityIsMigrating entityIsMigrating )
        {
            AZ::TickBus::Handler::BusConnect();
        }

        void MyFirstNetworkComponent::OnDeactivate( [[maybe_unused]] Multiplayer::EntityIsMigrating entityIsMigrating )
        {
            AZ::TickBus::Handler::BusDisconnect();
        }

        void MyFirstNetworkComponent::OnTick( [[maybe_unused]] float deltaTime, AZ::ScriptTimePoint time )
        {
            if ( HasController() )
            {
                auto* controller = static_cast<MyFirstNetworkComponentController*>( GetController() );
                controller->ModifyUpTime() = time.GetSeconds();
                AZ_Printf( "MyFirstNetworkComponent", "server = %f", GetUpTime() );
            }
            else
            {
                AZ_Printf( "MyFirstNetworkComponent", "client = %f", GetUpTime() );
            }
        }
    }
    ```

1. 现在，您有一个具有 network 属性的 multiplayer 组件。

### 监听客户端上的网络属性更改

但是，让我们对此进行改进。客户端不需要 tick，它可以监听更改。为此，请启用 `GenerateEventBindings`。
这就是我们目前所拥有的。

```xml
  <NetworkProperty Type="double"
                   Name="UpTime"
...
                   GenerateEventBindings="false"
...
                    />
```

修改 `GenerateEventBindings` 为 true:

```xml
  <NetworkProperty Type="double"
                   Name="UpTime"
...
                   GenerateEventBindings="true"
...
                    />
```

项目构建将生成一个新方法：`UpTimeAddEvent`。让我们在代码中使用它。

要监听组件中客户端的更改，请执行以下操作：

1. 创建事件处理程序。

    ```c++
    // create an event handler
    AZ::Event<double>::Handler m_uptimeChanged;
    ```

1. 创建回调方法。

    ```c++
    void OnUpTimeChanged( double uptime );
    ```

1. 将其分配给事件处理程序。

    ```c++
    // assign the callback to the event handler
    MyFirstNetworkComponent::MyFirstNetworkComponent()
        : m_uptimeChanged( [this]( double uptime ) {OnUpTimeChanged( uptime ); } )
    {
    }
    ```

1. 将事件处理程序连接到 `UpTimeAddEvent`。

    ```c++
    // connect the event handler to the generated UpTimeAddEvent method
    void MyFirstNetworkComponent::OnActivate( [[maybe_unused]] Multiplayer::EntityIsMigrating entityIsMigrating )
    {
        UpTimeAddEvent( m_uptimeChanged );
    }
    ```


1. 现在，无需在客户端上打勾，而是从回调中打印我们的日志消息：

    ```c++
    void MyFirstNetworkComponent::OnUpTimeChanged( double uptime )
    {
        AZ_Printf( "MyFirstNetworkComponent", "client = %f", uptime );
    }
    ```

进行这些更改后，`MyFirstNetworkComponent.h`应如下所示：

```c++
#pragma once

#include <Source/AutoGen/MyFirstNetworkComponent.AutoComponent.h>
#include <AzCore/Component/TickBus.h>

namespace MultiplayerSample
{
    class MyFirstNetworkComponent
        : public MyFirstNetworkComponentBase
        , public AZ::TickBus::Handler
    {
    public:
        AZ_MULTIPLAYER_COMPONENT( MultiplayerSample::MyFirstNetworkComponent, s_myFirstNetworkComponentConcreteUuid, MultiplayerSample::MyFirstNetworkComponentBase );

        static void Reflect( AZ::ReflectContext* context );

        MyFirstNetworkComponent();

        void OnInit() override;
        void OnActivate( Multiplayer::EntityIsMigrating entityIsMigrating ) override;
        void OnDeactivate( Multiplayer::EntityIsMigrating entityIsMigrating ) override;

        void OnTick( float deltaTime, AZ::ScriptTimePoint time ) override;

    private:
        AZ::Event<double>::Handler m_uptimeChanged;
        void OnUpTimeChanged( double uptime );
    };
}
```

并且 `MyFirstNetworkComponent.cpp`应如下所示：

```c++
#include <Source/Components/MyFirstNetworkComponent.h>

#include <AzCore/Serialization/SerializeContext.h>

namespace MultiplayerSample
{
    void MyFirstNetworkComponent::Reflect( AZ::ReflectContext* context )
    {
        AZ::SerializeContext* serializeContext = azrtti_cast<AZ::SerializeContext*>( context );
        if ( serializeContext )
        {
            serializeContext->Class<MyFirstNetworkComponent, MyFirstNetworkComponentBase>()
                ->Version( 1 );
        }
        MyFirstNetworkComponentBase::Reflect( context );
    }

    MyFirstNetworkComponent::MyFirstNetworkComponent()
        : m_uptimeChanged( [this]( double uptime ) {OnUpTimeChanged( uptime ); } )
    {
    }

    void MyFirstNetworkComponent::OnInit()
    {
    }

    void MyFirstNetworkComponent::OnActivate( [[maybe_unused]] Multiplayer::EntityIsMigrating entityIsMigrating )
    {
        UpTimeAddEvent( m_uptimeChanged );

        if ( HasController() )
        {
            AZ::TickBus::Handler::BusConnect();
        }
    }

    void MyFirstNetworkComponent::OnDeactivate( [[maybe_unused]] Multiplayer::EntityIsMigrating entityIsMigrating )
    {
        AZ::TickBus::Handler::BusDisconnect();
    }

    void MyFirstNetworkComponent::OnTick( [[maybe_unused]] float deltaTime, AZ::ScriptTimePoint time )
    {
        // only happens on Authority (server)
        auto* controller = static_cast<MyFirstNetworkComponentController*>( GetController() );
        controller->ModifyUpTime() = time.GetSeconds();
        AZ_Printf( "MyFirstNetworkComponent", "server = %f", GetUpTime() );
    }

    void MyFirstNetworkComponent::OnUpTimeChanged( double uptime )
    {
        AZ_Printf( "MyFirstNetworkComponent", "client = %f", uptime );
    }
}
```

### 网络组件控制器

让我们改进一下服务器代码和客户端代码之间的分离。目前，`MyFirstNetworkComponent`同时执行服务器和客户端任务。让我们把它分解开来。在 O3DE 多人游戏中，做到这一点的方法是使用“控制器”。控制器不存在 `Client` 角色中，但存在于 Authority 角色上。我们已经在前面的示例中看到了这一点：

```c++
auto* controller = static_cast<MyFirstNetworkComponentController*>( GetController() );
```

1. 返回到 codegen XML，`<o3de-multiplayersample>\Gem\Code\Source\AutoGen\MyFirstNetworkComponent.AutoComponent.xml`，其中控制器在属性`OverrideController`中提到：

    ```xml
    <Component Name="MyFirstNetworkComponent"
            Namespace="MultiplayerSample"
            OverrideComponent="true"
            OverrideController="false"
    ```

1. 将 `OverrideController` 更改为 true，以便我们可以在控制器中编写自定义逻辑。

    ```xml
    <Component Name="MyFirstNetworkComponent"
            Namespace="MultiplayerSample"
            OverrideComponent="true"
            OverrideController="true"
    ```

1. 构建项目。
1. 尝试构建代码时，您将收到预期的编译错误。这是因为 codegen 希望我们覆盖控制器基类。
1. 查看代码生成的头文件，`\path\to\o3de\build\-86ba765d\Gem\Code\Azcg\Generated\Source\AutoGen\MyFirstNetworkComponent.AutoComponent.h`, 您会发现 Codegen 修改了大注释，现在包含控制器的代码示例：

    ```c++
    /// You may use the classes below as a basis for your new derived classes. Derived classes must be marked in MyFirstNetworkComponent.AutoComponent.xml
    /*
        class MyFirstNetworkComponentController
            : public MyFirstNetworkComponentControllerBase
        {
        public:
            MyFirstNetworkComponentController(MyFirstNetworkComponent& parent);

            void OnActivate(Multiplayer::EntityIsMigrating entityIsMigrating) override;
            void OnDeactivate(Multiplayer::EntityIsMigrating entityIsMigrating) override;
        };

        MyFirstNetworkComponentController::MyFirstNetworkComponentController(MyFirstNetworkComponent& parent)
            : MyFirstNetworkComponentControllerBase(parent)
        {
        }

        void MyFirstNetworkComponentController::OnActivate([[maybe_unused]] Multiplayer::EntityIsMigrating entityIsMigrating)
        {
        }

        void MyFirstNetworkComponentController::OnDeactivate([[maybe_unused]] Multiplayer::EntityIsMigrating entityIsMigrating)
        {
        }
    */
    ```

1. 将此内容添加到我们现有的`<o3de-multiplayersample>\Gem\Code\Source\Components\MyFirstNetworkComponent.h` 和 `<o3de-multiplayersample>\Gem\Code\Source\Components\MyFirstNetworkComponent.cpp`.

1. 将所有服务器逻辑移动到`MyFirstNetworkComponentController`，并保留 `MyFirstNetworkComponent`中的客户端逻辑，如以下更新的文件所示，`<o3de-multiplayersample>\Gem\Code\Source\Components\MyFirstNetworkComponent.h`:

    ```c++
    #pragma once

    #include <Source/AutoGen/MyFirstNetworkComponent.AutoComponent.h>
    #include <AzCore/Component/TickBus.h>

    namespace MultiplayerSample
    {
        class MyFirstNetworkComponent
            : public MyFirstNetworkComponentBase
        {
        public:
            AZ_MULTIPLAYER_COMPONENT( MultiplayerSample::MyFirstNetworkComponent, s_myFirstNetworkComponentConcreteUuid, MultiplayerSample::MyFirstNetworkComponentBase );

            static void Reflect( AZ::ReflectContext* context );

            MyFirstNetworkComponent();

            void OnInit() override {}
            void OnActivate( Multiplayer::EntityIsMigrating entityIsMigrating ) override;
            void OnDeactivate( Multiplayer::EntityIsMigrating entityIsMigrating ) override;

        private:
            AZ::Event<double>::Handler m_uptimeChanged;
            void OnUpTimeChanged( double uptime );
        };

        class MyFirstNetworkComponentController
            : public MyFirstNetworkComponentControllerBase
            , public AZ::TickBus::Handler
        {
        public:
            MyFirstNetworkComponentController(MyFirstNetworkComponent& parent);

            void OnActivate(Multiplayer::EntityIsMigrating entityIsMigrating) override;
            void OnDeactivate(Multiplayer::EntityIsMigrating entityIsMigrating) override;

            void OnTick( float deltaTime, AZ::ScriptTimePoint time ) override;
        };
    }
    ```

1. 请注意头文件中的主要区别： `AZ::TickBus::Handler` 从 `MyFirstNetworkComponent` 移动到 `MyFirstNetworkComponentController`，在 `<o3de-multiplayersample>\Gem\Code\Source\Components\MyFirstNetworkComponent.cpp`中:

    ```c++
    #include <Source/Components/MyFirstNetworkComponent.h>

    #include <AzCore/Serialization/SerializeContext.h>

    namespace MultiplayerSample
    {
        void MyFirstNetworkComponent::Reflect( AZ::ReflectContext* context )
        {
            AZ::SerializeContext* serializeContext = azrtti_cast<AZ::SerializeContext*>( context );
            if ( serializeContext )
            {
                serializeContext->Class<MyFirstNetworkComponent, MyFirstNetworkComponentBase>()
                    ->Version( 1 );
            }
            MyFirstNetworkComponentBase::Reflect( context );
        }

        MyFirstNetworkComponent::MyFirstNetworkComponent()
            : m_uptimeChanged( [this]( double uptime ) { OnUpTimeChanged( uptime ); } )
        {
        }

        void MyFirstNetworkComponent::OnActivate( [[maybe_unused]] Multiplayer::EntityIsMigrating entityIsMigrating )
        {
            if (HasController() == false) // client only
            {
                UpTimeAddEvent( m_uptimeChanged );
            }
        }

        void MyFirstNetworkComponent::OnDeactivate( [[maybe_unused]] Multiplayer::EntityIsMigrating entityIsMigrating )
        {
        }

        void MyFirstNetworkComponent::OnUpTimeChanged( double uptime )
        {
            AZ_Printf( "MyFirstNetworkComponent", "client = %f", uptime );
        }

        /////////// Controller ////////////////

        MyFirstNetworkComponentController::MyFirstNetworkComponentController( MyFirstNetworkComponent& parent )
            : MyFirstNetworkComponentControllerBase( parent )
        {
        }

        void MyFirstNetworkComponentController::OnActivate( [[maybe_unused]] Multiplayer::EntityIsMigrating entityIsMigrating )
        {
            AZ::TickBus::Handler::BusConnect();
        }

        void MyFirstNetworkComponentController::OnDeactivate( [[maybe_unused]] Multiplayer::EntityIsMigrating entityIsMigrating )
        {
            AZ::TickBus::Handler::BusDisconnect();
        }

        void MyFirstNetworkComponentController::OnTick( [[maybe_unused]] float deltaTime, AZ::ScriptTimePoint time )
        {
            ModifyUpTime() = time.GetSeconds();
            AZ_Printf( "MyFirstNetworkComponent", "server = %f", GetUpTime() );
        }
    }
    ```

1. 我们的代码生成 xml 文件 `<o3de-multiplayersample>\Gem\Code\Source\AutoGen\MyFirstNetworkComponent.AutoComponent.xml`需要是：

    ```xml
    <?xml version="1.0"?>
    <Component Name="MyFirstNetworkComponent"
            Namespace="MultiplayerSample"
            OverrideComponent="true"
            OverrideController="true"
            OverrideInclude="Source/Components/MyFirstNetworkComponent.h"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <NetworkProperty Type="double"
                    Name="UpTime"
                    Init="0.0"
                    ReplicateFrom="Authority"
                    ReplicateTo="Client"
                    IsRewindable="true"
                    IsPredictable="true"
                    IsPublic="true"
                    Container="Object"
                    ExposeToEditor="false"
                    ExposeToScript="false"
                    GenerateEventBindings="true"
                    Description="Time since the start of the application" />
    </Component>
    ```

现在我们在 server 和 client logic之间实现了清晰的分离。

### 添加远程过程调用

现在，我们已经使用网络属性创建了从服务器到客户端的数据流，我们将添加远程过程调用 （RPC）。每当客户端获得新的`UpTime` 时，它都会调用新的 RPC - `SendConfirmUptime`，并将其获取的值发送回服务器。只是为了好玩。

1. 添加定义 RPC 的 codegen 部分：

    ```xml
    <RemoteProcedure Name="SendConfirmUptime"
                    InvokeFrom="Autonomous"
                    HandleOn="Authority"
                    IsPublic="false"
                    IsReliable="false"
                    GenerateEventBindings="false"
                    Description="Uptime confirmed by the client">
        <Param Type="double"
            Name="UpTime" />
    </RemoteProcedure>
    ```

    请注意，InvokeFrom 是 '`Autonomous`' 而不是 '`Client`'。'`Autonomous`'是一种特殊类型的客户端行为，其中客户端启动操作并可能将数据发送到服务器。

    `Autonomous`通常用于玩家角色控制器;在这种情况下，玩家必须能够自行操作，而无需等待服务器告诉它该做什么。同时，'Client' 只镜像服务器告诉他们做什么。

1. 默认情况下，只有玩家Prefab被标记为 Autonomous，因此在本教程中，我们将`MyFirstNetworkComponent`移动到玩家Prefab。

    {{<important>}}
    只有自治的实体才会有控制器，否则`GetController()`将在客户端上给出 null。如果您在`GetController`调用中得到 null，则您已将组件附加到非自治实体。请改为将它们附加到玩家Prefab，因为这些Prefab被服务器标记为自主。
    {{</important>}}

    您可以在 `<o3de-multiplayersample>\Prefabs\Player.prefab` 中找到 MultiplayerSample 项目的玩家Prefab。
    要修改`Player.prefab`，执行以下操作：

    ![Attaching Network Component to an Autonomous Entity](/images/learning-guide/tutorials/multiplayer/add_myfirstnetworkcomponent_to_player_prefab.png)

    1. 在关卡中临时实例化玩家Prefab。（您可以在 `<o3de-multiplayersample>\Prefabs\Player.prefab` 中找到 MultiplayerSample 项目的玩家Prefab。

    1. 通过将 `MyFirstNetworkComponent` 添加到 `player` 实体来修改玩家Prefab实例。

    1. 保存播放器Prefab。

    1. 从关卡中删除玩家Prefab。


1. 重新构建`MultiplayerSample.Static`项目。您会注意到，为我们的组件生成的头文件`\path\to\o3de\build\MultiplayerSample-6db9bd97\Gem\Code\Azcg\Generated\Source\AutoGen\MyFirstNetworkComponent.AutoComponent.h` 在`MyFirstNetworkComponentController`的大注释块中有一个新的空虚拟方法：

    {{<note>}}
    您可能会被此文件夹名称`MultiplayerSample-6db9bd97`抛出。这是 build 文件夹中的代码生成的文件夹。您的 build 文件夹中可能有不同的名称。
    {{</note>}}

    ```c++
    class MyFirstNetworkComponentController
    ...

        void HandleSendConfirmUptime(AzNetworking::IConnection* invokingConnection, const double& UpTime) override {}
    ```

1. 请注意这一点，并提供以下`HandleSendConfirmUptime`的实现：

    ```c++
    void MyFirstNetworkComponentController::HandleSendConfirmUptime([[maybe_unused]] AzNetworking::IConnection* invokingConnection,
        const double& upTime)
    {
        AZ_Printf("MyFirstNetworkComponent", "on server - client told us about %f\n", upTime);
    }
    ```

1. 客户端将引用其控制器来调用 RPC：

    ```c++
    void MyFirstNetworkComponent::OnUpTimeChanged(double uptime)
    {
        AZ_Printf("MyFirstNetworkComponent", "client = %f\n", uptime);
        static_cast<MyFirstNetworkComponentController*>(GetController())->SendConfirmUptime(uptime);
    }
    ```

1. 最后，向`MyFirstNetworkComponentController::OnActivate`添加一个检查，以确保 `MyFirstNetworkComponentController`在服务器上执行时仅连接到`TickBus`，而不是自治玩家客户端：
 ```c++
 void MyFirstNetworkComponentController::OnActivate([[maybe_unused]] Multiplayer::EntityIsMigrating entityIsMigrating)
 {
     if (!IsAutonomous())
     {
         AZ::TickBus::Handler::BusConnect();
     }
 }
 ```


1. 您的组件源代码应如下所示`\path\to\MultiplayerSample\Gem\Code\Source\Components\MyFirstNetworkComponent.h`:

    ```c++
    #pragma once

    #include <Source/AutoGen/MyFirstNetworkComponent.AutoComponent.h>
    #include <AzCore/Component/TickBus.h>

    namespace MultiplayerSample
    {
        class MyFirstNetworkComponent
            : public MyFirstNetworkComponentBase
        {
        public:
            AZ_MULTIPLAYER_COMPONENT( MultiplayerSample::MyFirstNetworkComponent, s_myFirstNetworkComponentConcreteUuid, MultiplayerSample::MyFirstNetworkComponentBase );

            static void Reflect( AZ::ReflectContext* context );

            MyFirstNetworkComponent();

            void OnInit() override {}
            void OnActivate( Multiplayer::EntityIsMigrating entityIsMigrating ) override;
            void OnDeactivate( Multiplayer::EntityIsMigrating entityIsMigrating ) override;

        private:
            AZ::Event<double>::Handler m_uptimeChanged;
            void OnUpTimeChanged( double uptime );
        };

        class MyFirstNetworkComponentController
            : public MyFirstNetworkComponentControllerBase
            , public AZ::TickBus::Handler
        {
        public:
            MyFirstNetworkComponentController(MyFirstNetworkComponent& parent);

            void OnActivate(Multiplayer::EntityIsMigrating entityIsMigrating) override;
            void OnDeactivate(Multiplayer::EntityIsMigrating entityIsMigrating) override;

            void OnTick( float deltaTime, AZ::ScriptTimePoint time ) override;

            void HandleSendConfirmUptime(AzNetworking::IConnection* invokingConnection, const double& UpTime) override;
        };
    }
    ```

1. 并且源文件 `\path\to\MultiplayerSample\Gem\Code\Source\Components\MyFirstNetworkComponent.cpp`应为：

    ```c++
    #include <Source/Components/MyFirstNetworkComponent.h>

    #include <AzCore/Serialization/SerializeContext.h>
    #include <Multiplayer/Components/NetBindComponent.h>

    namespace MultiplayerSample
    {
        void MyFirstNetworkComponent::Reflect(AZ::ReflectContext* context)
        {
            AZ::SerializeContext* serializeContext = azrtti_cast<AZ::SerializeContext*>(context);
            if (serializeContext)
            {
                serializeContext->Class<MyFirstNetworkComponent, MyFirstNetworkComponentBase>()
                    ->Version(1);
            }
            MyFirstNetworkComponentBase::Reflect(context);
        }

        MyFirstNetworkComponent::MyFirstNetworkComponent()
            : m_uptimeChanged([this](double uptime) { OnUpTimeChanged(uptime); })
        {
        }

        void MyFirstNetworkComponent::OnActivate([[maybe_unused]] Multiplayer::EntityIsMigrating entityIsMigrating)
        {
            if (IsNetEntityRoleAutonomous())
            {
                UpTimeAddEvent(m_uptimeChanged); // listen only on clients
            }
        }

        void MyFirstNetworkComponent::OnDeactivate([[maybe_unused]] Multiplayer::EntityIsMigrating entityIsMigrating)
        {
        }

        void MyFirstNetworkComponent::OnUpTimeChanged(double uptime)
        {
            AZ_Printf("MyFirstNetworkComponent", "client = %f\n", uptime);
            static_cast<MyFirstNetworkComponentController*>(GetController())->SendConfirmUptime(uptime);
        }

        /////////// Controller ////////////////

        MyFirstNetworkComponentController::MyFirstNetworkComponentController(MyFirstNetworkComponent& parent)
            : MyFirstNetworkComponentControllerBase(parent)
        {
        }

        void MyFirstNetworkComponentController::OnActivate([[maybe_unused]] Multiplayer::EntityIsMigrating entityIsMigrating)
        {
            if (!IsAutonomous())
            {
                AZ::TickBus::Handler::BusConnect();
            }
        }

        void MyFirstNetworkComponentController::OnDeactivate([[maybe_unused]] Multiplayer::EntityIsMigrating entityIsMigrating)
        {
            AZ::TickBus::Handler::BusDisconnect();
        }

        void MyFirstNetworkComponentController::OnTick([[maybe_unused]] float deltaTime, AZ::ScriptTimePoint time)
        {
            ModifyUpTime() = time.GetSeconds();
            AZ_Printf("MyFirstNetworkComponent", "server = %f\n", GetUpTime());
        }

        void MyFirstNetworkComponentController::HandleSendConfirmUptime([[maybe_unused]] AzNetworking::IConnection* invokingConnection,
            const double& upTime)
        {
            AZ_Printf("MyFirstNetworkComponent", "on server - client told us about %f\n", upTime);
        }
    }
    ```

1. 我们用于代码生成器的 xml，`<o3de-multiplayersample>\Gem\Code\Source\AutoGen\MyFirstNetworkComponent.AutoComponent.xml`:

    ```xml
    <?xml version="1.0"?>
    <Component Name="MyFirstNetworkComponent"
            Namespace="MultiplayerSample"
            OverrideComponent="true"
            OverrideController="true"
            OverrideInclude="Source/Components/MyFirstNetworkComponent.h"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <NetworkProperty Type="double"
                    Name="UpTime"
                    Init="0.0"
                    ReplicateFrom="Authority"
                    ReplicateTo="Client"
                    IsRewindable="true"
                    IsPredictable="true"
                    IsPublic="true"
                    Container="Object"
                    ExposeToEditor="false"
                    ExposeToScript="false"
                    GenerateEventBindings="true"
                    Description="Time since the start of the application" />
    <RemoteProcedure Name="SendConfirmUptime"
                    InvokeFrom="Autonomous"
                    HandleOn="Authority"
                    IsPublic="false"
                    IsReliable="false"
                    GenerateEventBindings="false"
                    Description="Uptime confirmed by the client">
        <Param Type="double"
            Name="UpTime" />
    </RemoteProcedure>
    </Component>
    ```

10. 此时，在 Play Mode 下运行游戏时，您应该会看到 Editor 控制台中发出的客户端和服务器日志：

    ![Console logs emitted while running game from editor with RPC](/images/learning-guide/tutorials/multiplayer/add_myfirstnetworkcomponent_run_game_with_rpc.png)
