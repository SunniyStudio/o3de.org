---
title: "[DCCsi]-Blender-Scene-导出器"
description: ""
toc: false
---

Blender O3DE 场景导出器是一个方便的工具，用于从 Blender 到 O3DE 的一键导出器。

此 DCCsi 工具对您的场景没有破坏性，因为它将导出网格和纹理的选定副本，并在文件导出期间将纹理链接重新路径化为网格。您可以在 .FBX 格式和 O3DE 执行组件实体。
您可以选择使用网格或子文件夹 'textures' 中的网格导出纹理。这将适用于带有或不带有打包纹理的 Blender 场景。

硬件和软件要求：

支持 Blender 3.+ Windows 10/11 64 位版本 /
O3DE 稳定版 21.11 版本 Windows 10/11 64 位版本


安装：

SceneExporter 文件夹被压缩到 Blender 插件中。

如何安装 Blender O3DE Scene Exporter Blender 插件：
    找到 in Add-Ons SceneExporter.zip，它应该位于：
    AtomLyIntegration\TechnicalArt\DccScriptingInterface\Tools\DCC\Blender\AddOns\SceneExporter.zip
    转到 Blender 首选项中的 附加组件 部分。
    点击 Install 安装 按钮。
    使用 File Browser 选择附加组件。
    从 'Add-ons' 部分级别启用附加组件。
    您现在可以在 Import-Export 中查看该插件或搜索 O3DE*。
    确保其 Checked to Active（选中激活）。
    这会将新的 O3DE 选项卡和新的 File Export to O3DE 选项添加到菜单中。

待办事项：添加 Linux 和 MacOS 跨平台支持：
- Linux: https://github.com/o3de/o3de/issues/7244
- MacOS: https://github.com/o3de/o3de/issues/7245

未来可能的功能：
- 将光源和/或摄像机转换/导出为 O3de 预制件
- 将 PrincipledBRDF 材质转换/导出为 Atom 源 .materials
