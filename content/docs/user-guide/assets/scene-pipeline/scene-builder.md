---
linkTitle: Scene Builder
title: Scene Builder
description: 与场景管道交互的专用资产生成器。
weight: 400
toc: true
---

通过创建 `AZ::SceneAPI::SceneCore::LoadingComponent`、 `AZ::SceneAPI::SceneCore::GenerationComponent`、 `AZ::SceneAPI::SceneCore::BehaviorComponent`和 `AZ::SceneAPI::SceneCore::ExportingComponent`组件，将场景构建逻辑存储在宝石中。这些 gem 被视为场景构建器 gem。

SceneAPI 分三个主要阶段处理源场景资产：加载、生成和导出。在加载阶段，通常从 FBX 等源文件填充场景图。这是通过 LoadingComponent 组件完成的。LoadingComponent 组件有三个不同的事件可以执行工作： 导入前（PreImport）、导入（Import）和导入后（PostImport）。在 Import（导入）事件中，SceneGraph 会被填充，但 PostImport（导入后）事件会接收一个不可变的 SceneGraph 对象，因此在 PostImport（导入后）事件中，SceneGraph 的内容是固定的。

在导出阶段，各种场景构建器会检查场景图，并将可用于游戏的产品资产写入磁盘。这是通过 ExportingComponent 组件完成的。ExportingComponent 组件响应的所有事件都会接收一个不可变的 SceneGraph 对象，因此它们也不能修改 SceneGraph。这意味着只有在导入事件中才能写入图形。

此外，每当用户要在编辑器中编辑 “场景设置 ”时，场景都会通过 LoadingComponent 组件进行处理，因此，如果总是在用户要编辑设置时运行网格优化器，则会导致效果不理想。为了让网格优化器在导出之前运行，我们调用了生成事件。在生成阶段，组件可以响应场景生成事件，并对场景图进行任意变换。网格优化器生成组件会识别清单中网格组选择的所有网格，并将优化后的网格添加到场景图中，以便其他场景构建器可以引用。

通过这种设计，我们可以对场景图进行其他类型的修改处理，如网格分割或动态动画生成。

## 创建场景生成器 gem

