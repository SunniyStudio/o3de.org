---
linkTitle: 项目配置
title: 将 Multiplayer Gem 添加到项目
description: 了解如何向 Open 3D Engine （O3DE） 项目或 Gem 添加多人游戏支持。包括创建占位符自动组件的说明。
weight: 200
---

当您在 **Open 3D Engine （O3DE）** 项目中需要多人游戏支持时，最简单的设置方法是使用下面的说明从 O3DE 多人游戏模板创建项目。

如果您的 O3DE 项目已在开发中，并且您想要为其添加多人游戏支持，则 [手动项目配置](#manual-project-configuration)上的说明将帮助您将 **多人游戏 Gem** 添加到现有项目。

## 从 Multiplayer 模板创建多人游戏项目 {#creating-project-from-template}

要从 O3DE 多人游戏模板创建多人游戏项目，您需要 O3DE 版本 22.10 或更高版本。您必须首先使用 **Project Manager** 将 [O3DE Extras GitHub repo](https://github.com/o3de/o3de-extras) 中的模板添加到您的 O3DE 模板库中。然后，您将能够从模板创建新项目。

1. 在 Project Manager中，选择 **New Project**，然后选择 **Create New Project**。有关使用 Project Manager 创建项目的帮助，请参考 [使用 Project Manager 创建项目](/docs/user-guide/project-config/project-manager/#creating-projects-using-project-manager)。

1. 选择 **Add remote template**。

1. 在 **Add a remote template** 对话框中，输入 `https://github.com/o3de/o3de-extras.git` 到 **Remote URL**中。然后点击 **Add**。这会将 GitHub 存储库注册为远程源。

    {{< image-width src="/images/user-guide/networking/multiplayer/add_extra_templates.png" width="700" alt="Enter O3DE Extras repo URL in Add Remote Template dialog box." >}}

1. 选择名为 **Multiplayer** 的新模板。这将为新项目选择模板。

1. 选择 **Download Template**（位于 New Project 窗口的右下角），然后选择将下载模板的位置。选择 **Download**。

1. 下载模板后，您可以使用其他 Gem 配置您的项目。当您准备好创建新项目时，请选择 **Create Project**。

1. 从项目的图标框中选择 **Build Project - Build Now** 构建项目。

1. 构建完成后，您的项目即可使用。你可以选择 **Open Editor** 在 **O3DE Editor**中打开项目。

1. 该项目带有一个`Demo`关卡，您可以从编辑器打开该关卡，然后使用 **Ctrl+G** 进行操作。

### 设置多人游戏模板的替代方法

要在不使用 Project Manager 的情况下设置多人游戏模板，您需要从 [O3DE Extras GitHub repo](https://github.com/o3de/o3de-extras) 下载模板，然后从命令行注册模板。

1. 从 O3DE Extras GitHub 存储库获取模板时，您可以选择下载标记的发行版或使用“git clone”从`development`分支获取最新的预发行代码。执行以下之一：

    a. 要从存储库下载发布版本，请打开 [O3DE Extras tags 页面]](https://github.com/o3de/o3de-extras/tags)。选择一个版本，然后选择以名称`Template_Multiplayer`开头的 zip 文件进行下载。将内容解压缩到您选择的目录中。

    b. 要从 `development` 分支下载最新的预发行代码，请在您的计算机上打开一个命令行窗口，然后使用`git clone`获取存储库的内容：

    ```cmd
    git clone https://github.com/o3de/o3de-extras.git
    ```

1. 打开一个命令行窗口，转到 O3DE 引擎目录，并使用`o3de register`命令注册多人游戏模板。

    ```cmd
    // Example using downloaded and unzipped release version.
    scripts\o3de.bat register -tp C:/Users/<YourUserName>/Downloads/template_multiplayer

    // Example using cloned o3de-extras repo.
    scripts\o3de.bat register -tp C:/o3de-extras/Templates/Multiplayer
    ```

1. 现在，您可以使用 Project Manager 或`o3de create-project`命令从此模板创建项目，如以下示例所示：

    ```cmd
    scripts\o3de.bat create-project -pp C:/o3de-projects/my-multiplayer-game -tn Multiplayer
    ```

## 手动项目配置

将多人游戏 Gem 的全部功能添加到 O3DE 项目需要编辑 CMake 脚本和源代码。这些更改支持：

* 链接到正确的核心库和 Gem。
* 构建 [auto-components](./autocomponents).
* 创建多人游戏组件描述符。
* 向多人游戏 Gem 注册组件。

{{< note >}}
由于 O3DE Gem 和项目都使用相同的 CMake 构建函数，因此您可以使用这些说明创建一个新的 Gem，以扩展多人游戏 Gem 的行为。
{{< /note >}}

### 启用 Multiplayer Gem

首先，在项目中添加并启用 Multiplayer Gem。有关帮助，请参阅 [在项目中添加和删除 Gem](/docs/user-guide/project-config/add-remove-gems/).

### 进行CMakeList.txt更改

确保 `<ProjectName>.Static` 目标包含正确的依赖项。
找到定义项目静态目标的 CMake 文件。例如，`<ProjectName>/Gem/Code/CMakeList.txt`。编辑`<ProjectName>.Static`目标，如下所示：
1. 在`FILES_CMAKE`部分，添加`<projectname>_autogen_files.cmake`。您将在后续步骤中创建此文件。
    ```cmake
        ly_add_target(
            NAME <ProjectName>.Static STATIC
            ...
            FILES_CMAKE
                ...
                <projectname>_autogen_files.cmake
    ```

1. 在 `BUILD_DEPENDENCIES PUBLIC` 部分中，添加 `AZ::AzNetworking`, `Gem::Multiplayer`, 和 `AZ::AzFramework`.
   ```cmake
    ly_add_target(
        NAME <ProjectName>.Static STATIC
        ...
        BUILD_DEPENDENCIES
            PUBLIC
                AZ::AzFramework
                AZ::AzNetworking
                Gem::Multiplayer
   ```

    {{< note >}}如果 `BUILD_DEPENDENCIES` 没包含 `PUBLIC`，添加它，如前面的代码示例所示。{{< /note >}}
1. 在`BUILD_DEPENDENCIES PRIVATE`中，添加`Gem::Multiplayer.Static`。
   ```cmake
    ly_add_target(
        NAME <ProjectName>.Static STATIC
        ...
        BUILD_DEPENDENCIES
            PRIVATE
                ...
                Gem::Multiplayer.Static
   ```
1. 同时，添加以下 `AUTOGEN_RULES` 到 `<ProjectName>.Static` 目标中:
   
   ```cmake
    ly_add_target(
        NAME <ProjectName>.Static STATIC
        ...
        AUTOGEN_RULES
            *.AutoComponent.xml,AutoComponent_Header.jinja,$path/$fileprefix.AutoComponent.h
            *.AutoComponent.xml,AutoComponent_Source.jinja,$path/$fileprefix.AutoComponent.cpp
            *.AutoComponent.xml,AutoComponentTypes_Header.jinja,$path/AutoComponentTypes.h
            *.AutoComponent.xml,AutoComponentTypes_Source.jinja,$path/AutoComponentTypes.cpp
   ```

在编辑 CMake 文件结束时， `<ProjectName>.Static` Target 应如下所示：

```cmake
ly_add_target(
    NAME <ProjectName>.Static STATIC
    NAMESPACE Gem
    FILES_CMAKE
        <projectname>_files.cmake
        <projectname>_autogen_files.cmake
    INCLUDE_DIRECTORIES
        PRIVATE
            Source
            .
        PUBLIC
            Include
    BUILD_DEPENDENCIES
        PUBLIC
            AZ::AzFramework
            AZ::AzNetworking
            Gem::Multiplayer
        PRIVATE
            ...
            Gem::Multiplayer.Static
    AUTOGEN_RULES
        *.AutoComponent.xml,AutoComponent_Header.jinja,$path/$fileprefix.AutoComponent.h
        *.AutoComponent.xml,AutoComponent_Source.jinja,$path/$fileprefix.AutoComponent.cpp
        *.AutoComponent.xml,AutoComponentTypes_Header.jinja,$path/AutoComponentTypes.h
        *.AutoComponent.xml,AutoComponentTypes_Source.jinja,$path/AutoComponentTypes.cpp
)
```

### 添加 AutoGen CMake 文件

接下来，创建一个名为 `<projectname>_autogen_files.cmake`  的新文件，并将其放在项目的 code 文件夹中。例如：`<ProjectName>/Gem/Code/<projectname>_autogen_files.cmake`。此文件的内容将 [auto-components](./autocomponents)的源模板添加到项目构建中。

```cmake
set(FILES
    ${LY_ROOT_FOLDER}/Gems/Multiplayer/Code/Include/Multiplayer/AutoGen/AutoComponent_Common.jinja
    ${LY_ROOT_FOLDER}/Gems/Multiplayer/Code/Include/Multiplayer/AutoGen/AutoComponent_Header.jinja
    ${LY_ROOT_FOLDER}/Gems/Multiplayer/Code/Include/Multiplayer/AutoGen/AutoComponent_Source.jinja
    ${LY_ROOT_FOLDER}/Gems/Multiplayer/Code/Include/Multiplayer/AutoGen/AutoComponentTypes_Header.jinja
    ${LY_ROOT_FOLDER}/Gems/Multiplayer/Code/Include/Multiplayer/AutoGen/AutoComponentTypes_Source.jinja
)
```

### 添加占位符自动组件

{{< known-issue link="https://github.com/o3de/o3de/issues/4058">}}
如果您已启用多人游戏自动组件，但尚未创建任何自动组件，则可能会遇到构建失败。解决方法是，请按照本节中的步骤创建占位符自动组件。
{{< /known-issue >}}

1. 在项目的`Code\Source\` 目录下，创建一个名为`AutoGen`的新文件夹。
   {{< note >}}这个 AutoGen 目录不一定是临时的。所有未来的多人游戏自动组件都可以位于此处。{{< /note >}}
1. 在`Code\Source\AutoGen`下，创建一个名为`MyFirstNetworkComponent.AutoComponent.xml`的新占位符自动组件文件。
   {{< note >}}本指南使用“MyFirstNetworkComponent”作为这个多人游戏自动组件的名称。您可以为组件指定任何名称，但请务必始终使用该名称。{{< /note >}}

1. 修改 `Code\Source\AutoGen\MyFirstNetworkComponent.AutoComponent.xml` 可包含以下内容：
    ```xml  
    <?xml version="1.0"?>

    <Component
        Name="MyFirstNetworkComponent"
        Namespace="<ProjectName>"
        OverrideComponent="false"
        OverrideController="false"
        OverrideInclude=""
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    </Component>
    ```
    {{< important >}}使用项目名称替换 `<ProjectName>`， 并确保该值用引号括起来。
    {{< /important >}}
1. 通过更新 `<projectname_files.cmake>` 向 CMake 注册占位符自动组件>。
    ```cmake
    set(FILES
        ...
        Source/AutoGen/MyFirstNetworkComponent.AutoComponent.xml
    )
    ```
{{< note >}}完成本指南中的设置步骤后，您可以删除占位符自动组件并创建新的自动组件，或将其用作起点。必须始终至少有一个 auto-component 。

要了解有关多人游戏自动组件的更多信息，请参阅 [多人游戏自动组件](../autocomponents)或按照介绍性的 [多人游戏教程](/docs/learning-guide/tutorials/multiplayer/first-multiplayer-component/) 进行操作。{{< /note >}}

### 进行Module.cpp更改

要使用多人游戏功能，您必须对项目的源代码进行一些小的更改，以便为多人游戏组件生成描述符。

对项目的 `Code/Source/<ProjectName>Module.cpp`进行以下更改：
1. 在文件顶部包含 `AutoComponentTypes.h` ：
    ```cpp
    #include <Source/AutoGen/AutoComponentTypes.h>
    ```

1. 编辑 `<ProjectName>Module` 构造函数以创建组件描述符，从而允许注册多人游戏组件。
    ```cpp
    MultiplayerSampleModule()
        : AZ::Module()
    {
        ...
        CreateComponentDescriptors(m_descriptors); //< Add this line to register your project's multiplayer components
    }
    ```
    {{< important >}}确保对  `CreateComponentDescriptors()` 的调用是构造函数的 *最后一行* 。
    {{< /important >}}

### 进行SystemComponent.cpp更改

最后，在添加代码以生成描述符后，您必须向 Multiplayer Gem 注册多人游戏组件。

对项目的 `Code/Source/<ProjectName>SystemComponent.cpp` 文件进行以下更改。
1. 在文件顶部包含 `AutoComponentTypes.h` 。
    ```cpp
    #include <Source/AutoGen/AutoComponentTypes.h>
    ```
2. 通过更新 `Activate()` 函数，向 Gem 注册多人游戏组件。

    ```cpp
    ...
    void <ProjectName>SystemComponent::Activate()
    {
        ...
        RegisterMultiplayerComponents(); //< Register our project's multiplayer components to assign NetComponentIds
    }
    ```

### 重新构建项目

编辑 CMake 和 C++ 文件后，始终需要配置和构建。您可以使用 [Project Manager](/docs/user-guide/project-config/project-manager/)重新构建，或通过 [命令行界面 （CLI）](/docs/user-guide/build/configure-and-build/) 进行配置和构建。
