---
linkTitle: Audio Area Environment
title: Audio Area Environment 组件
description: 使用 O3DE 中的Audio Area Environment组件，可对实体触发的声音应用环境效果。
toc: true
---

使用 **Audio Area Environment** 组件，您可以对实体触发的声音应用环境效果。要使用音频区域环境组件，还必须添加形状组件。

## Audio Area Environment 属性

**Audio Area Environment** 组件具有以下属性：

**Broad-phase Trigger Area**
将此属性链接到包含触发区域和形状的实体。该触发区域用于宽相位检查。**Audio Area Environment**组件会跟踪在触发区域内移动的任何实体。
默认值: None

**Environment name**
[Audio Translation Layer (ATL)](/docs/user-guide/interactivity/audio/audio-translation-layer)环境的名称，用于该区域的实体。
默认值: None

**Environment fade distance**
根据实体与形状的距离，环境量会逐渐减弱的形状周围的距离。只有非零的正值才有效。
默认值: 1.0

## 使用 Audio Area Environment 组件

设置**Audio Area Environment**组件需要两个实体。第二个实体与第一个实体中的**Audio Area Environment**组件相连，充当宽相位触发区域。当这些实体配置正确时，任何靠近或穿过第一个实体内部形状的实体都会对任何触发的声音应用环境量。

**设置Audio Area Environment 组件**

1. 在 O3DE 编辑器中，右键单击关卡中的视口，然后单击**Create new component entity**。

1. 点击**Tools**，**Entity Inspector**。确保在视口中选中了新的组件实体。

1. 在**Entity Inspector**中，单击**Add Component**、**Shape**，然后选择其中一个形状选项。

1. 单击 **Add Component**，**Audio**，**Audio Area Environment**。

1. 重复步骤 1 - 4，添加另一个具有 **Shape**（任意）组件和 **Trigger Area**组件（位于 **Scripting**下）的实体。

1. 放置两个实体并确定其大小，以满足以下条件：
  + 第二个实体的形状完全包含第一个实体的形状。
  + 第二个实体的形状至少比第一个实体的形状大**Environment fade distance**的值。这样，当实体进入外部触发区域时，**Audio Area Environment**组件就可以跟踪实体与内部形状的距离。

1. （可选）在**Entity Inspector**中，对于**Trigger Area**组件，使用**Tag Filters**过滤不想让**Audio Area Environment**组件处理的实体。
