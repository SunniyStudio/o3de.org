---
title: 将多人游戏逻辑分离为客户端和服务器启动器
description: 使用多人游戏 Gem 构建 Open 3D Engine 启动器，专门针对客户端和/或服务器。
linkTitle: 分离 Client 和 Server
weight: 450
---

**Multiplayer Gem** 支持在构建时进行代码分离，以创建仅包含客户端逻辑、仅包含服务器逻辑或同时包含客户端和服务器逻辑的代码。这允许用户通过排除不必要的逻辑和依赖项来创建较小大小的可执行文件。它还允许将一个可执行文件特有的潜在敏感 logic 隐藏给另一个可执行文件。例如，确保免费游戏的客户端可执行文件永远不包含任何服务器逻辑代码，这将减少黑客攻击或滥用的机会。

拆分功能会生成多种 build 类型：
* _GameLauncher_ 是仅限客户端的启动器。
* _ServerLauncher_ 是适用于专用服务器的仅限服务器的启动器。
* _UnifiedLauncher_ 同时提供这两种功能，并且适用于_client托管的 servers_，这些客户端可以同时托管和参与多人游戏会话。

此功能是通过各种构建机制实现的，在使用多人游戏 Gem 的任何 Gem 或项目中了解这些机制非常重要。

## 拆分客户端和服务器逻辑

多人游戏 Gem 包含可分为两类的代码文件：
1. 所有启动器类型都完全需要的文件。
2. 根据启动器类型及其依赖项有条件编译出部分的文件。

这些文件列表分布维护在`multiplayer_files.cmake` 和 `multiplayer_split_files.cmake`。

`multiplayer_files.cmake` 通常包含 Core 数据类型、Base 和 Core 类。 `multiplayer_split_files.cmake` 包含基于 AutoComponent 的 MultiplayerComponent 和依赖于它们的类型。

### CMake 设置

按 cmake 文件拆分将我们引向四个 Multiplayer 目标：

1. Common - 包含 `multiplayer_files.cmake`.
2. Client - 包含 `multiplayer_files.cmake` 加 `multiplayer_split_files.cmake` 为 Client 端进行有条件编译。
3. Server - 包含 `multiplayer_files.cmake` 加 `multiplayer_split_files.cmake` 为服务器有条件地编译。
4. Unified - 包含 `multiplayer_files.cmake` 加 `multiplayer_split_files.cmake` 为 Client 端和 Server 进行条件编译。

在包含 Multiplayer Gem 时，了解您的使用需求非常重要。如果使用需要拆分逻辑，建议创建分别指定`Multiplayer.Client`, `Multiplayer.Server `, 和 `Multiplayer.Unified`依赖项的 Client、Server 和 Unified 目标。如果您的使用不需要拆分逻辑，则`Multiplayer.Common`就足够了。

