---
title: "[Atom] 场景和渲染管线"
description: ""
toc: false
---

Atom RPI 中的 Scene 和 Render Pipeline 可以更轻松地在不同的视口中渲染不同的 3D 内容。
场景包含具有要渲染的数据的特征处理器列表。渲染管道定义如何渲染场景。
作为一个起点，要使用 RPI 系统渲染一些数据，你至少需要创建一个场景，并创建一个渲染管道添加到这个场景中。

每个场景都与其他场景独立。一个场景可能有许多渲染管道。将渲染管道添加到场景时，它会定义一种渲染此场景的方法。

在本文档中，您将学习如何创建场景和渲染管道，以及如何将它们用于一些典型的用例。

# 创建 RPI 场景

在创建 RPI::Scene 实例之前，您需要确定要在此场景中启用哪种类型的图形功能，以便您可以在创建场景时使用的 RPI::SceneDescriptor 中添加功能处理器。

注意：您尝试启用的这些功能处理器需要在已启用的 Gem 中找到。 

启用特征处理器的另一种方法是在创建场景后立即使用 EnableAllFeatureProcessors。此功能启用场景的所有可用 （Feature processor registered） 特征处理器。

创建场景并启用特征处理器后，需要将其激活并注册到 RPISystem，否则将无法正确更新或渲染。

```cpp
// Create a scene with all available feature processors
RPI::SceneDescriptor sceneDesc;
m_scene = RPI::Scene::CreateScene(sceneDesc);
m_scene->EnableAllFeatureProcessors();
m_scene->Activate(); 
RPI::RPISystemInterface::Get()->RegisterScene(m_scene);
        
// Create a scene with only DynamicDraw and AuxGeom features enabled
RPI::SceneDescriptor sceneDesc2;
sceneDesc2.m_featureProcessorNames.push_back("DynamicDrawFeatureProcessor");
sceneDesc2.m_featureProcessorNames.push_back("AuxGeomFeatureProcessor");
m_scene2 = RPI::Scene::CreateScene(sceneDesc);
m_scene2->Activate();
RPI::RPISystemInterface::Get()->RegisterScene(m_scene2);
```
注意：特征处理器名称是特征处理器类声明其AZ_RTTI的名称。例如，TransformServiceFeatureProcessor 的名称是 “AZ::Render::TransformServiceFeatureProcessor”，该类的AZ_RTTI如下所示：
```
AZ_RTTI(AZ::Render::TransformServiceFeatureProcessor, "{D8A2C353-2850-42F8-AA21-3979CBECBF80}", AZ::Render::TransformServiceFeatureProcessorInterface);
```

# 将 AzFramework 场景与 RPI 场景关联

Lumberyard 级别由具有组件的实体组成。为不同的“世界”创建的实体属于不同的 AzFramework 场景 （AzFramework：：Scene）。要呈现每个 AzFramework 场景的图形内容，我们需要创建与 RPI 场景的一对一关系。

请注意，如果您尝试渲染的图形内容不是来自零部件，而是直接从功能处理器创建的，则可以跳过此步骤。

对于现有的 AzFramework 场景 （例如，由 Lumberyard 创建的用于其关卡的默认场景），您可以使用以下代码获取该场景并将其与新创建的 RPI 场景相关联：

```cpp
AZ::RPI::ScenePtr m_scene;

// Get all created AzFramework::Scenes
AZStd::vector<AzFramework::Scene*> scenes;
AzFramework::SceneSystemRequestBus::BroadcastResult(scenes, &AzFramework::SceneSystemRequests::GetAllScenes);
AZ_Assert(scenes.size() > 0, "Error: Scenes missing during system component initialization"); // This should never happen unless scene creation has changed.
// Add RPI::Scene as a sub system for the default AzFramework Scene
const uint32_t DefaultAzSceneIndex = 0;
scenes[DefaultAzSceneIndex]->SetSubsystem(m_scene.get());       
```
有时，您可能希望创建一个新的 AzFramework 场景，以便为一组实体创建一个单独的世界并渲染它们。例如，Lumberyard Editor 中材质的预览窗口。此预览窗口与关卡窗口是独立的，您希望为此预览窗口设置自己的光照和后期效果。创建一个单独的 AzFramework 场景是有意义的，其中包含用于预览的实体。

要创建此类 AzFramework 场景，您需要先创建一个 AzFramework 实体上下文 （AzFrameWork::EntityContext），并使新创建的 AzFramework 场景使用此 EntityContext。然后，使用此实体上下文创建的任何实体都属于此场景。

