---
description: ' 在 Open 3D Engine with NVIDIA Blast 中对破坏模拟进行可视化调试。 '
title: NVIDIA Blast可视化调试器
weight: 700
draft: true
---


要使用 NVIDIA Blast 的可视化调试器，请启用 `BLAST_DEBUG` 控制台变量。

**启用变量**

1. 在编辑器控制台中，输入 **BLAST\_DEBUG 1**。

1. 在 **Perspective** 中单击以激活视口。

1. 按 **F3** 键将视图设置为线框。

1. 按 **Ctrl+P** 运行模拟并查看调试器。

1. 按 **Ctrl+P** 退出模拟。

在下面的示例中，当兔子掉落时，断裂块之间的键显示为绿线。撞击时，键在变弱时变为橙色，在断裂时变为红色。

![Debug visualization for NVIDIA Blast.](/images/user-guide/physx/blast/anim-nvidia-blast-debug.gif)
