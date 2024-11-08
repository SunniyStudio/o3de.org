---
linkTitle: Immediate Mode GUI (ImGui)
title: Immediate Mode GUI (ImGui) Gem
description: 即时模式 GUI （ImGui） Gem 提供了 3rdParty 库 Dear ImGui，可用于创建运行时即时模式叠加层，以便在 Open 3D Engine （O3DE） 项目中进行调试和分析。
toc: true
---

即时模式 GUI （ImGui） Gem 提供第三方库 [Dear ImGui](https://github.com/ocornut/imgui)，该库可用于创建运行时即时模式菜单和覆盖层，以便在 Open 3D Engine （O3DE） 中调试和分析信息。您可以使用它来访问现有的 O3DE 工具，或为您的组件或其他游戏代码设置您自己的 ImGui 菜单。

ImGui 的一大优点是，当 ImGui 游戏覆盖层不可见时，几乎没有开销，这是默认状态。它在使用时也非常轻便。

## 访问 ImGui 菜单

ImGui 可以通过在编辑器、游戏或服务器启动器中按 **~** 键来访问。

![ImGui Menus in Viewport](/images/user-guide/gems/reference/debug/imgui_menus.png)

> 如果渲染处于活动状态，则服务器启动器支持 ImGui。ImGui 要求 'rhi' cvar 不能为 null。

您可以使用键盘、鼠标或控制器来浏览 ImGui 菜单。

### 键盘导航

对于键盘，使用箭头键导航菜单，使用 **空格键** 激活或选择一个选项。

### 游戏控制器导航

您可以使用 `imgui_EnableController` [CVAR](#cvars) 来启用游戏控制器输入支持，或使用`imgui_EnableControllerMouse` [CVAR](#cvars)  来模拟带有游戏控制器的鼠标输入。

使用游戏控制器时，支持以下输入：

| 输入       | 描述                                                                  |
|----------|---------------------------------------------------------------------|
| 方向键      | Move Focus / Navigation <br/>Tweak 值（使用 A 激活时） <br/> 调整窗口大小（按住 X 时） |
| 左摇杆      | 滚动<br>移动窗口（按住 X 时）                                                  |
| X （左脸按钮） | 点击：切换菜单<br>按住 + L1/R1：聚焦窗口<br>按住 + 方向键：调整窗口大小<br />按住 + 左摇杆：移动窗口    |
| Y （上脸按钮） | 退出文本/屏幕键盘                                                           |
| B （右脸按钮） | 取消 / 关闭 / 退出                                                        |
| A （下脸按钮） | 使用方向键<br/>激活 / 打开 / 切换<br/>调整值（+ L1/R1 调整得更慢/更快）                    |

使用游戏控制器模拟鼠标输入时，支持以下输入：

| 输入       | 描述          |
|----------|-------------|
| 左摇杆      | 移动鼠标指针      |
| A （下脸按钮） | 鼠标左键 （BTN1） |
| B （右脸按钮） | 鼠标右键 （BTN2） |

您可以使用“`imgui_ControllerMouseSensitivity`”调整灵敏度。

## 离散输入模式
当 ImGui 可见时，输入将同时发送到 ImGui 和 O3DE 游戏/编辑器。有时，在任何给定时间只控制 ImGui 或游戏/编辑器是更可取的。为了实现这一点，ImGui Gem 支持离散输入模式。

默认情况下，discrete input 模式处于关闭状态。它可以在 ImGui O3DE 菜单中打开，也可以使用[CVAR](#cvars) `imgui_DiscreteInputMode`.

启用此模式后，ImGui 将获得第 2 种可见性状态，此时，当通过游戏控制器上的 HOME 按钮或 L3/R3 切换 ImGui 可见性时，它将在三种状态之间切换，而不仅仅是打开和关闭。

1. ImGui 已关闭 - ImGui 不可见。
2. 所有输入都转到 ImGui - ImGui 可见并接收所有输入。
3. 所有输入都转到游戏 - ImGui 可见，但输入将转到游戏。这允许 ImGui 分析工具和其他工具在与 O3DE 交互时在屏幕上可见。

> 您可以查看 ImGui 主菜单栏的右上角，以查看 ImGui 输入的当前状态。您还可以与此菜单交互以获取一些输入提示。

![ImGui Input Status](/images/user-guide/gems/reference/debug/imgui_input_and_status.png)

## 在组件中使用 ImGui

您将需要：

1. 确保 ImGui Gem 在您的项目中处于活动状态。请参阅 O3DE 指南 [在项目中添加 Gem](https://www.o3de.org/docs/user-guide/project-config/add-remove-gems/).
2. 注册 ImGui::ImGuiUpdateListenerBus::Handler 或使用`ImGuiLYCommonMenu`类作为基础。
3. 在处理程序中执行 ImGui 操作。

```cpp
        void MyImGuiComponent::OnImGuiUpdate()
        {
            // .. do imgui stuff ...
        }
```

## O3DE ImGui 工具示例

O3DE 附带了许多 ImGui 工具，因此您看到的选项将取决于项目中活动的特定 Gem。

* [调试 PhysX](/docs/user-guide/interactivity/physics/debugging/)
* [多人游戏助手](/docs/user-guide/gems/reference/multiplayer/multiplayer-gem/running/#running-locally-using-imgui-options)

## CVARs

ImGui 在运行时通过控制台或将它们放置在配置中使用以下控制台变量 （CVAR）。有关配置 CVAR 的更多信息，请参阅通用 [CVAR 指南](/docs/user-guide/appendix/cvars/) 。

| 名称                               | 说明                                                                       | 
|----------------------------------|--------------------------------------------------------------------------|
| imgui_AutoEnableComponents       | _已弃用_                                                                    |
| imgui_ControllerMouseSensitivity | 允许用户在使用 Controller Mouse Input （控制器鼠标输入） 模式时设置鼠标左摇杆移动的灵敏度。               |
| imgui_DiscreteInputMode          | 打开或关闭 Discrete Input Mode（离散输入模式），这有助于在使用 ImGui 时将输入通过管道传输到 ImGui 和/或游戏。 |
| imgui_EnableAssetExplorer        | _已弃用_                                                                    |
| imgui_EnableCameraMonitor        | _已弃用_                                                                    | 
| imgui_EnableEntityOutliner       | _已弃用_                                                                    |
| imgui_EnableController           | 切换上下文控制器支持功能。默认情况下，此选项处于关闭状态，但您可以使用它来自定义您在支持控制器的任何平台上的体验。                |
| imgui_EnableControllerMouse      | 切换 Virtual Mouse Controller 支持功能。有时比上下文控制器支持更可取。                         |
| imgui_EnableImGui                | 切换 ImGui 的可见性，如果在 .cfg 文件中，将在启动时可见。大致相当于按 HOME 或 L3/R3。                  |
 
