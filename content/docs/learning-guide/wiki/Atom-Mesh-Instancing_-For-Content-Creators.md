---
title: "[Atom] 面向内容创建者的网格实例化"
description: ""
toc: false
---

本文档涵盖了在 O3DE 中创建关卡时可能需要了解的有关实例化的信息。

另请参阅:
- [网格实例化：适用于着色器作者](https://github.com/o3de/o3de/wiki/%5BAtom%5D-Mesh-Instancing:-For-Shader-Authors)
- [网格实例化：适用于引擎维护者/贡献者](https://github.com/o3de/o3de/wiki/%5BAtom%5D-Mesh-Instancing:-For-Engine-Maintainers-Contributors)

# 可以实例化的内容
任何使用相同网格 + 材质组合的对象都可以在单个绘制调用中一起实例化，只要网格组件未启用“使用前向传递 IBL”设置即可。如果单个网格具有与另一个实体上的覆盖不同的材料覆盖，则不会一起实例化它们，但使用相同材料的其他网格仍将被实例化。

例如，假设您有一个房屋模型，其中有两个材质分配槽，一个用于墙壁，一个用于屋顶。然后，假设您在世界中放置两个带有房屋的实体，这两个实体都使用分配给屋顶的相同材质，但分配给墙壁的材质不同。即使并非所有材质分配都匹配，屋顶仍将在一次绘制中一起实例化，而墙壁将具有单独的、唯一的绘制调用。

在材质组件上使用材质属性覆盖（或通过脚本和材质组件总线）将在后台生成唯一的材质，从而导致具有该材质的任何网格都不会被实例化。因此，如果你在上面的例子中有两个房子，只是这次它们具有相同的墙材质，但其中一个使用材质组件中的材质实例编辑器来覆盖基础颜色，那么这些墙将再次没有被实例化在一起，即使它们都引用了磁盘上的相同材质。

注意：某些引擎支持以允许每个实例的变化，同时仍然使用单个绘制调用来覆盖特定属性，例如底色。这在 o3de 中是不支持的，要支持这一点，你需要 fork 并修改 MeshFeatureProcessor 以满足你的需要。

# 启用实例化
无需手动作即可将对象标识为可实例化的对象 - 这一切都在后台处理。但是，默认情况下，实例化作为一个整体处于禁用状态。可以通过 cvar 启用它： r_meshInstancingEnabled = true。启用实例化后，所有内容都将沿着实例化路径进行，即使是具有独特材质的独特对象也是如此。它们最终只会成为具有 1 个实例的实例化绘制调用。这会产生一些开销，因此对于不使用多个实例的场景，最好关闭 r_meshInstancingEnabled。最终，如果进行改进以更好地处理唯一对象和实例化对象，则可以放宽此约束，但如果要使用它，则必须在最初选择实例化。

# CVars
r_meshInstancingEnabled - 设置为 true 可在 MeshFeatureProcessor 中启用实例化绘制调用
r_meshInstancingEnabledForTransparentObjects - 设置为 true 可为 MeshFeatureProcessor 中的透明对象启用实例化绘制调用。仅当同一透明对象的多个实例，但没有多个不同的透明对象混合在一起时，才使用此选项。

例如：在这个场景中，一个球体在墙的前面，另一个在后面。但是，由于它们同时被实例化和绘制，因此它们要么都在前面，要么都在后面。在这种情况下，您不希望使用 instancing。但是，如果只有球体而没有透明墙，则可以启用实例化，并且它们仍将按相对于彼此的深度排序。
![SpheresInFront](https://user-images.githubusercontent.com/82224783/232828692-d5593130-6afb-4af1-9fb4-b82528d2f6c6.png)
![SperesBehind](https://user-images.githubusercontent.com/82224783/232828791-63834148-244c-4c5a-b925-cb5408d098a2.png)

r_meshInstancingBucketSortScatterBatchSize - 这可以通过在处理网格时更改在单个多线程任务中一起批处理的对象数量来调整以优化性能。在用于收集以下性能数字的机器上，任何值 >= 64 都没有明显的差异。但这也可能是由于正在测试的特定硬件/平台。

r_meshInstancingDebugForceUniqueObjectsForProfiling - 这是一个调试实用程序，供致力于实例化以测试性能的引擎维护人员使用，内容创建者可以忽略它。

# 性能测量示例
以下是一些示例场景，您可以使用这些场景来大致了解何时启用实例化可能会和可能不会带来性能优势。

为了保持一致性，所有性能测量均使用默认摄像机位置完成，并在以下设备上的 Editor 中测量：
 - 设备: PC
 - OS: Windows
 - 版本: 10
 - CPU: Intel I9-10900X
 - GPU: NVidia GeForce RTX 2080 SUPER
 - 内存: 64GB

## ROSCon 苹果采摘器演示
此场景混合了模型和材质，但相同模型 + 材质组合（树、草、苹果）的许多副本

![apple_orchard](https://user-images.githubusercontent.com/82672795/234366655-be5ac9ee-a548-48f1-bd81-df6e13c262c0.png)

Note: these measurements were taken at commit 671e309e4e50a6d832cdb4e2d19926c337b7647d, before the meshes had been merged

### Vulkan
22.5ms->12.5ms GPU

26ms->14.5 ms CPU

### DX12
21.7ms -> 19.7ms GPU

18.5ms -> 13.8ms CPU


## MultiplayerSample
此场景包含一些可以实例化的对象，但不会大量重复同一对象。尽管有一些可以实例化的东西，但开销会抵消任何收益。
![mps](https://user-images.githubusercontent.com/82672795/234366858-abb11e62-946c-46a7-a2ed-f39fa315a984.png)

### Vulkan
11.5ms GPU (no change)

8.8ms->9.4ms CPU

### DX12
17ms GPU (no change)

13ms CPU (no change)

## 10kVegInstancesTest
此场景包含同一对象的许多实例，这些实例是通过植被系统随时间生成的。
![AutomatedTesting_10kVegInstances](https://user-images.githubusercontent.com/82672795/234366956-b78b9f86-f759-41ab-b067-66390c892046.PNG)

注意：由于在有实例化和无实例化的情况下都会发生崩溃，因此未测量 Vulkan 时间

### DX12
Spawn Time: 16.7->11.2 seconds

15ms->10.8ms GPU

20.5ms->15ms CPU

## 10kEntityCpuPerfTest
此场景具有同一对象的许多实例，这些实例实例化为预制件。
![AutomatedTesting_10kEntityCpuPerfTest](https://user-images.githubusercontent.com/82672795/234367405-4a41f2bf-2944-43eb-94ff-860f8e94fef8.PNG)

### Vulkan
Spawn Time: 14.2->3.6 seconds

10.2ms->3.7ms GPU

16.2ms->9.1ms CPU

### DX12
Spawn Time: 8.8->4.6 seconds

11.8ms->10.1ms GPU

20.8ms->15.0ms CPU

# Prefabs
无需使用预制件系统即可使用实例化绘制调用。具有相同网格 + 材料组合的任意两个实体。但是，对同一预制件进行多个实例化是一种方便的方法，可确保对原始预制件所做的任何更改都应用于该预制件的所有实例，这样您就不会最终得到多个设置略有不同的实体，因此它们不会一起实例化。但是，通过材质组件（例如，更改基色）或通过脚本进行属性覆盖将导致网格无法一起实例化，即使该更改是在预制件中进行的，以便将其应用于该预制件的所有副本。在这种情况下，更好的工作流程是在材质编辑器中创建新材质，并在其中应用更改。如果您希望在创作材质时利用预制件的继承/覆盖属性来仅覆盖几个属性，则可以利用材质继承系统。具有相同父项的子材质不会一起实例化，但会同时实例化同一子材质的两个副本。
