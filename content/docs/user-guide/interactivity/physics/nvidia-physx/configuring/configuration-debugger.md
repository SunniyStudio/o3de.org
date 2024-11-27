---
linkTitle: 调试器配置
title: 调试器配置
description: ' 在 Open 3D Engine 中配置 PhysX 系统的调试。 '
weight: 600
---

在 **PhysX 配置**中，您可以指定如何与 **PhysX Visual Debugger （PVD）** 交互。PhysX Visual Debugger 是一个第三方应用程序，用于记录来自 O3DE 编辑器的 PhysX 数据。然后，您可以查看此数据以查看物理效果的显示方式。

有关更多信息，请参阅 NVIDIA [PhysX Visual Debugger](https://docs.nvidia.com/gameworks/content/gameworkslibrary/physx/guide/Manual/VisualDebugger.html#physxvisualdebugger) 文档。

## 安装 PhysX Visual Debugger

1. 开始使用, [下载](https://developer.nvidia.com/physx-visual-debugger) PhysX Visual Debugger.

    {{< note >}}
您必须拥有 NVIDIA 帐户才能下载 PhysX Visual Debugger。如果您还没有帐户，请创建并登录您的帐户。
{{< /note >}}

1. 按照安装步骤操作。

1. 安装应用程序后，打开 PhysX Visual Debugger。如果要从 O3DE Editor 记录数据，则必须运行此应用程序。

1. 在 O3DE 编辑器中，打开一个关卡或创建一个关卡，其中包含具有 PhysX 组件的实体。例如，您可以创建响应重力的简单动态实体。

## 在 O3DE 中配置 PhysX 调试器

1. 在 O3DE 编辑器中，选择 **Tools**、**PhysX Configuration**。

1. 单击 **Debugger** 选项卡。

1. 您可以指定以下设置。

    ![PhysX Visual Debugger settings.](/images/user-guide/interactivity/physics/nvidia-physx/configuring/physx-configuration-debugger-1.png)

1. 要验证 PhysX Visual Debugger 是否已连接到 O3DE，对于 **PVD Auto Connect**，选择 **Game** 或 **Editor**，然后进入游戏或编辑器模式。根据您选择的内容，以下消息将显示在 Editor Console 中。

    ```
    (PhysX) - Successfully connected to PhysX Visual Debugger.
    ```

1. 打开 PhysX Visual Debugger 以查看录制的信息。

    **示例:**
    
    ![Review the recorded data from O3DE Editor in PhysX Visual Debugger.](/images/user-guide/interactivity/physics/nvidia-physx/configuring/physx-configuration-debugger-2.png)

1. 您还可以使用以下控制台变量命令手动连接或断开 PhysX Visual Debugger 的连接。

   ```
   physx_PvdConnect
   ```

   ```
   physx_PvdDisconnect
   ```

   有关详细信息，请参阅 [调试 PhysX](/docs/user-guide/interactivity/physics/debugging/).
