---
linkTitle: 缓动类型
description: ' 了解不同的脚本化实体补间缓动类型如何影响 Open 3D Engine 中动画的外观。 '
title: 支持的 Tweener 缓动类型
weight: 200
---

可视化脚本化实体 Tweener 缓动类型概念的最简单方法是在描述位置随时间变化的图形上查看它。生成的线可以被认为是速度，但不一定如此。

自然界中物体的运动很少是均匀的。从地面发射的火箭飞船在首先获得动量时缓慢加速，然后在冲向天空时迅速加速。如果您在位置和时间图上查看此运动，则它可能在函数上看起来像四次或五次缓动。从高处掉落的弹力球加速向地面飞去，并以一定的速度击中它。球以该速度反弹回来，并在接近弧顶时减速，重力将其拉回以进行另一次反弹。在位置和时间图上查看，看起来像 bounce easing out 函数。

O3DE 支持以下 [补间缓动方法](../tweener-parameters) 作为 [缓入、缓出和缓出入](../tweener-parameters)  函数 （缓动类型） 。

{{< note >}}
以下图片引用自 [http://easings.net](http://easings.net).
{{< /note >}}

**Linear**
不会缓入或缓出。无加速或减速。持续运动，例如运动中的传送带。

![Linear easing function](/images/user-guide/interactivity/user-interface/animating/tweener-system/concepts/ui-animating-tweener-linear.png)

**Quadratic**
缓入函数从零速度开始，然后加速。
缓出函数以一定的速度开始，然后减速到零。
缓动 in-out 函数从零速度开始，加速到一半，然后减速到零。
下图显示了二次缓入、二次缓出和二次缓入-缓出函数。

![Quadratic easing in, out, and in-out functions](/images/user-guide/interactivity/user-interface/animating/tweener-system/concepts/ui-animating-tweener-quad.png)

**Cubic**
与二次曲线类似，但曲线更陡峭，这表示起初加速或减速的速度较慢，然后是快速的加速或减速。
下图显示了三次缓入、三次缓出和三次缓入-出函数。

![Cubic easing in, out, and in-out functions](/images/user-guide/interactivity/user-interface/animating/tweener-system/concepts/ui-animating-tweener-cubic.png)

**Quart**
类似于 cubic，但加速或减速速度更慢，然后加速或减速速度更快。
下图显示了四次缓入、四次缓出和四次缓入-出函数。

![Quartic easing in, out, and in-out functions](/images/user-guide/interactivity/user-interface/animating/tweener-system/concepts/ui-animating-tweener-quart.png)

**Quint**
类似于夸脱，但加速或减速速度更慢，然后加速或减速速度更快。
下图显示了五次缓入、五次缓出和五次缓入-出函数。

![Quintic easing in, out, and in-out functions](/images/user-guide/interactivity/user-interface/animating/tweener-system/concepts/ui-animating-tweener-quint.png)

**Sine**
基于 sine 或 cosine 函数。轻柔地缓入或缓出，速度几乎恒定，就像线性函数一样。
下图显示了正弦缓入、正弦缓出和正弦缓入-输出函数。

![Sinusoidal easing in, out, and in-out functions](/images/user-guide/interactivity/user-interface/animating/tweener-system/concepts/ui-animating-tweener-sine.png)

**Exponential**
类似于 cubic。
下图显示了指数缓入、指数缓出和指数缓入-出函数。

![Exponential easing in, out, and in-out functions](/images/user-guide/interactivity/user-interface/animating/tweener-system/concepts/ui-animating-tweener-cubic.png)

**Circular**
基于半个圆的方程式。下图中的图形垂直拉伸，以便您可以看到图形看起来像一个圆的一部分。
下图显示了循环缓入、循环缓出和循环缓入-缓出函数。

![Circular easing in, out, and in-out functions](/images/user-guide/interactivity/user-interface/animating/tweener-system/concepts/ui-animating-tweener-circ.png)

**Elastic**
弹性 easting out 函数的一个示例是拨动的吉他弦。琴弦以递减的频率上下移动，直到停止。缓入是相同的动作，但方向相反。
下图显示了弹性缓入、弹性缓出和弹性缓入-移出函数。

![Elastic easing in, out, and in-out functions](/images/user-guide/interactivity/user-interface/animating/tweener-system/concepts/ui-animating-tweener-elastic.png)

**Back**
功能向后缓动的一个例子是玩具车向后拉以给弹簧上弦，然后松开。
下图显示了 back ease in、back ease out 和 back ease in-out 函数。

![Back easing in, out, and in-out functions](/images/user-guide/interactivity/user-interface/animating/tweener-system/concepts/ui-animating-tweener-back.png)

**Bounce**
描绘弹跳运动。例如，从高处掉落的橡胶球在弹跳并最终静止时会显示弹跳缓出函数。
下图显示了 bounce ease in、bounce ease out 和 bounce ease in out 函数。

![Bounce easing in, out, and in-out functions](/images/user-guide/interactivity/user-interface/animating/tweener-system/concepts/ui-animating-tweener-bounce.png)
