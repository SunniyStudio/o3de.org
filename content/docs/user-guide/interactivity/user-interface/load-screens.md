---
linkTitle: Creating Load Screens
description: ' Create game and level loading screens with Open 3D Engine''s UI Editor, and then add the canvas file paths to settings in the game.cfg and level.cfg files. '
title: Creating Game and Level Load Screens
weight: 300
---

您可以使用 **UI 编辑器** 创建游戏或关卡加载屏幕。游戏加载屏幕在游戏加载时显示。关卡加载屏幕在关卡加载时显示。您可以为每个级别创建和定义加载屏幕。

在使用 RAD Game Tools 的 Bink 视频文件时，加载屏幕支持的不仅仅是 UI 画布。您可以指定 UI 画布或 Bink 视频文件的路径。此外，Bink 视频支持多线程加载屏幕，使加载屏幕能够在关卡加载时顺利渲染。

要定义游戏和关卡加载屏幕，您需要将文件路径设置为`game.cfg`和`level.cfg`中的参数。

## 定义游戏加载屏幕

要定义游戏加载屏幕，请首先执行以下操作之一：
+ 在 **UI 编辑器** 中创建加载屏幕画布，并将其保存在您的 O3DE 项目目录中。
+ 将 Bink 视频文件保存在您的 O3DE 项目目录中。

然后，在 `game.cfg` 中添加或修改参数，它位于项目目录的根目录下。

**若要将游戏加载屏幕参数添加到 `game.cfg`**，请执行以下操作

1. 在文本编辑器中，打开项目目录根目录下的 `game.cfg`。

1. 在 `game.cfg` 中添加或修改以下参数：
   + `game_load_screen_uicanvas_path` - 相对于项目路径的 `.uicanvas` 游戏加载屏幕文件的文件路径。如果您将 UI 画布用于加载屏幕，请使用此选项。
   + `game_load_screen_bink_path` - 相对于项目路径的 .bk2 游戏加载屏幕文件的文件路径。如果你将 Bink 视频用于加载屏幕，请使用此选项。
   + `game_load_screen_minimum_time` - 显示游戏加载屏幕的最短时间（以秒为单位）。防止短负载使负载屏幕闪烁的重要内容。0 表示没有最小值。默认值为 0。
   + `game_load_screen_sequence_to_auto_play` - 要在加载时播放的游戏加载屏幕动画序列的名称。
   + `game_load_screen_sequence_fix_fps` - 游戏加载屏幕动画在加载时播放的固定帧速率。默认值为 '`60`'。要忽略此设置并使用实时增量，请指定 '`-1`'。
   + `ly_EnableLoadingThread` - 实验的。设置为 1 可启用完全线程加载，其中加载屏幕绘制在不加载数据的线程上。目前仅支持 Bink 加载屏幕。

以下是 `game.cfg` 文件中这些参数的示例：

```
game_load_screen_uicanvas_path="UI\Canvases\UiAnimMultiSequence.uicanvas"
game_load_screen_minimum_time=5
game_load_screen_sequence_to_auto_play="TopRowMove"
game_load_screen_sequence_fix_fps=4.0
```

```
game_load_screen_bink_path="Videos\GameLoadingScreen.bk2"
game_load_screen_minimum_time=5
```

## 定义关卡加载界面

要定义关卡加载屏幕，请首先执行以下操作之一：
+ 在 **UI 编辑器** 中创建加载屏幕画布，并将其保存在您的关卡目录中。
+ 将 Bink 视频文件保存在您的关卡目录中。

然后，您在 `level.cfg` 中添加或修改参数，它位于 level 目录的根目录。

**将关卡加载界面参数添加到 `level.cfg`**

1. 在文本编辑器中，打开关卡目录根目录下的 `level.cfg`。

1. 在 `level.cfg` 中添加或修改以下参数：
   + `level_load_screen_uicanvas_path` - 相对于项目路径的 `.uicanvas` 关卡加载屏幕文件的文件路径。如果您将 UI 画布用于加载屏幕，请使用此选项。
   + `level_load_screen_bink_path` - 相对于工程路径的 .bk2 级别加载屏幕文件的文件路径。如果你将 Bink 视频用于加载屏幕，请使用此选项。
   + `level_load_screen_minimum_time` - 显示水平负载屏幕的最短时间（以秒为单位）。防止短负载使负载屏幕闪烁的重要内容。0 表示没有最小值。默认值为 0。
   + `level_load_screen_sequence_to_auto_play` - 要在加载时播放的关卡加载屏幕动画序列的名称。
   + `level_load_screen_sequence_fix_fps` - 关卡加载屏幕动画在加载时播放的固定帧速率。默认值为 '`60`'。要忽略此设置并使用实时增量，请指定 '`-1`'。
   + `ly_EnableLoadingThread` - 实验的。设置为 1 可启用完全线程加载，其中加载屏幕绘制在不加载数据的线程上。目前仅支持 Bink 加载屏幕。

以下是`level.cfg`文件中这些参数的示例：

```
level_load_screen_uicanvas_path="Levels\StarterGame\UiAnimMultiSequence.uicanvas"
level_load_screen_minimum_time=3
level_load_screen_sequence_to_auto_play="TopRowMove"
level_load_screen_sequence_fix_fps=4.0
```

```
level_load_screen_bink_path="Videos\IntroLevelLoadingScreen.bk2"
level_load_screen_minimum_time=3
```
