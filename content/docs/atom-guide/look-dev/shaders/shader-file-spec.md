---
title: "Shader 文件规范"
description: "Atom 渲染器中着色器文件 (`*.shader`) 的文件规范。"
toc: false
weight: 1000
---

着色器文件（`*.shader`）以 JSON 格式编写，包含以下元素。

- **Source**: AZSL 源文件 (`*.azsl`) 的文件路径。该路径可以是该着色器文件的相对路径，也可以是资产根目录的相对路径。
  
- **RasterState**: 光栅器的渲染状态配置。

- **DepthStencilState**: 输出合并器的深度模版状态。

- **CompilerHints**: 着色器编译器链的设置容器。每个平台对这些设置的支持程度可能不同。

  有关设置的完整列表，请参见 `Gems\Atom\RHI\Code\Source\RHI.Edit\ShaderCompilerArguments.cpp` 中的 `ShaderCompilerArguments::Reflect` 。
  {{< note >}}
  AZSLc 只是编译链中生成 HLSL 代码的第一个编译器。还有其他各种编译器，如 dxc 和 spirv-cross，可执行额外的翻译和编译。
  {{< /note >}}
  

- **ProgramSettings**: 着色器程序设置的容器。
  - **EntryPoints**: 要构建的着色器入口点列表。如果省略 EntryPoints 设置，构建器将查找以 VS、PS 或 CS 开头或结尾的有效函数，分别对应顶点着色器、片段着色器和计算着色器。
    - **Name**: 着色器入口点函数的名称，该函数在 AZSL 源文件 (`.azsl`) 中定义。
    - **Type**: 着色器函数的类型。支持的类型有 “Vertex”, “Fragment”, 和 “Compute”。
  
- **DrawList**: 绘制列表的名称，使用此着色器的 DrawItems 应在此列表中排队等待渲染。此名称必须与要渲染的一个或多个通道（`*.pass` 文件）中的绘制列表名称相匹配。

{{< note >}}
这是着色器资产文件中可用元素的高级细分。了解完整规范的最可靠方法是检查 `Gems\Atom\Code\Source\RPI.Edit\Shader\ShaderSourceData.cpp` 中的 `ShaderSourceData::Reflect()`。

有关上述各种呈现状态，如深度模版状态（DepthStencilState）和光栅状态（RasterState），请参见 `Gems\Atom\RHI\Code\Source\RHI.Reflect\RenderStates.cpp`。
{{< /note >}}


下面是着色器文件的示例，`MinimalPBR_ForwardPass.shader`。
```json
{
    "Source" : "./MinimalPBR_ForwardPass.azsl",

    "DepthStencilState" :
    {
        "Depth" :
        {
            "Enable" : true,
            "CompareFunc" : "GreaterEqual"
        },
        "Stencil" :
        {
            "Enable" : true,
            "ReadMask" : "0x00",
            "WriteMask" : "0xFF",
            "FrontFace" :
            {
                "Func" : "Always",
                "DepthFailOp" : "Keep",
                "FailOp" : "Keep",
                "PassOp" : "Replace"
            }
        }
    },

    "CompilerHints" : { 
        "DxcDisableOptimizations" : false
    },

    "ProgramSettings":
    {
      "EntryPoints":
      [
        {
          "name": "MinimalPBR_MainPassVS",
          "type": "Vertex"
        },
        {
          "name": "MinimalPBR_MainPassPS",
          "type": "Fragment"
        }
      ]
    },

    "DrawList" : "forward"
}

```
