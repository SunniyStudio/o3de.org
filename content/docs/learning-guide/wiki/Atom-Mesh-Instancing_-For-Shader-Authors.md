---
title: "[Atom] 着色器作者的网格实例化"
description: ""
toc: false
---

本文档涵盖了编写支持实例化的自定义着色器所需了解的所有内容。

另请参阅：
- [Mesh Instancing: For Content Creators](https://github.com/o3de/o3de/wiki/%5BAtom%5D-Mesh-Instancing:-For-Content-Creators)
- [Mesh Instancing: For Engine Maintainers/Contributors](https://github.com/o3de/o3de/wiki/%5BAtom%5D-Mesh-Instancing:-For-Engine-Maintainers-Contributors)

# 支持实例化

BasePBR_VertexData.azsli、BasePBR_VertexEval.azsli 和 BasePBR_PixelGeometryEval.azsli 是有关如何支持实例化的一个很好的示例，应参考这些实例化，但下面也对步骤进行了细分。

## 包括 InstancedTransforms.azsli
要表明着色器支持网格实例化，您只需添加
#include < Atom/Features/InstancedTransforms.azsli>
如果包含此文件，则应使用 InstancedTransforms.azsli 中的 GetObjectToWorld* 函数来访问对象到世界的转换，而不是 ObjectSrg：：GetWorldMatrix*。否则，MeshFeatureProcessor 将把所有可以实例化的对象合并到一个具有单个 ObjectSrg 的单个绘制调用中，如果您继续执行 ObjectSrg，它们将共享相同的变换。

## 定义 VsSystemValues 和 VsOutput
EvaluateVertexGeometry 宏现在采用第二个参数 SV，它是 SystemValues 的结构。如果您使用像 EvaluateVertexGeometry_BasePBR 这样的现有宏，那么它已经支持实例化，您可以继续。对于自定义 EvaluateVertexGeometry 函数，您可以定义自定义 VsSystemValues 结构，其方式与定义自定义 VsInput 结构相同：
```
#ifndef VsSystemValues
#define VsSystemValues  VsSystemValues_BasePBR
#endif

struct VsSystemValues_BasePBR
{
    uint m_instanceId;
};
```

如果使用帮助程序 ForwardPassVertexAndPixel.azsli 文件来创作顶点着色器，则它已经期望 m_instanceId 成为该结构的成员。如果您创作自己的顶点着色器，则需要使用 SV_InstanceID 来获取 instanceId，在 SystemValues 结构上设置该值，然后将其传递给 EvaluateVertexGeometry 函数。
```
VsOutput VertexShader(VsInput IN, uint instanceId : SV_InstanceID)
{
    VsSystemValues SV;
    SV.m_instanceId = instanceId;
    VsOutput OUT = EvaluateVertexGeometry(IN, SV);
    return OUT;
}
```

如果您的像素着色器使用对象到世界转换，则它还必须通过 GetObjectToWorld* 获取实例化转换。如果像素着色器的相应顶点着色器已经使用SV_InstanceID，则无法使用它，因此您需要通过 VsOutput 结构自行将 instanceId 传递给像素着色器。所以
```
uint m_instanceId : SV_InstanceID;
```
应添加到您的 VsOutput 结构中。在顶点着色器的某个位置（通常在 EvaluateVertexGeometry 中），您应该将
```
OUT.m_instanceId = SV.m_instanceId;
```

## 自定义 ObjectSrg
InstancedTransforms.azsli 中的 GetObjectToWorld* 函数使用 ObjectSrg::GetWorld* 作为支持实例化但未启用时的回退。如果您有自定义 ObjectSrg，则它必须实现所有这三个函数，否则在处理着色器时，您将在 AssetProcessor 中收到错误。SkinObjectSrg.azsli 就是一个例子。

## 忽略着色器中的实例化

如果出于某种原因不想支持实例化，请不要包含 InstancedTransforms.azsli，您可以继续照常使用 ObjectSrg。但是，如果您想与 o3de PBR 着色器共享任何代码，则需要注意一些事项，因此通常仅支持实例化是最直接的。

### 使用内置的 Depth 和 Shadowmap 着色器

通常，创建自定义材质时，仍使用 Atom 的 PBR 着色器使用的默认阴影贴图和深度着色器（请参阅 Gems/AtomContent/TestData/Assets/TestData/Materials/Types/AutoBrick.materialtype 作为示例）。这些着色器支持实例化，并包括 InstancedTransforms.azsli 文件，该文件包括一个根常量，用于存储实例化绘制调用的实例数据的偏移量。O3DE 要求 DrawPacket 中的所有 DrawItem（例如，前向通道、深度、阴影、运动矢量）共享一个通用的根常量布局。因此，您不能编写自己的不支持实例化的前向通道着色器，并将其与支持实例化的默认 Shadowmap 和 Depth 着色器结合使用。您需要编写自己的不支持实例化的深度着色器，或者更合理地更新前向通道着色器以支持实例化。

### EvaluateVertexGeometry 宏

如果使用的是帮助程序 ForwardPassVertexAndPixel.azsil 文件，该文件为你定义顶点着色器并将顶点数据传递给 EvaluateVertexGeometry 宏，并且正在使用自己的实现重写 EvaluateVertexGeometry，则可以忽略现在传递到 EvaluateVertexGeometry 中的第二个参数：
#define EvaluateVertexGeometry(IN, SV) MyEvaluateVertexGeometryFunction(IN)
