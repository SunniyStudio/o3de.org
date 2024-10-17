---
linktitle: 序列化
title: O3DE中的对象序列化
description: 将Open 3D Engine (O3DE)中的对象序列化为 XML、JSON 和其他格式，以便其他工具处理，或在运行会话之间加载。
---

**Open 3D Engine (O3DE)** 提供了对象序列化功能，可在会话之间持久保存对象，在客户端之间传输对象，或在**O3DE 编辑器**和引擎之间处理对象。基于 JSON 的序列化系统设计为人类可读和可编辑，而许多现有的 O3DE 工具都使用 XML 系统。O3DE 还提供二进制序列化，由**资产处理器**内部使用。

在本节文档中，您将学习如何为序列化注册类和枚举，以及如何使用 JSON 序列化系统。有关使用序列化上下文进行反射的更多信息，请参阅 [O3DE中的序列化上下文](/docs/user-guide/programming/components/reflection/serialization-context/) 和 [反射组件以进行序列化和编辑](/docs/user-guide/programming/components/reflection/reflecting-for-serialization)。
