---
title: "CPU-&-GPU-Debugging-Tools"
description: ""
toc: false
---

指向包含各种工具演示的视频的链接 - https://youtu.be/WIUhaVXFDkc

_NOTE：此页面的内容与 '`development`' 分支有关，与 '`main`' 分支或任何特定版本无关。
## 着色器调试符号

对于要调试的着色器，可以将以下编译器选项添加到“`.shader`”文件中。

```json
{
    "AddBuildArguments" :
    {
        "debug": true
    }
}
```

'`debug`' 将在发出着色器字节码的同一目录中发出 PDB 文件。

或者，您可以通过在“`.setreg`”文件中添加以下代码段来全局启用所有着色器的 PDB 生成（将其放在您的用户“`.o3de/Registry`”文件夹中）

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

保存着色器文件后，“`AssetProcessor`”将检测更改，重新编译着色器，并根据需要发出 pdb。请注意，在图形调试工具中，您需要将项目的着色器路径添加到 PDB 搜索路径。例如，对于材质类型着色器，可以在 `your_project_root/Cache/pc/materials/types` 中找到。

## RenderDoc （GPU 调试）

**注意：** 目前在 Windows 和 Linux 中可用

1. 安装 RenderDoc
2. 设置 CMake 标志`LY_RENDERDOC_ENABLED`（将 `LY_RENDERDOC_ENABLED=ON` 传递给你的 cmake configure 命令）。如果 RenderDoc 未安装在默认路径中或未找到，请使用 CMake 变量 `LY_RENDERDOC_PATH` 传递其安装路径（也在 cmake configure 命令期间传递）。
3. 使用`--enableRenderDoc`运行可执行文件
4. 将 RenderDoc 附加到正在运行的实例（注意：*不要*在 RenderDoc 中启动应用程序，因为我们在创建设备之前自己手动加载 RenderDoc DLL）。

请注意，在 DX12 上，还必须为 GPU 事件标记启用 Pix（请参阅下面的说明）。

## NVIDIA NSight （GPU 调试）

1. 安装 NVIDIA NSight
2. 启动 NSight，单击 Connection，启动Editor.exe并填写参数以启动引擎编辑器。
4. 启动编辑器后，使用 Capture for Live Analysis 捕获帧并进行调试。

可选：您可以将 NSight aftermath sdk 路径设置为 CMake 变量LY_AFTERMATH_PATH以查看 GPU 崩溃后的最小堆栈跟踪。但是 Nsight 应该可以在没有它的情况下工作。
