---
linkTitle: Game State Samples
title: Game State Samples Gem
description: Game State Samples Gem 提供一组示例游戏状态（基于通用 Game State Gem 构建），包括主要用户选择、主菜单、关卡加载、关卡运行和关卡暂停。
toc: true
---

游戏状态示例 Gem 使用 [GameState Gem](/docs/user-guide/gems/reference/gameplay/game-state) 来提供一组示例游戏状态，用于控制游戏的高级流程。

## 包含的游戏状态

Game State Samples Gem 包括以下游戏状态。这些状态通常出现在游戏的开始、中间和结尾。

* **Main menu state** - 允许通过单击按钮加载项目中的任何关卡。
* **Level loading state** - 显示占位符加载屏幕。
* **Level running state** - 在游戏运行时处于活动状态。
* **Level paused state** - 启用恢复或返回到主菜单以选择其他关卡。
* **Other states** - 对用户登录和注销以及控制器连接和断开连接做出反应的游戏状态。

### 游戏状态流

下图显示了 GameState Samples Gem 中默认游戏状态的流程。

![Flow of game states in the Game State Samples gem.](/images/user-guide/gems/gems-system-gem-game-state-samples-2.png)

## 可能的用途

以下是使用 Game State Samples Gem 的一些可能方法：

* **复制** - 将 Gem 复制到您的游戏项目中，以用作进一步自定义的起点。此方法提供了最大的灵活性。

* **派生** - 从代码派生以创建自己的游戏状态。如果您想保持与示例游戏状态相同的行为，但只进行少量的自定义，则建议使用此方法。例如，您可以创建如下所示的主菜单类：

  ```c++
  MyCustomMainMenu : public GameStateMainMenu
  ```

  然后，您可以通过继承自定义类，以加载不同的主菜单 UI Canvas。“派生”方法的缺点是它将一些逻辑放在 Gem 中，而将其余逻辑放在游戏代码中。这会使您的解决方案难以遵循或调试。

* **修改** - 直接修改 GameState Samples Gem。由于 Gem 不能依赖于游戏，因此无法与任何特定于游戏的代码进行有效通信，因此不建议使用此选项。
