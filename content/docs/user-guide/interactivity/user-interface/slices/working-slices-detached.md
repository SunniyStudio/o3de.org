---
linkTitle: 分离的 UI 切片
description: ' 在 Open 3D Engine 的 UI Editor 中创建分离的 UI 切片。 '
title: 创建分离的 UI 切片
weight: 400
---

您可以从现有 UI 切片实例创建分离的 UI 切片。当您创建分离的 UI 切片时，UI 系统会删除或展平对子切片的所有引用。使用上一节中的按钮示例，假设您将图像切片实例及其子文本切片实例保存为分离切片，并将其命名为`Button2`。分离的 `Button2` 不会引用任何其他切片;它是一个包含两个实体的切片。如果将更改推送到文本切片，则不会影响 `Button2` 切片的任何实例中的文本。

**创建分离的 UI 切片**

1. 选择一组元素的根，或者选择一个根和一个或多个子实体。该根中的子实体可以是单个元素、切片或两者的组合。

1. 右键单击所选内容，然后选择 **Make Detached Slice from Selected Entities**.

    {{< note >}}
如果您选择的任何元素是切片实例，则会显示两个选项： **Make Detached Slice from Selected Entities** 和 **Make Cascaded Slice from Selected Slices & Entities**.
{{< /note >}}

1. 使用描述性名称保存切片。
