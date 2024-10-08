---
linkTitle: 反射上下文
title: O3DE中的反射上下文
description: 学习如何使用 Open 3D Engine (O3DE) 中的各种反射上下文来序列化组件数据，使编辑器能够操作组件数据，并将组件方法、属性和事件公开给脚本系统。
weight: 300
---

使用**Open 3D Engine (O3DE)** 反射系统，使组件的 C++ 对象可用于序列化和编辑器操作，并使脚本系统可在运行时调用组件的方法和事件。

为此，O3DE 提供了以下反射上下文：

* [序列化上下文](serialization-context/) -- 通过 C++ 对象的反射，实现组件数据的持久化。
* [编辑上下文](edit-context) -- 公开组件的序列化 C++ 数据，以便在 **O3DE Editor** 中进行编辑。
* [行为上下文](behavior-context) -- 通过反射向脚本系统（如 Script Canvas 和 Lua）公开组件的方法、属性和事件。您可以在运行时使用这些脚本绑定来调用组件的方法、事件和事件处理程序。

所有这些反射上下文都使用了 C++ [构建器设计模式](https://en.wikipedia.org/wiki/Builder_pattern)。
