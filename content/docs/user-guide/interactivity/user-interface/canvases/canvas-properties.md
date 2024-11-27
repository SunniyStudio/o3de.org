---
linkTitle: 配置 Canvas 属性
description: ' 在 Open 3D Engine 的 UI 编辑器中设置 UI 画布的画布属性。 '
title: 配置 Canvas 属性
weight: 400
---

未选择任何元素时，画布属性将显示在 **UI 编辑器** **属性** 窗格中。

![Canvas properties in the Properties pane of the UI Editor](/images/user-guide/interactivity/user-interface/canvases/ui-editor-canvas-properties.png)

## Rendering 属性

以下属性定义画布的呈现方式：

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Draw order** | 此属性的值确定此画布相对于其他画布的绘制顺序。较高的数字会覆盖较低的数字。当版面具有相同的绘制顺序时，O3DE 会按照它们的加载顺序绘制它们。 | Int | `0` |
| **Is pixel aligned** | 如果启用，则通过将元素角的位置舍入到最接近的精确像素，使纹理看起来更清晰。例如，如果元素矩形的角的位置位于 （123.45， 678.90），则它将被舍入为 （123.00， 679.00）。| Boolean | `Enabled` |
| **Is text pixel aligned** | 如果启用，则通过将文本位置舍入到最近的像素来使文本看起来更清晰。当字体缩小时，会出现此规则的例外情况，在这种情况下，文本位置将舍入到最接近的像素。例如，如果文本元素将进行动画处理或移动，则可以考虑禁用此属性。 | Boolean | `Enabled` |
| **Render to texture** | 如果启用，画布将绘制到材质的 **Base Color** 纹理，而不是屏幕。您必须在 **Render Target** 属性中选择要用于纹理的 Attachment Image 资源。有关更多信息，请参阅 [在 3D 世界中放置 UI 画布](/docs/user-guide/interactivity/user-interface/canvases/placing-canvases-3d). | Boolean | `Disabled` |
| **Render Target** | 选择要将此画布渲染到的 Attachment Image 资产。要生成 Attachment Image 资产，请创建一个扩展名为`.attimage`的新源文件。请参阅下面的`.attimage`源文件示例。 <br> <br>*仅当 **Render to texture** 设置为 '`Enabled`' 时，此属性才可用。* | Attachment Image 资产 | None |

### Attachment Image 资产

此示例表示 Attachment Image 的 `.attimage`源文件的内容。

```json
{
    "Type": "JsonSerialization",
    "Version": 1,
    "ClassName": "AttachmentImageAsset",
    "ClassData": {
        "m_imageDescriptor": {
            "BindFlags": [
                "ShaderRead",
                "ShaderWrite",
                "Color"
            ],
            "Size": {
                "Width": 1280,
                "Height": 720
            },
            "Format": 19
        },
        "Name": "$UiCanvasTexture",
        "IsUniqueName": true
    }
}
```

有关 AttachmentImageAsset 类的更多详细信息，请参阅以下参考资料：

