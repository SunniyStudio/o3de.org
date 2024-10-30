---
title: Debug Rendering 组件
linktitle: Debug Rendering
description: '在Open 3D Engine (O3DE)中使用 Atom 渲染器的Debug Rendering关卡组件  '
toc: true
---

**Debug Rendering 关卡组件** 用于可视化场景的渲染信息，如反照率和粗糙度等材质属性，或直接/间接、漫反射/镜面等照明因素。


## 提供方 ##

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## 属性

{{< image-width "/images/user-guide/components/reference/atom/debug-rendering/debug-rendering-component.jpg" "500" "Debug Render Component" >}}


## Base 属性

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Enable Render Debugging** | 如果启用，则使用渲染调试。如果禁用，该组件中的属性将不起作用。 | Boolean | `Enabled` |
| **Debug View Mode** | 指定在视口中显示的调试信息。对于金属材料，`Base Color`显示用于金属反射的颜色，`Albedo`显示黑色，因为没有漫反射。 | `None`,  `Base Color`, `Albedo`, `Roughness`, `Metallic`, `Normal`, `Tangent`, `Bitangent`, `CascadeShadows` | `None` |


## Lighting 属性

如果只将物体的法线以颜色的形式输出到屏幕上，就很难判断物体的法线是否正确。**Lighting Source**属性中的 `Debug Light` 选项会关闭场景中的所有照明，只有一束不会投射阴影的定向光除外。您可以使用和旋转这束光来仔细检查场景中物体的法线和其他材质属性。

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Lighting Type** | 是显示漫反射照明、镜面反射照明，还是两者兼而有之。 | `Diffuse + Specular`, `Diffuse`, `Specular` | `Diffuse + Specular` |
| **Lighting Source** | 您可以选择显示水平仪灯光（`Direct + Indirect`）、隔离水平仪灯光的贡献（`Direct` 或 `Indirect`），或者禁用水平仪灯光并显示不投射阴影的调试方向灯（`Debug Light`）。调试方向灯可以通过下面的 **Debug Light-** 属性进行配置。  | `Direct + Indirect`, `Direct`, `Indirect`, `Debug Light` | `Direct + Indirect` |
| **Debug Light Azimuth** | 通过围绕 Z 轴旋转灯光来设置灯光的方位角。旋转值单位为度。 | -360.0 to 360.0 | `0.0` |
| **Debug Light Elevation** | 通过围绕 X 轴旋转灯光来设置灯光的仰角。旋转值单位为度。正值使灯光向下。负值则使灯光向上。 | -90.0 to 90.0 | `60.0` |
| **Debug Light Color** | 调试方向灯的颜色。 | Color | (`255`, `255`, `255`) |
| **Debug Light Intensity** | 调试方向灯的强度。 | 0.0 - 25.0 | `2.0` |


## Material Override 属性

ebug Render组件允许您覆盖场景中所有材质的材质值。这些额外的选项可以帮助您可视化和调试光照和材质。例如，为了更好地了解场景中的光照情况，您可以将所有物体的颜色覆盖为白色或灰色。

可以覆盖**Base Color**, **Roughness**, 和 **Metallic**值。可以启用或禁用**Normal Maps**和**Detail Normal Maps**，但不能覆盖。如果两者都禁用，那么场景中的材质将只使用顶点法线。

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Override Base Color** | 如果启用，会将场景中所有材质的基色覆盖为下面指定的值。 | Boolean | Disabled |
| **Base Color Value** | 用于覆盖场景中所有材质基色的值。例如，将其设置为红色，场景就会完全变成红色。 | 每个通道八位色彩: 0-255 | (`128`, `128`, `128`) |
| **Override Roughness** | 如果启用，会将场景中所有材质的粗糙度覆盖为下面指定的值。 | Boolean | Disabled |
| **Roughness Value** | 用于覆盖场景中所有材质粗糙度的值。例如，如果您想使场景变得非常光滑和反光，请将其设置为 `0.0`。 | 0.0 - 1.0 | `1.0` |
| **Override Metallic** | 如果启用，会将场景中所有材质的金属属性覆盖为下面指定的值。 | Boolean | Disabled |
| **Metallic Value** | 用于覆盖场景中所有材质的金属效果的值。例如，您可以将其设置为 `1.0`，使场景中的所有材质都变成金属。 | 0.0 - 1.0 | `0.0` |
| **Enable Normal Maps** | 如果启用，会激活场景中所有材质的法线贴图。如果禁用，它也会禁用细节法线贴图，因此在着色计算中只使用顶点法线。 | Boolean | Enabled |
| **Enable Detail Normal Maps** | 如果启用，将激活场景中所有材质的细节法线贴图。不提供或不支持细节法线贴图的材质不受此选项影响。如果禁用了**Enable Normal Maps**，细节法线贴图将被停用。 | Boolean | Enabled |


## Custom Debug 属性

在调试着色器时，立即访问可调整的值可能非常有用。调试渲染组件提供了对四个**调试布尔值**和四个**调试浮点值**的访问。这些值被传递到场景着色器资源组（SRG）中，任何包含 `SceneSRG` 的着色器都可以访问这些值。有关 SRG 的更多信息，请参阅[Shader Resource Groups](/docs/atom-guide/dev-guide/shaders/azsl/#shader-resource-groups)。

自定义调试浮点和布尔属性通过[SceneSrg.azsli](https://github.com/o3de/o3de/blob/development/Gems/Atom/Feature/Common/Assets/ShaderResourceGroups/SceneSrg.azsli) 和 [Debug.azsli](https://github.com/o3de/o3de/blob/development/Gems/Atom/Feature/Common/Assets/ShaderLib/Atom/Features/Debug.azsli)文件暴露给着色器。

{{< important >}}
这些变量仅用于帮助您在本地调试着色器。出于最佳实践的考虑，当你完成调试后，我们建议你删除对这些变量的任何使用。如果你正在向源代码库或你团队自己的代码库贡献着色器代码，删除这些变量尤为重要。否则，其他着色器作者在使用相同变量时可能会遇到不必要的副作用。
{{< /important >}}


## 示例

**Debug View Mode** 设置为 `Albedo`的示例:

{{< image-width "/images/user-guide/components/reference/atom/debug-rendering/albedo.jpg" "1500" "Screeshot with Debug View Mode set to Albedo" >}}


调试光照的示例。通过将 **Lighting Type** 设置为 `Diffuse` 和 **Lighting Source** 设置为`Indirect`:

{{< image-width "/images/user-guide/components/reference/atom/debug-rendering/indirect-diffuse.jpg" "1500" "Screeshot with Lighting Type set to Diffuse and Lighting Source set to Indirect" >}}


调试光照的示例：

{{< image-width "/images/user-guide/components/reference/atom/debug-rendering/debug-light.jpg" "1500" "Screenshot using the Debug Light" >}}


调试材质的示例，将 **Override Base Color** 设置为灰色 (`128`, `128`, `128`):

{{< image-width "/images/user-guide/components/reference/atom/debug-rendering/color-override.jpg" "1500" "Screenshot overriding the base color in the scene to grey" >}}

