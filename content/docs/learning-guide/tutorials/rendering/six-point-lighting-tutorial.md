---
linkTitle: 自定义照明
title: 使用 Flipbook 动画的自定义照明教程
description: 在 Open 3D Engine （O3DE） 的 Atom 渲染器中使用具有六点照明的翻书动画进行自定义照明的教程。
toc: true
---
本教程介绍了如何通过编写自定义着色器来制作六点光照材质类型，以将光照应用于动画 2D 对象，从而创建云/烟雾效果。

烟雾和云等体积效果可以用动画纹理表示。要渲染这些纹理效果并近似 3D 体积，您需要自定义材质类型。本教程中的技术通过使用六个切线光照贴图（表示烟雾的顶部、底部、左侧、右侧、正面和背面）来近似如何从任何给定方向照亮每个纹理。

六点照明材质类型使用纹理，如果光线来自相应的方向，则这些纹理会为纹理的照明部分着色。例如，下图显示了一缕烟雾，光照来自左侧，这与光照贴图中的绿色纹素相对应。红色纹素指示烟雾的哪些纹素应通过右侧的照明来照亮。因此，黄色 （绿色 + 红色） 纹素意味着应将左侧和右侧的照明应用于这些纹素。然后，可以使用此信息形成 6 个切线光照贴图，并相应地在材质上应用光照。

{{< image-width src="/images/learning-guide/tutorials/rendering/six-point-lighting/spl-comparison.png" width="50%" alt="Picture of a frame of a light map on the left with a picture of the cloud on right with lighting coming from the left." >}}

本教程涵盖以下概念：
* 编辑您自己的材质类型
* 使用 Lua 在 **材质编辑器** 中切换属性可见性
* 为材质制作动画
* 编辑像素着色器
   * 添加自定义表面
   * 添加自定义照明

## 创建材质类型
按照以下步骤创建六点照明材质类型。

