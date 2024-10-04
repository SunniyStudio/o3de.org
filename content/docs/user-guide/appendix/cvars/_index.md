---
title: 控制台变量 (CVARs)
---

控制台变量（CVAR）可用于设置 O3DE 中的各种编辑器和运行时选项。O3DE 随附了许多内置的 CVAR，但也支持配置和分组您自己的 CVAR 的选项。

## 使用CVAR

你可以在代码中使用[CVAR 宏函数](/docs/user-guide/programming/az-console/#console-variables-cvars)定义自己的 CVAR。您还可以在[控制台变量组](/docs/user-guide/editor/console-cvars-commands/#console-variable-groups)中将 CVAR 组合在一起，这样就可以一次性更改多个 CVAR。

通过[console](/docs/user-guide/editor/console/)在编辑器中调用CVAR。

在启动 O3DE 二进制文件时，通过`--{cvar name} {value}` 或 `--{cvar name}={value}`，您也可以在命令行中传递 CVAR。例如，`ServerLauncher.exe --sv_somebool=true`。

此外，您还可以在 [配置文件](/docs/user-guide/editor/console/#configuring-console-variables-in-configuration-files) 中添加 CVAR。

### 发现 CVAR

查找当前项目中所有可设置 CVAR 的最快方法是通过 [控制台变量窗口](/docs/user-guide/editor/console/#viewing-the-console-window)。调出控制台窗口将列出当前项目中的所有 CVAR，并允许你设置它们的值。

您也可以开始在控制台中键入 CVAR 名称的开头，然后使用 **Tab** 键显示自动完成并查看所有潜在匹配。例如，键入 `ed_` 后，您可以使用 **Tab** 键循环查看所有影响编辑器的 CVAR。

![Automatic CVAR expansion in the console](/images/user-guide/appendix/cvars/console_autoexpand.png)

### CVAR 控制台函数

有了内置的控制台命令，使用 CVAR 就更容易了：

| 名称               | 说明                                                                                                                                                            |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DumpCommandsVars | (遗留）将把所有 CVAR 及其值转储到名为 `consolecommandsandvars.txt` 的文件中。<br>更喜欢使用控制台窗口查看 CVAR 及其值。 | 
| resetcvars       | 将所有 CVAR 重置为初始值。                                                                                                                     |

## 常见的CVAR前缀

O3DE 使用以下前缀，以方便查找 CVAR：

| 前缀 | 用法                                                 |
|--------|----------------------------------------------------|
| bg_    | **B**oth **G**ames， 适用于可在客户端和服务器上使用的通用 CVAR。       |
| cl_    | 仅适用于客户端的 CVAR。                                     |
| ed_    | 仅适用于编辑器的 CVAR。                                     |
| net_   | 仅适用于底层网络的 CVAR。                                    |
| physx_ | 仅适用于关于NVIDIA PhysX的 CVAR。                          |
| r_     | 仅适用于关于渲染的 CVAR。                                    |
| s_     | 仅适用于关于音效的 CVAR。                                    |
| sv_    | 仅适用于服务器的 CVAR。                                     |
| sys_   | 仅适用于底层核心系统的 CVAR。 |

## O3DE中的CVAR的示例

许多 O3DE 功能都暴露了 CVAR，以帮助调试、故障排除和额外的可视化。这些功能包括

* [网络](/docs/user-guide/networking/settings/)
* [导航](/docs/user-guide/interactivity/navigation-and-pathfinding/recast-navigation/#visualizing-the-navigation-mesh)
* [物理](/docs/user-guide/interactivity/physics/debugging/#physx-debug-console-variables)