- [`AttachmentImageAsset` class API 参考](/docs/api/gems/atom/class_a_z_1_1_r_p_i_1_1_attachment_image_asset.html)
- [`ImageDescriptor` struct API 参考](/docs/api/gems/atom/struct_a_z_1_1_r_h_i_1_1_image_descriptor.html)
- [`DXGI_Format` 枚举定义](https://github.com/o3de/o3de/blob/38261d08007a1ad14135cf790cb855ccca4d8595/Gems/Atom/Asset/ImageProcessingAtom/Code/Source/Processing/AzDXGIFormat.h)

## Input 属性 

以下属性定义画布处理输入的方式：

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Handle positional** | 导致对位置输入（如鼠标移动、鼠标按钮单击和触摸屏输入）的自动响应。当交互式 UI 元素处于活动状态时（例如，带有 **Text input** 组件的元素），键盘输入也会导致自动响应。<br><br> 取消选择此属性的常见原因是画布不需要输入，或者您将游戏配置为处理所有输入并将所选输入传递给 UI 系统。| Boolean | `True` |
| **Consume all input** | 如果启用，则此画布将消耗 *所有* input 事件，而不管 canvas 是否处理特定的 input 事件。例如，如果画布 A 覆盖画布 B，则可能不希望画布 B 在画布 A 遮挡时处理任何输入，因此应在画布 A 上选择此属性。模式对话框是应选择此属性的画布的另一个示例。<br><br> 请注意，每当加载 Canvas 时，如果将其设置为使用所有输入，则它会从任何其他加载的 Canvas 中窃取输入。这包括设置为本身使用所有输入的画布。 | Boolean | `False` |
| **Handle multi-touch** | 如果启用，此属性将允许画布上的元素处理多点触控输入。这对于处理来自基于触摸的屏幕（如移动设备）的输入非常有用。 | Boolean | `True` |
| **Handle navigation** | 如果启用，则会导致对导航输入的自动响应。例如，在 PC 上，按箭头键将焦点从一个交互式 UI 元素移动到下一个交互式 UI 元素，按 Enter** 可激活交互式 UI 元素。建议为放置在游戏世界中的画布取消选择此属性。 | Boolean | `True` |
| **Navigation threshold** | 模拟输入值，例如，来自控制杆的模拟输入值，在处理导航命令之前必须超过该值。根据 UI 的输入敏感度需求调整此值。 | Float: 0.0 - 1.0 | `0.4` |
| **Navigation repeat delay** | 暂停的导航命令开始重复之前的延迟（以毫秒为单位）。根据 UI 的需要调整此值。| Int: 0 to infinity | `300` |
| **Navigation repeat period** | 在初始重复延迟之后，在保留的导航命令再次重复之前的延迟（以毫秒为单位）。根据 UI 的需要调整此值。 <br><br> 例如，如果您有一个菜单列表，您可以在其中按住一个按钮以导航到列表中的下一项，则导航属性设置的使用方式如下： <br><ol><li>按住超过 *导航阈值* 的按钮可导航到下一项。</li><li>继续按住等于 *navigation repeat delay* 的时间以第二次导航。</li><li>继续按住等于 *导航重复周期* 的时间，以第三次导航。此后，当您继续按住该按钮时，您将再次导航，每次经过等于导航周期的时间量。</li></ol> | Int: 0 to infinity | `150` |
| **First focus element** | 选择 **Handle navigation** 时显示。**第一个焦点元素** 指定在首次加载画布且未检测到鼠标时哪个元素获得焦点。有关元素导航的详细信息，请参阅 [First Focus Element](/docs/user-guide/interactivity/user-interface/components/interactive/properties/navigation/components-firstfocus). | UI Element | None |

## Tooltips 属性

以下属性定义画布显示工具提示的方式：

| 属性 | 说明 | 值 | 默认值 |
|-|-|-|-|
| **Tooltip display element** | 控制当用户将鼠标悬停在交互式元素上时游戏显示的元素。从下拉列表中选择一个元素。此列表由当前画布上包含 **TooltipDisplay 组件的元素组成。有关 **工具提示框** 组件的详细信息，请参阅 [工具提示组件](/docs/user-guide/interactivity/user-interface/components/components-tooltips). | 具有 TooltipDisplay 组件的 UI 元素。 | None |

## Editor Settings 属性 

以下属性定义 UI Editor 行为：

| 属性 | 说明 | 值                     | 默认值 |
|-|-|-----------------------|-|
| **Snap distance** | 在工具栏中选择 **对齐网格** 时网格上位置之间的距离。 | Float: 1.0 to infinity | `10.0` |
| **Snap rotation** | 使用旋转 Gizmo 旋转视区中的元素时，每个旋转步骤之间的度数。必须在工具栏中启用 **对齐网格**。 | Float: 1.0 to 359.0   | `10.0` |
| **Guide color** | 此画布上参考线的颜色。有关在 UI Editor 中使用参考线的更多信息，请参阅 [标尺和参考线](/docs/user-guide/interactivity/user-interface/editor/rulers-guides). | 每通道 8 位颜色： 0 - 255    | `61,255,64`
| **Texture atlases** | 此画布加载的纹理图集。在某些情况下，使用纹理贴图集可以减少绘制调用的数量，从而提高 UI 的性能。有关 *纹理集* 的更多信息，请参阅 [使用纹理集](/docs/user-guide/interactivity/user-interface/canvases/texture-atlases). |  `.texatlas`数组 | None |
