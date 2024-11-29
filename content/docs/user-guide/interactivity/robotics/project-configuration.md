---
linkTitle: 项目配置
title: ROS 2 项目配置
description: 如何在 Open 3D Engine （O3DE） 中使用 ROS 2 Gem 安装依赖项并构建项目。
weight: 200
toc: true
---

## 要求

* 带有 ROS 2 Humble 的 Ubuntu 22.04 或带有 ROS 2 Jazzy 的 Ubuntu 24.04。其他 Ubuntu 版本、ROS 2 版本和 Linux 发行版也可以工作，但不受支持。
  {{< important >}}
ROS 2 Gem 不适用于 Windows。
  {{< /important >}}
* [O3DE 在 Linux 上从源代码构建](/docs/welcome-guide/setup/setup-from-github/building-linux).
* ROS 2的[最新版](https://docs.ros.org/en/rolling/Releases.html#list-of-distributions )。此说明假定已安装`desktop`版本。否则，可能会缺少某些包。
  * O3DE ROS 2 已在 Ubuntu 22.04 上使用 [ROS 2 Humble](https://docs.ros.org/en/humble/Installation.html)  和 [ROS 2 Iron](https://docs.ros.org/en/iron/Installation.html) 进行了测试。

## 设置

### ROS 2 生态系统

#### 获取您的 ROS 2 工作区

要使用 ROS 2 Gem 构建或运行项目，您必须在控制台中 [source your ROS 2 workspace](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Configuring-ROS2-Environment.html)。确保 ROS 2 始终是来源的最好方法是将以下行添加到`~/.profile`文件中：
```
source /opt/ros/<distro>/setup.bash
```
将 `<distro>` 替换为 ROS 2 分配名称(例如 `humble`)。
然后，您必须注销并从 Ubuntu 登录，更改才能生效。

#### 自定义软件包

Gem 完全支持 [工作区叠加](https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html#source-the-overlay)。
在 ROS 2 安装的基础上获取您的工作区以包含自定义软件包。

Gem 附带了许多已包含和链接的 ROS 2 包，但您可能希望在项目中包含其他包。
为此，在你的项目的`Gem/CMakeLists.txt`中使用 `target_depends_on_ros2` 功能:

```
target_depends_on_ros2_packages(<your_target> <ros_package1> <ros_package2>)
```

#### 使用多个 ROS 版本

如果您安装了多个 ROS 2 版本，请确保您 [source](https://docs.ros.org/en/humble/Tutorials/Workspace/Creating-A-Workspace.html#source-the-overlay)是您想要使用的那个。您可以通过检查`ROS_DISTRO`环境变量 （`echo $ROS_DISTRO`） 的值来检查控制台中的版本来源。

> 如果项目之前是使用其他 ROS 版本构建的，您当前需要重新构建项目。

### 需要额外的 ROS 2 软件包

* gazebo_msgs: `ros-${ROS_DISTRO}-gazebo-msgs`
    * gazebo_msgs 用于机器人生成（不依赖于 Gazebo）。
* Ackermann messages: `ros-${ROS_DISTRO}-ackermann-msgs`
* Control toolbox `ros-${ROS_DISTRO}-control-toolbox`
* XACRO `ros-${ROS_DISTRO}-xacro`
* Vision msgs `ros-${ROS_DISTRO}-vision-msgs`

如果选择了 ROS 2 发行版的`desktop`安装，则其他所有内容都应该在那里。

使用这个有用的命令来安装：

```
sudo apt install ros-${ROS_DISTRO}-ackermann-msgs ros-${ROS_DISTRO}-control-toolbox ros-${ROS_DISTRO}-nav-msgs ros-${ROS_DISTRO}-gazebo-msgs ros-${ROS_DISTRO}-xacro ros-${ROS_DISTRO}-vision-msgs
```

### 克隆 Gem 存储库

ROS 2 Gem 位于 ['o3de/o3de-extras'](https://github.com/o3de/o3de-extras)存储库中。将 GitHub 存储库克隆到您的计算机上：

```
git clone https://github.com/o3de/o3de-extras
```

### 注册 Gem

要在任何 O3DE 项目中使用 ROS 2 Gem，您需要在 O3DE 中注册它。

为方便起见，请设置几个环境变量： `O3DE_HOME`到 O3DE 所在的位置 和 `O3DE_EXTRAS_HOME`
添加到克隆的 o3de-extras 存储库的路径中，例如：

```shell
export O3DE_HOME=${HOME}/o3de
export O3DE_EXTRAS_HOME=${HOME}/o3de-extras
```

运行以下命令以注册 ROS 2 Gem：
```bash
${O3DE_HOME}/scripts/o3de.sh register --gem-path ${O3DE_EXTRAS_HOME}/Gems/ROS2
```

### 注册机器人项目模板

机器人项目模板可以帮助您快速启动仿真项目。我们建议您注册机器人项目模板 Gem 及其资产 Gem，这些 Gem 是使用`o3de-extras`存储库下载的。

要注册机器人模板和资产：
1. 启用 Git Large File Storage （LFS）（如果尚未启用）。 Asset Gem 使用 LFS 存储大型文件。
    ```bash
    cd ${O3DE_EXTRAS_HOME}
    git lfs install && git lfs pull
    ```
2. 从 o3de-extras 注册以下模板和资产。
    ```bash
    ${O3DE_HOME}/scripts/o3de.sh register --all-gems-path ${O3DE_EXTRAS_HOME}/Gems/
    ${O3DE_HOME}/scripts/o3de.sh register --all-templates-path ${O3DE_EXTRAS_HOME}/Templates/
    ```
   
有关更多信息，请参阅 [添加和删除 Gem](/docs/user-guide/project-config/add-remove-gems/) 和 [注册 Gem](/docs/user-guide/project-config/register-gems/)。

### 创建一个新的机器人模拟项目

#### 机器人项目模板

工程模板是塑造初始工程的有用工具。
使用模板创建时，新项目可以从特定配置开始。包括某些已启用的 Gem 和起始关卡。

机器人项目模板旨在帮助您快速开始使用 ROS 2 在 O3DE 中模拟机器人。

#### ROS 2 项目模板

机器人有三个模板：
- [ROS 2 项目模板](https://github.com/o3de/o3de-extras/tree/development/Templates/Ros2ProjectTemplate):
  - 一个多功能、轻量级的模板，非常适合启动项目，包括一个带差动驱动的机器人。
- [Warehouse 项目模板](https://github.com/o3de/o3de-extras/tree/development/Templates/Ros2FleetRobotTemplate):
  - 带有 Proteus 机器人的照片级逼真仓库，易于定制和扩展（多机器人）。
- [Manipulation 项目模板](https://github.com/o3de/o3de-extras/tree/development/Templates/Ros2RoboticManipulationTemplate):
  - 包括两个带有机器人机械臂的级别：一个专注于码垛，另一个专注于研发。

:bulb: 模板存储库还包括一些示例，您可以按照其 README 文件进行试用。

#### 使用模板创建新项目

要使用模板创建项目，您可以使用 GUI 或命令行。
最快的方法是运行以下命令 (调整 `PROJECT_NAME`, `PROJECT_PATH` 和您想要的模板):

```shell
export PROJECT_NAME=MySimulationProject
export PROJECT_PATH=${HOME}/projects/${PROJECT_NAME}
${O3DE_HOME}/scripts/o3de.sh create-project --project-path $PROJECT_PATH --template-path ${O3DE_EXTRAS_HOME}/Templates/Ros2ProjectTemplate 
```

有关更多信息，请参阅 [项目创建](/docs/welcome-guide/create/)

### 构建

ROS 2 Gem 是在启用 ROS 2 Gem 的情况下构建 O3DE 项目时构建的。

在构建之前，请确保 [source your ROS 2 workspace](#source-your-ros-2-workspace) 。

为方便起见，下面是一个参数化 CMake 调用的示例：

```shell
cd $PROJECT_PATH
cmake -B build/linux -G "Ninja Multi-Config" -DLY_DISABLE_TEST_MODULES=ON -DCMAKE_EXPORT_COMPILE_COMMANDS=ON -DLY_STRIP_DEBUG_SYMBOLS=ON
cmake --build build/linux --config profile --target ${PROJECT_NAME} Editor ${PROJECT_NAME}.Assets 
```
{{<note>}}
在版本 24.09.0 之前，PhysX 5 是实验性的，并在引擎的源代码编译过程中进行编译。 
如果您使用的是版本 23.10.3 或更早版本，则需要指定一个附加标志：`-DAZ_USE_PHYSX5:BOOL=ON` :
```shell
cmake -B build/linux -G "Ninja Multi-Config" -DLY_DISABLE_TEST_MODULES=ON -DCMAKE_EXPORT_COMPILE_COMMANDS=ON -DLY_STRIP_DEBUG_SYMBOLS=ON -DAZ_USE_PHYSX5:BOOL=ON 
```
{{</note>}}
### 在 O3DE 编辑器中启动您的项目

构建项目后，运行以下命令以启动 Editor：

```shell
${PROJECT_PATH}/build/linux/bin/profile/Editor
```
