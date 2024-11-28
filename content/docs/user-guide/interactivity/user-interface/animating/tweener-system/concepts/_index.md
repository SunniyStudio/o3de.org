---
linkTitle: Tweener 概念
description: ' 了解 Open 3D Engine 脚本化实体补间系统中的补间如何根据补间类型解释两帧之间的移动。 '
title: Tweener 概念
weight: 100
---

补间生成两个图像之间的过渡，从而在两个帧之间产生平滑演变的外观。最简单的是补间创建线性过渡。例如，一个从屏幕顶部开始到屏幕底部结束的圆圈在两点之间以稳定、不变的速度移动。这类似于在传送带上移动的物体。要模拟重力，可以使用二次补间，它起初缓慢加速，然后在结束时急剧增加。这模拟了放置不反弹的对象。

当您想要模拟弹跳或弹性时，弹跳补间和弹性补间提供了灵活性。下面的动画显示了一个并轨的补间。球从屏幕外开始，到屏幕底部结束。在开始帧和结束帧之间，球似乎会反弹。

![Ball with bouncing tweener from off-screen to the bottom of the UI screen](/images/user-guide/interactivity/user-interface/animating/tweener-system/concepts/ui-animating-tweener.gif)

location 以外的属性也可以使用补间。以下动画显示了用于不透明度的线性补间和用于缩放的并轨补间。

![Linear tweener for opacity (transparent to opaque) and bouncing tweener for scale (small to large)](/images/user-guide/interactivity/user-interface/animating/tweener-system/concepts/ui-animating-tweener-1.gif)

补间是高度可定制的。您可以指定参数来更改属性，例如持续时间、开始前的延迟、播放次数、是否从指定值变为当前值（反之亦然）等。

有关可用参数及其使用方法，请参阅 [Tweener 支持的组件](../tweener-components)。你也可以 [创建时间线](../tweener-timeline)将动画与不同的补间类型链接在一起。
