---
title: "Lua 材质函数 API"
description: "Lua 材质函数允许自定义处理材质属性的逻辑。"
toc: true
---

Lua 材质函数允许自定义处理材质属性的逻辑。它们可以读取属性值、设置着色器输入和着色器选项、配置渲染状态、调整编辑器中材质属性的可见性等。

有关 Lua 编程语言的一般信息，请参阅 [学习 Lua](/docs/user-guide/scripting/lua/#learning-lua)。

{{< note >}}
**Lua 材质函数**有别于**[Lua 游戏脚本](/docs/user-guide/scripting/lua/)**。虽然它们都使用 Lua 语言，但应用程序接口和编程环境大多是分开的。EBuses、属性表和实体集成等功能是游戏脚本独有的，Lua 材质函数无法使用。[Lua 数学库](/docs/user-guide/scripting/lua/math-library/) 是 API 中唯一两个系统通用的部分。
{{< /note >}}

## 名称上下文

材质函数可以在隐式名称上下文中运行，在这种上下文中，脚本中出现的名称会附加某些前缀。这样，同一个 Lua 脚本就可以在不同的上下文中使用。

例如，一种材质类型可能有一个名为 “metallic的组，而另一种材质类型则有一个名为 “metalness ”的组。在这两种情况下，组都具有`texture` 和 `factor`属性。Lua 函数将以简单的`texture` 和 `factor`来引用这些属性，系统会在内部自动预置相应的名称上下文。材质属性、着色器输入（常量或图像）和着色器选项可能各有一个单独的名称上下文。

有关如何指定名称上下文的详细信息，请在[材质类型文件规范](/docs/atom-guide/look-dev/materials/material-type-file-spec/)中搜索 “前缀”。

## 主函数

材质系统希望在 Lua 脚本的全局范围内定义以下函数。

#### Process(`context`)

这是材质**用于渲染**时运行的主要函数。它在材质首次初始化或相关材质属性值发生变化时运行。 `context` 对象提供了访问 API 的途径，以便访问**材质属性和着色器**。

#### ProcessEditor(`context`)

这是材质**在工具中**编辑时运行的主要函数。它在材质首次初始化或相关材质属性值发生变化时运行。`context`对象提供了访问 API 的途径，以便访问**材质属性及其元数据**。

#### GetMaterialPropertyDependencies()

返回函数可以读取的所有材质属性的列表。对于未报告依赖关系而访问的属性，将报告错误。

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

每个 **GetMaterialPropertyValue\_** 函数都会获取一个`string`属性名称，并返回相应类型的值。必须使用与材质属性的数据类型相匹配的版本。材质属性必须在 [GetMaterialPropertyDependencies](#main-functions) 中列出。

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

返回给定名称的属性是否存在的布尔值。请注意，材质属性不必在 [GetMaterialPropertyDependencies](#main-functions) 中列出，因为此函数仅检查可用性，而不是**读取值**。

### SetShaderConstant\_

每个 **SetShaderConstant\_** 函数都需要一个 `string` 着色器输入名称和要设置的值。您必须使用与着色器输入数据类型相匹配的版本。

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

每个**SetShaderOptionValue\_** 函数都会接收一个 `string` 着色器选项名称和要设置的值。该值将应用于材质类型中所有具有给定名称选项的着色器。必须使用与着色器选项的数据类型相匹配的版本。着色器选项必须列在[GetShaderOptionDependencies](#main-functions)中。

  * **SetShaderOptionValue_bool**(`string`, `boolean`)
  * **SetShaderOptionValue_uint**(`string`, `number`)
  * **SetShaderOptionValue_enum**(`string`, `number`)

{{< note >}}
在 `ShaderItem` 对象中也有类似的 [SetShaderOptionValue\_](#setshaderoptionvalue_-1) 函数，可对单个着色器进行操作。
{{< /note >}}

### GetShaderCount()

返回材质类型中着色器的数量。

### GetShader(`number`)

返回给定索引处的 `ShaderItem` 或一个虚拟的 `ShaderItem` （如果索引超出范围）。参见 [ShaderItem 函数](#shaderitem-functions)。

### GetShaderByTag(`string`)

返回具有给定标签名称的`ShaderItem` ，如果未找到该名称，则返回一个虚拟的`ShaderItem` 。请参阅[ShaderItem 函数](#shaderitem-functions)和[材质类型文件规范中的着色器标记](/docs/atom-guide/look-dev/materials/material-type-file-spec/#shaders)。

### HasShaderWithTag(`string`)

返回一个 `boolean` 值，表示是否存在具有给定标签名称的着色器。请参阅[材质类型文件规范中的着色器标签](/docs/atom-guide/look-dev/materials/material-type-file-spec/#shaders)。

## ProcessEditor(context) 函数

这些函数在传递给 `ProcessEditor` 函数的 `context` 对象中可用。

### GetMaterialPropertyValue\_

每个 **GetMaterialPropertyValue\_** 函数都会获取一个`string`属性名称，并返回相应类型的值。您必须使用与材质属性的数据类型相匹配的版本。材质属性必须在 [GetMaterialPropertyDependencies](#main-functions)中列出。

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

### SetMaterialProperty

每个 **SetMaterialProperty** 函数都接收一个`string`属性名，并设置该属性的编辑器元数据的某些方面。请参阅 [材质类型文件规范中的属性布局](/docs/atom-guide/look-dev/materials/material-type-file-spec/#propertylayout) 中的相应项。请注意，材质属性不必在 [GetMaterialPropertyDependencies](#main-functions)中列出，因为这些函数只是设置元数据，而不是**读取数值**。

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
* **SetMaterialPropertyGroupVisibility**(`string`, [`MaterialPropertyVisibility`](#materialpropertyvisibility)): 类似于`SetMaterialPropertyVisibility`，设置整个组而不是单个属性的可见性。

## ShaderItem functions

ShaderItem 是一个 lua `userdata`项，具有以下功能：

### GetRenderStatesOverride()

返回一个 [`RenderStates`](#renderstates-functions) 对象，该对象可用于为任何可用的呈现状态设置重载。

### SetEnabled(boolean)

设置是否启用着色器。

### SetDrawListTagOverride(`string`)

覆盖着色器将使用的绘制列表标签名称。设置为空字符串可清除覆盖并恢复 *.shader* 文件中的值。参见[着色器文件规范](/docs/atom-guide/look-dev/shaders/shader-file-spec/)中的`DrawList`。

### SetShaderOptionValue\_

每个 **SetShaderOptionValue\_** 函数都使用一个`string`着色器选项名称和值来更新此着色器的一个选项。你必须使用与着色器选项的数据类型相匹配的版本。着色器选项必须在 [GetShaderOptionDependencies](#main-functions)中列出。
  * **SetShaderOptionValue_bool**(`string`, `boolean`)
  * **SetShaderOptionValue_uint**(`string`, `number`)
  * **SetShaderOptionValue_enum**(`string`, `number`)

{{< note >}}
在 `Process(context)` 中也有类似的 [SetShaderOptionValue\_](#setshaderoptionvalue_) 函数，可对单个着色器进行操作。
{{< /note >}}

## RenderStates 函数

`RenderStates` 是一个具有以下功能的 lua `userdata`项。每个渲染状态都有一个 **Set** 函数用于设置覆盖值，还有一个 **Clear** 函数用于清除覆盖值并恢复默认值。这些函数中的大部分都是对低级渲染 API 的轻量封装。

### Multisample State 函数

  * **SetMultisampleCustomPosition**(`number` multisampleCustomLocationIndex, `number` x, `number` y)
  * ClearMultisampleCustomPosition
  * **SetMultisampleCustomPositionCount**(`number`)
  * ClearMultisampleCustomPositionCount
  * **SetMultisampleCount**(`number`)
  * ClearMultisampleCount
  * **SetMultisampleQuality**(`number`)
  * ClearMultisampleQuality

### Raster State 函数

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

### Blend State 函数

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

### Depth/Stencil State 函数
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
  
## 枚举类型

这些枚举类型通过以下全局值反射到 lua 中：

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


