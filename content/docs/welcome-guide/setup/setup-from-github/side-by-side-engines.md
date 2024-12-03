---
title: 并排引擎
description: 了解如何在计算机上设置 Open 3D Engine （O3DE） 的多个配置。
weight: 500
toc: true
---

在前面的主题中，您学习了如何将 O3DE 引擎设置和配置为源引擎或预构建的 SDK 引擎。您还可以选择在并排配置中注册两个引擎。这是将引擎源代码开发与项目开发隔离开来的一种方法。

例如，如果引擎源位于`C:\o3de`中，而预构建的 SDK 引擎位于`C:\o3de-install`中，则可以为每个引擎指定自己的引擎名称，以便可以使用`o3de`脚本的`register`命令在 O3DE 清单中注册这两个引擎。要测试您的引擎修改，您可以使用源引擎项目创建说明构建测试项目，该说明在项目目录中构建引擎。当您准备好更新生产项目使用的 SDK 引擎时，您可以使用`INSTALL`目标从`C:\o3de`构建新版本的 SDK 引擎。这将更新`C:\o3de-install`中的二进制文件。

**更新引擎的名称**

1. 在引擎根目录中打开`engine.json`并更改`engine_name`字段。

1. 从该目录再次注册引擎。

    {{< tabs name="Engine registration instructions" >}}
    {{< tab name="Windows" codelang="cmd" >}}scripts\o3de.bat register --this-engine{{< /tab >}}
    {{< tab name="Linux" codelang="bash" >}}scripts/o3de.sh register --this-engine{{< /tab >}}
    {{< /tabs >}}

您从此引擎创建的新项目将使用新的引擎名称。要更新现有项目的已配置引擎，请在项目根目录中打开`project.json`，然后更改`engine`字段以使用新的引擎名称。
