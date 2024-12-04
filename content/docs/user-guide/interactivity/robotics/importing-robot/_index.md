---
linkTitle: 导入机器人
title: 导入机器人
description: 在 Open 3D Engine （O3DE） 中使用 ROS 2 Gem 从描述文件导入机器人。
weight: 450
toc: true
---

**Open 3D Engine （O3DE**中的[ROS 2 Gem](/docs/user-guide/gems/reference/robotics/ros2/)  包括一个 **机器人导入器** 工具，用于导入 ROS 2 生态系统中使用的机器人描述。

## 关于支持的 robot 描述格式

SDFormat、URDF 和 XACRO 是 ROS（机器人操作系统）生态系统中广泛使用的机器人描述标准。

[SDFormat](http://sdformat.org/)是一种基于 XML 的文件格式，用于描述 robot 模拟的对象和环境。SDFormat 最初是为[Gazebo](https://gazebosim.org/home) 模拟器开发的，现已发展成为一个涵盖模拟所有方面的综合标准，包括静态和动态对象、视觉材质和物理属性。

[Unified Robot Description Format (URDF)](http://wiki.ros.org/urdf) 是一种基于 XML 的文件格式，它以结构化和标准化的方式描述机器人的物理特性。它包括有关机器人的关节、链接、传感器和其他组件的信息，以及它们的属性，例如质量、惯性和几何形状。

[XML Macros (XACRO)](http://wiki.ros.org/xacro) 是一种宏语言，可简化 URDF 文件的创建和维护。XACRO 允许您使用 XML 宏生成 URDF 文件。在 XACRO 中，您可以定义参数并包含文件，这对于更改和迭代机器人模型非常有用。您还可以在多个机器人模型中扩展和重用 XACRO。

URDF、XACRO 和 SDFormat 文件包含完整的机器人描述，包括对外部几何文件的引用。虽然您可以直接在机器人描述文件中定义基元几何图形，但通常的做法是使用 DAE （Collada） 或 STL 等格式的外部网格文件来表示机器人的视觉和碰撞形状。

机器人模型通常以软件包的形式提供，其中包括机器人描述文件和具有可视化和碰撞形状的其他几何文件，可以是基元几何图形，也可以是外部网格文件。这些软件包可以作为 ROS 工作区分发，在 ROS 应用程序中使用起来更直接。

## Robot Importer 简介

使用 ROS 2 Gem 中包含的 Robot Importer 将机器人导入您的 O3DE 模拟项目。Robot Importer 具有以下功能：

- 指导您逐步完成导入过程。
- 允许您更改导入的参数，包括资产的搜索路径。
- 读取 SDFormat、URDF 和 XACRO 文件。
- 将所有必需的 assets 文件复制到 O3DE 项目的 `Assets` 文件夹中。
- 使用关节或经典刚体和关节组件创建具有多体结构的Prefab。

## 将机器人导入到你的模拟项目中

### 先决条件

根据机器人描述文件的内容，您可能需要使用软件包及其依赖项构建 ROS 工作区。这可确保 Robot Importer 可以找到所有必需的文件。在某些情况下，可以改为在向导中附加 Robot Importer 使用的路径，并完全跳过 ROS 工作区的构建。

要构建，请创建一个工作区并运行`colcon build`。在启动 O3DE 编辑器之前，不要忘记获取 ROS 2 工作区。

有关更多信息，请参阅机器人描述包的文档，该文档通常包含在 GitHub 存储库的 README 文件中。

### 使用 Robot Importer 加载机器人定义文件

1. 加载您的项目并在 O3DE 编辑器中选择一个级别。

2. 通过执行以下任一操作，从 Editor 启动 Robot Importer：
   - 选择 **Main Menu > Tools > Robot Importer**
   - 在工具栏中点击 **Robot Importer** 图标 
  
        ![Robot Importer](/images/user-guide/gems/ros2/URDF_importer_button.png)

3. 阅读 Robot Importer 中的信息页面。然后选择 **Next**.

4. 通过执行以下操作加载描述文件：

   - 通过单击 **[...]** 按钮选择要导入的 SDFormat、URDF 或 XACRO 文件。
   - 有许多选项可用于修改导入器的行为：
        * Use Articulations （使用关节） - 确定是否应该将 PhysX 关节组件用于关节和刚体。
        * 保留 URDF 固定关节 - 设置后，将保留在导入 URDF 文件时找到的任何固定关节。这可以防止 libsdformat 中的关节缩减逻辑合并这些关节的链接。
        * 修复 URDF 以与 libsdformat 兼容 - 它允许您导入一些不完全兼容的 URDF 或 XACRO 文件。启用此功能后，Robot Importer 将尝试调整 URDF 代码以使其兼容。
        * 路径解析器 - SDFormat、URDF 和 XACRO 文件几乎总是引用一些其他文件（如网格和纹理）。此功能允许您提示 Robot Importer 在何处查找这些资产。
                           在高级情况下，例如导入旧文件或尝试导入 ROS 1 包，此功能可能很有用。 \
                           **重要** 此功能不会影响 XACRO 解析，因为此时使用的是与 ROS 2 安装捆绑在一起的 XACRO。
        * 使用 `AMENT_PREFIX_PATH` - 使用 [AMENT_PREFIX_PATH](https://design.ros2.org/articles/ament.html) 环境变量来查找资产引用。这是 ROS 2 描述包的默认行为。
        * Prefix Replacement （前缀替换） - 这是一个映射，允许您确定前面描述的 Path Resolvers 的路径。
    - 点击 **Next**.

   ![Robot Importer](/images/user-guide/gems/ros2/URDF_importer_load_file.png)

5. （可选）如果要导入的文件包含参数的 XACRO 文件，请设置参数的值。要编辑参数，请 **双击** 该值。完成后，单击 **Next**。

    ![Robot Importer](/images/user-guide/gems/ros2/URDF_importer_XACRO_parameters.png)

    {{< note >}}
如何设置参数取决于要导入的 XACRO 项目。要成功导入，请参阅 XACRO 项目的文档。

如果您的 XACRO 项目导入失败，您将看到一条消息，其中包含 ROS 2 XACRO 可执行文件的输出，类似于以下内容。

![Robot Importer](/images/user-guide/gems/ros2/URDF_importer_fail.png)
    {{< /note >}}

6. （可选）如果 XACRO 或 URDF 包含一些缺失信息，将自动调整。
如果您在第 4 点中启用“修复 URDF 以与 libsdformat 兼容”选项，则此功能可用。
目前，一些常见问题已解决：
 - 缺少惯性矩阵，质量以默认值添加，
 - Robot Importer 更改名称以使关节名称唯一。
在此自动调整之后，您可以查看所做的更改，并浏览修改后的 URDF。
![Robot Importer](/images/user-guide/gems/ros2/URDF_fixing_result.png)

    {{< note >}}
通过“Fix URDF to be compatible with libsdformat”选项，您可以成功导入与模拟不兼容的 URDF 文件，但请记住调整生成的Prefab。
    {{< /note >}}
 
### 处理机器人资产

加载描述文件后，Robot Importer 会显示一个表格，其中包含模型中使用的所有资产的列表。
表的每一行都描述一个资产，信息分为以下列：

- **URDF/SDF mesh path** - 网格的相对文件路径，如 URDF 文件中所定义。
- **Resolved mesh from URDF/SDF** - 位于文件系统中的网格的绝对文件路径。
- **Product asset** - 生成的产品资产（例如`*.azmodel`, `*.pxmesh`）在 O3DE 项目的 `Cache` 目录中的相对文件路径。
- **Source asset** - O3DE 项目的 `Assets` 目录中源资产的相对文件路径。
- **Type** - 资产的类型。

    成功加载的资产带有绿色复选标记，而加载失败的资产则标记为红色。

要查看有关每个资产的其他信息，请 **双击** 其行。这将打开 O3DE Asset Processor，其中显示导入详细信息。此信息对于排查导入失败的资源问题特别有用。

完成后单击 **Next**。

![Robot Importer](/images/user-guide/gems/ros2/URDF_importer_mesh_list.png)

**如果导入资源时出现问题，该怎么办？**

如果 **Source asset** 字段标记为失败，则 Robot Importer 无法在文件系统中找到对网格文件的引用。
在这种情况下，请确保正确构建和获取机器人的描述包。
您可以尝试在第 4 点中提供一种自定义方法来解析前缀。

如果 **Product asset** 字段标记为失败，则 Asset Builder 无法构建产品资产。
请参阅 Asset Processor 输出或`/user/log`目录中的日志文件。

您可以立即解决问题，也可以在使用 Robot Importer 完成后解决问题：
- 要解决此问题，请单击 **Back** 并解决网格文件的问题。然后单击 **Next** 重试导入。
- 要稍后解决，请单击 **Next** 并完成 URDF 导入。然后在 O3DE 编辑器中添加缺失的网格。

### 创建机器人Prefab

1. 通过执行以下操作创建Prefab：

- 输入Prefab名称。
   - （可选） 选择生成位置。
    如果场景中添加了生成点，则可以在下拉菜单中选择一个生成点。
   -点击**Create Prefab**.

    ![Robot Importer](/images/user-guide/gems/ros2/URDF_importer_prefab_creation.png)

2. 创建Prefab后，查看生成的实体的摘要。

    ![Robot Importer](/images/user-guide/gems/ros2/URDF_importer_summary.png)

3. 最后，场景中有一个Prefab。您可以添加一个或多个 [传感器](/docs/user-guide/interactivity/robotics/concepts-and-components-overview/#sensors) 或使用 [ROS 2 车辆动力学](/docs/user-guide/interactivity/robotics/vehicle-dynamics/)或使用 [关节操作](/docs/user-guide/interactivity/robotics/joints-manipulation/)移动机器人。

![Robot Importer](/images/user-guide/gems/ros2/URDF_result.png)

### 重新导入机器人

使用 Robot Importer 重新导入 URDF 文件。在某些情况下，如果之前正确导入了资源（网格文件），则它们不会更新。要解决此问题并执行完全重新导入，请使用 Asset Browser （资源浏览器） 或在目录中删除项目资源。

### 更多信息

有关支持的 SDFormat 传感器的详细信息，请参阅 [SDFormat 传感器子页面](./sdformat-sensors.md)。有关 SDFormat 模型插件的其他信息，请参见 [SDFormat 插件子页面](./sdformat-plugins.md)。您可能有兴趣在 [Importing Turtlebot4](./importing-turtlebot4.md)  子页面查看 [Turtlebot4 robot](https://clearpathrobotics.com/turtlebot-4/) 的更详尽的导入描述。
