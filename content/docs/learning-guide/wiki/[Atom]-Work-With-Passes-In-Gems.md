---
title: "[Atom]-Work-With-Passes-In-Gems"
description: ""
toc: false
---

**Gems** and **Game Projects** can have their own Passes and Render Pipelines by providing their own Pass Template Mappings(s) assets. Pass Template Mapping assets must have extension `*.azasset`.  
  
There are two mechanisms to load Pass Template Mappings:    
1- Data-Driven (No C++): By creating asset-cache relative files with the following naming convention:  
1.1- For Gems: \<GEM ROOT PATH\>/Assets/Passes/\<GemName\>/AutoLoadPassTemplates.azasset  
1.2- For Game Projects: \<PROJECT ROOT PATH\>/Assets/Passes/\<ProjectName\>/AutoLoadPassTemplates.azasset  
1.3- For Game Projects: \<PROJECT ROOT PATH\>/Passes/\<ProjectName\>/AutoLoadPassTemplates.azasset  
  
2- With C++: By having your own System Component, which creates its own instance of `RPI::PassSystemInterface::OnReadyLoadTemplatesEvent::Handler` and during `SystemComponent::OnActivate()` registers the event handler with `RPI::PassSystemInterface::Get()->ConnectEvent(handler)`.  
  
Additionally if your Gem or Game Project wants to inject custom Passes into other Render Pipelines, for example, the Main Render Pipeline, then it most do so with C++. Your `FeatureProcessor` most override the `AddRenderPasses()` method.

# Details About Loading Pass Template Mappings With C++ (Optional).
  
The Pass System needs to have a Pass Template (*.pass asset) for each Pass before being able to instantiate a Pass. All Pass Templates must have a unique name (a string) to avoid name collision. Pass Template Mappings are assets with extension (*.azasset), which connect  the names to Pass Template (*.pass) assets.

The Pass Template Mapping(s) files should be loaded before the RenderPipelines. The best way is to have your own SystemComponent::OnActivate() do the registration. The SystemComponent should creates its own instance of `RPI::PassSystemInterface::OnReadyLoadTemplatesEvent::Handler` and during `SystemComponent::OnActivate()` registers the event handler with `RPI::PassSystemInterface::Get()->ConnectEvent(handler)`.

For example, the Atom Feature Common gem adds a  m_loadTemplatesHandler in its CommonSystemComponent.
```Cpp
// Gems\Atom\Feature\Common\Code\Source\CommonSystemComponent.h
RPI::PassSystemInterface::OnReadyLoadTemplatesEvent::Handler m_loadTemplatesHandler;

// Gems\Atom\Feature\Common\Code\Source\CommonSystemComponent.cpp
// setup handler for load pass template mappings
m_loadTemplatesHandler = RPI::PassSystemInterface::OnReadyLoadTemplatesEvent::Handler([this]() { this->LoadPassTemplateMappings(); });
RPI::PassSystemInterface::Get()->ConnectEvent(m_loadTemplatesHandler);

void CommonSystemComponent::LoadPassTemplateMappings()
{
   const char* passTemplatesFile = "Passes/PassTemplates.azasset";
   RPI::PassSystemInterface::Get()->LoadPassTemplateMappings(passTemplatesFile);
}


// In your gem system component ::Deactivate method add the disconnect call
m_loadTemplatesHandler.Disconnect()
```

# Naming

There is no restriction with pass template mappings file as long as it's *.azasset file. Gems may want to use unique prefix or sub-folder for the file to avoid it's overridden by the file from other gems. For example, the AtomSampleViewer project has a pass template mappings file at Assets/Passes/ASV/PassTemplates.azasset while the Atom Common Feature gem has a pass template mappings file at Assets/Passes/PassTemplates.azasset. 

Another potential naming conflict gems should avoid is the pass template naming conflict. Since the pass system is loading all pass template mappings files to one mapping (for faster search and simpler management) each gem may want to use unique names for their pass templates. Using a prefix might always be a good idea to name those templates. The pass system will report warnings duplicate named templates and only use the first loaded template with same name.
# Number of Mappings files

There is no limitation on how many pass template mappings files can one gem have since the gem has full control on how to load those mappings.

