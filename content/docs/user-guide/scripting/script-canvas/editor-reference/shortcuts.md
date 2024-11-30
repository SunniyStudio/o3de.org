---
linktitle: 编辑器快捷键
title: Script Canvas 编辑器快捷键
description: 了解 Open 3D Engine （O3DE） 中 Script Canvas 编辑器的键盘和鼠标快捷键。
weight: 100
---

**Script Canvas 编辑器** 包括以下键盘和鼠标快捷键：

## 键盘快捷键

|组合键 |行动 |描述 |
| --- | --- | --- |
| **Arrow keys** | Scroll graph | 向左、向右、向上或向下滚动图表。您必须通过单击节点或单击画布内的任意位置在图形画布中具有焦点。 |
| **Ctrl+C** | Copy | 将所选节点及其连接复制到剪贴板。 |
| **Ctrl+V** | Paste | 将剪贴板中复制的节点及其连接粘贴到活动图形中。 |
| **Ctrl+D** | Duplicate | 复制活动图形中的选定节点。这相当于使用 **Ctrl+C** 和 **Ctrl+V**。 |
| **Ctrl+Left Arrow** | Select inputs | 选择连接到当前所选节点的输入引脚的所有节点。 |
| **Ctrl+Right Arrow** | Select outputs | 选择连接到当前所选节点的输出引脚的所有节点。 |
| **Ctrl+Up Arrow** | Select connected nodes | 选择连接到当前所选节点的所有节点。 |
| **Esc** | Clear selection | 取消选择任何选定的节点。 |
| **Ctrl+Shift+P** | Screenshot | 创建所有选定节点周围区域的图像，并将其添加到剪贴板。如果未选择节点，则会将整个活动图形的图像添加到剪贴板中。 |
| **Shift+Left Arrow** | Align left | 沿左边缘对齐所有选定节点。 |
| **Shift+Right Arrow** | Align right | 沿右边缘对齐所有选定节点。 |
| **Shift+Up Arrow** | Align top | 沿顶部边缘对齐所有选定节点。|
| **Shift+Down Arrow** | Align bottom | 沿底部边缘对齐所有选定节点。 |
| **Ctrl+Alt+M** | Add comment |使用默认注释预设中的属性添加新注释。有关预设的信息，请参阅 [创建注释预设和编组预设](/docs/user-guide/scripting/script-canvas/editor-reference/nodes/organizing/creating-comment-and-group-presets). 请注意，NVIDIA 的 GeForce Experience 叠加层使用默认设置来打开/关闭干扰此热键的麦克风。 |
| **Ctrl+Alt+O** | Make group | 使用默认节点组预设中的属性创建新组。任何选定的节点都将成为新组的一部分，并包含在组边界内。有关预设的信息，请参阅 [创建注释预设和编组预设](/docs/user-guide/scripting/script-canvas/editor-reference/nodes/organizing/creating-comment-and-group-presets). |
| **Ctrl+Shift+H** | Ungroup | 取消对当前所选组的分组。 |
| **Ctrl+[0-9]** | Create bookmark | 在当前视图中创建书签并将其分配给指定的数字键。 如果选择已分配给书签或启用书签的组的编号，则系统会提示您重新分配现有书签。有关书签的更多信息，请参阅 [在 Script Canvas 中添加书签](/docs/user-guide/scripting/script-canvas/editor-reference/nodes/organizing/adding-bookmarks). 有关将组启用为书签的信息，请参阅 [对节点进行分组](/docs/user-guide/scripting/script-canvas/editor-reference/nodes/organizing/grouping-nodes). |
| **[0-9]** | Jump to bookmark | JUMPS 到与所按下的键关联的书签位置。 |
| **Ctrl+Plus Sign (+)** | Zoom in | 放大图表。 |
| **Ctrl+Minus Sign (-)** | Zoom out | 缩小图表。 |
| **Ctrl+Shift+Up Arrow** | Zoom to selection | 使视图以当前选定的节点为中心。 |
| **Ctrl+Shift+Down Arrow** | Show entire graph | 将整个图形居中到当前显示中。尽可能缩小以显示所有节点。 |
| **Ctrl+Shift+Left Arrow** | Show start of chain | 将视图以没有任何输入连接并通过其输出连接连接到所选节点的节点为中心。 |
| **Ctrl+Shift+Right Arrow** | Show end of chain | 将视图以没有任何输出连接并通过其输入连接连接到所选节点的节点为中心。 |
| **Ctrl+K** then **Ctrl+C** | Disable selected nodes | 禁用所选节点。禁用节点后，其颜色将变为灰色。禁用的节点在 Editor 中仍然可见，但不会在运行时运行。 |
| **Ctrl+K** then **Ctrl+U** | Enable selected nodes | 启用所选节点。 |

{{< note >}}
如果键盘快捷键似乎不适合您，则可能在后台运行的另一个进程已绑定该组合键。
{{< /note >}}

## 鼠标快捷键

以下快捷键使用鼠标或键盘和鼠标。

|用户操作 |结果 |描述 |
| --- | --- | --- |
| **Alt+LMB** on node | 断开并删除单个节点或组 |断开并删除单击的节点或组。 |
| **Alt+LMB** on connection | Delete a connection | 删除从活动图中单击的连接。 |
| **Alt+LMB** on slot | Delete connections | 从活动图形中删除与槽的任何连接。 确保在按 **Alt+左键单击** 之前突出显示要删除的连接。否则，您可以改为删除该节点。   |
| **MMB** on graph tab | Close graph |关闭与您单击的选项卡对应的打开图表。如果图表已更改且尚未保存，系统会提示您先保存它。 |
| **Mouse Wheel Up/Down** | Zoom in/out | 放大或缩小图表。 |
