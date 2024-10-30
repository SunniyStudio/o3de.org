---
title: Diffuse Global Illumination 组件
linktitle: Diffuse Global Illumination
description: Open 3D Engine (O3DE) Diffuse Global Illumination 组件参考。
toc: true
---

**Diffuse Global Illumination**组件是一个关卡组件，用于控制**Diffuse Probe Grid**组件所提供的全局照明质量等级。漫反射全局照明技术可计算整个环境中光线反弹、反射、散射和吸收的效果。它可以模拟现实世界中的照明行为，即物体既受到光源的照射，也受到其他物体反射光的照射。

{{< note >}}
要在**Atom 渲染器**中使用光线追踪功能，您的 GPU 必须支持 DirectX Shader Model 6.3 或更高版本。
{{< /note >}}


## 提供方

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)


## 依赖

None


## 属性

![Diffuse Global Illumination base properties](/images/user-guide/components/reference/atom/diffuse-gi-component-ui.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Quality Level** | 设置场景中所有Diffuse Probe Grid组件的质量级别。质量级别越高，产生的伪影越少，逼真度越高，但性能和内存消耗也越大。  | `Low`, `Medium`, `High` | `Low` |
