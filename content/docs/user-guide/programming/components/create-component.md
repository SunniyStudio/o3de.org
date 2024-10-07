---
linkTitle: 创建组件
title: 在O3DE中创建组件
description: 学习如何在C++中创建Open 3D Engine (O3DE)组件。
weight: 200
---

**Open 3D Engine (O3DE)**中的**组件**是一个简单的类，它继承自 O3DE 的 `AZ::Component`。组件的行为由其反映的数据和激活时的动作决定。本节将向您展示如何以编程方式创建 O3DE 组件。有关在**O3DE 编辑器**中添加和自定义组件的信息，请参阅[组件参考](/docs/user-guide/components/reference)。

虽然每个组件都是独一无二的，但创建新组件的典型步骤如下：

1. 从默认组件模板或现有组件中创建组件类。
1. 在父 Gem 模块中注册组件。
1. 使用`Init()`, `Activate()`, 和 `Deactivate()`函数实现组件类接口。
1. 反射组件数据，以便进行序列化和编辑。
1. 反射组件数据和方法进行脚本化。
1. 定义组件服务，如提供的服务和所需的服务。

## 创建组件类

使用 O3DE `scripts` 目录中的 `o3de` CLI 脚本从默认组件模板中创建一个通用运行时组件。对于编辑器组件和系统组件，你可以分别在 [编辑器组件](editor-components) 和 [系统组件](system-components)中找到它们的模板代码。

