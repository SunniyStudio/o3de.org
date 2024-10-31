---
title: PostFX Layer 组件
linktitle: PostFX Layer
description: 'Open 3D Engine (O3DE) PostFX Layer组件参考'
toc: true
---

**PostFx Layer** 组件控制如何在场景中应用后处理特效（PostFX），如 Bloom、Deferred Fog 和景深。例如，您可以控制各种后期特效的混合方式，或设置哪些摄像机会受到后期特效层的影响。

## 提供方 ##

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## 依赖

None


## Base 属性

![PostFX Layer base properties](/images/user-guide/components/reference/atom/post-processing-modifiers/postfx-layer.png)

| 属性 | 说明 | 值 | 默认值 |
| - | - | - | - |
| **Layer Category** | 指定存在 PostFX Layer 组件的图层。 | Layer Category element | `Default` |
| **Priority in Default** | 优先级值，用于确定该组件与多个 PostFX 混合时的顺序。具有相同图层类别的ostFX Layer不能具有相同的优先级。 | `0` to `20` | `0` |
| **Weight** | 与其他PostFX Layer混合时，手动内插一个PostFX Layer的特效权重。 `0`的权重最小，`1` 的权重最大。其他Processing Volume组件也可以调节该PostFX Layer的权重。 | `0.0` to `1.0` | `1.0` |
| **Select Camera Tags Only** | 只有包含指定标记的Camera实体才会受到此 PostFX Layer的影响。 | Tag element | Empty |
| **Excluded Camera Tags** | 包含指定标记的Camera实体不受此 PostFX Layer的影响。 | Tag element | Empty|

## 使用 Layer 类别

每个 PostFX Layer组件都必须分配给一个Layer Category。每个Layer Category都是一个键值对，包含一个 64 位的整数值，用于排列其优先级，“0 ”为最高优先级。当多个 PostFX Layer处于活动状态时，它们首先按图层类别值排序，然后再按其分配的Layer类别中的默认优先级值排序。

更新预设Layer Category列表：

1. 从 **Tools** 菜单，打开Asset Editor。

1. 在Asset Editor中，转到 **File** &rarr; **Open**。

1. 搜索 `default.postfxlayercategories`，并在Asset Editor中打开它。

1. 点击 {{< icon "caret-closed.svg" >}} **Layer Categories Caret** 展开列表。

1. 点击 {{< icon "add.svg" >}} **Add** 按钮。

1. 提供一个Layer Category 键 (名称) 并选择 **OK** 向列表添加一个新的 Layer Category。

1. 必要时修改Layer Category值。默认值为`0`，这使得新Layer Category具有最高优先级。

1. 在 Asset Editor中，从 **File** 菜单选择 **Save** 或按下 **Ctrl+S** 键保存你的修改。

你也可以创建一个新的Layer Category 列表。

1. 在 Asset Editor中，到 **File** &rarr; **New** &rarr; **PostFx Layer Categories**。

1. 按照以上步骤在列表中添加一个新Layer Categories。

1. 在 Asset Editor 中，从 **File** 菜单选择 **Save** 或按下 **Ctrl+S** 键保存你的修改。


## 使用 Camera Tag

您可以通过应用于摄像机实体的**Tag**组件，从 PostFX 图层组件中包含或排除摄像机。在**Select Camera Tags Only**和**Excluded Camera Tags**属性中指定的标签必须与摄像机实体的标签组件相匹配。

在本例中， PostFX Entity有一个PostFX Layer组件，该组件只影响具有`MainCamera`标签的Camera Entity。这就会影响具有`MainCamera`标记的摄像机实体。

![Using camera tags with PostFX Layer](/images/user-guide/components/reference/atom/post-processing-modifiers/postfx-layer-camera-tags-1.png)

![Using camera tags with PostFX Layer](/images/user-guide/components/reference/atom/post-processing-modifiers/postfx-layer-camera-tags-2.png)
