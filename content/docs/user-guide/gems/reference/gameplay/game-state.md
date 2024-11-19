---
linkTitle: Game State
title: Game State Gem
description: Game State Gem提供在 Open 3D Engine （O3DE） 项目中确定和管理游戏状态的功能。
toc: true
---

Game State Gem 可帮助您在高级别管理和确定项目运行时所处的状态。由于 Game State Gem 使用堆栈来管理游戏状态，因此返回到之前的状态非常简单。

{{< note >}}
有关游戏状态的示例实现，请参阅 [Game State Samples Gem](/docs/user-guide/gems/reference/gameplay/game-state-samples。Game State Samples Gem （游戏状态样本） Gem 取决于 Game State Gem。您可以在 Game State Samples Gem 中自定义游戏状态，以满足您的项目要求。
{{< /note >}}

## 检查代码

Game State Gem 管理抽象游戏状态的堆栈（或 [pushdown automaton](https://en.wikipedia.org/wiki/Pushdown_automaton)）。Game State Gem 包括以下代码成员：

* `IGameState` - 所有具体游戏状态类都必须从中派生的抽象接口。

* `GameStateRequests` - 其他系统用于提交与游戏状态相关的请求的[EBus](/docs/user-guide/programming/messaging/ebus/)接口。

* `GameStateNotifications` - 其他系统用于侦听与游戏状态相关的事件的事件总线接口。

* `GameStateSystemComponent` - 实现 `GameStateRequestBus` 接口，并通过 `GameStateNotificationBus` 发送事件。

### IGameState

`IGameState` 是所有具体游戏状态类都必须从中派生的抽象接口。该接口定义了跟踪游戏状态变化的方法，如以下源代码摘录所示，地址为 `\Gems\GameState\Code\Include\GameState\GameState.h`。

```c++
//! Called when this game state is pushed onto the stack.
virtual void OnPushed() {};

//! Called when this game state is popped from the stack
virtual void OnPopped() {};

//! Called when this game state is set as the active game state.
virtual void OnEnter() {};

//! Called when this game state is replaced as the active game state.
virtual void OnExit() {};

//! Called each frame while this game state is the active game state.
virtual void OnUpdate() {};
```

### GameStateRequests

`GameStateRequests`事件总线中的方法执行基本任务，例如创建、推送和弹出游戏状态，或者获取活动游戏状态或活动游戏状态类型。有关完整的源代码，请参阅 `\Gems\GameState\Code\Include\GameState\GameStateRequestBus.h` 文件。

```c++
 //! Create a new game state.
 //! \tparam GameStateType - The game state type to create.
 //! \param[in] checkForOverrides - True to should check for an override, false otherwise.
 //! \return - A shared pointer to the new game state that was created.
template<class GameStateType>
static AZStd::shared_ptr<IGameState> CreateNewOverridableGameStateOfType(bool checkForOverride = true);

 //! Create a new game state and push it onto the stack to make it the active game state.
 //! New game states are created and stored in the stack using a shared_ptr, so they are
 //! destroyed automatically after they are popped off the stack (assuming that nothing
 //! else retains a reference - for example, through GameStateNotifications::OnActiveGameStateChanged).
 //! \tparam GameStateType - The game state type to create and activate.
 //! \param[in] checkForOverrides - True to check for an override, false otherwise.
template<class GameStateType>
static void CreateAndPushNewOverridableGameStateOfType(bool checkForOverride = true);

//! Pop  game states from the stack until the active game state is of the specified type.
//! \tparam GameStateType - The game state type in the stack that you want to be active.
//! \return True if the active game state is now of the specified type, false otherwise.
template<class GameStateType>
static bool PopActiveGameStateUntilOfType();

//! Query whether the active game state is of the specified type.
//! \tparam GameStateType - The game state type to check whether it is active.
//! \return - True if the active game state is of the specified type, false otherwise.
template<class GameStateType>
static bool IsActiveGameStateOfType();

//! Query whether the game state stack contains a game state of the specified type.
//! \tparam GameStateType - The game state type to check whether it is in the stack.
//! \return - True if the stack contains a game state of the specified type, false otherwise.
template<class GameStateType>
static bool DoesStackContainGameStateOfType();

//! Update the active game state. Called during the AZ::ComponentTickBus::TICK_GAME
//! priority update of the AZ::TickBus, but can be called independently any time if needed.
virtual void UpdateActiveGameState() = 0;

//! Request the active game state (if any)
//! \return - A shared pointer to the active game state (empty if there is none).
virtual AZStd::shared_ptr<IGameState> GetActiveGameState() = 0;

//! Push a game state onto the stack, making it become the active game state.
//! If newGameState is already found in the stack, the call fails and returns false. However,
//! it is possible for multiple instances of the same game state type to occupy the stack.
//! \param[in] newGameState - The new game state to push onto the stack.
//! \return - True if the game state was successfully pushed onto the stack, false otherwise.
virtual bool PushGameState(AZStd::shared_ptr<IGameState> newGameState) = 0;

//! Pop the active game state from the stack. This deactivates the active game state and
//! makes the game state below it in the stack (if any) the active game state again.
//! \return - True if the active game state was successfully popped, false otherwise.
virtual bool PopActiveGameState() = 0;

//! Pop all game states from the stack, leaving it empty.
virtual void PopAllGameStates() = 0;

//! Replace the active game state with another game state that becomes the active state.
//! If the stack is currently empty, newGameState is pushed to become the active state.
//! If newGameState is already found in the stack, the call fails and returns false. However,
//! it is possible for multiple instances of the same game state type to occupy the stack.
//! This differs from calling PopActiveGameState followed by PushGameState(newGameState),
//! which would result in the state below the currently active state being activated then
//! immediately deactivated when newGameState is pushed onto the stack; calling this will
//! leave the state below the currently active state unchanged.
//! \param[in] newGameState - The new game state with which to replace the active game state.
//! \return - True if the active game state was successfully replaced, false otherwise.
virtual bool ReplaceActiveGameState(AZStd::shared_ptr<IGameState> newGameState) = 0;

//! Query whether the game state stack contains a game state of the specified type.
//! \param[in] gameStateTypeId - The game state type to check whether it is in the stack.
virtual bool DoesStackContainGameStateOfTypeId(const AZ::TypeId& gameStateTypeId) = 0;
```

### GameStateNotifications

当前一个游戏状态和新的游戏状态之间发生转换时，将调用`GameStateNotificationBus` `OnActiveGameStateChanged` 方法。有关源代码，请参阅`\Gems\GameState\Code\Include\GameState\GameStateNotificationBus.h`文件。

```c++
//! Called when a game state transition occurs.
//! \param[in] oldGameState - The old game state being transitioned from (can be null).
//! \param[in] newGameState - The new game state being transitioned into (can be null).
virtual void OnActiveGameStateChanged(AZStd::shared_ptr<IGameState> oldGameState, AZStd::shared_ptr<IGameState> newGameState) {}
```
