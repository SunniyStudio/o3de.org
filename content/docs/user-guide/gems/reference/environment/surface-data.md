---
linkTitle: Surface Data
title: Surface Data Gem
description: Surface Data Gem 提供了从 Open 3D Engine （O3DE） 项目中的网格和地形等表面发出信号或标签的功能。
weight: 400
toc: true
---

Surface Data Gem 使表面和网格能够发出传达各种属性的信号或标签。这些信号和标签具有多种用途，例如创建包含和排除区域、对齐和定位对象（如植被）以及应用物理材质。

## 添加Surface Tag名称

可以通过创建新的 **Surface Tag Name List** 资产来添加新的 **Surface Tag** 名称。这可以通过以下步骤完成：

1. 从 **Tools** 菜单中打开 **Asset Editor**。
1. 选择 **File** -> **New** -> **Surface 标签名称列表**。
1. 单击**Surface Tag List List**行右侧的 {< icon "add.svg" >}} **Add** 按钮，该按钮将添加自定义 **Surface Tag** 名称元素。
1. 在“**Surface Tag**名称”中填写您想要使用的标签名称元素。

![Adding surface tag names.](/images/user-guide/gems/reference/surface-data/surface-data-custom-tag.png)

保存 **Surface Tag Name List** 资产后，您添加的标签将在任何 **Surface Tag** 数据字段中可用。

![Using surface tag names.](/images/user-guide/gems/reference/surface-data/surface-data-tag-usage.png)
