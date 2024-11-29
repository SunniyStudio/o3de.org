---
linkTitle: 部署仿真
title: 部署仿真
description: 使用 O3DE 部署无头测试工具。
toc: true
weight: 520
---

## 概述

机器人模拟通常作为 CI/CD 管道的一部分进行部署。在这种情况下，不建议使用 Editor 运行模拟。

## 部署发布版游戏启动器

详细说明请参考 [打包与发布](docs/user-guide/packaging/) 部分。部署机器人模拟类似于部署游戏，但结果是具有 ROS 2 通信通道的可执行文件。

## 分步项目模板发布示例

这是一个示例，说明如何构建无需安装 O3DE 即可运行的独立包。
我们将使用 [项目模板](/docs/user-guide/interactivity/robotics/overview/#templates)之一创建一个新项目，该项目将构建为单体 GameLauncher。
这是一个最小用例的分步示例。
您可以使用不同的项目名称并修改流程（例如，通过捆绑资产）。
请注意，需要满足以下几个先决条件：

- 从源代码安装O3DE
- [ROS2](https://github.com/o3de/o3de-extras/tree/development/Gems/ROS2) Gem 已注册
- [WarehouseSample](https://github.com/o3de/o3de-extras/tree/development/Gems/WarehouseSample) Gem 已注册
- [RosRobotSample](https://github.com/o3de/o3de-extras/tree/development/Gems/RosRobotSample) Gem 已注册

如果您打算构建其他项目模板，请确保启用其他 Gem。

### 创建项目

导出指向必要路径的变量：
```bash
export O3DE_HOME=/home/User/github/o3de/
export O3DE_EXTRAS_HOME=/home/User/github/o3de-extras/
export PROJECT_NAME=Ros2Project
export PROJECT_PATH=`pwd`/${PROJECT_NAME}
```

调整变量 `O3DE_HOME` 和 `O3DE_EXTRAS_HOME` 的位置以匹配您的环境。
如果要部署现有项目，请将 `PROJECT_NAME` 调整为项目的名称，将 `PROJECT_PATH` 调整为项目的位置。

从模板创建项目：
```bash
${O3DE_HOME}/scripts/o3de.sh create-project --project-path $PROJECT_PATH --template-path ${O3DE_EXTRAS_HOME}/Templates/Ros2ProjectTemplate -f 
```

## 工具集构建

构建工具集以处理所有必要的资产：
```bash
cd ${PROJECT_PATH}
cmake -B build/linux -G "Ninja Multi-Config" -DLY_DISABLE_TEST_MODULES=ON -DCMAKE_EXPORT_COMPILE_COMMANDS=ON -DLY_STRIP_DEBUG_SYMBOLS=ON
cmake --build build/linux --config profile --target ${PROJECT_NAME} ${PROJECT_NAME}.Assets
```

在上面的命令中，系统会指示您构建 `${PROJECT_NAME}.Assets` 的目标，从而触发构建期间所有资源的处理。在较大的项目中，建议直接运行 AssetProcessor。请参阅 [处理资产](docs/user-guide/packaging/windows-release-builds/#process-assets)部分以了解更多信息。请注意，您可以重复使用您的开发版本。
请注意，您可以重复使用您的开发版本。
在这种情况下，您需要运行 Asset Processor 并验证是否构建了所有资产（没有任何错误）。

## 整体式构建

处理资产后，您可以构建整体式构建，其中包括静态链接的所有 Gem 和大多数库。
它还不包括像 Qt 这样的大型纯编辑器框架。
请参阅 [创建项目游戏发布布局](docs/user-guide/packaging/windows-release-builds/#create-a-project-game-release-layout)以了解更多信息。

要执行整体式构建，请使用 `install` 目标：
```bash
cmake -B build/linux_mono -S . -G "Ninja Multi-Config"  -DLY_MONOLITHIC_GAME=1
cmake --build build/linux_mono --target install --config release
```

在安装目录 `${PROJECT_PATH}/install/bin/Linux/release/Monolithic/` 中，您可以找到一个包含所有必要的二进制文件和存档的软件包，以便在云端或其他计算机上运行仿真。

```
# tree ${PROJECT_PATH}/install/bin/Linux/release/Monolithic/
.
├── Cache
│   └── linux
│       └── engine.pak
├── libVkLayer_khronos_validation.so
├── Ros2Project.GameLauncher
├── Ros2Project.ServerLauncher
├── Ros2Project.UnifiedLauncher
└── VkLayer_khronos_validation.json
```

# 无头模拟

模拟通常需要无头运行（例如，作为 CI/CD 管道的一部分）。
这可以通过在 GameLauncher 中禁用 RHI 来实现。
要无头运行，请从 `-NullRenderer -rhi=null` 开始。
```bash 
./Ros2Project.GameLauncher -NullRenderer -rhi=null
```
上述命令应该适用于没有 GPU 的环境。
如果您的模拟依赖于 ROS 2 Camera 组件，您将需要 RHI。
在这种情况下，可以使用 `console-mode` 开关进行模拟：
```bash
./Ros2Project.GameLauncher -console-mode
```
