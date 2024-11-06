---
title: Compound Shape 组件
linktitle: Compound Shape
description: ' Open 3D Engine (O3DE) Compound Shape 组件参考。'
weight: 100
---



**Compound Shape**组件是复杂形状的容器。一个实体只能包含一个 Shape 组件。要构建复杂的形状，Compound Shape 组件可以引用任意数量的实体，每个实体都有自己的 Shape 组件。Compound Shape （复合形状） 组件不是一个网格，而是一个辅助几何体，可用于定义形状渐变、音频、植被、PhysX 以及任何可利用 Shape EBus 的应用程序的体积。有关使用 Shape 组件的更多信息，请参阅 [Shape 组件](/docs/user-guide/components/reference/shape/)。

## 提供者

[O3DE Core (LmbrCentral) Gem](/docs/user-guide/gems/reference/o3de-core)

## 依赖

包含 [Shape 组件](./) 的一个或多个实体。

## Compound Shape 属性

![Compound Shape component properties](/images/user-guide/components/reference/shape/compound-shape-component-ui-01.png)

复合形状组件包含包含形状组件的引用实体的 **Child Shape Entities** 列表。

* 要将项添加到 **Child Shape Entities** 列表，请选择列表顶部的 {{< icon "add.svg" >}} 图标。
* 要从列表中删除项目，请选择项目右侧的 {{< icon "delete.svg" >}} 图标。
* 要清除列表，请选择组件界面右上角的  {{< icon "delete.svg" >}} 图标按钮。
* 要将包含形状组件的实体分配给列表中的插槽，请选择 {{< icon "picker.svg" >}} 进入拾取模式，然后在 Perspective 视图或 Entity Outliner 中单击实体来选择实体。

Compound Shape 组件引用的实体不需要是场景层次结构中具有 Compound Shape 的实体的子项。但是，如果形状应该一起移动，则建议将形状实体设为场景层次结构中 Compound Shape 实体的子实体。

组成复合形状的形状的尺寸应在各个子形状实体组件中设置，以确保准确的碰撞和重叠检测。使用实体的 Transform （变换） 组件中的 Scale （缩放） 属性更改形状或复合形状的尺寸可能会导致不可预知的结果。

对于 EBus（事件总线）请求，Compound Shapes 为整个形状组件总线提供服务。但是，列表中的每个形状都会增加请求的成本，例如测试冲突或重叠。

请参阅 [Shape 组件 Ebus 接口](./#shape-component-ebus-interface) 以获取所有 Shape 组件可用的功能说明。
