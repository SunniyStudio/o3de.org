---
title: 使用 PostFX Shape Weight Modifier 修改摄像机曝光
linktitle: 使用 PostFX Shape Weight Modifier
description: 使用 PostFX Shape Weight Modifier 在 Open 3D Engine （O3DE） 中修改曝光控制。
toc: true
---

在此示例中，您将设置一个控制摄像机曝光的球形后处理体积。当摄像机移入和移出体积时，其曝光会发生变化。

后处理卷需要两个实体：一个定义 PostFX，另一个定义 PostFX 卷。此示例使用以下实体：

- **PostFX**: 此实体包含一个 **Exposure Control** 组件和一个 **PostFX Layer** 组件，用于设置摄像机从环境中接收的默认光量。
  
- **PostFX Volume**: 此实体包含一个 **PostFX Shape Weight Modifier** 组件和一个 **Sphere Shape** 组件，用于定义 PostFX 体积。它还包含一个 **Exposure Control** 组件和一个 **PostFX Layer** 组件，当摄像机通过 PostFX 体积时，这些组件可以调整摄像机的曝光度。

要使用后处理体积：

1. 在 **O3DE 编辑器 ** 中，创建一个默认关卡。

2. 为 PostFX 创建一个实体。右键单击 **Perspective**，然后选择 **Create entity**。
  
3. 在新的 PostFX 实体中，添加以下组件：
    
    - PostFX Exposure Control
    
    - PostFX Layer

4. 在 Exposure Control （曝光控制） 组件中，将 **Manual Compensation** 属性设置为 '`1.0`'。这将为相机设置默认的明亮曝光。


5. 为 PostFX Volume创建一个实体。右键单击 **Perspective** 旁边的 **Shader Ball** 实体，并选择 **Create entity**。

6. 在新的 PostFX Volume 实体中，添加以下四个组件：
    
    - PostFX Exposure Control
    
    - PostFX Layer
    
    - PostFX Shape Weight Modifier
    
    - Sphere Shape

7. 在 Exposure Control （曝光控制） 组件中，将**Manual Compensation** 设置为 `-3.5` 以降低 PostFX 体积内的摄像机曝光。
    
8. 在 PostFX Layer组件中，将**Layer Category** 属性设置为 `Volume`。这可确保 Exposure Control （曝光控制） 组件在 Sphere Shape （球体形状） 组件的体积内处于活动状态。

9.  在PostFX Shape Weight Modifier组件中，将Fall-off Distance属性设置为`3.0`。这会增加从默认曝光设置到体积内部曝光设置的过渡距离，使过渡逐渐进行。
 
10. 在Sphere Shape组件中，将**Radius** 设置为 `3.0`。要查看卷的调试视图，请点击Perspective右上角的 {{< icon "helpers.svg" >}} **Toggle viewport helpers** 按钮。

11. 点击工具栏的 {{< icon "play.svg" >}} **Play Game** 按钮或按下**Ctrl+G**，运行关卡。使用鼠标和 W、S、A、D 移动相机。曝光度会随着摄像机在 PostFX Volume中移动而改变。按 **Esc** 退出播放模式。

下面的图像系列演示了 PostFX 卷在此示例中的工作原理。如左图所示，当摄像机位于体积之外时，场景会显得很亮。在右侧，当摄像机进入体积时，场景会变暗。

![Using PostFX Volumes](/images/user-guide/components/reference/atom/post-processing-modifiers/postfx-example.png)
