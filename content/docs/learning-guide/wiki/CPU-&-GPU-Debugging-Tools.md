---
title: "CPU-&-GPU-Debugging-Tools"
description: ""
toc: false
---

Link to a video that contains demo for various tools - https://youtu.be/WIUhaVXFDkc

_NOTE: The contents of this page pertain to the `development` branch, not the `main` branch or any particular release.
## Shader Debug Symbols

For shaders you wish to debug, you may add the following compiler options to the `.shader` file.

```json
{
    "AddBuildArguments" :
    {
        "debug": true
    }
}
```

`debug` will emit PDB files in the same directory the shader bytecode is emitted. 

Alternatively, you can enable PDB generation for all shaders globally by adding the following snippet in a `.setreg` file (place this in your user `.o3de/Registry` folder)

```jsonc
// Atom.setreg
{
    "O3DE": { 
        "Atom": {
            "RHI": {
                "GraphicsDevMode": true
            }
        }
    }
}
```

After saving the shader file, the `AssetProcessor` will detect the changes, recompile the shader, and emit pdbs as needed. Note that in your graphics debugging tool, you'll need to add your project's shader paths to the PDB search paths. For the material type shaders for example, this would be found in `your_project_root/Cache/pc/materials/types`.

## RenderDoc (GPU debugging)

**Note:** Currently available in Windows and Linux

1. Install RenderDoc
2. Set the CMake flag `LY_RENDERDOC_ENABLED` (pass `LY_RENDERDOC_ENABLED=ON` to your cmake configure command). If RenderDoc was not installed in the default path or is not found, use the CMake variable `LY_RENDERDOC_PATH` to pass the path where it was installed (also passed during cmake configure command).
3. Run the executable with `--enableRenderDoc`
4. Attach RenderDoc to the running instance (note: do *not* launch the app within RenderDoc as we load the RenderDoc DLL ourselves manually just prior to device creation).

Note that on DX12, Pix must also be enabled for GPU event markers (see instructions below).

## NVIDIA NSight (GPU debugging)

1. Install NVIDIA NSight
2. Launch NSight, click on Connection, launch the Editor.exe with arguments filled in to launch the engine editor.
4. After the editor launched, use Capture for Live Analysis to capture a frame and debug.

Optional: you may set your NSight aftermath sdk path as the CMake variable LY_AFTERMATH_PATH to view a minimal stacktrace after a GPU crash. But Nsight should work without it.
