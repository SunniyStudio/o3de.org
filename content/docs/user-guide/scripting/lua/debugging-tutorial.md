---
linkTitle: 使用 Lua 编辑器进行调试
title: 使用 Lua 编辑器进行调试
description: 使用 Lua 编辑器在 Open 3D Engine （O3DE） 中调试 Lua 脚本。
toc: true
weight: 900
---

**Open 3D Engine （O3DE）** **Lua Editor** （Lua IDE） 提供了一个直观的集成开发环境 （IDE），让您在创建或扩展游戏时可以轻松编写、调试和编辑 Lua 脚本。Lua Editor 是一个独立的应用程序，但您可以直接在 **O3DE Editor** 的 Tools 菜单中打开它。

本教程介绍如何使用 O3DE Lua 编辑器对示例脚本执行调试操作。

## 将示例 Lua 脚本添加到实体

1. 从 **Tools** 菜单打开 Lua Editor。 

1. 从 **File** 菜单中选择 **New** 以创建新的 Lua 脚本。

1. 将以下代码 **Copy** 并 **Paste** 到新脚本中。

    ```lua
    -- ConstantRotation.lua
    
    local ConstantRotation =
    {
        Properties =
            {
            Rotation = { default = Vector3(0, 0, 90), description = "Constant rotation (in degrees) to apply over time." },
            },
    }

    function ConstantRotation:OnActivate()
        self.rotationRadians = self.Properties.Rotation;
        self.rotationRadians.x = Math.DegToRad(self.rotationRadians.x);
        self.rotationRadians.y = Math.DegToRad(self.rotationRadians.y);
        self.rotationRadians.z = Math.DegToRad(self.rotationRadians.z);
        self.tickBusHandler = TickBus.Connect(self)
    end

    function ConstantRotation:OnTick(deltaTime, timePoint)
        TransformBus.Event.RotateAroundLocalX(self.entityId, self.rotationRadians.x * deltaTime);
        TransformBus.Event.RotateAroundLocalY(self.entityId, self.rotationRadians.y * deltaTime);
        TransformBus.Event.RotateAroundLocalZ(self.entityId, self.rotationRadians.z * deltaTime);
    end

    function ConstantRotation:OnDeactivate()
        self.tickBusHandler:Disconnect()
    end

    return ConstantRotation
    ```
    
1. 将脚本 **Save** 为项目目录中的`ConstantRotation.lua`。

1. **关闭** Lua 编辑器。

1. 在 **Entity Outliner** 中，选择要向其添加 **Lua Script** 组件的实体。

1. 在 **Entity Inspector** 中，选择 **Add Component**，然后选择 **Scripting**、**Lua Script**。

1. 在 **Entity Inspector** 中，找到 **Lua Script** 组件，然后单击 {{< icon "browse-edit-select-files.svg" >}} 以打开文件浏览器。

1. 在 **Pick Lua Script** 窗口中，选择 `ConstantRotation.lua`，然后选择 **OK**。

1. 在 **Lua Script** 组件中，单击 {{< icon "open-in-internal-app.svg" >}} 以启动 Lua 编辑器。

    ![Launch Lua Editor from Lua Script component in O3DE Editor](/images/user-guide/scripting/lua/lua-component-open-in-lua-editor.png)
    
## 连接到 O3DE 编辑器

[**Remote Tools Gem**](/docs/user-guide/gems/reference/debug/remote-tools) 促进 O3DE 应用程序之间的本地连接。 