1. 从 GitHub 下载或克隆 [`o3de/sample-code-gems`](https://github.com/o3de/sample-code-gems)  存储库。

1.  移动`atom_gems/AtomTutorials/Templates/SixPointLighting/` 中的所有文件到 `{your-project-path}/Materials/Types`。根据需要创建文件夹。这些模板文件为您设置了一切，以便您开始创建自己的自定义表面和照明。
   
1. 移动`atom_gems/AtomTutorials/Assets/SixPointLighting/Objects/` 中的所有文件到 `{your-project-path}/Objects`中。 
   
   {{< note >}}
   这些纹理由 GitHub 上的 [peeweek/Unity-URP-SmokeLighting](https://github.com/peeweek/Unity-URP-SmokeLighting/tree/main/Assets/VFX/SmokeLighting/Textures/2D)提供，并根据 MIT 许可证分发。
   {{< /note >}}

1. 在文本编辑器中打开 `{your-project-path}\Materials\Types\SixPointLighting.materialtype` 。

1. 在 `propertyLayout` > `propertyGroups`下，替换所有`{your-path-to-o3de}` 为引擎的适当路径。
   
   例如， `C:/o3de/Gems/Atom/Feature/Common/Assets/Materials/Types/MaterialInputs/BaseColorPropertyGroup.json`。


### 六点照明材质类型属性
六点照明材质类型包含以下属性。您将在整个教程中使用这些属性。它们被定义在`SixPointLighting_Common.azsli` 和 `SixPointLighting_ForwardPass.azsl`中 

| 属性 | 说明 | 类型 |
| :-- | :-- | :-- |
| `o_sixPointTexturePackMode` | 指示要使用的纹理包模式。 | Shader option |
| `m_topLeftRightBottomMap` | 定义上-左-右-下光照贴图。 | Texture |
| `m_frontBackMap` | 定义前后光照贴图。 | Texture |
| `m_rightLeftTopMap` | 定义右-左-上光照贴图。 | Texture |
| `m_bottomBackFrontMap` | 定义 bottom-back-front 光照贴图。 | Texture |
| `o_enableDepthTexture` | 切换是否使用深度纹理。 | Boolean Shader option |
| `m_depthMap` | 深度纹理贴图。| Texture |
| `m_depthScale` | 缩放深度纹理。 | Float |
| `m_rowCount` | 翻书动画中的行数。 | Int |
| `m_columnCount` | flipbook 动画中的列数 | Int |
| `o_enableDebugFrame` | 如果启用，则激活对动画的单个帧的调试。 | Boolean Shader option |
| `m_debugFrame` |启用 `o_enabledDebugFrame` 时要调试的帧编号。 | Int |

{{< note >}}
本教程中不会使用涉及 `depth` 的所有内容，包括深度通道和三个属性，因为我们缺少深度贴图纹理。但是，最终文件中的 `SixPointLighting_DepthPass_WithPS.azsl` 确实提供了调整深度的代码，因此如果您对我们如何调整深度像素着色器感兴趣，可以查看一下。
{{< /note >}}

## 编写 Lua 函数以在 Material Editor 中切换可见性
六点光照材质类型允许使用六个切线光照贴图，这些光照贴图对应于纹理中的六种颜色。但是，每个纹理最多只能包含四个通道（红色、绿色、蓝色、alpha），因此该技术需要两个纹理。用于每个纹理的通道可以由美工人员决定，但本教程中的此材质类型将支持颜色通道到方向映射的两个选项。该映射稍后将用于确定适当的照明。

* TopLeftRightBottom_FrontBack 选项
   * First texture:
      * Top : Red 
      * Left : Green
      * Right : Blue
      * Bottom : Alpha
   * Second texture:
      * Front : Red
      * Back : Green
* RightLeftTop_BottomBackFront 选项
   * First texture:
      * Right : Red 
      * Left : Green
      * Top : Blue
   * Second texture:
      * Bottom : Red
      * Back : Green
      * Front : Blue

在 `SixPointLightingPropertyGroup.json`中，则两个选项的两个纹理集已经有四个属性。它们也已在 `SixPointLighting_Common.azsli` 中定义。但是，您需要为材质类型提供 `.lua` 脚本，以便从 **材质编辑器** 的 *Texture Pack Mode* 下拉列表中选择一个选项时，仅显示相应的属性。

1. 打开 `SixPointLighting_TexturePackEnum.lua`。请注意两个函数 `GetMaterialPropertyDependencies()` 和 `ProcessEditor()`。 `GetMaterialPropertyDependencies()`获取材质属性的值。然后，`ProcessEditor()`可以使用属性值来启用和禁用材质编辑器中属性的可见性。

1. 按照 `sixPointLighting.TLRB` 的可见性的启用和禁用方式，根据需要启用和禁用其他纹理选项：

   ```lua
   if(texturePackMode == TexturePackMode_TpLftRtBt_FrBck) then
      -- TopLeftRightBack is the first texture, FrontBack is the second. Disable RightLeftTop and BottomBackFront.
      context:SetMaterialPropertyVisibility("sixPointLighting.TLRB", MaterialPropertyVisibility_Enabled)
      context:SetMaterialPropertyVisibility("sixPointLighting.FB", MaterialPropertyVisibility_Enabled)
      context:SetMaterialPropertyVisibility("sixPointLighting.RLT", MaterialPropertyVisibility_Hidden)
      context:SetMaterialPropertyVisibility("sixPointLighting.BBF", MaterialPropertyVisibility_Hidden)
   elseif(texturePackMode == TexturePackMode_RtLftTp_BtBckFr) then
      -- RightLeftTop is the first texture, BottomBackFront is the second. Disable TopLeftRightBack and FrontBack.
      context:SetMaterialPropertyVisibility("sixPointLighting.TLRB", MaterialPropertyVisibility_Hidden)
      context:SetMaterialPropertyVisibility("sixPointLighting.FB", MaterialPropertyVisibility_Hidden)
      context:SetMaterialPropertyVisibility("sixPointLighting.RLT", MaterialPropertyVisibility_Enabled)
      context:SetMaterialPropertyVisibility("sixPointLighting.BBF", MaterialPropertyVisibility_Enabled)
   end
   ```

## 制作六点照明材质
现在，六点照明材质类型属性已向 Material Editor 公开，您可以创建六点照明材质。

1. 打开 **材质编辑器**，并使用 6 点照明材质类型制作新材质。
   
1. 在 **Inspector**中找到 **Six Point Lighting** 属性。
   
1. 请注意，默认的 **Texture Pack Mode** 是 `TpLftRtBt_FrBck`。下面对应于此纹理包模式的两个属性，以及另一个纹理包模式的属性被隐藏。

1. 为 **Texture Pack Mode** 选择 `RtLftTp_BtBckFr` 并观察属性如何变化。

1. 相应地设置以下属性：
    * **Six Point Lighting**
      * **Texture Pack Mode**: `RtLftTp_BtBckFr`
      * **Right Left Top**: `SmokeBall01_6Way_RLT_8x8.png`
      * **Bottom Back Front**: `SmokeBall01_6Way_BBF_8x8.png`
      * **Rows in Flipbook**: `8.0`
      * **Columns in Flipbook**: `8.0`
    * **Base Color**
      * **Texture**: `SmokeBall01_ColorCC_8x8.png`
      * **Use Texture**: Disabled
         {{< note >}}
         您不想将纹理用作基础颜色，因为它会使材质变色。但是，要对不透明度使用纹理的 Alpha 通道，必须设置基色纹理属性。
         {{< /note >}}
    * **Opacity**
      * **Opacity Mode**: `Blended`
      * **Alpha Source**: `Packed`
         {{< note >}}
         `Packed` Alpha 源意味着材质将使用来自基础颜色纹理的 Alpha 通道。
         {{< /note >}}
      * **Factor**: `1.0`
      * **Alpha affects specular**: `1.0`
    * **UVs**
      * **Center** > **U**: `0.0`
      * **Center** > **V**: `0.0`
    * **General Settings**
      * **Double-sided**: Enabled
         {{< note >}}
         启用此设置将允许渲染材质的背面。
         {{< /note >}}
   
1.  在 **编辑器** 中，创建一个具有 **Mesh** 和 **Material** 组件的实体。为 **Mesh** 选择一个平面 （`o3de/Gems/Atom/Tools/MaterialEditor/Assets/MaterialEditor/ViewportModels/Plane_1x1.fbx`） 和您刚刚为该材质创建的材质。

{{< image-width src="/images/learning-guide/tutorials/rendering/six-point-lighting/material.png" width="100%" alt="Material added." >}}

截至目前，实体应该只显示包含所有帧的整个 alpha 纹理。

{{< image-width src="/images/learning-guide/tutorials/rendering/six-point-lighting/all-frames.png" width="100%" alt="All frames of the six-point lighting animation texture." >}}

## 添加动画
下一步是向材质添加动画。纹理包含动画的所有帧，因此您将以编程方式迭代这些帧。

1. 打开`SixPointLighting_Common.azsli`.
   
1. 在底部，添加一个函数，根据时间获取正确帧在纹理映射中的位置。

   ```glsl
   float2 GetUvForCurrentFrame(float2 baseUv)
   {
      // Fixed frequency of 30hz
      // Get the current frame
      float frame = (float)(((double)SceneSrg::m_time / (33.3333)) * 1000.0) % (MaterialSrg::m_columnCount * MaterialSrg::m_rowCount);

      if(o_enableDebugFrame)
      {
         // The frame input by the material is 1-indexed, so subtract 1 here to make it 0-indexed
         frame = MaterialSrg::m_debugFrame - 1.0f;
      }

      // Get the row/column of the frame
      float frameColumn = floor(frame % MaterialSrg::m_columnCount);
      float frameRow = floor(frame / MaterialSrg::m_columnCount) % MaterialSrg::m_rowCount;
      
      float2 invColumnRowCounts = float2(1.0f, 1.0f) / float2(MaterialSrg::m_columnCount, MaterialSrg::m_rowCount);
      float2 sixPointUv = (baseUv + float2(frameColumn, frameRow)) * invColumnRowCounts;

      return sixPointUv;
   }
   ```

   {{< note >}}
   如果为特定帧启用了调试，则会出现条件`if(o_enableDebugFrame)`，这可以通过 **材质编辑器** 进行设置。如果启用，则此功能使用指定的帧而不是当前帧。此功能有助于确保在特定帧中正确应用光照。
   {{< /note >}}

1. 打开`SixPointLighting_ForwardPass.azsl`进行一些最终编辑，以查看动画的实际效果。
   
   1. 查找 `ForwardPassPS_Common`。
   
   1. 查找定义表面的位置： `Surface surface`。
   
   1. 在它的正下方，找到一个用于*Alpha & Clip*的部分。编辑 `alpha` 值以使用不透明度贴图并使用当前帧的 UV：
   
   ```glsl
   float2 baseColorUv = IN.m_uv[MaterialSrg::m_baseColorMapUvIndex];
   float2 sixPointUv = GetUvForCurrentFrame(baseColorUv);

   float alpha = GetAlphaInputAndClip(MaterialSrg::m_baseColorMap, MaterialSrg::m_opacityMap, sixPointUv, sixPointUv, MaterialSrg::m_sampler, MaterialSrg::m_opacityFactor, o_opacity_source);
   ``` 

1. 再次打开 **编辑器** 并查看动画！您尚未应用任何自定义照明，因此您应该只看到带有 Alpha 纹理的基色动画。

{{< video src="/images/learning-guide/tutorials/rendering/six-point-lighting/animation.mp4" autoplay="true" loop="true" width="100%" muted="true" info="Video of the animation of the cloud." >}}

## 制作自定义表面
要使六点照明正常工作，必须向自定义表面添加一些材质属性。 *表面* 由定义材质的外观和感觉以及它与光照的交互方式的属性组成。例如，`metallic` 属性定义物体的金属感，而 `albedo` 属性表示材质反射的光量。

对于此自定义曲面，必须添加 6 个方向、切线和双切线的属性。

六个定向浮点数定义纹素的每个方向接收的光强度。例如，如果纹素应反射来自上方的大部分光线，则顶部浮点数将约为 255.0（RGB 比例中的最大值）。因此，对于大部分被来自上方的光线遮挡的纹素，顶部浮点数应更接近 0.0。纹素的定向照明强度是一种艺术选择，在使用数字内容创建 （DCC） 工具烘焙纹理时，它可能基于预先计算的评估。

在从纹理中查找光线贡献之前，需要 `tangent` 和 `bitangent` 属性将世界空间照明方向转换为切线空间。
   
1. 打开 `SixPointSurface.azsli`。
   
1. 在 `Surface` 类中，在 `BasePbrSurfaceData` 列表下，定义六个方向的属性，即 `tangent` 和 `bitangent`。

   ```glsl
   float top;
   float left;
   float right;
   float bottom;
   float frontside;
   float backside;
   float3 tangent;
   float3 bitangent;
   ```

您可以初始化并在以后使用 Surface 的这些属性来定义照明。

## 编辑像素着色器
现在，在像素着色器中，您将集成表面并初始化值。这将准备材质以允许自定义照明。

1. 打开 `EvaluateSixPointSurface.azsli`。在 `EvaluateSixPointSurface` 函数中，您将进行两项主要更改：为当前帧使用正确的 UV，并初始化您添加到六点表面的新属性。

  在运行时，此函数在 `SixPointLighting_ForwardPass.azsl` 中调用。
   
1. 获取动画当前帧的 UV。
   
   1. 查找 *Base Color* 部分。
   
   1. 通过调用您之前编写的函数 `GetUvForCurrentFrame()` 来获取当前帧的 UV。
   
   1. 在调用`GetBaseColorInput()`时，将 `baseColorUv` 参数替换为`sixPointUv`。

   ```glsl
   float2 baseColorUv = uv[MaterialSrg::m_baseColorMapUvIndex];
   float2 sixPointUv = GetUvForCurrentFrame(baseColorUv);
   float3 sampledColor = GetBaseColorInput(MaterialSrg::m_baseColorMap, MaterialSrg::m_sampler, sixPointUv, MaterialSrg::m_baseColor.rgb, o_baseColor_useTexture);
   float3 baseColor = BlendBaseColor(sampledColor, MaterialSrg::m_baseColor.rgb, MaterialSrg::m_baseColorFactor, o_baseColorTextureBlendMode, o_baseColor_useTexture);
   ```

1. 初始化六点表面属性。
   
   1. 找到 *Specular* 部分。
   
   1. 根据纹理包模式，设置使用材质输入添加的六个定向表面属性。您需要处理两个 texture pack 模式选项并相应地设置属性：
   
   ```glsl
   if(o_sixPointTexturePackMode == SixPointTexturePackMode::TpLftRtBt_FrBck)
   {
      float4 topLeftRightBottom = MaterialSrg::m_topLeftRightBottomMap.Sample(MaterialSrg::m_sampler, sixPointUv);
      float4 frontBack = MaterialSrg::m_frontBackMap.Sample(MaterialSrg::m_sampler, sixPointUv);
      surface.top = topLeftRightBottom.r;
      surface.left = topLeftRightBottom.g;
      surface.right = topLeftRightBottom.b;
      surface.bottom = topLeftRightBottom.a;
      surface.frontside = frontBack.r;
      surface.backside = frontBack.g;
   }
   else
   {
      float4 rightLeftTop = MaterialSrg::m_rightLeftTopMap.Sample(MaterialSrg::m_sampler, sixPointUv);
      float4 bottomBackFront = MaterialSrg::m_bottomBackFrontMap.Sample(MaterialSrg::m_sampler, sixPointUv);
      surface.right = rightLeftTop.r;
      surface.left = rightLeftTop.g;
      surface.top = rightLeftTop.b;
      surface.bottom = bottomBackFront.r;
      surface.backside = bottomBackFront.g;
      surface.frontside = bottomBackFront.b;
   }
   ```

1. 紧接着初始化 `tangent` 和 `bitangent` 曲面属性：
   ```glsl
   surface.tangent = tangents[0];
   surface.bitangent = bitangents[0];
   ```

## 添加自定义照明
现在，您已经设置了六点表面，可以使用新的表面属性来应用自定义照明。您将创建两种类型的照明：定向照明和基于图像的照明 （IBL）。_Directional lighting_ 是来自单个方向的光源。_IBL_ 模拟来自实体周围环境的全向反射、类似环境的照明。

### 添加自定义定向照明
如前所述，您将制作一个光照贴图，该贴图使用光照方向来确定要照亮六个面的哪个组合。然后，您将使用亮度和纹素的定向照明强度来计算该特定纹素上的整体照明。

1. 打开 `SixPointLighting.azsli`。
   
   1. 注意顶部的 `#include <SixPointSurface.azsli>` 行。这就是在以下函数中引用表面的方法。
   
   1. 注意 `GetSpecularLighting()` 函数，返回 `float3(0.0f, 0.0f, 0.0f)`。_Specular lighting_ 模拟将光线反射到摄像机中的反光对象上的亮点。对于六点照明，您不需要镜面反射照明，因为它不能有效地应用于 2D 纹理。此外，烟雾和云效果是无光泽的对象，不需要镜面反射照明。

   1. 请注意函数 `GetDiffuseLighting()`。您将对其进行编辑以获得所需的效果。

      六点光照`ForwardPassPS_Common`着色器使用默认的`ApplyDirectLighting()`函数，该函数将迭代应用于此对象的光源，并为每个光源调用这些自定义的`GetDiffuseLighting()` 和 `GetSpecularLighting()`函数。
      
1. 编辑 `GetDiffuseLighting()` 并编写一个 helper 函数。
   
   漫射照明模拟来自入射方向的光线的散射方式。六点光照应使用漫射光照，因为着色器应采用光照的方向并应用它来计算光照贴图。

   1. 编写一个辅助函数来计算光照贴图。
      
      首先，将光线的方向转换为切线空间。然后，根据光线方向选择正确的 horizontal， vertical及 depth 侧。最后，找到光线的整体强度。 

      ```glsl
      float ComputeLightMap(const float3 dirToLightWS, const Surface surface)
      {
         float3 dirToLightTS = WorldSpaceToTangent(dirToLightWS, surface.normal, surface.tangent, surface.bitangent);
         float hMap = (dirToLightTS.x > 0.0f) ? (surface.right) : (surface.left);   // Picks the correct horizontal side.
         float vMap = (dirToLightTS.y > 0.0f) ? (surface.bottom) : (surface.top);   // Picks the correct vertical side.
         float dMap = (dirToLightTS.z > 0.0f) ? (surface.frontside) : (surface.backside);  // Picks the correct front/back side
         float lightMap = hMap*dirToLightTS.x*dirToLightTS.x + vMap*dirToLightTS.y*dirToLightTS.y + dMap*dirToLightTS.z*dirToLightTS.z; // Pythagoras!
         return lightMap;
      }
      ```

   1. 在 `GetDiffuseLighting()`中，调用`ComputeLightMap()`函数并应用结果：
   
      ```glsl
      float3 GetDiffuseLighting(Surface surface, LightingData lightingData, float3 lightIntensity, float3 dirToLight)
      {
         float lightMap = ComputeLightMap(dirToLight, surface);
         float3 diffuse = lightMap.rrr;
         
         diffuse *= lightIntensity;
         return diffuse;
      }
      ```

太好了，定向照明完成了！现在，您的材质应该在 **编辑器** 中具有光照。尝试在材质周围添加更多具有 **Directional Light** 组件的实体，以查看不同的效果。例如，尝试移动光线以指向材质的顶部，并查看光照如何做出相应的响应！此外，根据需要调整 **Directional Light** 组件中光源的 **Intensity**，使云看起来更逼真。您的材质还将同时响应其他光源类型和多个光源。

{{< video src="/images/learning-guide/tutorials/rendering/six-point-lighting/directional-lighting.mp4" autoplay="true" loop="true" width="100%" muted="true" info="Video of changing direction of the light applied onto the six-point lighting cloud." >}}

### 添加基于图像的光照
您可能会注意到，云中的阴影大多是灰色的，这不能很好地反映环境。如果关闭所有光照并旋转材质，则六点光照材质会不自然地更改颜色。因此，您还将在六点照明材料类型中自定义 IBL。

在 3D 对象上，IBL 的工作原理是将光线投射从材质上每个像素的法线发送到天空盒。光线投射采用天空盒的颜色，并将该颜色反射到材质上。由于六点光照材质是 2D 对象，因此不能使用此方法;所有光线投射都将从平面的法线发送。因此，您可以使用六个方向来近似 IBL，而不是使用法线。

{{< note >}}
请注意，适当的深度贴图将提供适当的法线，因此 3D IBL 方法可能有效。但是，由于本教程不涉及深度，因此我们通过自定义 IBL 提供这种近似方法。
{{< /note >}}

对于每个像素，您将在六个方向上执行光线投射。这将获得每个方向的天空盒的颜色。然后，您将这些颜色分别乘以纹素的定向照明强度。最后，将它们相加得到整体 IBL。

1. 打开 `SixPointLighting.azsli`。

1. 查找 `ApplyIBL`。在 forward pass 中调用此函数以应用 IBL。无需编辑此功能。
   
   {{< note >}}
   请注意，没有镜面反射 IBL。与定向照明类似，IBL 不应具有六点照明材料类型的任何镜面反射照明。
   {{< /note >}}
   
1. 在 `GetIblDiffuse()` 上方，添加一个辅助函数 （`GetIblSample()`），该函数将 `direction` 从切线空间转换为世界空间，并使用结果向量对天空盒进行采样。

   ```glsl
   float3 GetIblSample(Surface surface, float3 direction) 
   {
      float3 irradianceDir = TangentSpaceToWorld(direction, surface.normal, surface.tangent, surface.bitangent);
      irradianceDir = MultiplyVectorQuaternion(irradianceDir, SceneSrg::m_iblOrientation);
      float3 diffuseSample = SceneSrg::m_diffuseEnvMap.Sample(SceneSrg::m_samplerEnv, GetCubemapCoords(irradianceDir)).rgb;

      return diffuseSample;
   }
   ```

1. 删除当前在`GetIblDiffuse()`中的代码，并为切线空间中的六个方向中的每个方向调用辅助函数。
   
   ```glsl
   float3 rightSample = GetIblSample(surface, float3(1.0f, 0.0f, 0.0f));
   float3 leftSample = GetIblSample(surface, float3(-1.0f, 0.0f, 0.0f));
   float3 topSample = GetIblSample(surface, float3(0.0f, -1.0f, 0.0f));
   float3 bottomSample = GetIblSample(surface, float3(0.0f, 1.0f, 0.0f));
   float3 frontsideSample = GetIblSample(surface, float3(0.0f, 0.0f, 1.0f));
   float3 backsideSample = GetIblSample(surface, float3(0.0f, 0.0f, -1.0f));
   ```

   {{< note >}}
   `topSample` 使用向量`{0.0, -1.0, 0.0}`，因为 O3DE 使用 DirectX 约定，其中在 2D 平面上，左上角的向量是`{0.0, 0.0}`，左下角的向量是`{0.0, 1.0}`。因此，向量`{0.0, -1.0, 0.0}`指向顶部。
   {{< /note >}}

1. 通过将所有采样的颜色相加并返回适当的颜色来计算整体颜色。

   ```glsl
   float3 GetIblDiffuse(Surface surface, float3 diffuseResponse)
   {
      float3 rightSample = GetIblSample(surface, float3(1.0f, 0.0f, 0.0f));
      float3 leftSample = GetIblSample(surface, float3(-1.0f, 0.0f, 0.0f));
      float3 topSample = GetIblSample(surface, float3(0.0f, -1.0f, 0.0f));
      float3 bottomSample = GetIblSample(surface, float3(0.0f, 1.0f, 0.0f));
      float3 frontsideSample = GetIblSample(surface, float3(0.0f, 0.0f, 1.0f));
      float3 backsideSample = GetIblSample(surface, float3(0.0f, 0.0f, -1.0f));

      float3 totalDiffuseSample = (leftSample * surface.left) 
                                 + (rightSample * surface.right) 
                                 + (topSample * surface.top) 
                                 + (bottomSample * surface.bottom) 
                                 + (frontsideSample * surface.frontside) 
                                 + (backsideSample * surface.backside);

      return diffuseResponse * surface.albedo * totalDiffuseSample;
   }
   ```

   {{< tip >}}
   将采样的颜色乘以 surface 属性是实现此光照近似值的关键。回想一下，如果光线来自相应的方向，surface 属性为我们提供了纹素上光线的强度。因此，将采样的颜色乘以强度可以适当地缩放颜色值。

   例如，考虑一个纹理，其中 `surface.top` 是强烈的（大约 `255.0`），而 `surface.bottom` 是温和的（大约 `0.0`）。因此，在纹理的顶部，`bottomSample` 对颜色没有影响。
   {{< /tip >}}

1. 打开 Editor 并关闭所有灯光。您应该看到材质上的颜色反映了天空盒的颜色（顶部为蓝色，底部为橙色）。

   {{< video src="/images/learning-guide/tutorials/rendering/six-point-lighting/ibl.mp4" autoplay="true" loop="true" width="100%" muted="true" info="Video of rotating the six-point lighting material to see how IBL affects the " >}}

1. 再次打开灯光，观察 IBL 如何与定向照明配合使用！

   {{< video src="/images/learning-guide/tutorials/rendering/six-point-lighting/final.mp4" autoplay="true" loop="true" width="100%" muted="true" info="Video of changing direction of the light applied onto the six-point lighting cloud." >}}

太棒了，您添加了自定义定向照明和 IBL！

## 下载 AtomTutorial Gem 示例
现在，您已经完成了本教程，您可以将结果与 [o3de/sample-code-gems 存储库](https://github.com/o3de/sample-code-gems)中 **AtomTutorials** Gem 中的六点照明工作版本进行比较。您可以从 [atom_gems/AtomTutorials/Assets/SixPointLighting/](https://github.com/o3de/sample-code-gems/tree/main/atom_gems/AtomTutorials/Assets/SixPointLighting)中的存储库下载最后的六个点光照文件并将它们放置在您的项目中，也可以下载 Gem 并将其添加到引擎中（参见 [在项目中添加和删除 Gem](/docs/user-guide/project-config/add-remove-gems/)）。

恭喜，您现在已经完成了本教程！