例如， [MultiplayerSample](https://github.com/o3de/o3de-multiplayersample) 使用并构建多人游戏 Gem 中的 MultiplayerComponent。因此，它定义了自己的 Client、Server 和 Unified 目标。

{{< note >}}
以下 CMake 示例是缩写。
{{< /note >}}

```cmake
    ly_add_target(
        NAME MultiplayerSample.Client.Static STATIC
        NAMESPACE Gem
        FILES_CMAKE
            multiplayersample_autogen_files.cmake
            multiplayersample_files.cmake
            ${pal_dir}/multiplayersample_${PAL_PLATFORM_NAME_LOWERCASE}_files.cmake
        BUILD_DEPENDENCIES
            PUBLIC
                Gem::DebugDraw
                Gem::PhysX
                Gem::Multiplayer
            PRIVATE
                Gem::Multiplayer.Client.Static
                Gem::PhysX.Static
                Gem::DebugDraw.Static
                Gem::ImGui.Static
        AUTOGEN_RULES
            *.AutoComponent.xml,AutoComponent_Header.jinja,$path/$fileprefix.AutoComponent.h
            *.AutoComponent.xml,AutoComponent_Source.jinja,$path/$fileprefix.AutoComponent.cpp
            *.AutoComponent.xml,AutoComponentTypes_Header.jinja,$path/AutoComponentTypes.h
            *.AutoComponent.xml,AutoComponentTypes_Source.jinja,$path/AutoComponentTypes.cpp
    )

    ly_add_target(
        NAME MultiplayerSample.Server.Static STATIC
        NAMESPACE Gem
        FILES_CMAKE
            multiplayersample_autogen_files.cmake
            multiplayersample_files.cmake
            ${pal_dir}/multiplayersample_${PAL_PLATFORM_NAME_LOWERCASE}_files.cmake
        BUILD_DEPENDENCIES
            PUBLIC
                Gem::PhysX
                Gem::Multiplayer
            PRIVATE
                Gem::Multiplayer.Server.Static
                Gem::PhysX.Static
        AUTOGEN_RULES
            *.AutoComponent.xml,AutoComponent_Header.jinja,$path/$fileprefix.AutoComponent.h
            *.AutoComponent.xml,AutoComponent_Source.jinja,$path/$fileprefix.AutoComponent.cpp
            *.AutoComponent.xml,AutoComponentTypes_Header.jinja,$path/AutoComponentTypes.h
            *.AutoComponent.xml,AutoComponentTypes_Source.jinja,$path/AutoComponentTypes.cpp
    )

    ly_add_target(
        NAME MultiplayerSample.Unified.Static STATIC
        NAMESPACE Gem
        FILES_CMAKE
            multiplayersample_autogen_files.cmake
            multiplayersample_files.cmake
            ${pal_dir}/multiplayersample_${PAL_PLATFORM_NAME_LOWERCASE}_files.cmake
        BUILD_DEPENDENCIES
            PUBLIC
                Gem::DebugDraw
                Gem::PhysX
                Gem::Multiplayer
            PRIVATE
                Gem::Multiplayer.Unified.Static
                Gem::PhysX.Static
                Gem::DebugDraw.Static
                Gem::ImGui.Static
        AUTOGEN_RULES
            *.AutoComponent.xml,AutoComponent_Header.jinja,$path/$fileprefix.AutoComponent.h
            *.AutoComponent.xml,AutoComponent_Source.jinja,$path/$fileprefix.AutoComponent.cpp
            *.AutoComponent.xml,AutoComponentTypes_Header.jinja,$path/AutoComponentTypes.h
            *.AutoComponent.xml,AutoComponentTypes_Source.jinja,$path/AutoComponentTypes.cpp
    )
```

同时，Multiplayer_ScriptCanvas只需要核心数据类型，因此它只使用`Multiplayer.Common`.

```cmake
    ly_add_target(
        NAME ${gem_name}.Static STATIC
        NAMESPACE Gem
        FILES_CMAKE
            scriptcanvas_multiplayer_files.cmake
            scriptcanvas_autogen_files.cmake
        BUILD_DEPENDENCIES
            PUBLIC
                Gem::ScriptCanvas
            PRIVATE
                Gem::Multiplayer.Common.Static
    )
```
### 条件编译

MultiplayerComponent 受条件编译的约束。这是使用宏`AZ_TRAIT_CLIENT` 和 `AZ_TRAIT_SERVER`完成的。特定于客户端的 logic 应该包装在前者中，而特定于服务器的 logic 应该包装在后者中。这种方法的动机是允许在 MultiplayerComponents 中使用特定于目标的逻辑，而不需要特定于目标的文件（即带或不带 BaseComponent 的 ServerComponent 和 ClientComponent）。

在多人游戏 Gem 的 cmake 中，观察每个目标是否根据目标启用或禁用这些特征。例如，Server 启用`AZ_TRAIT_SERVER`，同时禁用`AZ_TRAIT_CLIENT`。使用这些目标将带来宏定义。

{{< note >}}
以下 CMake 示例是缩写。
{{< /note >}}

```cmake
    ly_add_target(
        NAME Multiplayer.Client.Static STATIC
        NAMESPACE Gem
        FILES_CMAKE
            multiplayer_split_files.cmake
        COMPILE_DEFINITIONS
            PUBLIC
                AZ_TRAIT_CLIENT=1
                AZ_TRAIT_SERVER=0
        BUILD_DEPENDENCIES
            PUBLIC
                AZ::AzCore
                AZ::AzFramework
                AZ::AzNetworking
                Gem::Multiplayer.Common.Static
    )

    ly_add_target(
        NAME Multiplayer.Server.Static STATIC
        NAMESPACE Gem
        FILES_CMAKE
            multiplayer_split_files.cmake
        COMPILE_DEFINITIONS
            PUBLIC
                AZ_TRAIT_CLIENT=0
                AZ_TRAIT_SERVER=1
        BUILD_DEPENDENCIES
            PUBLIC
                AZ::AzCore
                AZ::AzFramework
                AZ::AzNetworking
                Gem::Multiplayer.Common.Static
    )

    ly_add_target(
        NAME Multiplayer.Unified.Static STATIC
        NAMESPACE Gem
        FILES_CMAKE
            multiplayer_split_files.cmake
        COMPILE_DEFINITIONS
            PUBLIC
                AZ_TRAIT_CLIENT=1
                AZ_TRAIT_SERVER=1
        BUILD_DEPENDENCIES
            PUBLIC
                AZ::AzCore
                AZ::AzFramework
                AZ::AzNetworking
                Gem::Multiplayer.Common.Static
    )
```

### AutoComponents

AutoComponents 使用 `AZ_TRAIT_SERVER` 和 `AZ_TRAIT_CLIENT`。根据组件元素的规范，它们将有条件地排除 logic。例如，给定一个在客户端调用并在服务器上处理的 RPC，则调用信号将包装在 `AZ_TRAIT_CLIENT` 中，而处理程序将包装在 `AZ_TRAIT_SERVER`中。从 AutoComponents 继承的类需要遵循这些用法才能正确编译。

请考虑以下 RPC：

```xml
    <RemoteProcedure Name="SendClientInput" InvokeFrom="Autonomous" HandleOn="Authority" IsPublic="true" IsReliable="false" GenerateEventBindings="false" Description="Client to server move / input RPC">
        <Param Type="Multiplayer::NetworkInputArray" Name="inputArray"  />
        <Param Type="AZ::HashValue32" Name="stateHash" />
    </RemoteProcedure>
```

这将生成以下 AutoComponent 签名：

```cpp
    //! SendClientInput Invocation
    //! Client to server move / input RPC
    //! HandleOn Authority
    #if AZ_TRAIT_CLIENT
    void SendClientInput(const Multiplayer::NetworkInputArray& inputArray, const AZ::HashValue32& stateHash);
    #endif

    #if AZ_TRAIT_SERVER
    //! SendClientInput Handler
    //! Client to server move / input RPC
    //! HandleOn Authority
    virtual void HandleSendClientInput(AzNetworking::IConnection* invokingConnection, const Multiplayer::NetworkInputArray& inputArray, const AZ::HashValue32& stateHash) {}
    #endif
```

从此 AutoComponent 继承并覆盖 HandleSendClientInput 的组件也需要将其类似地包装在 `AZ_TRAIT_SERVER` 中。