{{< note >}}
远程工具 Gem 必须 [在您的项目中启用](/docs/user-guide/project-config/add-remove-gems/#enabling-or-disabling-gems) 才能进行调试。

**远程工具 Gem** 行为在发布版本中被禁用。
{{< /note >}}

Remote Tools Gem 会在 Lua Editor 启动时自动启动，并且必须在后台运行，Lua Editor 才能找到可以连接的目标。 由于调试功能是通过网络套接字启用的，因此必须先将 Lua Editor 连接到运行脚本的目标，然后才能进行调试。在本教程中，您将连接到 O3DE Editor。

1. 在 Lua Editor 工具栏中，选择 **Target: None**，然后选择 **Editor(*ID*)** 以连接到 O3DE Editor。

    ![Target selector](/images/user-guide/scripting/lua/lua-editor-debugger-target-editor.png)
    
    {{< note >}}
您可能需要展开 Lua Editor 窗口，才能看到 Lua Editor 工具栏上接下来几个步骤的按钮。
{{< /note >}}

1. 在 Lua Editor 工具栏中，将 **Context** 设置保留为 **Default** 作为调试上下文。默认设置适用于调试组件实体脚本，例如本教程中的脚本。

    ![Context selector](/images/user-guide/scripting/lua/lua-editor-debugger-context-choose.png)

1. **Debugging** 图标变为绿色，表示 Lua Editor 和 O3DE Editor 已连接：

    ![Lua Editor connected to O3DE Editor](/images/user-guide/scripting/lua/lua-editor-debugger-connected-icon.png)

    单击 **Class Reference** 中的 **Classes** 以显示可用的 Lua 库。您可以对 事件总线 **EBuses**和 全局变量 **Globals**执行相同的操作。

    ![Classes Reference](/images/user-guide/scripting/lua/lua-editor-debugger-class-reference-pane.png)
    
    ![Classes](/images/user-guide/scripting/lua/lua-editor-debugger-class-reference-pane-open.png)
    
    {{< note >}}
类引用功能仅对默认上下文和组件实体脚本处于活动状态。
{{< /note >}}

## 设置断点

连接后，您可以通过设置断点来暂停脚本。

1. 在 Lua Editor 工具栏中，单击 **Breakpoints** 图标 ![Breakpoints Icon](/images/user-guide/scripting/lua/lua-editor-debugger-breakpoints-icon.png) 以显示 **Breakpoints** 窗口。

1. 在 Lua 编辑器中，点击 `constantrotation.lua` 脚本中的一个或多个行号来设置一个或多个断点。添加断点时，每个断点的行号和脚本路径将添加到 **Breakpoints** 窗口中。

1. 在 O3DE 编辑器中，按 **Ctrl+G** 运行游戏，或单击视区底部的 **Simulate** 图标以启用游戏模拟并运行脚本。Lua Editor 将打开，并在遇到的第一个断点处停止一个黄色标记。

    ![Debugger stopped on breakpoint](/images/user-guide/scripting/lua/lua-editor-debugger-stopped-on-breakpoint.png)

    当执行在断点处停止时，**Lua Locals**、**Stack** 和 **Watched Variables** 窗格中将提供更多信息。

1. 点击 **Stack** 图标 ![Stack Icon](/images/user-guide/scripting/lua/lua-editor-debugger-stack-icon.png) 以显示 **Stack** 窗口。

1. 点击 **Lua Locals** 图标 ![Lua Locals Icon](/images/user-guide/scripting/lua/lua-editor-debugger-lua-locals-icon.png) 以显示局部 Lua 变量。

1. 点击 **Watched Variables** 图标 ![Watched Variables Icon](/images/user-guide/scripting/lua/lua-editor-debugger-watched-variables-icon.png) 以打开 **Watched Variables** 窗口，您可以在其中指定要监视的变量。

1. 按 F11 键几次以逐步执行代码。请注意 **Stack**, **Lua Locals**, 和 **Watched Variables** 窗口的内容是如何变化的。

    {{< tip >}}
为了更加方便，您可以浮动或停靠这些窗口。
    {{< /tip >}}

1. 要从调试中分离，请单击**Debugging**.

    ![Click to detach from debugging](/images/user-guide/scripting/lua/lua-editor-debugger-detach-icon.png)

1. 在 O3DE 编辑器中，按 **Esc** 停止游戏。

## 调试时可用的选项

下表总结了调试时可用的常用选项。

| **图标** | **动作** | **键盘快捷键** | **说明** |
| --- | --- | --- | --- |
| ![Run in Editor Icon](/images/user-guide/scripting/lua/lua-editor-debugger-run-in-editor.png) | Run in Editor | **Alt+F5** | 在 O3DE 编辑器中运行。 |
| ![Run on Target Icon](/images/user-guide/scripting/lua/lua-editor-debugger-run-on-target.png) | Run on Target | **Ctrl+F5** | 将脚本发送到连接的目标并运行它。 |
| ![Run/Continue Icon](/images/user-guide/scripting/lua/lua-editor-debugger-run-continue.png) | Run/Continue | **F5** | 运行或继续运行当前脚本。 |
| ![Step Into Icon](/images/user-guide/scripting/lua/lua-editor-debugger-step-into.png) | Step Into | **F11** | 单步执行当前行上调用的函数。 |
| ![Step Out Icon](/images/user-guide/scripting/lua/lua-editor-debugger-step-out.png) | Step Out | **Shift+F11** | 跳出被调用的函数。 |
| ![Step Over Icon](/images/user-guide/scripting/lua/lua-editor-debugger-step-over.png) | Step Over | **F10** | 单步执行在当前行上调用的函数。 |
| ![Toggle Breakpoint Icon](/images/user-guide/scripting/lua/lua-editor-debugger-toggle-breakpoint.png) | Toggle Breakpoint | **F9** | 启用或禁用当前行上的断点。 |