场景构建器框架在称为构建器 gem 的工具 gem 中使用。gem 添加的类派生于场景构建组件基类：`AZ::SceneAPI::SceneCore::LoadingComponent`、`AZ::SceneAPI::SceneCore::GenerationComponent`、`AZ::SceneAPI::SceneCore::BehaviorComponent`或`AZ::SceneAPI::SceneCore::ExportingComponent`。新组件使用`BindToCall'方法挂钩所需的场景构建事件。最后，类描述符被添加到 gem 模块描述中。

### 场景构建类示例

所有场景构建组件都源自其中一个基础场景构建类。

```cpp
class TheLoadingComponent
    : public AZ::SceneAPI::SceneCore::LoadingComponent
{
public:
    AZ_COMPONENT(TheLoadingComponent, "{E5E65E21-0BCD-4874-84B8-33E10CCAEE94}", AZ::SceneAPI::SceneCore::LoadingComponent);

    TheLoadingComponent();
    ~TheLoadingComponent() override = default;

    void Activate() override;
    void Deactivate() override;
    static void Reflect(AZ::ReflectContext* context);

    AZ::SceneAPI::Events::ProcessingResult OnPostImportEventContext(AZ::SceneAPI::Events::PostImportEventContext& context);
};
```

该类通过从 `AZ::SceneAPI::SceneCore::LoadingComponent` 子类，然后添加一个接受 PreExportEventContext 上下文的 OnPostImportEventContext 方法，以挂钩 PostImportEventContext 事件。

```cpp
TheLoadingComponent::TheLoadingComponent()
{
    BindToCall(&TheLoadingComponent::OnPostImportEventContext);
}
```

该示例将 OnPostImportEventContext 函数附加到 OnPostImportEventContext 上，以便在每次转换和导入过程开始时将此上下文发送到 CallProcessorBus。

场景导入、转换和导出过程使用 CallProcessorBus 来移动数据并触发其他工作。CallProcessorBus 的运行方式与典型的 EBuses 不同，因为它没有一组可以调用的特定函数。相反，它的工作方式类似于伪远程过程调用，通常函数的参数存储在上下文中。CallProcessorBus 提供了一个用于注册和触发上下文调用的地方。根据上下文的类型，将执行相应的功能。为了方便使用，一个名为 CallProcessorBinder 的绑定层允许绑定到一个将上下文作为参数并执行所有路由的函数。这种方法的好处之一是，它提供了多个地方来挂接自定义代码，而无需更新现有代码。

```cpp
void TheLoadingComponent::Reflect(AZ::ReflectContext* context)
{
    AZ::SerializeContext* serializeContext = azrtti_cast<AZ::SerializeContext*>(context);
    if (serializeContext)
    {
        serializeContext->Class<TheLoadingComponent, AZ::SceneAPI::SceneCore::LoadingComponent>()->Version(1);
    }
}
```

该类将 TheLoadingComponent 反映为 LoadingComponent，这样场景管道就能在导出事件中找到该组件。

```cpp
class SceneBuilderExampleModule
    : public AZ::Module
{
public:
    AZ_RTTI(SceneBuilderExampleModule, "{36AA9C0F-7976-40C7-AF54-C382AC5B16F6}", AZ::Module);

    SceneBuilderExampleModule()
        : AZ::Module()
    {
        m_descriptors.insert(m_descriptors.end(), 
        {
            TheLoadingComponent::CreateDescriptor()
        });
    }
};
```

场景生成器组件在```AZ::Module````类中注册。SceneBuilderExampleModule 是 gem 的入口点。要扩展 SceneAPI，必须在这里注册加载、生成和导出组件。SceneAPI 库需要专门的初始化。对于任何要使用的 SceneAPI，请务必尽早重复以下两行。省略这些调用或过晚调用可能会导致问题，如丢失 EBus 事件。

## LoadingComponent

LoadingComponent 事件在场景管道导入源场景资产文件（如 `.fbx` 或 `.stl` 文件）时发生。如果场景生成器想修改场景图，则应注册一个 LoadingComponent 组件，以便挂钩到 `PreImport`,`Import` 或 `PostImport` 事件。场景图是在导入事件期间填充的，因此在导入后，场景图的内容是固定的。

AZ::SceneAPI::SceneCore::LoadingComponent事件上下文：

* PreImportEventContext -- 表示即将导入场景图
* ImportEventContext -- 表示场景已准备就绪，可以从源数据导入场景图；可以更改场景
* PostImportEventContext -- 表示导入已完成，数据可以使用（如果没有错误）；场景不可变

## GenerationComponent

场景通过 LoadingComponent 组件加载。当场景流水线希望场景构建器修改节点内容或向 SceneGraph 添加节点时，就会发出 GenerationComponent 事件。每个 GenerationComponent 事件都会发送可变场景对象。

AZ::SceneAPI::SceneCore::GenerationComponent 事件上下文：

* PreGenerateEventContext -- 场景生成步骤即将发生的信号
* GenerateEventContext -- 向 “场景 ”中添加新数据（如程序生成的对象）的信号
* GenerateLODEventContext -- 场景中应添加新 LOD 的信号
* GenerateAdditionEventContext -- 将 UV、切线和位切线等新数据添加到 “场景 ”的信号
* GenerateSimplificationEventContext -- 应运行数据简化/复杂性降低的信号
* PostGenerateEventContext -- 生成步骤完成的信号

## ExportComponent

在导出阶段，各种场景构建器会检查场景图，并将可用于游戏的产品资产写入磁盘。这是通过 ExportingComponent 组件完成的。ExportingComponent 组件响应的所有事件都会接收一个不可变的 SceneGraph 对象，因此它们也不能修改 SceneGraph。这意味着只有在导入和生成事件期间，图形才是可写的。

`AZ::SceneAPI::SceneCore::ExportComponent` 事件上下文：

