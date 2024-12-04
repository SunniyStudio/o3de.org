---
linktitle: 文本输入验证
title: 文本输入验证中的错误消息
description: 了解如何在 Open 3D Engine （O3DE） 中使用 Blue Jay Design System （BJDS） 在文本输入验证中设计错误/警告/成功/信息消息。
weight: 500
toc: true
---

文本输入验证是系统检查用户是否提供正确信息的过程。对于无效文本，有效的通知消息可帮助用户了解问题以及如何解决该问题。

系统验证文本输入，并仅在用户与输入字段交互后显示通知。如果他们输入的数据不正确，请通过在输入字段旁边显示相应的图标来告知用户发生了什么。您还可以在工具提示中提供更多信息，当用户将鼠标悬停在输入字段上时，将显示该工具提示。

**示例**

![Input Validation Error or Failure](/images/tools-ui/text-input-validation/text-input-validation.png)


## 错误/失败图标

![Error/failure icon for text input validation](/images/tools-ui/text-input-validation/input-validation-error-or-failure.png)

使用此图标表示文本输入验证消息中的错误或失败。对于没有空格图标的较小输入字段，您可以排除此图标。用于文本输入验证的 error/failure 图标与标准的 error/failure 图标不同，以避免在使用这两个图标时产生混淆。

## 提示工具

对于文本输入验证，当用户将鼠标悬停在图标或输入字段上时，会显示工具提示。在这种情况下，工具提示包含可帮助用户解决无效输入字段的信息。 


### 规格

在创建用于文本输入验证的工具提示时，请查看以下规范：

* 仅当用户将鼠标悬停在图标和输入字段上时，才显示工具提示，而不显示任何其他 UI 元素。

* 以纯文本形式编写工具提示消息。不要包含丰富的信息，例如图像和格式化文本。

* 只要用户将鼠标悬停在元素上，就会显示工具提示。

![Text Input Validation Tooltips](/images/tools-ui/text-input-validation/text-input-validation-tooltips.png)
