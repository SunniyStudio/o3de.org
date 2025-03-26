---
title: "[Atom]-Mesh-Instancing_-For-Shader-Authors"
description: ""
toc: false
---

This document covers everything you need to know to author custom shaders that support instancing.

See also:
- [Mesh Instancing: For Content Creators](https://github.com/o3de/o3de/wiki/%5BAtom%5D-Mesh-Instancing:-For-Content-Creators)
- [Mesh Instancing: For Engine Maintainers/Contributors](https://github.com/o3de/o3de/wiki/%5BAtom%5D-Mesh-Instancing:-For-Engine-Maintainers-Contributors)

# Supporting Instancing

BasePBR_VertexData.azsli, BasePBR_VertexEval.azsli, and BasePBR_PixelGeometryEval.azsli are a good example of how to support instancing that should be referred to, but the steps are also broken out below.

## Include InstancedTransforms.azsli
To signal that a shader supports mesh instancing, all you must do is add
#include <Atom/Features/InstancedTransforms.azsli>
If you include this file, you should use the GetObjectToWorld* functions in InstancedTransforms.azsli to access the object-to-world transforms instead of ObjectSrg::GetWorldMatrix*. Otherwise, the MeshFeatureProcessor is going to combine all objects that can be instanced into a single draw call with a single ObjectSrg, and they will all share the same transforms if you continue going through the ObjectSrg.

## Define VsSystemValues and VsOutput
The EvaluateVertexGeometry macro now takes a second argument, SV, which is a struct for SystemValues. If you use an existing macro like EvaluateVertexGeometry_BasePBR, then it already supports instancing and you can move on. For a custom EvaluateVertexGeometry function, you can define a custom VsSystemValues struct the same way you can define a custom VsInput struct:
```
#ifndef VsSystemValues
#define VsSystemValues  VsSystemValues_BasePBR
#endif

struct VsSystemValues_BasePBR
{
    uint m_instanceId;
};
```

If you are using the helper ForwardPassVertexAndPixel.azsli file to author your vertex shader, it will already expect m_instanceId to be a member of that struct. If you author your own vertex shader, you'll need to use SV_InstanceID to get the instanceId, set that value on your SystemValues struct, and then pass that to your EvaluateVertexGeometry function.
```
VsOutput VertexShader(VsInput IN, uint instanceId : SV_InstanceID)
{
    VsSystemValues SV;
    SV.m_instanceId = instanceId;
    VsOutput OUT = EvaluateVertexGeometry(IN, SV);
    return OUT;
}
```

If your pixel shader uses object-to-world transforms, then it must also get the instanced transforms via GetObjectToWorld*. Pixel Shaders cannot use SV_InstanceID if their corresponding vertex shader already uses it, so you need to pass the instanceId to the pixel shader yourself via the VsOutput struct. So
```
uint m_instanceId : SV_InstanceID;
```
should be added to your VsOutput struct. And somewhere in your vertex shader (typically inside EvaluateVertexGeometry) you should set 
```
OUT.m_instanceId = SV.m_instanceId;
```

## Custom ObjectSrg
The GetObjectToWorld* functions in InstancedTransforms.azsli use ObjectSrg::GetWorld* as a fallback for when instancing is supported, but not enabled. If you have a custom ObjectSrg, it must implement all three of those functions or you will get an error in the AssetProcessor when processing the shader. SkinObjectSrg.azsli is an example of this.

## Ignoring Instancing in Shaders

If you for some reason don't want to support instancing, don't include InstancedTransforms.azsli and you can continue using the ObjectSrg as normal. However, there are some caveats if you want to share any code with the o3de PBR shaders, so in general it's most straightforward to just support instancing.

### Using built-in Depth and Shadowmap shaders

It is common to create a custom material that still uses the default shadowmap and depth shaders that Atom's PBR shaders use (see Gems/AtomContent/TestData/Assets/TestData/Materials/Types/AutoBrick.materialtype as an example). These shaders support instancing and include the InstancedTransforms.azsli file, which includes a root constant that stores the offset to the instance data for an instanced draw call. O3DE has a requirement that all DrawItems in a DrawPacket (e.g., forward pass, depth, shadow, motion vector) share a common root constant layout. So you cannot author your own forward pass shader that does not support instancing and use it in combination with the default Shadowmap and Depth shaders that do. You'll need to either author your own depth shader that doesn't support instancing, or more reasonably update your forward pass shader to support instancing.

### EvaluateVertexGeometry Macro

If you are using the helper ForwardPassVertexAndPixel.azsil file, which defines a vertex shader for you and passes the vertex data to an EvaluateVertexGeometry macro, and you are overriding EvaluateVertexGeometry with your own implementation, you can ignore the second argument that is now passed into EvaluateVertexGeometry:
#define EvaluateVertexGeometry(IN, SV) MyEvaluateVertexGeometryFunction(IN)