* `PreExportEventContext` - 表示即将导出所包含的场景。
* `ExportEventContext` - 提示场景需要将包含的场景导出到指定目录。
* `PostExportEventContext` - 导出完成并写入（如果成功）到指定目录的信号。

## BehaviorComponent

`AZ::SceneAPI::SceneCore::BehaviorComponent` 组件是小逻辑单元，只要场景管道初始化和激活，它们就会存在。这些组件可以对场景中发生的各种事件做出反应，并进行适当的更改、添加或删除。行为组件的主要用途是修改场景清单中的规则和组，以便间接修改导出组件。

行为组件（BehaviorComponent）的处理方式与其他场景管道组件类型有些不同，因为它不使用 BindCall() 方法，而是同时参与编辑器和资产处理器。行为组件旨在根据资产使用的场景产品激活 ManifestMetaInfoBus 和/或 ManifestMetaInfoBus。

## AssetImportRequestBus

该总线处理修改场景清单的场景管道事件。

`AZ::SceneAPI::Events::AssetImportRequestBus` 事件：

* `GetGeneratedManifestExtension` - 获取生成的清单的文件扩展名
* `PrepareForAssetLoading` - 在资产加载开始前，将调用此调用以进行所需的初始化
* `LoadAsset` - 在给定场景中的给定路径处开始加载资产
* `FinalizeAssetLoading` - 可用于完成加载过程中的任何工作，例如调整场景图中已加载的内容
* `UpdateManifest` - 所有装载工作完成后，可以使用此调用来调整舱单。

``RequestingApplication`` 输入参数指发送事件的应用程序类型，如 O3DE 编辑器或 O3DE 资产处理器。

``ManifestAction``输入参数指应用程序从组件请求的行为类型。``ConstructDefault``值通常用于 O3DE 编辑器，为源场景生成默认场景清单条目。``Update``值用于指示行为组件应根据现有场景图生成场景清单组和规则。

## ManifestMetaInfoBus

ManifestMetaInfoBus 用于收集和管理源资产场景文件的场景清单中的用户体验元素。例如，它在编辑器中用于获取组类型选项卡的名称以及组在场景设置对话框中可管理的规则。

`AZ::SceneAPI::Events::ManifestMetaInfoBus` 事件：

* `GetCategoryAssignments` - 获取所有类别和该类别所列类别标识符的列表。
* `GetIconPath` - 获取与给定组或规则相关的图标路径。
* `GetAvailableModifiers` - 获取目标接受的修改器（如分组规则）列表。
* `InitializeObject` - 根据场景初始化给定的清单组或规则。
* `ObjectUpdated` - 更新现有组或规则时调用。

## 场景日志示例

[Scene logging example gem](https://github.com/o3de/o3de/tree/development/Gems/SceneLoggingExample)

场景日志示例演示了如何通过在管道中添加额外日志来扩展 SceneAPI。SceneAPI 是一组库，用于处理加载场景文件并将内容转换为Open 3D Engine及其编辑器可以加载的数据。

使用的方法如下：

1. SceneBuilder 和 SceneData 会加载场景文件（例如 .fbx）并将其转换为存储在内存中的图形。
2. SceneCore 和 SceneData 用于创建一个清单，其中包含有关如何导出文件的说明。
3. SceneData 分析清单和内存图形并创建默认值。
4. 场景设置允许通过用户界面更新清单。
5. ResourceCompilerScene 使用清单中的指令和内存图中的数据创建资产。这些资产可供 Open 3D Engine使用。

gem 示例演示了以下主要功能：

* 初始化 SceneAPI 库。(参见 SceneLoggingExampleModule.cpp）
* 添加一个 LoadingComponent 来挂钩场景加载并对加载事件作出反应。(参见Processing/LoadingTrackingProcessor）
* 扩展场景设置用户界面，使用行为组件设置默认值。(请参阅 Groups/LoggingGroup 和 Behaviors/LoggingGroupBehavior）
* 添加导出组件（ExportingComponent）以挂钩场景转换和导出事件。(请参阅Behaviors/ExportTrackingProcessor）。
