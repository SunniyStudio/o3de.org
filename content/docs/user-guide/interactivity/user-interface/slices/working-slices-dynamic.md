---
linkTitle: 标记动态切片
description: ' 在 Open 3D Engine 的 UI Editor 中标记动态切片。 '
title: 标记动态切片
weight: 700
---

标记为动态切片的切片可以像任何其他切片一样使用，但也可以在运行时实例化。

您可以在 **资源浏览器** 中将切片标记为动态。

**将切片标记为动态切片**
+ 右击 `.slice` 文件并选择 **Set Dynamic Slice**.

要在运行时实例化 UI 切片，请在 UI 元素上使用 **UiSpawner** 组件。这会导致系统在激活时自动生成动态切片。它还向 Lua 和 C++ 公开了一条总线，允许在需要时实例化切片。
