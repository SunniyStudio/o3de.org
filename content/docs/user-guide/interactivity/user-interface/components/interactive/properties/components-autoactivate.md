---
linkTitle: Auto Activate
description: ' 在 O3DE UI 编辑器中为交互式元素启用 Auto Activate ，以将元素设置为自动激活。 '
title: Auto Activate
weight: 500
---

**Auto Activate** 设置适用于在按下后仍保持活动状态的交互式元素。这些选项包括滑块、滚动条、滚动框和文本输入。

设置“Auto Activate”后，元素在使用方向键导航到时会自动激活，而在使用方向键离开时将停用。用户无需按 **Enter** 和 **Back** 键来激活或停用该元素。

UI 的设计必须使用户不会卡在激活的元素上。例如，设置为自动激活的水平滑块的左侧或右侧不应有其他交互式元素。这是因为在活动滑块上按左键或右键会移动滑块手柄。
