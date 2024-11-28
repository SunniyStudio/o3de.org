---
linkTitle: 配置字体属性
description: 在 Open 3D Engine 中配置游戏 UI 的字体属性，例如资源路径和效果。
title: 配置字体属性
weight: 400
---

您可以通过配置影响字体外观和用法的各种属性来定义 UI 字体的外观。

您可以在 `.font` 文件（一个 XML 文件）中定义字体的属性。以下示例显示了默认的 UI 字体 XML 文件，该文件位于 `Engine/Fonts/default-ui.font`.

```
<fontshader>
    <font path="Vera.ttf" fontsize="32"/>
    <effect name="default">
        <pass>
        </pass>
    </effect>
    <effect name="drop_shadow">
        <pass>
        </pass>
        <pass>
            <color r="0" g="0" b="0" a="1"/>
            <pos x="1" y="1"/>
        </pass>
</effect>
</fontshader>
```

默认 UI 字体 XML 使用 Vera 字体。它定义了一个字体纹理，该纹理可以容纳 128 个唯一字符或 32x32 像素的字形。该字体包括两种表示形式，分别使用效果标签 `default` 和 `drop_shadow` 定义。对于 `default` 效果，字体按原样呈现。对于`drop_shadow`效果，字体首先按原样呈现。第二个渲染过程生成黑色字体，与第一个过程相比偏移 1 个像素。这将创建字符的基本阴影效果。

