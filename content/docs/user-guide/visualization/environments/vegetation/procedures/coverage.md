---
linkTitle: 扩大覆盖范围
title: 扩大植被覆盖范围
description: 在 Open 3D Engine中扩展植被，覆盖整个关卡。
weight: 200
toc: true
---

创建植被块后，您可以扩展它，使植被覆盖整个关卡。

**扩大植被覆盖范围**

1. 创建一个实体并将其命名为 *WorldBox*。

1. 点击**Add Component**，选择 “方框形状 ”组件。

1. 在**Box Shape**组件中，指定 x 轴、y 轴和 z 轴的值以匹配您的关卡。例如，如果您创建了一个纹理尺寸为 512 x 512 的关卡，请指定类似的值，如 `512.0`、`512.0` 和 `64.0`。

    ![Create an entity to cover your level.](/images/user-guide/vegetation/dynamic/expanding-vegetation-coverage-1.png)

1. 选择 **BasicCoverage** 实体。

1. 在 **Shape Reference** 组件上，对于 **Shape Entity Id**，选择 **WorldBox** 实体。

    ![Specify the WorldBox entity for the Shape Reference component.](/images/user-guide/vegetation/dynamic/expanding-vegetation-coverage-2.png)

    **示例:** 植被会出现在整个关卡中。在关卡中移动时，植被会动态出现。

    ![Specify a world box entity so that vegetation appears for the entire level.](/images/user-guide/vegetation/dynamic/expanding-vegetation-coverage-3.png)
