---
description: ' 使用控制台命令修改和配置 Open 3D Engine 运行时应用程序。 '
title: '通用远程控制台'
draft: true
---

您可以使用**控制台**命令来修改和配置 O3DE 运行应用程序。在个人电脑上，**控制台**可通过O3DE编辑器或游戏使用。但在移动平台上，您必须使用一个单独的基于 Windows 的应用程序，即通用远程控制台（Universal Remote Console）。通过通用远程控制台，您可以使用运行O3DE游戏的机器的IP地址连接到O3DE的远程实例。

通用远程控制台需要使用个人电脑，可与安卓和iOS系统一起使用。您的移动设备和电脑需要在同一网络上，防火墙应配置为允许通过 4600 端口进行通信。

**为了使用通用远程控制台**

1. 运行`Tools\RemoteConsole\RemoteConsole.exe`

![The Remote Console](/images/user-guide/programming/remote-console.png)

1. 要查看 O3DE 日志系统的输出，请单击 **Full Log** 选项卡。

**连接移动设备上的 O3DE 游戏**

1. 在`system_platform_asset.cfg`文件中，输入控制台变量`log_RemoteConsoleAllowedAddresses=[IP address]`，其中 IP 地址是远程控制台运行的计算机地址。

   对于Android，使用`system_android_es.cfg`文件。

   对于iOS，使用`system_ios_ios.cfg`文件。

1. 保存文件。

1. 在更新`.cfg`文件后，将游戏部署到移动设备。

   要了解更多信息，请查看[O3DE的Android支持](/docs/user-guide/platforms/android/)<!-- and [iOS support for O3DE](/docs/user-guide/platforms/ios/)-->.

1. 如果游戏已经运行，请重启它。

1. 在通用远程控制台中，点击工具栏中的 **Targets** 。

1. 在**Custom IP**下输出设备的IP地址。

如果您的网络允许您为每个设备分配固定的 IP 地址，您可以编辑 `params.xml` 文件并添加新的目标设备，如下例所示。该文件与通用远程控制台位于同一目录，您可以在应用程序运行时对其进行编辑。

```
<Targets>
<Target name="PC" ip="localhost" port="4600"/>
<Target name="Android" ip="192.168.1.247" port="4600"/>
</Targets>
```

这样，您就可以从设备列表中进行选择，而不必每次都输入 IP 地址。连接成功后，右下角的状态指示灯将变为绿色。

## 提交命令

在窗口底部的**Type a command**框中，输入类似下面的命令。该控件具有自动完成功能，对于某些命令（如map），还可以检测可用选项。

包含以下命令：
+ `cl_DisableHUDText` - 禁用HUD文本
+ `g_debug_stats` - 启用游戏事件调试
+ `r_DisplayInfo` - 显示渲染信息
+ `r_ProfileShaders` - 显示着色器的分析信息
