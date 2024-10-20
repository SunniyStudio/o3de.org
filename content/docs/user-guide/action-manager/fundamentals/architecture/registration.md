---
title: 注册
linktitle: 注册
description: 动作管理器注册 API 调用的最佳实践和具体细节。
weight: 202
---

动作管理器架构要求项目在使用前必须注册。如果调用 API 时出现未知标识符，尤其是由于错字造成的未知标识符，系统可以通过这一措施提供错误信息。
第二个限制是在编辑器启动时注册所有项目，但这一限制并未强制执行。由于系统是为可扩展性和定制化而设计的，因此在规定的时间注册所有元素可以让其他系统使用这些元素，同时仍然执行严格的所有权规定。

为了简化注册过程，动作管理器架构提供了一组在编辑器启动时触发的通知钩子。这些通知按项目类型划分；这使得系统可以确定，在调用与不同项目类型相对应的钩子时，它们所需的项目（但已在其他地方定义）是否已经注册。例如，动作上下文注册钩子首先被调用，因此当动作注册钩子被触发时，我们可以假设所有的动作上下文都已注册并可用。

## 注册钩子

#### `OnActionContextRegistrationHook`

注册动作上下文的同步信号。

#### `OnActionContextModeRegistrationHook`

同步信号，用于注册动作上下文模式。
可以假定在调用该信号时，动作上下文已经注册。

#### `OnActionUpdaterRegistrationHook`

同步信号，用于注册 “动作更新器”。

#### `OnMenuBarRegistrationHook`

同步信号，用于注册菜单条。

#### `OnMenuRegistrationHook`

同步信号，用于注册菜单。

#### `OnToolBarAreaRegistrationHook`

同步信号，用于注册工具栏区域。

#### `OnToolBarRegistrationHook`

同步信号，用于注册工具栏。

#### `OnActionRegistrationHook`

注册动作的同步信号。
可以假定在调用此功能时，Action Context 和 Action Updaters 已经注册。

#### `OnWidgetActionRegistrationHook`

用于注册 Widget 动作的同步信号。
`
#### `OnActionContextModeBindingHook`

将动作与动作上下文模式绑定的同步信号。
可以假定在调用该信号时，动作上下文模式已经注册。

#### `OnMenuBindingHook`

同步信号，用于将动作/小部件添加到菜单，将菜单添加到菜单栏。
调用该信号时，可以假定已经注册了动作、小部件动作、菜单和菜单栏。

#### `OnToolBarBindingHook`

同步信号，用于向工具栏添加动作/部件/菜单，以及向工具栏区域添加工具栏。
可以假定，在调用该信号时，动作、小工具动作、菜单、工具栏和工具栏区域已经注册。

#### `OnPostActionManagerRegistrationHook`

任何注册后活动的同步信号。
这主要是为了保证系统的未来发展，但在采用这种方法之前，应尽可能使用其他钩子。


## 访问注册钩子

### C++

要处理注册钩子，您需要将您的类的实例连接到`ActionManagerRegistrationNotificationBus`。
钩子会在编辑器初始化时调用，即在主窗口、Gem和编辑器系统初始化之后，但在用户界面显示给用户之前。因此，类实例需要在编辑器初始化结束前连接到总线；通常，使用系统组件是最佳选择。

让您的类扩展 `ActionManagerRegistrationNotificationBus`.

```
class TestSystemComponent
    : private AzToolsFramework::ActionManagerRegistrationNotificationBus::Handler
{
}
```

在类的初始化过程中，连接总线。这可以在构造函数、系统组件的`Init` 或 `Activate`函数或任何其他形式的初始化函数中进行。
记得在析构函数中也要断开连接。

```
void TestSystemComponent::Init()
{
    ActionManagerRegistrationNotificationBus::Handler::BusConnect();
}
```

然后，该类就可以酌情处理这些功能。

```
void TestSystemComponent::OnActionRegistrationHook()
{
    // ...
}

```

### Python

Gem还可以通过添加到Gem的`Editor\Scripts\`文件夹中的`bootstrap.py`文件来处理注册钩子。

```
import azlmbr.action as action
import azlmbr.bus as bus

def OnActionRegistrationHookHandler(parameters):
    # Create an Action

handler = action.ActionManagerRegistrationNotificationBusHandler()
handler.connect()
handler.add_callback('OnActionRegistrationHook', OnActionRegistrationHookHandler)
```
