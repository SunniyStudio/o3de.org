---
linkTitle: NVIDIA Blast
description: ' 使用 NVIDIA Blast Gem 模拟 Open 3D Engine 项目中的破坏。 '
title: NVIDIA Blast Gem
draft: true
---

NVIDIA Blast Gem 使用 NVIDIA Blast 库在 Open 3D Engine 中提供快速、高保真的破坏模拟。

{{< note >}}
适用于 O3DE 的 NVIDIA Blast 需要 SideFX Houdini 商业或独立许可证才能创建资产。学徒执照是不够的。有关 Houdini 的更多信息，请参阅 [SideFX 的主页](https://www.sidefx.com/).
NVIDIA Blast Gem 附带的预编译 Houdini 增效工具需要 Houdini 18.0。
{{< /note >}}

有关 NVIDIA Blast 开发人员的信息，请参阅 [使用 NVIDIA Blast 进行模拟破坏](/docs/user-guide/interactivity/physics/nvidia-blast/)。

## NVIDIA Blast Gem 提供的功能

NVIDIA Blast Gem 提供以下内容：

* **Blast Family Mesh Data**组件，该组件将 NVIDIA Blast 网格添加到实体中。

* **Blast Family**组件，该组件为实体启用 NVIDIA Blast 模拟。

* **Blast Configuration** 编辑器位于 O3DE 编辑器的 **Tools** 菜单中。

* **Blast Materials**为 **Asset Editor**中可用的 NVIDIA Blast 资产设置物理属性。

* **Blast Script Canvas** 用于编写破坏模拟脚本的节点。

* 用于 SideFX Houdini 的插件和 Houdini 数字资产，用于破碎几何体并导出 NVIDIA Blast 资产。

* Python 资产生成器，用于处理 NVIDIA Blast 资产并生成爆炸切片。

* 一个公共 C++ API，允许其他系统和 Gem 访问 NVIDIA Blast 模拟功能。

## 启用NVIDIA Blast Gem

1. 使用 **Project Manager** 将 NVIDIA Blast Gem 添加到您的项目中。NVIDIA Blast Gem 需要以下 Gem 作为依赖项：

   * **LmbrCentral**

   * **PhysX**

   {{< important >}}
   虽然不是必需的，但我们强烈建议您使用 NVIDIA Blast Gem 启用 [Python 资产生成器 Gem](/docs/user-guide/gems/reference/script/python/python-asset-builder)。NVIDIA Blast Gem 包括一个 Python 资产构建器脚本，该脚本可自动处理 NVIDIA Blast 的网格资产并创建 Blast 切片资产。
   {{< /important >}}

1. 配置构建：

   ```cmd
   cmake -B <CMake build dir> -S . -G "Visual Studio 16" 
   ```

   {{< note >}}
使用`Visual Studio 16`作为 Visual Studio 2019 的生成器，使用`Visual Studio 17`作为 Visual Studio 2022 的生成器。有关每个受支持平台的常用生成器的完整列表，请参阅 [配置项目](/docs/user-guide/build/configure-and-build/#configuring-projects)。
   {{< /note >}}

1. 构建项目：

   ```cmd
   cmake --build <CMake build dir> --target <Project name> --config profile -- -m
   ```
