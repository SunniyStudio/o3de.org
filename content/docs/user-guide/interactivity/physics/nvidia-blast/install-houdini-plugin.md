---
description: ' 安装 Houdini 插件，使用 NVIDIA Blast 在 Open 3D Engine 中创建用于模拟的破坏资产。 '
title: 安装适用于 NVIDIA Blast 的 SideFX Houdini 插件
weight: 100
draft: true
---


{{< note >}}
适用于 O3DE 的 NVIDIA Blast 需要 SideFX Houdini 商业或独立许可证才能创建资产。学徒执照是不够的。有关 Houdini 的更多信息，请参阅 [SideFX 的主页](https://www.sidefx.com/).

随 **NVIDIA Blast** Gem 提供的预编译 Houdini 插件需要 Houdini 18.0。
{{< /note >}}

**内容**
+ [安装 NVIDIA Blast插件](#install-nvidia-blast-plug-ins)
+ [启用 Blast 工具架](#enable-the-blast-tool-shelf)

## 安装 NVIDIA Blast 插件

**安装插件**

1. 从`houdini`目录运行`install_plugin.bat`，位于`Gems\Blast\houdini`。

1. 请求时授予安装程序脚本管理员权限。

   这些插件安装在`C:\Users\user_name\Documents\houdini18.0\`。

1. 要验证安装，您可以在 Houdini 用户目录中检查以下文件。

   `C:\Users\user_name\Documents\houdini18.0\otls` 目录包含以下三个文件：
   + `blastExport.hda`
   + `fractureHierarchy.hda`
   + `fractureSingle.hda`

   `C:\Users\user_name\Documents\houdini18.0\toolbar` 目录包含以下三个文件：
   + `default.shelf`
   + `Fracture.shelf`
   + `shelf_tool_assets.json`

   `C:\Users\user_name\Documents\houdini18.0\dso` 目录包含以下五个文件：
   + `BlastExportPlugin.dll`
   + `BlastExportPlugin.exp`
   + `BlastExportPlugin.ilk`
   + `BlastExportPlugin.lib`
   + `BlastExportPlugin.pdb`

   `C:\Users\user_name\Documents\houdini18.0\dependencies` 目录包含 11 个`NvBlast*.dll` 文件和 4个 `PhysX*.dll` 文件。

1. 验证安装脚本是否已将 `houdini` 用户目录添加到 Windows 中的 **PATH** 环境变量中：

   `C:\Users\user_name\Documents\houdini18.0\`

{{< note >}}
如果 Houdini 在为 Houdini 安装 NVIDIA Blast 工具之前正在运行，请重新启动 Houdini 以使更改生效。
{{< /note >}}

{{< important >}}
一些 Houdini 安装在`C:\Users\user_name\houdini18.0\`而不是 `Documents` 目录中创建 Houdini 用户目录。如果 Houdini 没有加载 NVIDIA Blast 工具，请检查 Houdini 是否在`C:\Users\user_name\houdini18.0\`创建了用户目录。如果该目录存在且包含 Houdini 文件（如 `dso.cache`），请将上述文件移动到 `C:\Users\user_name\houdini18.0\` 中，并将`C:\Users\user_name\houdini18.0\` 添加到 **PATH** 环境变量中。
{{< /important >}}

## 启用 Blast 工具架

Houdini 的 NVIDIA Blast 安装包括一个工具架，您可以启用该工具架以加快为 NVIDIA Blast 创建资产的过程。

**要启用 Blast 工具架**

1. 在 Houdini 中，在**工具栏**右侧选择 **+** 按钮。

1. 从列表中，选择 **Shelves** 公开 **Shelf list**。

1. 从 **Shelf list** 选择 **Fracture tools for Blast** 将工具架添加到 **工具栏**。

![Enable the NVIDIA Blast tool shelf in Houdini.](/images/user-guide/physx/blast/ui-blast-houdini-shelf-enable.png)