1. 使用[`create-from-template`](/docs/user-guide/project-config/cli-reference/#create-from-template) 命令在目标目录下创建组件。

    {{< tabs name="从模板创建组件" >}}
    {{% tab name="Windows" %}}

格式:

```cmd
scripts\o3de.bat create-from-template -dp <destination-dir> -dn <component-name> -tn DefaultComponent -kr -r ${GemName} <gem-name>
```

{{< tip >}}
如果希望保留默认许可证，请加入 `-kl` 参数。
{{< /tip >}}

模板会将 “Component（组件）”添加到您提供的组件名称中。

提供组件的命名空间作为 `${GemName}` 占位变量的替代值。在许多情况下，该命名空间与 Gem 名称相同。

例如，要在 `MyGem` 命名空间中创建名为 `MyTestComponent` 的组件，请执行以下操作：

```cmd
scripts\o3de.bat create-from-template -dp MyCode -dn MyTest -tn DefaultComponent -kr -r ${GemName} MyGem
```

{{< note >}}
在 Windows PowerShell 中使用替换参数时，必须在任何 `$` 替换变量周围使用单引号。例如：`-r '${GemName}' MyGem`。
{{< /note >}}

要在现有的目录中创建组件，比如正在进行的 Gem 的 `Code` 目录，可以在 `create-from-template` 命令中添加 `-f` 选项，强制在那里创建组件文件。

例如，要在 Gem 的 `Code` 目录中的 `MyGem` 命名空间创建名为 `MyTestComponent` 的组件，请执行以下操作：

```cmd
scripts\o3de.bat create-from-template -dp C:\Gems\MyGem\Code -dn MyTest -tn DefaultComponent -kr -r ${GemName} MyGem -f
```

    {{% /tab %}}
    {{% tab name="Linux" %}}

格式:

```bash
scripts/o3de.sh create-from-template -dp <destination-dir> -dn <component-name> -tn DefaultComponent -kr -r ${GemName} <gem-name>
```

{{< tip >}}
如果希望保留默认许可证，请加入 `-kl` 参数。
{{< /tip >}}

模板会将 “Component（组件）”添加到您提供的组件名称中。

提供组件的命名空间作为 `${GemName}` 占位变量的替代值。在许多情况下，该命名空间与 Gem 名称相同。

例如，要在 `MyGem` 命名空间中创建名为 `MyTestComponent` 的组件，请执行以下操作：

```bash
scripts/o3de.sh create-from-template -dp MyCode -dn MyTest -tn DefaultComponent -kr -r ${GemName} MyGem
```

要在现有的目录中创建组件，比如正在进行的 Gem 的 `Code` 目录，可以在 `create-from-template` 命令中添加 `-f` 选项，强制在那里创建组件文件。

例如，要在 Gem 的 `Code` 目录中的 `MyGem` 命名空间创建名为 `MyTestComponent` 的组件，请执行以下操作：

```bash
scripts/o3de.sh create-from-template -dp Gems/MyGem/Code -dn MyTest -tn DefaultComponent -kr -r ${GemName} MyGem -f
```

    {{% /tab %}}
    {{< /tabs >}}

1. 默认组件模板会生成三个源文件。如果您没有在所需的源代码目录中创建这些文件，请立即将它们移至该目录：

    * `Include/<gem-name>/<component-name>Interface.h`
    * `Source/<component-name>Component.cpp`
    * `Source/<component-name>Component.h`

   通常情况下，总线接口头被放置在 Gem 的公共 include 目录中：`Code/Include/<gem-name>`。两个组件源文件放在 Gem 的 `Code/Source`目录下。

    {{< note >}}
   如果决定将接口头放在与 `Include/<gem-name>` 不同的目录中，则必须调整组件头中的 include 路径。
    {{< /note >}}

1. 将接口头添加到 Gem 的 API CMake 源文件列表中。例如：`<Gem>/Code/mygem_api_files.cmake`.

1. 将这两个组件文件添加到 Gem 的私有 CMake 源文件列表中。例如：`<Gem>/Code/mygem_private_files.cmake`.

## 注册组件

注册组件可使系统的模块管理器为组件提供基本服务。其中一些服务包括根据依赖关系以适当顺序加载系统组件，以及将类型信息与各种反射上下文关联起来。

要注册组件，必须在 Gem 的 `AZ::Module` 实现的构造函数中添加组件描述符。

1. 找到你的 Gem 的`AZ::Module`类。如果你使用默认的 Gem 模板创建了 Gem，请在 `<Gem>/Code/Source/MyGemModuleInterface.h`中查找。

1. 在类的实现中包含组件的头文件。

    示例:

    ```cpp
    #include <MyComponent.h>
    ```

1. 在类构造函数中，添加对组件静态 `CreateDescriptor` 函数的调用。

    示例:

    ```cpp
    MyGemModuleInterface()
    {
        m_descriptors.insert(m_descriptors.end(), {
            // ...
            MyComponent::CreateDescriptor()
        });
    }
    ```

有关 Gem 模块系统的更多信息，请参阅 [Gem 模块系统](/docs/user-guide/programming/gems/overview) 中的概述。

## 实现组件类接口

要实现组件接口，请从以下步骤开始，了解组件类的必备和可选成员：

1. 继承组件基类。

    每个组件都必须在其继承祖先的某个地方包含 `AZ::Component`。非编辑器组件通常直接继承自 `AZ::Component`，如下例所示：

    ```cpp
    class MyComponent
        : public AZ::Component
    ```

    编辑器组件通常继承自 `EditorComponentBase`。通过这些组件，您可以拥有不同于运行时所需的编辑器特定功能。您可以在编辑器组件中实现编辑器功能，在运行时组件中实现运行时功能。有关更多信息和其他实现要求，请参阅 [编辑器组件](./editor-components.md)。

    编辑器组件类示例：

    ```cpp
    class MyEditorComponent
        : public AzToolsFramework::Components::EditorComponentBase
    ```

   您还可以创建自己的组件类层次结构，以提供更多的组件类型。

1. 使用 `AZ_COMPONENT` 宏来定义组件的通用唯一标识符（UUID）。宏包含两个参数：

    * 组件类型名称。为避免名称冲突，我们建议您在任何类型的 `AZ_RTTI` 宏（如 `AZ_COMPONENT`）中使用命名空间。

    * 唯一的 UUID。您可以使用任何 UUID 生成器来生成该值。Visual Studio 通过**工具**、**创建 GUID** 提供了这一功能。使用**注册表格式**设置，然后复制并粘贴生成的值。
      下面是一个 `AZ_COMPONENT` 宏示例：

    ```cpp
    AZ_COMPONENT(MyGem::MyComponent, "{0C09F774-DECA-40C4-8B54-3A93033EC381}");
    ```

1. 覆盖基类函数。

    要定义组件的行为，需要覆盖三个 `AZ::Component`函数：`Init`, `Activate`, 和 `Deactivate`：

    ```cpp
    void Init() override       {} // optional
    void Activate() override   {}
    void Deactivate() override {}
    ```

   这些函数说明如下：

    * `Init()`

      (可选）只为给定实体调用一次，以初始化组件的内部状态。虽然`Init()`函数初始化了组件，但在系统调用组件的`Activate()`函数之前，组件是不活动的。我们建议您在组件处于非活动状态时尽量减少组件的 CPU 和内存开销。

    * `Activate()`

        (必须）当拥有实体被激活时调用，前提是该组件依赖的所有服务和组件都存在并处于激活状态。`Activate()`函数总是在它所依赖的任何组件**之后**被调用。有关如何指定依赖关系的信息，请参阅 [定义和使用组件服务](services)。通常在`Activate()`函数中，组件会执行设置程序、连接到**事件总线（EBus）**、分配资源或请求资产。

    * `Deactivate()`

      (必填）当拥有实体停用时调用。停用的顺序与激活相反，因此您的组件会先于其依赖的组件停用。作为最佳实践，应确保停用时组件的占用空间最小。组件应释放所有资源，并断开与所有 EBus 的连接。一般来说，停用应与激活对称。

      停用后不一定会销毁。一个实体可以在不被销毁的情况下被停用和重新激活，因此你应该确保你的组件能有效地支持这种行为。最终，当一个实体被销毁时，O3DE 会首先调用 `Deactivate()`。在编写组件时要注意这一点。

1. 根据需要在静态 `Reflect()` 函数中使用反射上下文，使系统的其他部分也能使用你的组件对象。

   所有组件都是 AZ 反射类。由于所有组件都必须是可序列化和可编辑的，因此它们必须包含一个 `Reflect()` 函数，如下例所示：

    ```cpp
    // Required Reflect function.
    static void Reflect(AZ::ReflectContext* context);
    ```

   `Reflect()`函数也是向脚本系统（如 Script Canvas 和 Lua）公开方法、属性和事件的地方。

   更多信息，请参阅[反射组件以便序列化和编辑](reflection/reflecting-for-serialization)。

1. 执行组件服务功能，以定义提供的、依赖的、需要的和不兼容的服务。

   要定义这些逻辑服务，请使用以下功能：

    ```cpp
    // Optional functions for defining provided and dependent services.
    static void GetProvidedServices(AZ::ComponentDescriptor::DependencyArrayType& provided);
    static void GetDependentServices(AZ::ComponentDescriptor::DependencyArrayType& dependent);
    static void GetRequiredServices(AZ::ComponentDescriptor::DependencyArrayType& required);
    static void GetIncompatibleServices(AZ::ComponentDescriptor::DependencyArrayType& incompatible);
    ```

   有关如何实施这些组件服务的详情，请参阅[定义和使用组件服务](services)。
