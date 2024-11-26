---
linkTitle: NVIDIA Cloth
title: NVIDIA Cloth Gem
description: NVIDIA Cloth Gem 提供了使用 NVIDIA Cloth 库创建快速、逼真的布料模拟的功能。
toc: true
---

物理布料模拟可以创建更加身临其境的环境和角色。NVIDIA Cloth Gem 使用 NVIDIA Cloth 库在 Open 3D Engine 中提供快速、强大的布料模拟。

有关使用 NVIDIA 布料的信息，请参阅 [使用 NVIDIA 布料模拟布料](/docs/user-guide/interactivity/physics/nvidia-cloth/).

## NVIDIA Cloth Gem 提供的功能

NVIDIA Cloth Gem 提供以下功能：

* **FBX Settings** 中用于网格和 Actor 导入的 **Cloth** 修饰符。

* **Cloth** 组件，用于包含 **Mesh** 或 **Actor** 组件的实体。

* 可在 **Animation Editor** 中添加到角色的布料碰撞体。

* 网格和Actor示例资源和Prefab位于： `\Gems\NvCloth\Assets\`.

* 一个公共 C++ API，允许其他系统和 Gem 访问布料模拟功能。

## 启用 NVIDIA Cloth Gem

要启用 NVIDIA Cloth Gem，请执行以下操作：

1. 使用 Project Manager 将 NVIDIA Cloth Gem 添加到您的项目中。NVIDIA Cloth Gem 需要以下 Gem 作为依赖项：

   * **LmbrCentral**
   
   * **Emotion FX Animation**

1. 配置内部版本：

   ```cmd
   cmake -B <CMake build dir> -S . -G "Visual Studio 16" 
   ```

   {{< note >}}
   使用`Visual Studio 16`作为 Visual Studio 2019 的生成器，使用`Visual Studio 17`作为 Visual Studio 2022 的生成器。有关每个受支持平台的常用生成器的完整列表，请参阅 [配置项目](/docs/user-guide/build/configure-and-build/#configuring-projects)。
   {{< /note >}}

1. 构建您的项目：

   ```cmd
   cmake --build <CMake build dir> --target <Project name> --config profile -- -m
   ```
