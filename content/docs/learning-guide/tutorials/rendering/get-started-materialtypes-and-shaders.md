---
linktitle: 材质类型和着色器
title: "材质类型和着色器入门"
description: "在 Atom 渲染器中创建新的材质类型和着色器。"
toc: true
---

在本教程中，您将使用简单的无光照颜色着色器创建新的材质类型。本教程涵盖以下关键概念：

- 创建材质和着色器所需的文件。
- AZSL 中的基本着色器编程概念，包括着色器资源组。
- 创建着色器资源文件。
- 创建材质类型并附加着色器。
- 使用 Asset Processor 调试着色器。

## 1.设置文件

开发着色器和材质的组合以创建表面外观的过程称为 *外观开发*。对于 Atom 渲染器，此过程需要多个相互依赖的文件，包括以下内容：

- `.azsl`: 此文件包含 AZSL 着色器代码。AZSL 着色器代码包括着色器资源组、着色器输入和输出以及着色器程序。
- `.shader`: 此文件包含描述着色器资产的数据，并以 JSON 格式构建。它引用“`.azsl`”文件并配置 **AZSLc** 着色器编译器。
- `.materialtype`: 此文件包含材质类型数据，并以 JSON 格式构建。材质类型引用一个或多个`.shader`文件来创建外观，定义材质类型的属性，并将材质类型属性链接到引用的着色器中的变量。
- `.material`: 此文件包含材质数据，并以 JSON 格式构建。材质必须从材质类型创建。材质继承材质类型的属性和着色器。这些属性的值在 `.material` 文件中设置，用于描述材质的外观。可以使用 Material Editor 创建材质。

Asset Processor 从这些文件编译运行时资源。将新资源文件保存到磁盘或修改现有资源文件时，Asset Processor 会自动检测更改并处理文件。要使 Asset Processor 检测新文件或更新的文件，这些文件必须位于项目的子目录中，或者位于 Asset Processor 正在监视的目录中。在本教程中，请使用名为 `Materials` 的项目子目录。

在项目的 `Materials` 子目录中，创建以下文件：

- `MyUnlitColor.azsl`
- `MyUnlitColor.shader`
- `MyUnlitColor.materialtype`

{{< note >}}
`.material` 文件是在本教程后面使用 **Material Editor** 创建的。
{{< /note >}}

## 2.作者 AZSL 着色器代码 （.azsl）

您可以在包含以下组件的`MyUnlitColor.azsl`文件中编写 AZSL 着色器代码：

- 预处理器指令
- 着色器资源组 （SRG）
- 顶点和片段着色器输入和输出结构
- 顶点和片段着色器程序

### 预处理器指令

在文件的最顶部，是一组预处理器指令。着色器程序可以使用共享的着色器资源组 （SRG） 和可重用的 AZSL 代码，它们包含在 .srgi 和 .azsli 文件中。

在本教程中，您需要以下 `#include` 预处理器指令：

- `viewsrg.srgi`: 定义共享视图 SRG。
- `DefaultObjectSrg.azsli`: 定义对象 SRG。
- `ForwardPassOutput.azsli`: 定义 ForwardPassOutput 结构。
- `SrgSemantics.azsl`: 定义 SRG 语义。每当您想要定义 SRG 时，SRG 都必须继承自 SRG 语义。

在 `MyUnlitColor.azsl` 文件中添加以下代码：

```cpp
#pragma once

#include <viewsrg.srgi>
#include <Atom/Features/PBR/DefaultObjectSrg.azsli>
#include <Atom/Features/Pipeline/Forward/ForwardPassOutput.azsli>
#include <Atom/Features/SrgSemantics.azsli>
```

接下来，您将了解这些引用的 SRG：它们是什么，它们在整个着色器代码中如何使用，以及如何定义新的 SRG。

### 着色器资源组

在整个着色器程序中，您需要存储、使用和传递常量数据。在 AZSL 中，这是通过着色器资源组 （SRG） 处理的。SRG 包含可在整个系统中共享的着色器资源和 uniform。SRG 绑定到特定的频率（例如每帧视图、每个对象、每个素材等）。它们设计为在多个着色器之间共享，因此单个材质类型只能定义一个材质 SRG。

在本教程中，MyUnlitColor.azsl 使用以下 SRG：

- ViewSrg: 这是由 `viewsrg.srgi` 提供的。ViewSrg 包含与摄像机相关的数据，例如 view、projection、inverse viewProjection 矩阵和剔除视锥体。
- ObjectSrg: 这是由 `DefaultObjectSrg.azsli` 提供的。ObjectSrg 包含特定于正在渲染的对象或几何体的数据，例如对象的位置。
- MaterialSrg: 这是在此 `.azsl` 文件中定义的，并包含特定于材料类型的数据。

