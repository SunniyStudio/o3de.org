---
title: Decal 组件
linktitle: Decal
description: "Open 3D Engine (O3DE) Decal 组件参考。"
toc: true
---

**Decal** 组件可让实体将材质投射到网格上。在一个网格上可以应用大量重叠的贴花。

## 提供方 ##

[Atom Gem](/docs/user-guide/gems/reference/rendering/atom/atom/)

## 限制 ##

Decal 组件具有以下限制:
- 虽然贴花纹理可以是任意分辨率，但贴花系统最多只能支持 5 种不同的纹理分辨率。您可以创建其他贴花纹理，但纹理分辨率必须是已在使用的 5 种尺寸之一。
- 所有贴花材质都必须指定基色和法线贴图。如果任何贴花材质缺少法线贴图，那么任何贴花都不会使用法线贴图。
- 基色和法线纹理的大小必须相同。
- 贴花材质中的不透明度仅支持基色纹理的 alpha 通道。虽然一般材质都支持单独纹理中的不透明度，但这并不能正确显示在贴花上。

## 属性

![Decal component UI](/images/user-guide/components/reference/atom/decal-component-ui/decal-component-ui-01.png)

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Attenuation Angle** | 控制表面与贴花投影之间的角度对贴花不透明度的影响程度。数值越大，贴花越透明，角度越大，贴花就不会环绕表面。  | `0.0` 到 `1.0` | `1.0` |
| **Opacity** | 决定贴花的透明程度。 | `0.0` 到 `1.0` | `1.0` |
| **Sort Key** | 设置贴花的排序顺序。当多个贴花投射到一个表面上时，数值较高的贴花会显示在上面。 | `0` 到 `255` | `16` |
| **Material** | 贴花使用的材质。 |  |  |

## 示例

衰减角度设置为 0 的贴花示例。衰减越小，意味着物体的包裹性越强。

![Decal component attenuation angle 0 example](/images/user-guide/components/reference/atom/decal-component-ui/decal-component-attenuation-angle-0.png)

衰减角度设为 1 的贴花示例。衰减越大，物体的缠绕越少。

![Decal component attenuation angle 1 example](/images/user-guide/components/reference/atom/decal-component-ui/decal-component-attenuation-angle-1.png)

黑色焦痕贴纸的排序键比橙色泥土贴纸大，因此在最上面。

![Decal component sorting order example](/images/user-guide/components/reference/atom/decal-component-ui/decal-component-sorting-example.png)
