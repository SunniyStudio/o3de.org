---
title: "材质类型文件规范"
description: "材质类型文件（`*.materialtype`）是以 JSON 格式编写的。"
toc: true
---

材质类型文件(`*.materialtype`)采用书面 JSON 格式，包含以下元素。

## **description**  
提供材质类型作者的描述或评论。

## **version**  
表示材质类型的当前版本号。使用此材质类型创建的任何材质都会在其[`materialTypeVersion`](material-file-spec/#materialtypeversion-optional)字段中保存此值。

## **versionUpdates**  

为引用该 `.materialtype`旧版本的`.material`文件提供向后兼容性。本节包括一个更新步骤列表，可将材质数据从一个版本更新到下一个版本。每当作者对材质类型做出会破坏现有材质的改动（如重命名属性）时，他们可以递增`version`号（前面已讨论过），并在此处提供新版本更新说明。然后，任何加载旧版`.material`文件的系统都可以自动升级，使其与最新的材质类型兼容。

* **toVersion**: 表示引入这些更改时的材质类型版本。更新步骤将应用于 [`materialTypeVersion`](material-file-spec/#materialtypeversion-optional) 编号小于此值的任何材质。

* **actions**: 应用此更新时要执行的操作列表。每个操作首先会有一个 `op` 参数，用于指示要执行的操作。根据使用的操作，还必须指定其他参数。可用的操作及其所需参数如下：
1
| op            | 参数 1 | 说明          | 参数 2 | 说明        |
|---------------|-------------|----------------------|-------------|--------------------|
| rename        | from        | 旧的属性名称    | to          | 新的属性名称  |
| setValue      | name        | 要设置的属性名称 | value       | 新属性值，必须是给定属性的适当类型 |

{{< note >}}
要升级`.material`源文件以匹配最新版本的材质类型，请在**材质编辑器**中打开它，然后保存。
{{< /note >}}

#### 示例

```json
    ...
    "version": 5,
    "versionUpdates": [
        {
            "toVersion": 4,
            "actions": [
                {"op": "rename", "from": "opacity.doubleSided", "to": "general.doubleSided"}
            ]
        },
        {
            "toVersion": 5,
            "actions": [
                {"op": "rename", "from": "irradiance.color", "to": "irradiance.manualColor"},
                {"op": "setValue", "name": "irradiance.irradianceColorSource", "value": "Manual"}
            ]
        }
    ],
    ...
```

## **propertyLayout**
本节定义了 “材质编辑器 ”中可用的属性集，以及它们将连接到哪些着色器输入或着色器选项。属性按组排列，每个组可以包含其他组，形成一个层次结构。材质编辑器的属性检查器会相应地将属性组织到组面板中。

通常的做法是使用 `$import` 功能将属性组定义 “因子化 ”为单独的 JSON 文件。这样就可以在多个 `.materialtype` 文件中重复使用属性组。

* **propertyGroups**: 属性组的顶层列表。每个组包含以下内容：
  * **name**: 该组的标识符。该值必须采用 C 风格格式，并且在同级组中是唯一的。
  * **displayName**: 该组的给定名称，将显示在 “材质编辑器 ”中。
  * **description**: 该组的指定描述，将作为工具提示出现在材质编辑器中。
  * **shaderInputsPrefix**: 该组中出现的任何着色器输入名称（如着色器常量或着色器图像）都将自动预置此值。这在使用 `$import` 将共享组定义包含在另一个组中时非常有用。
  * **shaderOptionsPrefix**: 任何出现在该组中的着色器选项名称都会自动预置该值。这在使用 `$import` 将共享组定义包含在另一个组中时非常有用。
  * **properties**: 定义将出现在该组中的属性列表。
    * **name**: 该属性的标识符。该值必须采用 C 风格格式，并且在同级属性中是唯一的。
    * **displayName**: 该属性在材质编辑器中的给定名称。
    * **description**: 该属性的指定描述，将作为工具提示出现在材质编辑器中。
    * **visibility**: 该属性的初始可见性。默认为`Enabled`。可能的值有：
      - Enabled
      - Disabled
      - Hidden
    * **type**: 该属性的数据类型。支持的类型包括：
      - Bool
      - Int
      - UInt
      - Float
      - Vector2
      - Vector3
      - Vector4
      - Color
      - Image
      - Enum
    * **enumValues**: 定义 `Enum` 类型属性可能值的名称列表。
    * **enumIsUv**: 如果 `true`，且 `type` 设置为 `Enum`，则使用[`uvNameMap`](#uvnamemap)中的 UV 名称作为可能的枚举值。
    * **defaultValue**: 该属性的默认值。必须根据标准 JSON 序列化（参见 [O3DE 数据类型的 JSON 序列化](/docs/user-guide/programming/serialization/json-data-types)）使用适当的数据类型指定该值。如果没有提供缺省值，那么根据数据类型，缺省值将是 `false`, `0`或空。
    * **min**: 用户可使用滑块或类似 UI widget 配置的属性最小值。可与 `max`、`softMax` 或两者结合使用。
    * **max**: 用户可以使用滑块或类似 UI widget 配置的属性的最大值。可与 `min`、`softMin` 或两者结合使用。
    * **softMin**: 用户可使用滑块或类似 UI widget 配置的属性最小值。不过，用户也可以手动输入一个更小的值。可与 `max`、`softMax` 或两者结合使用。
    * **softMax**: 用户可使用滑块或类似 UI widget 配置的属性最大值。不过，用户也可以手动输入更大的值。可与 `min`、`softMin` 或两者结合使用。
    * **step**: 表示滑块或类似 UI widget 使用的增量大小。
    * **vectorLabels**: 为 `Vector` 类型属性的元素提供标签列表。默认情况下，标签为 “X”、“Y”、“Z”、“W”。
    * **connection**: 定义将属性值传递给着色器的连接。每个连接都定义了连接的`type`、`name`和可选的`shaderIndex`。连接是可选的，如果省略，则可通过材质函数将属性连接到着色器。
      * **type**: 进行哪种类型的连接：
        - ShaderInput - 连接到 SRG_PerMaterial ShaderResourceGroup中的着色器常量或着色器图像。
        - ShaderOption - 连接到着色器选项。
      * **name**: 要连接的着色器输入或着色器选项的名称。
      * **shaderIndex**: 要连接到[`shaders`](#shaders)列表中哪个着色器的索引。默认情况下，它会连接到所有具有此名称选项的着色器。
  * **propertyGroups**: 其他属性组可以嵌套在这个属性组中。
  * **functors**: 用于自定义处理该组和子组中属性的材质函数列表。所有属性引用、着色器输入名称和着色器选项名称都以该属性组为本地范围。例如，如果该组名为 “baseColor”（基色），并有一个属性 “textureMap”（纹理图），那么函数可以简单地将该属性引用为 “texture”（纹理），系统会自动将其解释为 “baseColor.texture”（基色.纹理）。同样，“shaderInputsPrefix”（着色器输入前缀）和 `shaderOptionsPrefix`（着色器选项前缀）也会自动应用。例如，如果 `shaderInputsPrefix` 是 “baseColor_”，而着色器有一个名为 “baseColor_tex ”的纹理输入，那么函数可以将纹理输入引用为简单的 “tex”，系统会自动将其转换为 “baseColor_tex”。有关材质函数的更多信息，请参阅下面的 [`functors`](#functors) 部分。

#### 示例

```json
...
    "propertyLayout": {
        "propertyGroups": [
            {
                "name": "iris", 
                "shaderInputsPrefix": "m_iris_",
                "shaderOptionsPrefix": "o_iris_",
                "displayName":  "Iris",
                "description":  "Material properties for the iris.",
                "propertyGroups": [
                    {
                        "$import": "MaterialInputs/BaseColorPropertyGroup.json"
                    },
                    {
                        "$import": "MaterialInputs/NormalPropertyGroup.json"
                    },
                    { 
                        "$import": "MaterialInputs/RoughnessPropertyGroup.json"
                    }
                ]
            },
            {
                "name": "sclera", 
                "shaderInputsPrefix": "m_sclera_",
                "shaderOptionsPrefix": "o_sclera_",
                "displayName":  "Sclera",
                "description":  "Material properties for the sclera.",
                "propertyGroups": [
                    {
                        "$import": "MaterialInputs/BaseColorPropertyGroup.json"
                    },
                    {
                        "$import": "MaterialInputs/NormalPropertyGroup.json"
                    },
                    { 
                        "$import": "MaterialInputs/RoughnessPropertyGroup.json"
                    }
                ]
            },
            {
                "name": "eye",
                "displayName": "Eye parameters",
                "description": "Properties to control eye-specific rendering behavior",
                "properties": [
                    {
                        "name": "irisDepth",
                        "displayName": "Iris Depth",
                        "description": "Distance from the object origin to the plane (XZ) where the iris lays.",
                        "type": "float",
                        "defaultValue": 0.48,
                        "min": 0.0,
                        "softMax": 1.0,
                        "connection": {
                            "type": "ShaderInput",
                            "name": "m_irisDepth"
                        }
                    },
                    {
                        "name": "irisRadius",
                        "displayName": "Iris Radius",
                        "description": "Radius of the iris. It extends the iris/sclera mask to get more samples from one or the other (no UV deformation/stretching).",
                        "type": "float",
                        "defaultValue": 0.2,
                        "min": 0.0,
                        "softMax": 0.5,
                        "connection": {
                            "type": "ShaderInput",
                            "name": "m_eyeIrisRadius"
                        }
                    },
                    ...
                ]
            },
            { 
                "name": "specularF0",
                "displayName": "Specular Reflectance f0",
                "description": "The constant f0 represents the specular reflectance at normal incidence (Fresnel 0 Angle). Used to adjust reflectance of non-metal surfaces.",
                "properties": [
                    {
                        "name": "factor",
                        "displayName": "Factor",
                        "description": "The default IOR is 1.5, which gives you 0.04 (4% of light reflected at 0 degree angle for dielectric materials). F0 values lie in the range 0-0.08, so that is why the default F0 slider is set on 0.5.",
                        "type": "Float",
                        "defaultValue": 0.5,
                        "min": 0.0,
                        "max": 1.0,
                        "connection": {
                            "type": "ShaderInput",
                            "name": "m_specularF0Factor"
                        }
                    },
                    {
                        "name": "textureMap",
                        "displayName": "Texture",
                        "description": "Texture for defining surface reflectance.",
                        "type": "Image",
                        "connection": {
                            "type": "ShaderInput",
                            "name": "m_specularF0Map"
                        }
                    },
                    {
                        "name": "useTexture",
                        "displayName": "Use Texture",
                        "description": "Whether to use the texture, or just default to the Factor value.",
                        "type": "Bool",
                        "defaultValue": true
                    },
                    {
                        "name": "textureMapUv",
                        "displayName": "UV",
                        "description": "Specular reflection map UV set",
                        "type": "Enum",
                        "enumIsUv": true,
                        "defaultValue": "Tiled",
                        "connection": {
                            "type": "ShaderInput",
                            "name": "m_specularF0MapUvIndex"
                        }
                    }
                ],
                "functors": [
                    {
                        "type": "UseTexture",
                        "args": {
                            "textureProperty": "textureMap",
                            "useTextureProperty": "useTexture",
                            "dependentProperties": ["textureMapUv"],
                            "shaderOption": "o_specularF0_useTexture"
                        }
                    }
                ]
            },
            { 
                "$import": "MaterialInputs/SubsurfaceAndTransmissionPropertyGroup.json"
            },
            { 
                "name": "general",
                "displayName": "General Settings",
                "description": "General settings.",
                "properties": [
                    {
                        "name": "applySpecularAA",
                        "displayName": "Apply Specular AA",
                        "description": "Whether to apply specular anti-aliasing in the shader.",
                        "type": "Bool",
                        "defaultValue": false,
                        "connection": {
                            "type": "ShaderOption",
                            "name": "o_applySpecularAA"
                        }
                    },
                    {
                        "name": "enableMultiScatterCompensation",
                        "displayName": "Multiscattering Compensation",
                        "description": "Whether to enable multiple scattering compensation.",
                        "type": "Bool",
                        "connection": {
                            "type": "ShaderOption",
                            "name": "o_enableMultiScatterCompensation"
                        }
                    }
                ]
            }
        ]
    },
...
```

## **version** (已弃用)

`propertyLayout`部分中的版本号已不再使用，取而代之的是之前显示的 [`version`](#version) 版本号。

## **groups** (已弃用)
`propertyGroups`（[propertyLayout](#propertylayout) 中）所取代，它可以支持在同一位置定义属性和组的任意数量的组级别。引擎仍可加载旧格式，但任何新的材质类型文件都应使用新格式。

{{< note >}}
旧版本的材质类型文件格式用独立的`groups` 和properties`部分来组织`propertyLayout`。这就造成了一些限制，例如将属性分组限制为只有一个级别。这样就很难找出可重复使用的属性组。
{{< /note >}}

## **properties** (已弃用)
这已被`propertyGroups`替代。相似的有[**groups (已弃用)**](#groups-deprecated)。

## **shaders**  

用于渲染此类型材质的着色器文件（`*.shader`）的引用数组。默认情况下，所有着色器都已启用，但可以使用 [`functors`](#functors) 关闭它们。

每个着色器项目都包含以下值。

* **file**: 着色器文件的路径。该路径必须与资产根目录或材质类型文件相对。
* **tag**: 该着色器项目的唯一名称，可用于在材质类型定义的其他地方（如材质函数中）引用该着色器。它必须是一个 C 样式标识符。
* **options**: 使用键/值对列表为该着色器中的任何选项设置初始值。

#### 示例
在本例中，我们引用了 ShadowMap 和 DepthPass 着色器。
```JSON
"shaders": [
    {
        "file": "../ShadowMap.shader",
        "tag": "shadowmap",
        "options": {
            "o_depthBiasMode": "DepthBiasMode::Auto"
        }
    },
    {
        "file": "../DepthPass.shader",
        "tag": "depth"
    },
    ...
]
```

## **functors**
材质函数数组。每个函数都会读取材质属性值，执行一些逻辑或计算，并相应设置着色器输入。这些函数可以用 Lua 或 C++ 来定义。每个函数数据包含以下内容：

* **type**: 函数类型的名称。可能的值有:
    - `Lua` - 用于自定义 Lua 脚本函数
    - 专用函数类型的名称（在 C++ 中定义）。
* **args**: 一个对象，包含要发送给该函数的所有参数的键/值对。参数因函数类型而异。

引擎提供了几种核心函数类型，在此一一列举。

{{< note >}}
其他宝石或游戏项目可以添加更多的函数类型。请尝试搜索 “RegisterMaterialFunctor ”的源代码，以了解可能存在的其他类型。
{{< /note >}}

### Lua

该函数类型使用自定义 Lua 脚本运行。它是最灵活的函数类型，几乎可用于任何情况。不过，与其他函数类型相比，它的性能较差。

有关 Lua 脚本本身的详细信息，请参阅 [Lua材质函数 API](/docs/atom-guide/dev-guide/materials/lua-material-functor-api/)  。

参数：
* **file**: 包含材质函数代码的 `.lua` 文件的路径。该路径必须相对于资产根目录或材质类型文件。
* **propertyNamePrefix**: 函数中出现的任何属性名称都将自动预置该值。请注意，这仅适用于在材质类型顶层定义的函数。属性组中的函数将从属性组中获取前缀。
* **srgNamePrefix**: 函数中出现的任何 ShaderResourceGroup 字段名称都将自动预置此值。请注意，这仅适用于在材质类型顶层定义的函数。属性组中的函数将从属性组中获取前缀。
* **optionsNamePrefix**: 函数中出现的任何着色器选项名称都将自动预置此值。请注意，这仅适用于在材质类型顶层定义的函数。属性组中的函数将从属性组中获取前缀。

#### 示例
```json
    ...
    {
        "type": "Lua",
        "args": {
            "file": "Materials/Types/StandardPBR_Roughness.lua",
            "propertyNamePrefix": "layer2_roughness.",
            "srgNamePrefix": "m_layer2_",
            "optionsNamePrefix": "o_layer2_"
        }
    },
    ...
```

### UseTexture

如果UseTexture **shader option**  **属性**为 false，或`Image`类型属性没有图像绑定，则将UseTexture **shader option** 设置为 false。

参数:
* **textureProperty**: `Image`类型属性的名称。
* **useTextureProperty**: `bool`类型属性的名称。如果纹理属性为空，该属性将被隐藏。
* **dependentProperties**: (可选）在未使用纹理时无关的其他属性列表。当纹理为空或未使用时，这些属性将被隐藏或禁用。
* **shaderTags**: (可选）如果提供，着色器选项将只在此着色器列表中设置。请参阅 [Shaders](#shaders) 部分中的 **tags** 。
* **shaderOption**: 要设置的使用纹理着色器选项的名称。

#### 示例
```json
    ...
    {
        "type": "UseTexture",
        "args": {
            "textureProperty": "baseColor.textureMap",
            "useTextureProperty": "baseColor.useTexture",
            "dependentProperties": ["baseColor.textureMapUv", "baseColor.textureBlendMode"],
            "shaderOption": "o_baseColor_useTexture"
        }
    },
    ...
```

### Transform2D

读取用户友好的属性（如旋转和缩放），并将其转换为二维变换矩阵供着色器使用。

参数:
* **transformOrder**: 表示旋转、平移和缩放顺序的字符串数组。建议始终使用 `[ "Rotate", "Translate", "Scale" ]`。
* **centerProperty**: `Vector2` 类型属性的名称。定义旋转和缩放的中心。
* **scaleProperty**: `float`属性的名称。控制变换的整体比例。
* **scaleXProperty**: `float`属性的名称。控制 X 比例。
* **scaleYProperty**: `float`属性的名称。控制 Y 比例。
* **translateXProperty**:`float`属性的名称。控制 X 平移。
* **translateYProperty**: `float`属性的名称。控制 Y 平移。
* **rotateDegreesProperty**: `float`属性的名称。控制旋转的度数。
* **float3x3ShaderInput**: 用于最终变换矩阵的 `float3x3` 着色器常量的名称。
* **float3x3InverseShaderInput**: (可选）用于最终变换矩阵逆变换的 `float3x3` 着色器常量的名称。某些着色器可能需要此常量，例如使用视差映射效果的着色器。

#### 示例
```json
    {
        "type": "Transform2D",
        "args": {
            "transformOrder": [ "Rotate", "Translate", "Scale" ],
            "centerProperty": "center",
            "scaleProperty": "scale",
            "scaleXProperty": "tileU",
            "scaleYProperty": "tileV",
            "translateXProperty": "offsetU",
            "translateYProperty": "offsetV",
            "rotateDegreesProperty": "rotateDegrees",
            "float3x3ShaderInput": "m_uvMatrix",
            "float3x3InverseShaderInput": "m_uvMatrixInverse"
        }
    }
```

### ConvertEmissiveUnit

这是一个专门的函数，用于一组标准化的发射照明属性。它允许枚举属性选择光强度单位（尼特或 EV100），并相应地转换光强度属性的值。

参数:
* **intensityProperty**: `float` 属性的名称。指定发射光的强度。
* **lightUnitProperty**: `Enum` 类型属性的名称。表示无量纲值的单位。
* **shaderInput**: 将接收最终强度值的 `float` 着色器常量的名称。假定单位为尼特。
* **ev100Index**: 根据 `lightUnitProperty` 的定义，与 EV100 的枚举值相对应的整数值。
* **nitIndex**: 根据 `lightUnitProperty` 的定义，与 nits 的枚举值相对应的整数值。
* **ev100MinMax**: 由两个浮点数组成的数组，表示在 _EV100_ 模式下运行时的最小和最大强度值。
* **nitMinMax**: 由两个浮点数组成的数组，表示在 _nits_ 模式下运行时的最小和最大强度值。

#### 示例
```json
    {
        "type": "ConvertEmissiveUnit",
        "args": {
            "intensityProperty": "intensity",
            "lightUnitProperty": "unit",
            "shaderInput": "m_emissiveIntensity",
            "ev100Index": 0,
            "nitIndex" : 1,
            "ev100MinMax": [-10, 20],
            "nitMinMax": [0.001, 100000.0]
        }
    },
```

### HandleSubsurfaceScatteringParameters
这是一个专门的函数，用于控制表层下散射和透光特性的标准化属性集。请参阅 [`Skin.materialtype`](https://github.com/o3de/o3de/blob/development/Gems/Atom/Feature/Common/Assets/Materials/Types/Skin.materialtype) ，了解其用法示例。

#### 示例
```json
    {
        "type": "HandleSubsurfaceScatteringParameters",
        "args": {
            "mode": "subsurfaceScattering.transmissionMode",
            "scale": "subsurfaceScattering.transmissionScale",
            "power": "subsurfaceScattering.transmissionPower",
            "distortion": "subsurfaceScattering.transmissionDistortion",
            "attenuation": "subsurfaceScattering.transmissionAttenuation",
            "shrinkFactor": "subsurfaceScattering.shrinkFactor",
            "transmissionNdLBias": "subsurfaceScattering.transmissionNdLBias",
            "distanceAttenuation": "subsurfaceScattering.distanceAttenuation",
            "tintColor": "subsurfaceScattering.transmissionTint",
            "thickness": "subsurfaceScattering.thickness",
            "enabled": "subsurfaceScattering.enableSubsurfaceScattering",
            "scatterDistanceColor": "subsurfaceScattering.scatterColor",
            "scatterDistanceIntensity": "subsurfaceScattering.scatterDistance",
            "scatterDistanceShaderInput": "m_scatterDistance",
            "parametersShaderInput": "m_transmissionParams",
            "tintThickenssShaderInput": "m_transmissionTintThickness"
        }
    },
```

### OverrideDrawList (已弃用)
由使用 Lua 函数的`SetDrawListTagOverride`函数取代。

{{< note >}}
在使用 Lua 函数类型之前，该函数用于覆盖着色器的绘制列表名称。
{{< /note >}}

## **uvNameMap**
这将默认标识符映射到网格 UV 流。加载网格时，运行时会尝试使用这些名称匹配输入几何体的 UV 流。

对于每个条目，键必须与用于 `float2` 顶点输入的语义相匹配。值应该是一个用户友好的名称，与流的预期目的一致，并与其他材质类型中类似流的名称一致。默认情况下，O3DE 中的所有核心材质类型都使用 _Tiled_ 和 _Unwrapped_ 这两个名称。Tiled 表示平铺，因此可以接受重叠的船体。Unwrapped（无包裹）表示用于光贴图和类似用途，因此在 UV 空间中不应该重叠船体。

#### 示例
```json
    "uvNameMap": {
        "UV0": "Tiled",
        "UV1": "Unwrapped"
    }
```