{{< note >}}
字体可以是字体系列的一部分\(使用 `.fontfamily` 扩展名\)，但您可以使用不属于字体系列的独立字体。有关字体系列的更多信息，请参阅 [创建字体系列](/docs/user-guide/interactivity/user-interface/fonts//create-font-families).
{{< /note >}}

使用以下标签、属性和值来定义字体的关键功能。

| Tag | 说明 | 属性 |
| --- | --- | --- |
| **font** | 包含定义资源路径、大小和其他字体质量的关键属性。 | See [Font Tag Attributes](#font-tag-attributes). |
| **effect** | 充当父标签以传递 children 标签。对构成效果的 pass 标签进行分组。 | **name** -- Name of the effect. |
| **effectfile** | 指定包含效果标签的 XML 文件的路径。 | **path** -- 指定包含效果标记的 XML 文件的路径的字符串。 |
| **pass** |  **effect** 标签的子标签。您可以添加多个 **pass** 标签作为单个 **effect** 标签的子项。使用影响文本渲染的各种参数定义文本的渲染过程。**pass** 标签可以相互叠加，从而使效果具有独特的外观。   | 没有属性。父级 定义文本效果的以下子标签： <li>**color**</li><li> **pos** or **offset**</li><li> **blend** or **blending**</li> |
| **color** | 一种文本效果，它是 pass 标签的子标签。定义文本颜色。  |  使用以下属性通过浮点值定义效果的强度： <li>Minimum: `0.0`</li><li> Maximum: `1.0f`</li><li> **r** -- Red </li><li> **g** -- Green</li><li> **b** -- Blue</li><li> **a** -- Alpha (`0` is transparent. `1.0f` is opaque)</li>|
| **pos** 或 **offset** | 一种文本效果，它是 pass 标签的子标签。设置文本的位置。 |  使用以下属性设置具有整数值的文本位置： <li> **x** -- 相对于文本位置沿 X 轴偏移文本</li><li> **y** -- 相对于文本位置沿 Y 轴偏移文本</li> |
| **blend** 或 **blending** | 一种文本效果，它是 pass 标签的子标签。定义文本的 Alpha 混合行为。 | 使用以下属性定义文本的 Alpha 混合行为： <li>**src** -- 文本的 Alpha 混合的源颜色值</li><li> **dst** -- 文本的 Alpha 混合的目标颜色值</li><li> **type** -- 具有由值确定的预配置设置的 Alpha 混合行为</li>对 **src** 和 **dst** 属性使用以下值： <li>`zero`</li><li> `one`</li><li> `srcalpha` 或 `src_alpha`</li><li> `invsrcalpha` 或 `inv_src_alpha`</li><li> `dstalpha` or `dst_alpha`</li><li> `invdstalpha` 或 `inv_dst_alpha`</li><li> `dstcolor` 或 `dst_color`</li><li> `srccolor` 或 `src_color`</li><li> `invdstcolor` 或 `inv_dst_color`</li><li> `invsrccolor` 或 `inv_src_color`</li> 对 **type** 属性使用以下值：<li>`modulate-src_alpha` 和 `inv_src_alpha`（1 个减去源 Alpha）混合</li><li>`additive-src_alpha` 和 `one` （用于目标）混合</li>  |

## Font Tag Attributes 
**Font** 标签属性定义字体的关键属性，例如用于显示字体的 TTF/OTF 资产的路径，以及影响字体呈现质量的其他属性。

{{< note >}}
这些属性中的大多数对字体渲染质量有直接影响。有关详细信息，请参阅 [配置字体呈现质量](/docs/user-guide/interactivity/user-interface/fonts/rendering).
{{< /note >}}

| 属性 | 示例 | 说明 |
| --- | --- | --- |
| **path** |  <pre><font path="Vera.ttf" ... /></pre>  | <li>Type: `String`</li><li>TTF 或 OTF 字体文件资产的路径。</li>  |
| **fontsize** |  <pre>< ... fontsize="32"/></pre>  | <li>Type: `Integer`</li><li>以像素为单位定义用于在字体纹理中存储字形（字符）的槽的正方形大小。为了获得像素完美的渲染质量，此大小应与渲染字体时指定的大小匹配。</li>   |
| **w** |  <pre>< ... w="512" h="256" /></pre>  | <li>Type: `Integer`</li><li>以像素为单位定义字体纹理的宽度。</li> |
| **h** |  <pre>< ... w="512" h="256" /></pre>  | <li>Type: `Integer`</li><li>以像素为单位定义字体纹理的宽度。</li> |
| **widthslots** |  <pre> ... widthslots="8" heightslots="8" /></pre>  | <li>Type: `Integer`</li><li>Default: `16`</li><li>定义沿字体纹理的 X 轴的字符或字形槽的数量。</li><li>在此示例中，字体纹理为 `512x512`. **widthslots** 和 **heightslots** 被设置为 `8`. 这将为每个字符提供 `64x64` 的空间。</li> |
| **heightslots** |  <pre>< ... widthslots="8" heightslots="8" /></pre>  | <li>Type: `Integer`</li><li>Default: `8`</li><li>定义沿字体纹理的 y 轴的字符或字形槽的数量。</li> |
| **sizeratio** |  <pre>< ... sizeratio="0.6" /></pre>  | <li>Type: `Float`</li><li>Default: `0.8`</li><li>在渲染到字体纹理中时，对字符或字形应用均匀缩放。</li><li>默认缩放通常是理想的。您可以针对比例不寻常的字体（如非常长的字体或宽字体）调整此值。</li> |
| **sizebehavior** |  <pre>< ... sizebehavior="rerender" /></pre>  | <li>Type: `String`</li><li>Value: `rerender`</li><li>以新大小再次渲染文本。当文本的渲染大小与字体纹理的字形槽大小不同时，提高字体外观质量。</li><li>当文本大小更改时，将应用简单的转换缩放。当文本变大或变小时，质量会明显下降。</li><li>重新呈现的文本可以提高该质量，具体取决于字体。由于重新呈现需要时间，因此在某些情况下并不理想，例如对于经常更改大小的动画文本。</li> |
| **hintbehavior** |  <pre>< ... hintbehavior="nohinting" /></pre>  | <li>Type: `String`</li><li>配置字体的提示属性。可能的值：</li><li> `default` -- 字体提供的提示行为。您还可以省略 **hintbehavior** 标签以使用默认提示。</li><li>`nohinting` -- 禁用提示。</li><li>`autohint` -- 从字体程序派生的提示行为。</li> |
| **hintstyle** |  <pre>< ... hintstyle="light" /></pre>  | <li>Type: `String`</li><li>配置提示文本的外观。可能的值：</li><li>`normal` -- 字体提供的外观。您还可以省略 **hintsytle** 标签以使用默认提示。</li><li>`light` -- 外观更模糊，可以更准确地表示字体中字形的形状。</li> |
| **smooth** |  <pre>< ... smooth="blur" smooth_amount="3" /></pre>  | <li>Type: `String`</li><li>配置应用于字体的平滑处理。应用的平滑量由 **smooth_amount** 属性定义。可能的值：</li><li>`blur` -- 将简单的毛刺效果应用于字体纹理中存储的字符或字形。使用 **smooth_amount** 设置用于模糊处理的迭代次数。</li><li>`supersample` -- 通过 **smooth_amount** 对字体纹理中的字符或字形进行超级采样：`1` 为 2x, 和 `2` 为 4x.</li><li>`none` -- 违约。未应用平滑。</li> |
| **smooth\_amount** |  <pre>< ... smooth="supersample" smooth_amount="1" /></pre>  | <li>Type: `Integer`</li><li>定义应用于字体的平滑量。应用的平滑类型由 **smooth** 属性定义。</li> |
