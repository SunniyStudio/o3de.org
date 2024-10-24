---
description: ' 进一步了解动画编辑器的文件类型。 '
title: '动画编辑器文件类型'
---

当您将`.fbx`文件从DCC导入到O3DE编辑器时，Asset Processor会创建您在**动画编辑器**中使用的文件。下面的示例显示了如何创建和修改文件类型。有关处理角色和动画文件的更多信息，请参阅 [使用 FBX 设置自定义 FBX 资产导出](/docs/user-guide/assets/scene-settings/)。

![See an overview of how Animation Editor files are created and processed in O3DE.](/images/user-guide/actor-animation/animation-editor-asset-processor-files.png)

以下是**动画编辑器**中使用的文件类型：

## 文件类型 

将`.fbx`文件导入 O3DE 时，Asset Processor 会生成以下用于**动画编辑器**的文件类型：
+ 当`.fbx`文件至少有一个骨骼时，就会创建`.actor`文件。您希望使用角色的蒙皮网格作为`.actor`文件。`.actor`文件是显示动画的角色。
+ 当带有骨骼的`.fbx`文件至少有一个关键帧时，就会创建`.motion`文件。如果您的 `.fbx` 文件有动画关键帧，则会创建一个 `.motion` 文件。您的 `.motion` 文件包含在构建动画图之前添加到动作集的动画。
+ 当`.fbx`文件至少有一个材质时，就会创建`.mtl`文件，大多数 DCC 工具都是这种情况。如果您在**材质编辑器**中对材质进行了更改，`.mtl`文件就不再是`.fbx`文件的子文件，而`.mtl`文件则是`.fbx`文件源目录下的同级文件。您可以使用文本编辑器或**材质编辑器**对`.mtl`文件进行其他修改。

创建`.motion` 文件时，也会创建一个`.actor`文件。您要在**动画编辑器**中使用的`.actor`文件通常是从 DCC 导出的绑定姿势中的蒙皮网格。在**动画编辑器**中跟踪您要用作`.actor`文件的蒙皮网格。您可以进入**FBX 设置**工具，删除不需要的Actor文件。

## 动画图形所需的文件类型

在**动画编辑器**中创建动画图时，动画图必须包含以下文件：
+ `.actor`
+ `.motion`
+ `.motionset`
+ `.animgraph`

## 保存动画编辑器文件

在**动画编辑器**中保存项目会创建一个“.工作区 ”文件。工作区保存了您正在使用的Actor、动作、动作集和动画图。打开工作区时，**动画编辑器**会加载这些文件，这样您就可以从上次离开的地方继续工作。

**要保存你的工作区**
+ 在**Animation Editor**中，选择 **File**，然后选择以下之一：
  + **Save Workspace**
  + **Save Workspace As**

保存 `.actor`和`.motion`文件时，**动画编辑器**会在源文件`.fbx`旁边创建一个 `.assetinfo`文件。`.assetinfo`文件存储了`.actor`和`.motion`文件的配置和设置。

为`.actor`文件保存的设置包括 “角色名称”、“动作提取节点”、“排除边界”、“碰撞网格设置 ”和 “镜像设置”。

为`.motion`文件保存的设置包括动作提取捕捉高度选项和动作事件。

**要保存`.actor` 和 `.motion` 文件:**
+ 在 **Animation Editor**中，完成以下之一：
  + 点击 **Save All** 保存对 `.actor`, `.motion`, `.motionset`, 和 `.animgraph` 文件的修改。 对话框会提示您选择要保存的文件。
  + 单击**Save Workspace**保存当前工作区。如果没有保存工作区，则会出现一个对话框，让您为工作区命名，并将其保存到首选目录。
  + 单击 **Save Workspace As**，以不同的名称或将工作区保存到其他目录。
+ 要单独保存动作文件，请单击**Motions** 窗格中的 “保存 ”图标。
+ 要单独保存Actor文件，请单击**Actor Manager**窗格中的保存图标。
