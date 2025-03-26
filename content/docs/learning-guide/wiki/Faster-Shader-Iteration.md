---
title: "Faster-Shader-Iteration"
description: ""
toc: false
---

## Disabling Shader Compilation For Unused Platforms.
By default, shaders are compiled for all target platforms which can result in excessive churn when attempting to quickly iterate on a shader. While compiling for other target platforms is important to test correctness, it may be useful to temporarily target a specific platform. To do this, a settings registry change is needed.

```
// o3de-root/Registry/AssetProcessorPlatformConfig.setreg
"Platform pc": {
    // "tags": "tools,renderer,dx12,vulkan,null"
    "tags": "tools,renderer,dx12"
}
```

The change above for example disables SPIR-V bytecode and null-platform shader generation and leaves DXIL generation for the DX12 backend. Be sure to revert the change and re-test before submission though!

## Disabling Shader Variant Compilation.
If performance is not a concern, then it is possible to disable Shader Variant compilation:  
Disable .azshadervariant compilation (via registry setting in: `\o3de\Gems\Atom\Asset\Shader\Registry\atom_shaders.setreg`
