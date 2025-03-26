---
title: "[Atom]-Scene-and-Render-Pipeline"
description: ""
toc: false
---

The Scene and Render Pipeline in Atom RPI make render different 3d content in different viewport more easily.
A scene contains a list of feature processors which have the data to be rendered. A render pipeline defines how to render a scene. 
As a staring point, to use RPI system to render some data, you need to at least create a scene and create a render pipeline add to this scene. 

Each scene is independent with other scenes. A scene may have many render pipelines. When a render pipeline is added to a scene, it defines one way to render this scene. 

In this document, you will learn how to create scenes and render pipelines and how to use them for some typical use cases. 

# Create a RPI Scene

Before create a RPI::Scene instance, you want to decide what kind of graphics features you want to enable within this scene so you can add the feature processors in the RPI::SceneDescriptor which is used when creates the scene.

Note: these feature processors you are trying to enable need to locate in the gems which you have enabled. 

Another way to enable feature processors is to use EnableAllFeatureProcessors right after scene is created. This function enables all available (feature processor registered ) feature processors for the scene. 

After a scene is created and feature processors are enabled, it need to be activated and registered to RPISystem, otherwise it won't be updated or rendered properly. 

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
Note: the feature processor name is the name where the feature processor class declare their AZ_RTTI. For example, the name for TransformServiceFeatureProcessor is "AZ::Render::TransformServiceFeatureProcessor" and this is how the class's AZ_RTTI looks like:
```
AZ_RTTI(AZ::Render::TransformServiceFeatureProcessor, "{D8A2C353-2850-42F8-AA21-3979CBECBF80}", AZ::Render::TransformServiceFeatureProcessorInterface);
```

# Associate a AzFramework Scene With a RPI Scene 

Lumberyard level is composed by entities with components. Entities created for different "world" belong to different AzFramework scenes(AzFramework::Scene). To render the graphics content of each AzFramework scene, we need to create one to one relationship to the RPI scene. 

Note, if the graphics content you are trying to render are not from components but are created from feature processors directly, you can skip this step.

For existing AzFramework scene, such as the default scene created by Lumberyard which used for its level, you can get the scene and associate it with newly create RPI scene with this code:

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
Sometime you may want to create a new AzFramework scene to create a separate world for a set of entities and render them. For example, a preview window for a material in Lumberyard Editor. This preview window is independent with the level window and you want setup its own lighting and post effects for this preview window. It makes sense to create a separate AzFramework scene which contains entities for preview. 

To create such a AzFramework scene, you need to create a AzFramework entity context (AzFrameWork::EntityContext) first and make the newly created AzFramework scene to use this EntityContext. Then any entity created with this entity context belongs to this scene.

This is showing how to create a AzFramework scene from scratch:
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
# Create And Add Render Pipelines To A Scene

A render pipeline defines how to render a scene. And you can add more than one render pipelines for a scene. A render pipeline can be created via an .azasset which contains a RenderPipelineDescriptor or it can be created directly from a RenderPipelineDescriptor. 

One common case of one scene has multiple render pipelines is a level editor has different viewports showing which to render a level with different cameras at same time such as perspective, top, front and left. This can be fairly easy to setup with Atom's RPI. 

The following code setup two render pipelines with two different methods and add them to the scene. Each of the render pipeline renders to a different window.
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
A render pipeline is not necessary always render to a window (swapchain). It can also render to a texture.  There are several cases in Atom which uses render pipelines for other purpose.

For example:
* There is a render pipeline to which uses brdf function to render a brdf lookup texture which is generated in the beginning after the application started. This render pipeline only need to be rendered for one frame which you can set the RenderPipelineDescriptor::m_executeOnce to true.   
* Generating the material preview icon. We create a scene and a render pipeline to render the material preview to a texture then read it back to cpu and convert it to QImage.
* Bake textures for environment probes. The render pipeline for one environment probe is added to current level scene when bake is triggered.  

Example: Switching Render Pipelines For A Viewport

Switching the render pipeline which renders to a viewport is simple operation. Sometime you may need the function to render your scene is a different style at run time. 

All you need to do it to remove the old render pipeline from the scene add the new render pipeline to the scene. You can also save the create render pipelines and switch between them freely without creating them repeatedly. 

The following code removes the original render pipeline and saves it ( for recovery) and switch to use another render pipeline for the same viewport (m_windowContext)
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
Note: the render pipelines can't have same name when they are added to scene. 
