---
linkTitle: 控制台
title: 使用O3DE 控制台
description: 使用 O3DE 编辑器中的控制台更新控制台变量的值或键入命令。
weight: 500
---

在 O3DE 编辑器中，控制台窗口显示所有编辑器命令、进程和输出的运行列表。例如，当你删除一个实体时，控制台会显示操作和输出。你可以使用控制台输入或修改控制台变量（CVAR）。控制台变量是一种可以在 O3DE 编辑器中操作的变量类型。

## 查看控制台窗口

您可以直接在控制台窗口中输入命令，或搜索和编辑控制台变量。

**查看控制台窗口**

1. 在O3DE编辑器中，选择**Tools**, **Console**。

1. 点击左上角的**X**图标，打开**Console Variables**窗口。**Console Variables**窗口列出了所有可用的控制台变量。

    ![View all available console variables in the console window.](/images/shared/console-x-window.png)

1. 输入名称搜索特定的控制台变量。要了解控制台变量的更多信息，请点击名称。

    ![View console variables in the console window.](/images/user-guide/console-variables.png)

## 导出所有控制台命令和变量

您可以检索控制台命令和变量的完整列表，包括它们的说明和分配值。

**导出所有控制台命令和变量的列表**

1. 在**Console**窗口中，输入以下命令：DumpCommandsVars

1. 导航至 O3DE 目录，然后打开 `consolecommandandvars.txt` 文件。
**示例**

       您可以在文件中看到可用的命令和变量。

       ![Open the consolecommandandvars.txt file to see all console commands and variables.](/images/user-guide/console-variables-test-file.png)

1. 你可以指定一个子字符串参数来限制你想要的结果。例如，DumpCommandsVars i\_ 命令会导出所有以 `i_` 开头的命令和变量，如 `i_giveallitems` 和 `i_debug_projectiles`。

## 配置控制台变量

控制台变量也可以在代码中设置或在配置文件中指定。控制台变量按以下顺序执行：

1. 配置文件：
     - 项目目录中的 `game.cfg`
     - 用于游戏系统的 `system_gamesystem.cfg`文件
     - `engine\config\user.cfg` 文件
     - 项目的关卡目录中的`level.cfg`文件

1. 编码

1. 直接输入控制台变量

执行顺序也是覆盖顺序。例如，代码中设置的控制台变量会覆盖配置文件中设置的控制台变量\(`level.cfg`覆盖 `user.cfg`，以此类推\)。在流程图中设置的控制台变量会覆盖代码中设置的任何相同控制台变量。最后，直接输入控制台的控制台变量会覆盖所有其他控制台变量设置。

### 在控制台中配置控制台变量

您可以在控制台中为控制台变量指定值，以便将更改应用到关卡。

**在控制台配置控制台变量**

执行以下操作之一：
      - 在**Console Variables**窗口中，搜索变量名，双击变量值，然后输入想要的值。
      - 在**Console**命令行中，输入控制台变量及其值，然后按**Enter**。例如，输入 r\_DisplayInfo=1 命令可在视口中显示调试信息。

### 在配置文件中配置控制台变量

你可以在配置文件中为控制台变量指定值，如关卡配置文件 \( `level.cfg`\)。

**使用配置文件配置控制台变量**

1. 导航到包含配置文件的目录。例如，如果要配置 `level.cfg` 文件，请导航到项目中的 `Levels\level_name` 目录。

1. 使用文本编辑器编辑或创建文件。

1. 指定控制台变量名称和值。例如：`r_DisplayInfo=1` 在视口中显示调试信息。

1. 保存文件。
