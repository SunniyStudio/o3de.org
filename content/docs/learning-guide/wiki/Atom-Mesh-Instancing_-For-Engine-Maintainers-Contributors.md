---
title: "[Atom] 面向引擎维护者的网格实例化贡献者"
description: ""
toc: false
---

本文档涵盖了修改、更新和维护实例化实现所需了解的所有内容

另请参阅:
- [网格实例化：适用于内容创建者](https://github.com/o3de/o3de/wiki/%5BAtom%5D-Mesh-Instancing:-For-Content-Creators)
- [网格实例化：适用于引擎维护者/贡献者](https://github.com/o3de/o3de/wiki/%5BAtom%5D-Mesh-Instancing:-For-Engine-Maintainers-Contributors)

# 高级概述
当 r_meshInstancingEnabled = true 时，MeshFeatureProcessor 使用 TYPE_RPI_VisibleObjectList 向剔除系统注册cullabes。这告诉剔除系统不要直接将绘制项目提交到视图，而是为每个视图创建一个可见对象列表，然后 MeshFeatureProcessor 可以处理该列表，以根据每个视图的当前可见内容生成每个视图实例缓冲区。我们存储在每个视图实例缓冲区中的唯一实例数据是 objectID，它可用于查找 ObjectToWorld、ObjectToWorldInverseTranspose 和 ObjectToWorldPrev 转换矩阵。有一个 MeshInstanceManager 类，给定一个 'key' （模型 + 材质 + lod 索引 + 网格索引） 确定给定网格 + 材质属于哪个 '组' 。这些组位于 StableDynamicArray 中，该数组将组存储在页面中。每个页面都被视为 Bucket 排序中的一个 Bucket。在处理 VisibleObjectList 时，MeshFeatureProcessor 将首先将可见对象排序到各自的存储桶中，然后在这些存储桶中进行排序，以便 objectId 先按实例组排序，然后按深度排序。对存储桶进行排序后，数据将合并以创建一个填充有可见 objectId 的每个视图实例的缓冲区。

# 高级性能注意事项
可见对象列表的处理会产生开销，对于具有大量可见对象的场景，绘制计数的减少会大大抵消该开销。但是，初始实现具有全有或全无的实例化方法，因此即使是场景中只有一个实例的对象也将是此过程的一部分。因此，默认情况下，实例化处于禁用状态。bucket 排序在处理大多数唯一对象的情况方面做得很合理，但不够好，无法一直启用。有一个 debug cvar 可用于更轻松地验证这一点：r_meshInstancingForceOneObjectPerDrawCall。