### 定义材质 SRG

接下来，您需要定义一个材质 SRG 来存储需要按每个材质频率更新的数据。材质 SRG 必须继承 `SRG_PerMaterial` 语义。`SRG_PerMaterial` 包含特定于该材质的数据。在本教程中，您将创建一个简单的无光照颜色材质，因此材质 SRG 必须包含一个变量来保存颜色值。

如下面的代码所示，定义一个名为“`UnlitColorSrg`”的 SRG，该 SRG 继承自“`SRG_PerMaterial`”。然后，定义保存颜色值的变量 `m_unlitColor` 。

```cpp
ShaderResourceGroup UnlitColorSrg : SRG_PerMaterial
{
    float3 m_unlitColor;
}
```

{{< note >}}
`MyUnlitColor.materialtype` 文件必须在 `m_unlitColor` 和 color 属性之间建立连接。您将在本教程的后面部分执行此操作。
{{< /note >}}

接下来，您将编写使用这些 SRG 的顶点和片段着色器。

### 定义顶点结构体

着色器必须具有输入和输出，以传递常量数据并与渲染管道的其余部分通信。在 AZSL 中编写着色器程序遵循与 HLSL 中类似的做法（请参阅 [Microsoft DirectX HLSL 文档](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl))。

顶点着色器输入和输出在结构体 `VertexInput` 和 `VertexShaderOutput` 中定义。输入和输出的类型由 HLSL 语义指示（请参阅[Microsoft DirectX HLSL - 语义](https://docs.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-semantics) 文档）。

此简单着色器的顶点着色器程序只需要几何体的位置。您可以使用以下代码定义顶点输入和输出结构：

```cpp
struct VertexShaderInput
{
    float3 m_position : POSITION;
};

struct VertexShaderOutput
{
    float4 m_position : SV_Position;
};
```

### 顶点着色器

顶点着色器程序入口点在函数 `MainVS` 中定义。以下顶点着色器具有一个基本功能，该函数通过应用一系列矩阵转换将对象的位置从模型空间转换为剪辑空间：

```cpp
VertexShaderOutput MainVS(VertexInput IN)
{
    VertexShaderOutput OUT;
    float3 worldPosition = mul(ObjectSrg::GetWorldMatrix(), float4(IN.m_position, 1)).xyz;
    OUT.m_position = mul(ViewSrg::m_viewProjectionMatrix, float4(worldPosition, 1.0));        
    return OUT;
}
```

### 片段着色器

片段着色器程序入口点在函数 `MainPS` 中定义。它采用 `VertexShaderOutput` 作为输入。生成的片段着色器输出存储在`ForwardPassOutput.azsli`中定义的`ForwardPassOutput`结构中。

由于这是无光照的单个着色器颜色，因此只需设置漫反射颜色的值并禁用镜面反射照明。

以下示例显示了 fragment shader 程序：

```cpp
ForwardPassOutput MainPS(VertexShaderOutput IN)
{
    ForwardPassOutput OUT;

    // standard ForwardPassOutput G-buffer layout
    OUT.m_diffuseColor = float4(UnlitColorSrg::m_unlitColor, -1.0); // Subsurface scattering is disabled
    OUT.m_specularColor = float4(0.0, 0.0, 0.0, 0.0);   // Disable specular lighting

    return OUT;
}
```

### 完整的 `MyUnlitColor.azsl`

以下是完整的 AZSL 代码：

```cpp
#pragma once

#include <viewsrg.srgi>
#include <Atom/Features/PBR/DefaultObjectSrg.azsli>
#include <Atom/Features/Pipeline/Forward/ForwardPassOutput.azsli>
#include <Atom/Features/SrgSemantics.azsli>

ShaderResourceGroup UnlitColorSrg : SRG_PerMaterial
{
    float3 m_unlitColor;
}

struct VertexShaderInput
{
    float3 m_position : POSITION;
};

struct VertexShaderOutput
{
    float4 m_position : SV_Position;
};

VertexShaderOutput MainVS(VertexShaderInput IN)
{
    VertexShaderOutput OUT;
    float3 worldPosition = mul(ObjectSrg::GetWorldMatrix(), float4(IN.m_position, 1)).xyz;
    OUT.m_position = mul(ViewSrg::m_viewProjectionMatrix, float4(worldPosition, 1.0));        
    return OUT;
}

ForwardPassOutput MainPS(VertexShaderOutput IN)
{
    ForwardPassOutput OUT;

    // standard ForwardPassOutput G-buffer layout
    OUT.m_diffuseColor = float4(UnlitColorSrg::m_unlitColor, -1.0); // Subsurface scattering is disabled
    OUT.m_specularColor = float4(0.0, 0.0, 0.0, 0.0);   // Disable specular lighting

    return OUT;
}
```

## 3.创作着色器资源数据 （.shader）

“`.shader`”文件定义元数据，以便为引用的“`.azsl`”源文件配置着色器编译器，并指定渲染管道应如何使用着色器。`.shader` 文件负责以下操作：

- 引用“`.azsl`”源文件。
- 配置深度、模板和混合状态。
- 指定着色器编译器提示。
- 指定顶点和片元着色器程序的入口点。
- 指定应在渲染队列中绘制此着色器的位置。

### `MyUnlitColor.shader`的组件

“`.shader`”文件中的配置包含以下属性：

Source
: 此着色器引用“`.azsl`”源文件“`MyUnlitColor.azsl`”。

DepthStencilState
: `CompareFunc` 属性将比较运算符设置为 `GreaterEqual`，以便丢弃深度值低于深度缓冲区中当前值的像素。这是必需的，因为 Atom 实现了反向深度缓冲区，并使用了在照明通道之前运行的单独深度预通道。

Entry Points
: 默认情况下，资产生成器会将以“`VS`”或“`PS`”开头或结尾的任何函数（对应于顶点和片段着色器）识别为着色器入口点。要显式标识着色器的入口点，请指定函数的名称和着色器程序的类型。

- 顶点着色器的入口点位于 MainVS 函数处。
- 片段着色器的入口点位于 MainPS 函数处。

DrawList
: 指定绘制列表的名称，使用此着色器的绘制项目应在其中排队等待渲染。通常，名称应与 `.pass` 中的抽奖列表名称匹配。

### 完整的 `MyUnlitColor.shader`

以下是完整的着色器描述：

```json
{
    "Source": "MyUnlitColor.azsl",

    "DepthStencilState": {
        "Depth": {
            "Enable": true,
            "CompareFunc": "GreaterEqual"
        }
    },

    "ProgramSettings": {
        "EntryPoints": [
            {
                "name": "MainVS",
                "type": "Vertex"
            },
            {
                "name": "MainPS",
                "type": "Fragment"
            }
        ]
    },

    "DrawList": "forward"
}
```

## 4.编写材质类型数据 （.materialtype）

`MyUnlitColor.materialtype` 文件包含 JSON 格式的材质类型数据。此材质类型用于在 **材质编辑器** 中创建材质。材质类型负责以下因素：

- 链接到着色器。
- 定义属性组
- 定义属性组的材料属性。

### 链接到着色器

链接到 `.materialtype` 中的着色器定义了材质类型用于计算表面外观的着色器。它还将着色器的变量公开给材质类型，以便它们可以链接到材质属性。在本教程中，“`color`”属性连接到变量“`m_unlitColor`”，该变量在 `MyUnlitColor.azsl` 中定义，并由 `MyUnlitColor.shader` 文件引用。

在本教程中，`MyUnlitColor` 材质类型链接到一些着色器：

- `MyUnlitColor.shader`：这是您在本教程前面创建的着色器。`color` 属性连接到变量 `m_unlitColor`，该变量在 `MyUnlitColor.azsl` 中定义，并由 `MyUnlitColor.shader` 文件引用。
- `DepthPass.shader`：需要此着色器来写入深度缓冲区。
- `Shadowmap.shader`：此着色器使对象能够投射阴影。

{{< note >}}
`DepthPass.shader` 和 `Shadowmap.shader` 是 Atom 提供的常用着色器。
{{< /note >}}

这将生成以下示例：

```json
"shaders": [
        {
            "file": "./MyUnlitColor.shader"
        },
        {
            "file": "Shaders/Depth/DepthPass.shader"
        },
        {
            "file": "Shaders/Shadow/Shadowmap.shader"
        }
]	
```

### 定义材质属性

在 `.materialtype` 文件中，材料属性在 `propertyLayout` 容器内的属性组中定义。组容器将属性排列成可折叠的组，显示在 Material Editor 的 **Inspector** 中。每个属性都有多个属性，包括 id、name、type、default value 以及指向 `.materialtype` 文件引用的着色器之一中的变量的链接。

在本教程中，有一个材质属性：color。要在 propertyLayout 容器中定义 color 属性，请执行以下操作：

1. 在`groups`容器中定义 `settings` 属性组。
2. 在`properties`容器中定义`settings`属性组，以保存属于设置`property`组的属性的详细信息。
3. 在 `properties` 的 `settings` 属性组中定义 `color` 属性。

这将生成以下示例：

```json

    "propertyLayout": {
        "version": 3,
        "groups": [
            {
                "id": "settings",
                "displayName": "Settings"
            }
        ],
        "properties": {
            "settings": [
                {
                    "id": "color",						// Unique, C-style identifier 		
                    "displayName": "Color",				// Name (appears in the Material Editor)
                    "type": "Color",					// Data type
                    "defaultValue": [ 0.5, 0.0, 0.5 ],	// Default value (depends on data type)
                    "connection": {						// Establish a connection between the property and a variable in the shader.
                        "type": "ShaderInput",			// - Any changes made to this 'color' property serialize to the 'm_unlitColor' 
                        "id": "m_unlitColor"            // - variable in the UnlitSingleColor shader.
                    }
                }
            ]
        }
    }

```

概括地说，上面的示例完成了几件事：

- 定义材料属性。
- 创建与着色器源代码 （`.azsl`） 使用的变量的连接。
- 设置元数据，用于配置材质属性在 Material Editor 的 Inspector 中的显示方式。

### 完整的 `MyUnlitColor.materialtype`

以下是完整的 materialtype 定义：

```json
{
    "description": "Renders a model unlit with a single suface color value.",
    "version": 3,
    "shaders": [
        {
            "file": "./MyUnlitColor.shader"
        },
        {
            "file": "Shaders/Depth/DepthPass.shader"
        },
        {
            "file": "Shaders/Shadow/Shadowmap.shader"
        }
    ],
    "propertyLayout": {
        "groups": [
            {
                "id": "settings",
                "displayName": "Settings"
            }
        ],
        "properties": {
            "settings": [
                {
                    "id": "color",
                    "displayName": "Color",
                    "type": "Color",
                    "defaultValue": [ 0.5, 0.0, 0.5 ],
                    "connection": {
                        "type": "ShaderInput",
                        "id": "m_unlitColor"
                    }
                }
            ]
        }
    }
}
```

## 5.使用 Asset Processor 进行编译和调试

使用完整的 `MyUnlitColor.materialtype`、`MyUnlitColor.shader` 和 `MyUnlitColor.azsl` 文件，您可以确保它们成功编译。

Asset Processor 会监控项目的子目录，并在检测到新文件或更新文件时自动编译和调试资源。由于这些文件位于项目的 Materials 目录中，因此 Asset Processor 可以检测它们、编译着色器并构建 materialtype。Asset Processor 在 Material Editor 打开时自动启动，并在系统托盘的后台运行。您还可以从构建配置的目录启动 Asset Processor，例如“`<build>/bin/profile/`”或“`<install>/bin/<platform>/profile/Default`”。 

如果 Asset Processor 遇到错误，它会在系统托盘上方显示一条消息。要查看最近处理的任务的结果，请在系统托盘中选择 Asset Processor 的图标以打开它。您可以通过按 **已完成** 降序对 **资产状态** 列表进行排序来查看最近作业的结果。

如果作业显示错误或警告，请在 Asset Status 列表中选择该作业，然后在下面的 **Event Log Details** 列表中查看日志。事件日志包含可用于解决处理特定资产时发出的任何错误或警告的详细信息。

## 6.使用 Material Editor 创建材质

如果所有文件都成功构建，您可以基于 MyUnlitColor 材质类型创建材质。

要使用 Material Editor 创建新材质，请执行以下操作：

1. 从构建配置的目录中启动 Material Editor，例如`<build>/bin/profile/` or `<install>/bin/<platform>/profile/Default`.  
2. 从材质编辑器的 **File** 菜单中，选择 **New**，或按 **Ctrl+N** 创建新材质。
3. 在**Create New Material**对话框中，在**Material Type**列表中选择**MyUnlitColor**。
4. 选择 **Select Material Filename**下方的文件夹图标。确保文件浏览器已选中项目的 `Materials` 目录，并将材质命名为 `MyUnlitColor.material`。选择 **确定** 关闭文件浏览器和 **Create New Material** 对话框。
5. 新材质将在 资源浏览器 中自动选择，并且其设置组和颜色属性可在材质编辑器的 Inspector 中使用。
6. 通过在 **Color** 属性字段中指定逗号分隔的 RGB 值列表来更改材质颜色，或者选择色板以打开颜色选取器。
7. 通过从 **File** 菜单中选择 **Save** 或按 **Ctrl+S** 来保存对材质的更改。

![Creating a material from MyUnlitColor material type](/images/learning-guide/tutorials/rendering/unlit-color-material.jpg)

就是这样！您创建了一个 AZSL 着色器、一个新的材质类型和一个材质。

## 深入探索

查看以下页面，了解有关 Atom 中的材质和着色器的更多信息。

- [着色器系统和参考](/docs/atom-guide/dev-guide/shaders/): 了解 Shader System，包括 AZSL 语言。
- [材质系统](/docs/atom-guide/dev-guide/materials/): 了解 Atom 中材质系统的基础技术细节。
