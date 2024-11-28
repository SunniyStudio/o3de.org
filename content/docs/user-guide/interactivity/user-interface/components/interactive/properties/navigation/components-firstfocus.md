---
linkTitle: First Focus Element
description: ' 使用 First Focus Element 属性可确定在 O3DE 中首次加载画布时哪个元素获得第一个焦点。 '
title: First Focus Element
weight: 100
---

要确定在首次加载画布时哪个元素获得第一个焦点，请在 [Canvas属性](/docs/user-guide/interactivity/user-interface/canvases/canvas-properties) 中设置 **First Focus Element**。当未检测到鼠标时，**First Focus Element** 在 canvas 加载时接收焦点。如果未设置 **First Focus Element**，或者如果检测到鼠标，则在用户从键盘或鼠标提供方向输入之前，任何元素都不会获得焦点。此时，最靠近画布左上角的元素将获得焦点。

元素获得焦点后，到其他元素的导航由为每个交互式组件定义的 [navigation 属性](./) 控制。
