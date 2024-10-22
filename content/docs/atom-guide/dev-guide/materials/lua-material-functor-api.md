---
title: "Lua 材质函数 API"
description: "Lua 材料函数允许自定义处理材料属性的逻辑。"
toc: true
---

Lua 材质函数允许自定义处理材质属性的逻辑。它们可以读取属性值、设置着色器输入和着色器选项、配置渲染状态、调整编辑器中材质属性的可见性等。

有关 Lua 编程语言的一般信息，请参阅 [学习 Lua](/docs/user-guide/scripting/lua/#learning-lua)。

{{< note >}}
**Lua 材质函数**有别于**[Lua 游戏脚本](/docs/user-guide/scripting/lua/)**。虽然它们都使用 Lua 语言，但应用程序接口和编程环境大多是分开的。EBuses、属性表和实体集成等功能是游戏脚本独有的，Lua 材质函数无法使用。[Lua 数学库](/docs/user-guide/scripting/lua/math-library/) 是 API 中唯一两个系统通用的部分。
{{< /note >}}

## 名称上下文

材质函数可以在隐式名称上下文中运行，在这种上下文中，脚本中出现的名称会附加某些前缀。这样，同一个 Lua 脚本就可以在不同的上下文中使用。

例如，一种材料类型可能有一个名为 “metallic的组，而另一种材料类型则有一个名为 “metalness ”的组。在这两种情况下，组都具有`texture` 和 `factor`属性。Lua 函数将以简单的`texture` 和 `factor`来引用这些属性，系统会在内部自动预置相应的名称上下文。材质属性、着色器输入（常量或图像）和着色器选项可能各有一个单独的名称上下文。

有关如何指定名称上下文的详细信息，请在[材料类型文件规范](/docs/atom-guide/look-dev/materials/material-type-file-spec/)中搜索 “前缀”。

## 主函数

材质系统希望在 Lua 脚本的全局范围内定义以下函数。

#### Process(`context`)

这是材料**用于渲染**时运行的主要函数。它在材质首次初始化或相关材质属性值发生变化时运行。 `context` 对象提供了访问 API 的途径，以便访问**材质属性和着色器**。

#### ProcessEditor(`context`)

这是材料**在工具中**编辑时运行的主要函数。它在材料首次初始化或相关材料属性值发生变化时运行。`context`对象提供了访问 API 的途径，以便访问**材料属性及其元数据**。

#### GetMaterialPropertyDependencies()

返回函数可以读取的所有材料属性的列表。对于未报告依赖关系而访问的属性，将报告错误。

#### GetShaderOptionDependencies()

返回函数可能设置的所有着色器选项的列表。对于未报告依赖关系而访问的选项，将报告错误。(请注意，C++ API 中的`Material::SetSystemShaderOption`函数只能在**未**声明为依赖关系或未被材质类型使用的着色器选项上调用）。

**示例**:
```lua
function GetMaterialPropertyDependencies()
    return {"textureMap", "useTexture"}
end
 
function GetShaderOptionDependencies()
    return {"o_metallic_useTexture"}
end

function Process(context)
    local textureMap = context:GetMaterialPropertyValue_Image("textureMap")
    local useTexture = context:GetMaterialPropertyValue_bool("useTexture")
    context:SetShaderOptionValue_bool("o_metallic_useTexture", useTexture and textureMap ~= nil)
end

function ProcessEditor(context)
    local textureMap = context:GetMaterialPropertyValue_Image("textureMap")
    local useTexture = context:GetMaterialPropertyValue_bool("useTexture")

    if(nil == textureMap) then
        context:SetMaterialPropertyVisibility("useTexture", MaterialPropertyVisibility_Hidden)
        context:SetMaterialPropertyVisibility("textureMapUv", MaterialPropertyVisibility_Hidden)
        context:SetMaterialPropertyVisibility("factor", MaterialPropertyVisibility_Enabled)
    elseif(not useTexture) then
        context:SetMaterialPropertyVisibility("useTexture", MaterialPropertyVisibility_Enabled)
        context:SetMaterialPropertyVisibility("textureMapUv", MaterialPropertyVisibility_Disabled)
        context:SetMaterialPropertyVisibility("factor", MaterialPropertyVisibility_Enabled)
    else
        context:SetMaterialPropertyVisibility("useTexture", MaterialPropertyVisibility_Enabled)
        context:SetMaterialPropertyVisibility("textureMapUv", MaterialPropertyVisibility_Enabled)
        context:SetMaterialPropertyVisibility("factor", MaterialPropertyVisibility_Hidden)
    end
end
```

## 全局实用函数

这些函数可在全局范围内使用。

* **Error**(`string`): 向 `AZ_Error`报告错误信息。
* **Warning**(`string`): 向 `AZ_Warning`报告警告信息。
* **Print**(`string`): 向`AZ_TracePrintf`报告信息。

## Process(context) 函数

这些函数在传递给 `Process` 函数的 `context` 对象中可用。

### GetMaterialPropertyValue\_

每个 **GetMaterialPropertyValue\_** 函数都会获取一个`string`属性名称，并返回相应类型的值。必须使用与材料属性的数据类型相匹配的版本。材料属性必须在 [GetMaterialPropertyDependencies](#main-functions) 中列出。

  * **GetMaterialPropertyValue_bool**(`string`): 返回`boolean`
  * **GetMaterialPropertyValue_int**(`string`): 返回`number`
  * **GetMaterialPropertyValue_uint**(`string`): 返回`number`
  * **GetMaterialPropertyValue_enum**(`string`): 返回`number`
  * **GetMaterialPropertyValue_float**(`string`): 返回`number`
  * **GetMaterialPropertyValue_Vector2**(`string`): 返回`Vector2`类型。查看[Lua Math Library](/docs/user-guide/scripting/lua/math-library/).
  * **GetMaterialPropertyValue_Vector3**(`string`): 返回`Vector3`类型。查看[Lua Math Library](/docs/user-guide/scripting/lua/math-library/).
  * **GetMaterialPropertyValue_Vector4**(`string`): 返回`Vector4`类型。查看[Lua Math Library](/docs/user-guide/scripting/lua/math-library/).
  * **GetMaterialPropertyValue_Color**(`string`): 返回`Color`类型。查看[Lua Math Library](/docs/user-guide/scripting/lua/math-library/).
  * **GetMaterialPropertyValue_Image**(`string`): 返回`userdata`类型的泛型指南，如果图像可用；否则返回`nil`。

### HasMaterialProperty(`string`)

Returns a boolean whether a property with the given name exists. Note the material property does not have to be listed in [GetMaterialPropertyDependencies](#main-functions) because this function is checking availability only, not *reading values*.

### SetShaderConstant\_

Each **SetShaderConstant\_** function takes a `string` shader input name and value to set. You must use the version that matches the data type of the shader input.

  * **SetShaderConstant_bool**(`string`, `boolean`)
  * **SetShaderConstant_int**(`string`, `number`)
  * **SetShaderConstant_uint**(`string`, `number`)
  * **SetShaderConstant_float**(`string`, `number`)
  * **SetShaderConstant_Vector2**(`string`, `Vector2`)
  * **SetShaderConstant_Vector3**(`string`, `Vector3`)
  * **SetShaderConstant_Vector4**(`string`, `Vector4`)
  * **SetShaderConstant_Color**(`string`, `Color`)
  * **SetShaderConstant_Matrix3x3**(`string`, `Matrix3x3`)
  * **SetShaderConstant_Matrix4x4**(`string`, `Matrix4x4`)

### SetShaderOptionValue\_

Each **SetShaderOptionValue\_** function takes a `string` shader option name and value to set. The value will be applied to all shaders in the material type that have an option with the given name. You must use the version that matches the data type of the shader option. The shader option must be listed in [GetShaderOptionDependencies](#main-functions).

  * **SetShaderOptionValue_bool**(`string`, `boolean`)
  * **SetShaderOptionValue_uint**(`string`, `number`)
  * **SetShaderOptionValue_enum**(`string`, `number`)

{{< note >}}
There are similar [SetShaderOptionValue\_](#setshaderoptionvalue_-1) functions in the `ShaderItem` object that operate on a single shader.
{{< /note >}}

### GetShaderCount()

Returns the number of shaders in the material type.

### GetShader(`number`)

Returns a `ShaderItem` at a given index, or a dummy `ShaderItem` if the index is out of bounds. See [ShaderItem functions](#shaderitem-functions). 

### GetShaderByTag(`string`)

Returns a `ShaderItem` that has a given tag name, or a dummy `ShaderItem` if the name is not found. See [ShaderItem functions](#shaderitem-functions) and [Shader tags in the Material Type File Specification](/docs/atom-guide/look-dev/materials/material-type-file-spec/#shaders). 

### HasShaderWithTag(`string`)

 Returns a `boolean` whether a shader with the given tag name exists. See [Shader tags in the Material Type File Specification](/docs/atom-guide/look-dev/materials/material-type-file-spec/#shaders).

## ProcessEditor(context) functions

These functions are available in the `context` object that is passed to the `ProcessEditor` function.

### GetMaterialPropertyValue\_

Each **GetMaterialPropertyValue\_** function takes a `string` property name and Returns a value of the appropriate type. You must use the version that matches the data type of the material property. The material property must be listed in [GetMaterialPropertyDependencies](#main-functions).

  * **GetMaterialPropertyValue_bool**(`string`): 返回`boolean`
  * **GetMaterialPropertyValue_int**(`string`): 返回`number`
  * **GetMaterialPropertyValue_uint**(`string`): 返回`number`
  * **GetMaterialPropertyValue_enum**(`string`): 返回`number`
  * **GetMaterialPropertyValue_float**(`string`): 返回`number`
  * **GetMaterialPropertyValue_Vector2**(`string`): 返回`Vector2`类型。查看[Lua Math Library](/docs/user-guide/scripting/lua/math-library/).
  * **GetMaterialPropertyValue_Vector3**(`string`): 返回`Vector3`类型。查看[Lua Math Library](/docs/user-guide/scripting/lua/math-library/).
  * **GetMaterialPropertyValue_Vector4**(`string`): 返回`Vector4`类型。查看[Lua Math Library](/docs/user-guide/scripting/lua/math-library/).
  * **GetMaterialPropertyValue_Color**(`string`): 返回`Color`类型。查看[Lua Math Library](/docs/user-guide/scripting/lua/math-library/).
  * **GetMaterialPropertyValue_Image**(`string`): 返回generic pointer `userdata` type if an image is available, or `nil` otherwise.

### SetMaterialProperty

Each **SetMaterialProperty** function takes a `string` property name and sets some aspect of the property's editor metadata. See corresponding items in [Property Layout in Material Type File Specification](/docs/atom-guide/look-dev/materials/material-type-file-spec/#propertylayout). Note the material property does not have to be listed in [GetMaterialPropertyDependencies](#main-functions) because these functions are just setting metadata, not *reading values*.

  * **SetMaterialPropertyDescription**(*`string`*, *`string`*)
  * **SetMaterialPropertyMinValue_int**(`string`, `number`)
  * **SetMaterialPropertyMinValue_uint**(`string`, `number`)
  * **SetMaterialPropertyMinValue_float**(`string`, `number`)
  * **SetMaterialPropertyMaxValue_int**(`string`, `number`)
  * **SetMaterialPropertyMaxValue_uint**(`string`, `number`)
  * **SetMaterialPropertyMaxValue_float**(`string`, `number`)
  * **SetMaterialPropertySoftMinValue_int**(`string`, `number`)
  * **SetMaterialPropertySoftMinValue_uint**(`string`, `number`)
  * **SetMaterialPropertySoftMinValue_float**(`string`, `number`)
  * **SetMaterialPropertySoftMaxValue_int**(`string`, `number`)
  * **SetMaterialPropertySoftMaxValue_uint**(`string`, `number`)
  * **SetMaterialPropertySoftMaxValue_float**(`string`, `number`)
* **SetMaterialPropertyGroupVisibility**(`string`, [`MaterialPropertyVisibility`](#materialpropertyvisibility)): Similar to `SetMaterialPropertyVisibility`, sets the visibility of an entire group rather than a single property.

## ShaderItem functions

ShaderItem is a lua `userdata` item with the following functions:

### GetRenderStatesOverride()

Returns a [`RenderStates`](#renderstates-functions) object that can be used to set overrides for any available render state.

### SetEnabled(boolean)

Sets whether the shader should be enabled or not.

### SetDrawListTagOverride(`string`)

Overrides the draw list tag name that the shader will use. Set to empty string to clear the override and restore the value from the *.shader* file. See `DrawList` in the [Shader File Specification](/docs/atom-guide/look-dev/shaders/shader-file-spec/).

### SetShaderOptionValue\_

Each **SetShaderOptionValue\_** function takes a `string` shader option name and value to update one of this shader's options. You must use the version that matches the data type of the shader option. The shader option must be listed in [GetShaderOptionDependencies](#main-functions).
  * **SetShaderOptionValue_bool**(`string`, `boolean`)
  * **SetShaderOptionValue_uint**(`string`, `number`)
  * **SetShaderOptionValue_enum**(`string`, `number`)

{{< note >}}
There are similar [SetShaderOptionValue\_](#setshaderoptionvalue_) functions in `Process(context)` that operate on a single shader.
{{< /note >}}

## RenderStates functions

`RenderStates` is a lua `userdata` item with the following functions. For each render state there is a **Set** function for setting an override value and a **Clear** function for clearing the override and restoring the default value. Most of these functions are just light wrappers over lower level render APIs.

### Multisample State functions

  * **SetMultisampleCustomPosition**(`number` multisampleCustomLocationIndex, `number` x, `number` y)
  * ClearMultisampleCustomPosition
  * **SetMultisampleCustomPositionCount**(`number`)
  * ClearMultisampleCustomPositionCount
  * **SetMultisampleCount**(`number`)
  * ClearMultisampleCount
  * **SetMultisampleQuality**(`number`)
  * ClearMultisampleQuality

### Raster State functions

  * **SetFillMode**([`FillMode`](#fillmode))
  * ClearFillMode
  * **SetCullMode**([`CullMode`](#cullmode))
  * ClearCullMode
  * **SetDepthBias**(`number`)
  * ClearDepthBias
  * **SetDepthBiasClamp**(`number`)
  * ClearDepthBiasClamp
  * **SetDepthBiasSlopeScale**(`number`)
  * ClearDepthBiasSlopeScale
  * **SetMultisampleEnabled**(`boolean`)
  * ClearMultisampleEnabled
  * **SetDepthClipEnabled**(`boolean`)
  * ClearDepthClipEnabled
  * **SetConservativeRasterEnabled**(`boolean`)
  * ClearConservativeRasterEnabled
  * **SetForcedSampleCount**(`number`)
  * ClearForcedSampleCount

### Blend State functions

  * **SetAlphaToCoverageEnabled**(`boolean`)
  * ClearAlphaToCoverageEnabled
  * **SetIndependentBlendEnabled**(`boolean`)
  * ClearIndependentBlendEnabled
  * **SetBlendEnabled**(`boolean`)
  * ClearBlendEnabled
  * **SetBlendWriteMask**(`number`)
  * ClearBlendWriteMask
  * **SetBlendSource**([`BlendFactor`](#blendfactor))
  * ClearBlendSource
  * **SetBlendDest**([`BlendFactor`](#blendfactor))
  * ClearBlendDest
  * **SetBlendOp**([`BlendOp`](#blendop))
  * ClearBlendOp
  * **SetBlendAlphaSource**([`BlendFactor`](#blendfactor))
  * ClearBlendAlphaSource
  * **SetBlendAlphaDest**([`BlendFactor`](#blendfactor))
  * ClearBlendAlphaDest
  * **SetBlendAlphaOp**([`BlendOp`](#blendop))
  * ClearBlendAlphaOp

### Depth/Stencil State functions
  * **SetDepthEnabled**(`boolean`)
  * ClearDepthEnabled
  * **SetDepthWriteMask**([`DepthWriteMask`](#depthwritemask))
  * ClearDepthWriteMask
  * **SetDepthComparisonFunc**([`ComparisonFunc`](#comparisonfunc))
  * ClearDepthComparisonFunc
  * **SetStencilEnabled**(`boolean`)
  * ClearStencilEnabled
  * **SetStencilReadMask**(`number`)
  * ClearStencilReadMask
  * **SetStencilWriteMask**(`number`)
  * ClearStencilWriteMask
  * **SetStencilFrontFaceFailOp**([`StencilOp`](#stencilop))
  * ClearStencilFrontFaceFailOp
  * **SetStencilFrontFaceDepthFailOp**([`StencilOp`](#stencilop))
  * ClearStencilFrontFaceDepthFailOp
  * **SetStencilFrontFacePassOp**([`StencilOp`](#stencilop))
  * ClearStencilFrontFacePassOp
  * **SetStencilFrontFaceFunc**([`ComparisonFunc`](#comparisonfunc))
  * ClearStencilFrontFaceFunc
  * **SetStencilBackFaceFailOp**([`StencilOp`](#stencilop))
  * ClearStencilBackFaceFailOp
  * **SetStencilBackFaceDepthFailOp**([`StencilOp`](#stencilop))
  * ClearStencilBackFaceDepthFailOp
  * **SetStencilBackFacePassOp**([`StencilOp`](#stencilop))
  * ClearStencilBackFacePassOp
  * **SetStencilBackFaceFunc**([`ComparisonFunc`](#comparisonfunc))
  * ClearStencilBackFaceFunc
  
## Enum types
  
These enum types are reflected to lua with the following global values:

#### MaterialPropertyVisibility

* MaterialPropertyVisibility_Enabled
* MaterialPropertyVisibility_Disabled
* MaterialPropertyVisibility_Hidden

#### FillMode

* FillMode_Solid
* FillMode_Wireframe
* FillMode_Invalid

#### CullMode

* CullMode_None
* CullMode_Front
* CullMode_Back
* CullMode_Invalid

#### ComparisonFunc

* ComparisonFunc_Never
* ComparisonFunc_Less
* ComparisonFunc_Equal
* ComparisonFunc_LessEqual
* ComparisonFunc_Greater
* ComparisonFunc_NotEqual
* ComparisonFunc_GreaterEqual
* ComparisonFunc_Always
* ComparisonFunc_Invalid

#### BlendFactor

* BlendFactor_Zero
* BlendFactor_One
* BlendFactor_ColorSource
* BlendFactor_ColorSourceInverse
* BlendFactor_AlphaSource
* BlendFactor_AlphaSourceInverse
* BlendFactor_AlphaDest
* BlendFactor_AlphaDestInverse
* BlendFactor_ColorDest
* BlendFactor_ColorDestInverse
* BlendFactor_AlphaSourceSaturate
* BlendFactor_Factor
* BlendFactor_FactorInverse
* BlendFactor_ColorSource1
* BlendFactor_ColorSource1Inverse
* BlendFactor_AlphaSource1
* BlendFactor_AlphaSource1Inverse
* BlendFactor_Invalid

#### BlendOp

* BlendOp_Add
* BlendOp_Subtract
* BlendOp_SubtractReverse
* BlendOp_Minimum
* BlendOp_Maximum
* BlendOp_Invalid

#### DepthWriteMask

* DepthWriteMask_Zero
* DepthWriteMask_All
* DepthWriteMask_Invalid

#### StencilOp

* StencilOp_Keep
* StencilOp_Zero
* StencilOp_Replace
* StencilOp_IncrementSaturate
* StencilOp_DecrementSaturate
* StencilOp_Invert
* StencilOp_Increment
* StencilOp_Decrement
* StencilOp_Invalid


