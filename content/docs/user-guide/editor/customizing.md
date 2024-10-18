---
linkTitle: 自定义编辑器布局
title: 自定义编辑器布局
description: 通过停靠窗口和工具栏、自定义工具栏和菜单以及更新编辑器设置和 Open 3D Engine 的全局偏好设置来定制 O3DE 编辑器。
weight: 150
---

您可以通过调整窗口和工具的停靠方式、自定义工具栏和菜单的显示方式以及更新全局编辑器设置来定制您的工作区。

{{< note >}}
要更改工具栏图标的大小设置，请打开 `editor.cfg` 文件，并为 `ed_toolbarIconSize` 参数输入一个值。默认情况下，工具栏图标大小设置为 `0`（32 像素）。
{{< /note >}}

## 停靠窗口和工具栏

在界面元素或编辑器边缘拖动窗口或工具栏时，会出现停靠目标，显示可以停靠的位置。这些目标会出现在窗格的顶部、底部、左侧和右侧象限。你可以相对于任何打开的窗格停靠窗口，无论该窗格是已经停靠、作为标签浮动，还是被拆分成一列或一行。

- 要分割一行或一列，可将窗口或工具栏拖放到停靠目标上。

    ![Split the column into two panes](/images/user-guide/editor-customize-splitting-column.gif)

- 要将窗口或工具栏停靠为标签页，可将其放置在窗格中间的停靠目标上。

    ![Dock the window into a tabbed view](/images/user-guide/editor-customize-docking-tabs.gif)

- 要将窗口或工具栏停靠到编辑器窗口，可将其放置到外部停靠目标上。这会在现有栏旁边创建一个新栏。

    ![Dock the window to the editor window and create a new column](/images/user-guide/editor-customize-docking-to-editor-window.gif)

- 要解除窗口或工具栏的停靠，请拖动标题栏并将选择窗口移开。避开停靠目标，防止意外重新停靠窗口。为防止意外停靠，在停靠目标变为活动之前会出现短暂的延迟。您也可以通过右键单击标题栏并选择**Undock**来解除窗口的停靠。

    ![Undock the window from the tabbed view](/images/user-guide/editor-customize-undocking-tabs.gif)

- 要防止窗口停靠，请在移动窗口时按住 **Ctrl**。

- 要将窗口固定到位，请将窗口靠近固定窗口。捕捉对窗格的顶部、底部、左侧和右侧边框都有效。

## 自定义工具栏和菜单

您还可以对工具栏和菜单进行个性化设置。

1.  右键单击顶部工具栏，选择**Customize**。

1. 在**Customize**窗口的**Toolbars**中，创建、重命名和删除任何自定义工具栏和菜单，或将其重置为默认设置。

    ![Toolbars tab in the Customize window](/images/user-guide/editor-customizing-toolbars.png)

1. 在**Commands**选项卡中，将菜单命令拖放到任何菜单类别中。

    ![Commands tab in the Customize window](/images/user-guide/editor-customizing-commands.png)

## 更改首选项

您可以更改默认设置，自定义编辑器的外观和功能。

1. 在O3DE编辑器中，选择**Edit**, **Editor Settings**, **Global Preferences**。

    ![General Settings tab in the Preferences window](/images/user-guide/editor-preferences.png)

### General Settings 

您可以更改 O3DE 编辑器的常规设置和文件设置。

**General Settings**

| 参数 | 说明 |
| --- | --- |
| Show Geometry Preview Panel |  显示所选对象的预览窗口。  |
| Enable Source Control |  启用Perforce版本控制。 |
| Clear Console at game startup | 如果启用，进入游戏模式后，控制台中的文字将被清除。 | 
| Console Background |  更改控制台的背景颜色模式。默认值： `Dark` |
| Auto-load last level at startup |  加载上次加载的关卡。  |
| Show Time in Console |  在控制台窗口中显示时间。 |
| Toolbar Icon Size |  调整工具栏图标大小。 默认值： `16` (32 像素)。 |
| Stylus Mode |  为平板电脑和其他指向设备启用手写笔模式。 |
| Enable Scene Inspector (EXPERIMENTAL) | 启用检查 .fbx 文件等文件中场景的选项。 |
| Restore Viewport Camera on Game Mode Exit |  退出游戏模式时，将摄像机恢复到原始变换。 |

**Messaging**

| 参数 | 说明 |
| --- | --- |
| Show Welcome to O3DE at startup |  在启动时显示 **欢迎使用 O3DE** 对话框。  |
| Show Error: Circular dependency |  当向目标添加切片实例会产生循环资产依赖时，会显示错误信息。所有其他对切片的重写都将被保存。  |

**Undo**

| 参数 | 说明 |
| --- | --- |
| Undo Levels |  指定撤消一个关卡的最大次数。 默认值： `50`  |
| Undo Slice Override Saves | 允许您撤销对切片的覆盖保存。  |

**Selection**

| 参数 | 说明 |
| --- | --- |
| Deep selection range |  |
| Stick duplicate to cursor |  |

**Vertex Snapping**

| 参数 | 说明 |
| --- | --- |
| Vertex Cube Size |  调整顶点立方体大小。  |
| Render Penetrated Boundboxes |  渲染被穿透的绑定盒。  |

**Metrics**

| 参数 | 说明 |
| --- | --- |
| Enable Metrics Tracking |  启用指标跟踪。 |

**Slices**

| 参数 | 说明 |
| --- | --- |
| New Slices Dynamic By Default | 创建切片时，切片会被设置为动态。 |

**Files**