下面演示了如何从头开始创建 AzFramework 场景：
```cpp
AZ::RPI::ScenePtr m_scene;
AzFramework::Scene* m_frameworkScene;
AZStd::unique_ptr<AzFramework::EntityContext> m_entityContext;


// Create RPI::Scene
RPI::SceneDescriptor sceneDesc;
m_scene = RPI::Scene::CreateScene(sceneDesc);
m_scene->EnableAllFeatureProcessors();
m_scene->Activate();
RPI::RPISystemInterface::Get()->RegisterScene(m_scene);

// Create a new EntityContext and AzFramework::Scene, and link them together via SetSceneForEntityContextId
m_entityContext = AZStd::make_unique<AzFramework::EntityContext>();
m_entityContext->InitContext();
Outcome<AzFramework::Scene*, AZStd::string> createSceneOutcome = Failure<AZStd::string>("SceneSystemRequests bus not responding.");
AzFramework::SceneSystemRequestBus::BroadcastResult(createSceneOutcome, &AzFramework::SceneSystemRequests::CreateScene, m_sceneName);
AZ_Assert(createSceneOutcome, "%s", createSceneOutcome.GetError().data());
m_frameworkScene = createSceneOutcome.GetValue();
bool success = false;
AzFramework::SceneSystemRequestBus::BroadcastResult(success, &AzFramework::SceneSystemRequests::SetSceneForEntityContextId, m_entityContext->GetContextId(), m_frameworkScene);

// Link RPI::Scene to the AzFramework::Scene
m_frameworkScene->SetSubsystem(m_scene.get());
```
# 创建渲染管道并将其添加到场景

渲染管道定义如何渲染场景。您可以为一个场景添加多个渲染管道。渲染管道可以通过包含 RenderPipelineDescriptor 的 .azasset 创建，也可以直接从 RenderPipelineDescriptor 创建。

一个场景具有多个渲染管道的一种常见情况是，关卡编辑器具有不同的视区，显示要同时使用不同的摄像机渲染关卡的视区，例如透视、顶部、正面和左侧。使用 Atom 的 RPI 可以很容易地设置。

以下代码使用两种不同的方法设置两个渲染管道，并将它们添加到场景中。每个渲染管线渲染到不同的窗口。
```cpp
AZStd::m_defaultPipelineAssetPath; // The azasset's path in cache
AZ::RPI::WindowContext* m_windowContext1; // The window context for a native window 1
AZ::RPI::WindowContext* m_windowContext2; // The window context for a native window 2

// Create a render pipeline from an asset
Data::Asset<RPI::AnyAsset> pipelineAsset = RPI::AssetUtils::LoadAssetByProductPath<RPI::AnyAsset>(m_defaultPipelineAssetPath.c_str(), RPI::AssetUtils::TraceLevel::Error);
RPI::RenderPipelinePtr renderPipeline1 = RPI::RenderPipeline::CreateRenderPipelineForWindow(pipelineAsset, *m_windowContext1);
m_scene->AddRenderPipeline(renderPipeline);

// Create a render pipeline from a descriptor
RPI::RenderPipelineDescriptor pipelineDesc;
pipelineDesc.m_mainViewTagName = "MainCamera";          //Surface shaders render to the "MainCamera" tag
pipelineDesc.m_name = "SecondPipeline";                 //Sets the unique name for this pipeline
pipelineDesc.m_rootPassTemplate = "ComplexPipeline";    //References a template in %Project%\Passes\PassTemplates.azasset
RPI::RenderPipelinePtr renderPipeline2 = RPI::RenderPipeline::CreateRenderPipelineForWindow(pipelineDesc, *m_windowContext2);
m_scene->AddRenderPipeline(m_pipeline2)
```
渲染管道不一定总是渲染到窗口 （交换链）。它还可以渲染到纹理。 在 Atom 中，有几种情况将渲染管道用于其他目的。

例如：
* 有一个渲染管道，它使用 brdf 函数来渲染 brdf 查找纹理，该纹理在应用程序启动后开始时生成。此渲染管线只需渲染一帧，您可以将 RenderPipelineDescriptor::m_executeOnce 设置为 true。  
* 生成材质预览图标。我们创建一个场景和一个渲染管道，将材质预览渲染到纹理，然后将其读回 cpu 并将其转换为 QImage。
* 烘焙环境探测器的纹理。触发 bake 时，一个环境探测器的渲染管线将添加到当前关卡场景中。 

示例：切换视口的渲染管道

将渲染到视口的渲染管线切换为作很简单。有时，您可能需要函数在运行时渲染场景是不同的样式。

您只需要从场景中删除旧的渲染管道，即可将新的渲染管道添加到场景中。您还可以保存创建的渲染管道并在它们之间自由切换，而无需重复创建它们。

以下代码删除原始渲染管道并保存它（用于恢复），然后切换为对同一视口使用另一个渲染管道 （m_windowContext）
```cpp
// save original render pipeline first and remove it from the scene
AZ::RPI::ScenePtr defaultScene = AZ::RPI::RPISystemInterface::Get()->GetDefaultScene();
m_originalPipeline = defaultScene->GetDefaultRenderPipeline();
defaultScene->RemoveRenderPipeline(m_originalPipeline->GetId());
m_originalPipeline->SetEnabled(false);

// add the checker board pipeline
AZ::RPI::RenderPipelineDescriptor pipelineDesc;
pipelineDesc.m_mainViewTagName = "MainCamera";
pipelineDesc.m_name = "Checkerboard";
pipelineDesc.m_rootPassTemplate = "CheckerboardPipeline";
m_cbPipeline = AZ::RPI::RenderPipeline::CreateRenderPipelineForWindow(pipelineDesc, *m_windowContext);
defaultScene->AddRenderPipeline(m_cbPipeline);
m_cbPipeline->SetDefaultView(m_originalPipeline->GetDefaultView());
```
注意：渲染管线在添加到场景时不能具有相同的名称。