This means gem may create different pass template mappings for different platform and only load the mappings for current platform if there is some complicated use case.

# Gems Require Render Pipeline Change

Some gems may need to modify the main(default) render pipeline which is defined in Atom common feature gem. 
Atom has a solution to address the most common use case, inject new passes to main render pipeline. 

The FeatureProcessor provide a virtual function AddRenderPasses() which is called when a RenderPipeline is added to a Scene. Developer can override this function in the gem and make modification there (usually inserts a new pass). 

In O3DE, this method is used by few gems such as LyShine(UI) Gem, AtomHair Gem and Terrain Gem which all need inject new passes to default render pipeline.
For example, this is the code in LyShine gem which inserts a LyShineParent pass to main render pipeline. 
```Cpp
    void LyShineFeatureProcessor::AddRenderPasses(AZ::RPI::RenderPipeline* renderPipeline)
    {
        // Get the pass request to create LyShine parent pass from the asset
        const char* passRequestAssetFilePath = "Passes/LyShinePassRequest.azasset";
        auto passRequestAsset = AZ::RPI::AssetUtils::LoadAssetByProductPath<AZ::RPI::AnyAsset>(
            passRequestAssetFilePath, AZ::RPI::AssetUtils::TraceLevel::Warning);
        const AZ::RPI::PassRequest* passRequest = nullptr;
        if (passRequestAsset->IsReady())
        {
            passRequest = passRequestAsset->GetDataAs<AZ::RPI::PassRequest>();
        }
        if (!passRequest)
        {
            AZ_Error("LyShine", false, "Failed to add LyShine parent pass. Can't load PassRequest from %s", passRequestAssetFilePath);
            return;
        }

        // Return if the pass to be created already exists
        AZ::RPI::PassFilter passFilter = AZ::RPI::PassFilter::CreateWithPassName(passRequest->m_passName, renderPipeline);
        AZ::RPI::Pass* pass = AZ::RPI::PassSystemInterface::Get()->FindFirstPass(passFilter);
        if (pass)
        {
            return;
        }

        // Create the pass
        AZ::RPI::Ptr<AZ::RPI::Pass> lyShineParentPass  = AZ::RPI::PassSystemInterface::Get()->CreatePassFromRequest(passRequest);
        if (!lyShineParentPass)
        {
            AZ_Error("LyShine", false, "Create LyShine parent pass from pass request failed");
            return;
        }

        // Insert the LyShineParentPass before UIPass
        bool success = renderPipeline->AddPassBefore(lyShineParentPass, AZ::Name("UIPass"));
        // only create pass resources if it was success
        if (!success)
        {
            AZ_Error("LyShine", false, "Add the LyShine parent pass to render pipeline [%s] failed",
                renderPipeline->GetId().GetCStr());
        }
    }
```
The pass request which is used to create the LyShineParentPass is also save in a data file LyShinePassRequest.azasset. And this is how it looks
```JSON
{
    "Type": "JsonSerialization",
    "Version": 1,
    "ClassName": "PassRequest",
    "ClassData": {
        "Name": "LyShinePass",
        "TemplateName": "LyShineParentTemplate",
        "Connections": [
            {
                "LocalSlot": "ColorInputOutput",
                "AttachmentRef": {
                    "Pass": "DebugOverlayPass",
                    "Attachment": "InputOutput"
                }
            },
            {
                "LocalSlot": "DepthInputOutput",
                "AttachmentRef": {
                    "Pass": "DepthPrePass",
                    "Attachment": "Depth"
                }
            }
        ]
    }
}
```

Notes:

There are few recommendation and limitations with this render pipeline modification solution. 
It's recommended to only insert new passes to the render pipeline in the gem. The gem shouldn't change existing passes in current pipeline since it doesn't have ownership of them. 
If there are more then one passes you need to insert to the render pipeline, try to group them under one ParentPass so you would need to insert one ParentPass to the render pipeline. Although, you can still insert multiple passes to different location in the render pipeline. And there is no restriction with that. 
The new injected passes can output existing attachments in the current render pipeline but it can't output new attachments. This is because output new attachments requires update connections for passes after the injected passes which we don't have good support for. And also with majority use cases, there are no need to create new ones other than using existing ones. 
