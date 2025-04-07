---
title: "[Atom]-在 Gem 中使用通道"
description: ""
toc: false
---

**Gem** 和 **游戏项目** 可以通过提供自己的 Pass Template Mappings（s） 资源来拥有自己的 Pass 和 Render Pipeline。Pass 模板映射资产必须具有扩展名`*.azasset`。

有两种机制可以加载 Pass Template 映射：   
1- 数据驱动（无 C++）：通过使用以下命名约定创建资产缓存相对文件：
1.1- 用于 Gems: \<GEM ROOT PATH\>/Assets/Passes/\<GemName\>/AutoLoadPassTemplates.azasset  
1.2- 用于游戏项目: \<PROJECT ROOT PATH\>/Assets/Passes/\<ProjectName\>/AutoLoadPassTemplates.azasset  
1.3- 用于游戏项目: \<PROJECT ROOT PATH\>/Passes/\<ProjectName\>/AutoLoadPassTemplates.azasset  
  
2- 使用 C++：通过拥有自己的系统组件，该组件创建自己的 `RPI::PassSystemInterface::OnReadyLoadTemplatesEvent::Handler` 实例，并在 `SystemComponent::OnActivate()` 期间使用 `RPI::PassSystemInterface::Get()->ConnectEvent(handler)` 注册事件处理程序。 

此外，如果您的 Gem 或游戏项目想要将自定义通道注入其他渲染管道（例如主渲染管道），则大多数情况下使用 C++ 来实现。您的 `FeatureProcessor` 大多数会覆盖 `AddRenderPasses()` 方法。

# 有关使用 C++ 加载通道模板映射的详细信息（可选）。
  
通道系统需要为每个通道提供一个通道模板（*.pass 资产），然后才能实例化通道。所有 Pass Templates 都必须具有唯一的名称 （字符串） 以避免名称冲突。通道模板映射是扩展名为 （*.azasset） 的资产，用于将名称连接到通道模板 （*.pass） 资产。

Pass Template Mapping（s） 文件应在 RenderPipelines 之前加载。最好的方法是让自己的 SystemComponent::OnActivate（） 进行注册。SystemComponent 应创建自己的 `RPI::PassSystemInterface::OnReadyLoadTemplatesEvent::Handler` 实例，并在 `SystemComponent::OnActivate()` 期间使用 `RPI::PassSystemInterface::Get()->ConnectEvent(handler)` 注册事件处理程序。

例如，Atom Feature Common Gem 在其 CommonSystemComponent 中添加了一个m_loadTemplatesHandler。
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

# 命名

只要是 *.azasset 文件，通道模板映射文件就没有限制。Gem 可能希望为文件使用唯一的前缀或子文件夹，以避免它被其他 Gem 中的文件覆盖。例如，AtomSampleViewer 项目在 Assets/Passes/ASV/PassTemplates.azasset 中有一个通道模板映射文件，而 Atom Common Feature Gem 在 Assets/Passes/PassTemplates.azasset 中有一个通道模板映射文件. 

Gem 应避免的另一个潜在命名冲突是通道模板命名冲突。由于通道系统将所有通道模板映射文件加载到一个映射中（以便更快地搜索和简化管理），因此每个 Gem 可能希望为其通道模板使用唯一的名称。使用前缀来命名这些模板可能始终是一个好主意。pass 系统将报告警告、重复命名的模板，并且仅使用第一个加载的同名模板。
# 映射文件数

一个 Gem 可以具有多少个通道模板映射文件没有限制，因为 Gem 可以完全控制如何加载这些映射。

这意味着 Gem 可以为不同的平台创建不同的通道模板映射，并且仅在存在一些复杂的用例时才加载当前平台的映射。

# Gem 需要更改渲染管线

某些 Gem 可能需要修改 Atom 通用功能 Gem 中定义的主（默认）渲染管道。
Atom 有一个解决方案来解决最常见的用例，即向主渲染管道注入新的通道。

FeatureProcessor 提供了一个虚拟函数 AddRenderPasses（），当 RenderPipeline 添加到场景时，将调用该函数。开发人员可以在 Gem 中覆盖此函数，并在其中进行修改（通常插入新的通道）。

在 O3DE 中，LyShine（UI） Gem、AtomHair Gem 和 Terrain Gem 等少数 Gem 使用此方法，它们都需要向默认渲染管道注入新通道。
例如，这是 LyShine Gem 中的代码，它将 LyShineParent 传递插入到主渲染管道。
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
用于创建 LyShineParentPass 的通行证请求也保存在数据文件 LyShinePassRequest.azasset 中。这就是它的外观
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

笔记：

此渲染管道修改解决方案几乎没有建议和限制。
建议仅将新通道插入到 Gem 中的渲染管道。Gem 不应更改当前管道中的现有通道，因为它没有这些通道的所有权。
如果需要将多个通道插入到渲染管道中，请尝试将它们分组在一个 ParentPass 下，以便需要将一个 ParentPass 插入到渲染管道中。不过，您仍然可以将多个通道插入到渲染管道中的不同位置。这没有任何限制。
新注入的通道可以输出当前渲染管道中的现有附件，但不能输出新附件。这是因为输出新附件需要在注入的通道之后更新通道的连接，而我们没有很好的支持。而且，对于大多数用例，除了使用现有用例外，无需创建新的用例。
