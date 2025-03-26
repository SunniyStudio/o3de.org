---
title: "[Atom]-Mesh-Instancing_-For-Content-Creators"
description: ""
toc: false
---

This document covers information you might need to know about instancing if you are creating levels in O3DE.

See also:
- [Mesh Instancing: For Shader Authors](https://github.com/o3de/o3de/wiki/%5BAtom%5D-Mesh-Instancing:-For-Shader-Authors)
- [Mesh Instancing: For Engine Maintainers/Contributors](https://github.com/o3de/o3de/wiki/%5BAtom%5D-Mesh-Instancing:-For-Engine-Maintainers-Contributors)

# What can be instanced
Any object that uses the same mesh + material combination can be instanced together in a single draw call, as long as the mesh component does not enable the 'Use Forward Pass IBL' setting. If individual meshes have material overrides that differ from the overrides on another entity, they will not be instanced together, but the other meshes that do use the same materials will still be instanced. 

For example, imagine you have a house model with two material assignment slots, one for the walls and one for the roof. Then imagine you place two entities with the house in the world, both using the same material assigned to the roof, but with different materials assigned to the walls. Even though not all material assignments match, the roofs will still be instanced together in a single draw, while the walls will have separate, unique draw calls.

Using a material property override on the material component (or via scripting and the material component bus) will generate a unique material under the hood, causing any mesh with that material to not be instanced. So if you had two houses from the above example, only this time they have the same wall material, but one of them uses the material instance editor in the material component to override the base color, the walls will again end up not being instanced together, even though they are both referring to the same material on disk.

Note: some engines support overriding specific properties such as base color in a way that allows per-instance variation while still using a single draw call. This is not supported in o3de, and to support this you would need to fork and modify the MeshFeatureProcessor to meet your needs.

# Enabling Instancing
There is no manual work needed to identify an object as something that can be instanced - it is all handled under the hood. However, instancing as a whole is disabled by default. It can be enabled via a cvar: r_meshInstancingEnabled = true. Once instancing is enabled, everything goes down the instancing path, even unique objects with unique materials. They will just end up as instanced draw calls with 1 instance. This incurs some overhead, so for scenes that don't use multiple instances it's best to leave r_meshInstancingEnabled off. Eventually this constraint can be relaxed if improvements are made to better handle unique objects alongside instanced objects, but initially you must opt into instancing if you want to use it.

# CVars
r_meshInstancingEnabled - Set to true to enable instanced draw calls in the MeshFeatureProcessor
r_meshInstancingEnabledForTransparentObjects - Set to true to enable instanced draw calls for transparent objects in the MeshFeatureProcessor. Use this only if you have many instances of the same transparent object, but don't have multiple different transparent objects mixed together.

For example: In this scene, one orb is in front of the wall, the other is behind. But because they are being instanced and drawn at the same time, they are either both in front or both behind. In this case, you would not want to use instancing. But if there were only orbs and no transparent walls, you could enable instancing and they would still be sorted by depth relative to each other.
![SpheresInFront](https://user-images.githubusercontent.com/82224783/232828692-d5593130-6afb-4af1-9fb4-b82528d2f6c6.png)
![SperesBehind](https://user-images.githubusercontent.com/82224783/232828791-63834148-244c-4c5a-b925-cb5408d098a2.png)

r_meshInstancingBucketSortScatterBatchSize - This can be tweaked to tune performance by changing the number of objects that are batched together in a single multithreaded task while processing meshes. In practice on the machine used to gather performance numbers below, there was no noticeable difference for any value >= 64. But that could also be due to the particular hardware/platform that was being tested.

r_meshInstancingDebugForceUniqueObjectsForProfiling - This is a debug utility for engine maintainers working on instancing to test performance, and can be ignored by content creators.

# Example Performance Measurements
Here are some example scenes you can use to get a rough idea of when enabling instancing might and might not have a performance benefit.

All performance measurements were done using default camera positions for consistency, and measured in the Editor on the following device:
 - Device: PC
 - OS: Windows
 - Version: 10
 - CPU: Intel I9-10900X
 - GPU: NVidia GeForce RTX 2080 SUPER
 - Memory: 64GB

## ROSCon apple picker demo
This scene has a mix of models and materials, but many copies of the same model + material combinations (trees, grass, apples)

![apple_orchard](https://user-images.githubusercontent.com/82672795/234366655-be5ac9ee-a548-48f1-bd81-df6e13c262c0.png)

Note: these measurements were taken at commit 671e309e4e50a6d832cdb4e2d19926c337b7647d, before the meshes had been merged

### Vulkan
22.5ms->12.5ms GPU

26ms->14.5 ms CPU

### DX12
21.7ms -> 19.7ms GPU

18.5ms -> 13.8ms CPU


## MultiplayerSample
This scene has some objects that can be instanced, but does not repeat the same object heavily. Despite having some things that can be instanced, the overhead offsets any gains.
![mps](https://user-images.githubusercontent.com/82672795/234366858-abb11e62-946c-46a7-a2ed-f39fa315a984.png)

### Vulkan
11.5ms GPU (no change)

8.8ms->9.4ms CPU

### DX12
17ms GPU (no change)

13ms CPU (no change)

## 10kVegInstancesTest
This scene has many instances of the same object, which are spawned over time via the vegetation system.
![AutomatedTesting_10kVegInstances](https://user-images.githubusercontent.com/82672795/234366956-b78b9f86-f759-41ab-b067-66390c892046.PNG)

Note: no Vulkan times were measured due to a crash that occurs both with and without instancing

### DX12
Spawn Time: 16.7->11.2 seconds

15ms->10.8ms GPU

20.5ms->15ms CPU

## 10kEntityCpuPerfTest
This scene has many instances of the same object, which are instantiated as prefabs.
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
It is not necessary to use the prefab system in order for something to use instanced draw calls. Any two entities with the same mesh + material combination. However, making multiple instantiations of the same prefab is a convenient way to make sure that any change made to the original prefab is applied to all instances of that prefab so that you don't end up with multiple entities that have slightly different settings making it so they are not instanced together. However, making a property override via the material component (to, for example, change the base color) or via scripting will cause the meshes to not be instanced together, even if that change is made in a prefab such that it applies to all copies of that prefab. A better workflow in this case would be to create a new material in the material editor and apply the change there. If you're looking to leverage the inheritance/override properties of prefabs while authoring materials to override just a few properties, instead you could leverage the material inheritance system. Child materials that have the same parent will not be instanced together, but two copies of the same child material will be.
