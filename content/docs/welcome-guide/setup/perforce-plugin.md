---
linktitle: 设置 Perforce 插件
title: 在 Open 3D Engine 中设置 Perforce 插件
description: 了解如何配置 Perforce 插件，以将 Perforce 用作 Open 3D Engine （O3DE） 项目的版本控制解决方案。
weight: 700
draft: true
---



O3DE 与 Perforce 集成，作为项目版本控制解决方案。该引擎使用`p4 set`命令来配置设置，并使用 Perforce 可视化客户端 （P4V） 选择性地同步和提交更改的资产。

您可以使用 **Perforce Settings** 对话框来配置 O3DE 与 Perforce 的连接方式。当对话框打开时，将缓存和填充以下设置：

+ **Server** \(`P4PORT`\)
+ **User** \(`P4USER`\)
+ **Workspace** \(`P4CLIENT`\)
+ **Charset** \(`P4CHARSET`\)

{{< note >}}
`P4_<P4PORT>_CHARSET`也会被缓存。如果此值与您当前的 `P4PORT` 值匹配，则使用此值;否则，使用`P4CHARSET`的值。例如，如果`P4PORT` 设置为 **my.perforce.server.com:1666**，则将使用值 **P4\_my.perforce.server.com:1666**。
{{< /note >}}

如果您的 Perforce 连接设置是使用覆盖 `p4 set` 命令的方法配置的，则某些值可能无法修改。以下连接方法可能会覆盖修改设置的功能：

+ 配置 - 配置文件将覆盖此连接设置。如果检测到，则显示配置文件的路径。如果未检测到，您可以检查`P4CONFIG`的设置。
+ 环境 - 您的系统环境将覆盖此连接设置。您可以检查系统的控制面板以删除这些覆盖。

**要使用 Perforce 插件菜单**

1. 在 O3DE 编辑器中，单击底部工具栏中的 P4 图标。

   {{< note >}}
将鼠标悬停在图标上可显示连接状态。
   {{< /note >}}

1. 在下拉菜单中，您可以执行以下操作：

   + 单击 **Enable** 或 **Disable** 以切换插件。**Enable** 设置允许您联机工作。**Disable**设置强制您脱机工作。

      {{< note >}}
在脱机模式下不跟踪更改。如果您脱机工作，则必须在重新连接到 Perforce 时手动协调您的工作。
      {{< /note >}}

   + 单击 **Settings** 以查看或修改您的 Perforce 设置。

   要恢复默认设置，请单击每个值的 **Reset**。完成后，单击 **OK** 以应用您的更改。