| 参数 | 说明 |
| --- | --- |
| Backup on Save |  保存时创建备份文件。  |
| Maximum Save Backups |  指定保存备份的最大数量。  |
| Standard Temporary Directory |  指定要使用的默认临时目录。默认 = `[root]\Temp`  |
| Auto Save Camera Tag Points |  保存修改后的摄像机标记点。  |
| Scripts Editor |  指定脚本使用的文本编辑器。  |
| Shaders Editor |  指定着色器使用的文本编辑器。  |
| BSpace Editor |  指定用于混合空间的文本编辑器。  |
| Texture Editor |  指定用于纹理的程序。 |
| Animation Editor |  指定用于动画的程序。  |
| Enable |  启用自动备份。 |
| Time Interval |  指定自动备份的频率（分钟）。  |
| Maximum Backups |  指定自动备份的最大次数。 |
| Remind Time |  指定自动备份提醒的频率（分钟）。  |

### Viewport 

您可以更改视口的默认设置。

**General**

| 参数 | 说明 |
| --- | --- |
| Synchronize 2D Viewports |  实现 2D 视口的同步移动和相互对应。  |
| Perspective View FOV |  指定视口的视野范围。  |
| Perspective View Aspect Ratio |  指定视口宽高比的长度，其中 height = `1`.  |
| Enable Right-Click Context Menu |  启用右键单击视口中出现的上下文菜单。  |
| Show 4:3 Aspect Ratio Frame |  显示 4:3 宽高比，以显示游戏模式下的可见内容。  |
| Highlight Selected Geometry |  高亮显示所选几何体。  |
| Highlight Selected Vegetation |  高亮显示所选植被。 |
| Highlight Geometry on Mouse Over |  悬停时高亮显示几何图形。  |
| Hide Cursor when Captured |  显示或隐藏视口中的鼠标指针。 |
| Drag Square Size |  指定拖动方格的大小，以防止选择时意外移动对象。 |
| Display Object Links |  在视口中显示实体链接。  |
| Display Animation Tracks |  显示轨迹视图中任何对象的动画路径。一行 = 一帧。  |
| Always Show Radii |  显示某些实体的影响范围（半径）。  |
| Always Show Prefab Bounds |  显示Prefab边界助手。  |
| Always Show Prefab Objects |  显示Prefab对象辅助工具。  |
| Show Bounding Boxes |  显示每个对象周围的包围盒。  |
| Always Draw Entity Labels |  显示实体名称。  |
| Always Show Trigger Bounds |  显示触发器边界帮助器。  |
| Show Object Icons |  显示对象图标。  |
| Scale Object Icons with Distance |  根据距离缩放对象图标。  |
| Show Helpers of Frozen Objects |  显示冻结对象辅助图标。  |
| Fill Selected Shapes |  突出显示所选形状的内部区域。  |
| Show Snapping Grid Guide |  在视口中显示网格。 |
| Display Dimension Figures |  显示所选资产的测量尺寸；必须启用辅助工具。  |
| Swap X/Y Axis |  反转 x 轴和 y 轴。  |
| Map Texture Resolution |  指定显示地图的分辨率。  |
| Enabled |  显示对象名称。  |
| Distance |  指定文本标签的可见距离。  |
| Prefab Bounding Box |  指定Prefab边界框的颜色。 |
| Group Bounding Box |  指定组边界框的颜色。 |
| Entity Bounding Box |  指定实体边界框的颜色。 |
| Bounding Box Highlight Alpha |  指定要添加到边界框中的高亮 Alpha 值。  |
| Geometry Color |  指定几何体颜色。  |
| Solid Brush Geometry Color |  指定实体画笔几何体的颜色。  |
| Geometry Highlight Alpha |  指定要添加到几何体上的高光 Alpha 值。  |
| Child Geometry Highlight Alpha |  指定要添加到子几何体上的高光 Alpha 值。 |

**Movement**

| 参数 | 说明 |
| --- | --- |
| Camera Movement Speed |  指定视口中所有移动的速度。  |
| Camera Rotation Speed |  指定控制视口摄像机时的移动速度。  |
| Fast Movement Scale |  指定摄像机速度的乘数；例如，数值为 2 时，摄像机的移动速度会加倍。  |
| Wheel Zoom Speed |  指定使用鼠标滚轮时相机变焦的速度。  |
| Invert Y Axis |  当按住鼠标右键并向上或向下移动鼠标时，反转摄像机在 Y 轴上的移动方向。  |
| Invert Pan |  当按住鼠标中键并向左或向右移动鼠标时，反转摄像头的移动方向。  |

**Gizmos**

| 参数 | 说明 |
| --- | --- |
| Size |  指定 xyz 轴小工具的大小。 |
| Text Labels |  显示 xyz 轴标签。  |
| Max Count |  指定屏幕上可同时显示的 xyz 轴小工具的最大数量。 |
| Helpers Scale |  指定屏幕辅助工具（包括 AIAnchors、Tagpoints 和 CoverSurfaces）的大小。  |
| Tagpoint Scale Multiplier |  指定标记点辅助球的缩放比例和基本辅助缩放比例值。  |
| Ruler Sphere Scale |  指定使用 **Ruler** 工具时定位球大小的比例。 |
| Ruler Sphere Transparency |  指定使用 **Ruler** 工具时定位器球体的透明度级别。  |

**Debug**

| 参数 | 说明 |
| --- | --- |
| Show Mesh Statistics |  显示可选对象的细节信息，如tris 和verts。  |
| Warning Icons Draw Distance |  指定在视口中显示警告图标的距离。  |
| Show Scale Warnings |  显示已缩放对象的图标和警告文字。  |
| Show Rotation Warnings |  显示旋转过的对象的图标和警告文字。  |

### Experimental Features

您可以更改实验功能的默认设置，例如全面照明。

**Lighting Settings**

| 参数 | 说明 |
| --- | --- |
| Total Illumination |  启用全面照明功能。  |
