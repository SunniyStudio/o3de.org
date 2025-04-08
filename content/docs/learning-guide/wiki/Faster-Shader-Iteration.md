---
title: "更快的着色器迭代"
description: ""
toc: false
---

## 为未使用的平台禁用着色器编译。
默认情况下，将针对所有目标平台编译着色器，这可能会导致在尝试快速迭代着色器时出现过多的改动。虽然针对其他目标平台进行编译对于测试正确性很重要，但临时针对特定平台可能很有用。为此，需要更改设置注册表。

```
// o3de-root/Registry/AssetProcessorPlatformConfig.setreg
"Platform pc": {
    // "tags": "tools,renderer,dx12,vulkan,null"
    "tags": "tools,renderer,dx12"
}
```

例如，上述更改禁用了 SPIR-V 字节码和 null 平台着色器生成，并将 DXIL 生成留给 DX12 后端。不过，请务必在提交之前撤销更改并重新测试！

## 禁用 Shader Variant Compilation。
如果不考虑性能，则可以禁用 Shader Variant 编译： 
禁用 .azshadervariant 编译（通过注册表设置： `\o3de\Gems\Atom\Asset\Shader\Registry\atom_shaders.setreg`
