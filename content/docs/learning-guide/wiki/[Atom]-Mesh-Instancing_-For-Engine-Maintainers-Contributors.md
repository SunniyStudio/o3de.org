---
title: "[Atom]-Mesh-Instancing_-For-Engine-Maintainers-Contributors"
description: ""
toc: false
---

This document covers everything you need to know to modify, update, and maintain the instancing implementation

See also:
- [Mesh Instancing: For Content Creators](https://github.com/o3de/o3de/wiki/%5BAtom%5D-Mesh-Instancing:-For-Content-Creators)
- [Mesh Instancing: For Engine Maintainers/Contributors](https://github.com/o3de/o3de/wiki/%5BAtom%5D-Mesh-Instancing:-For-Engine-Maintainers-Contributors)

# High Level Overview
When r_meshInstancingEnabled = true, the MeshFeatureProcessor registers cullables with the culling system using the TYPE_RPI_VisibleObjectList. This tells the culling system not to submit the draw items to views directly, but instead it creates a list of visible objects for each view, which the MeshFeatureProcessor can then process to generate per-view instance buffers based on what is currently visible for each view. The only instance data we store in the per-view instance buffers is the objectID, which can be used to look up the ObjectToWorld, ObjectToWorldInverseTranspose, and ObjectToWorldPrev transformation matrices. There is a MeshInstanceManager class that, given a 'key' (model + material + lod index + mesh index) determines what 'group' a given mesh + material belong to. These groups live in a StableDynamicArray, which stores the groups in pages. Each page is treated as a bucket in a bucket sort. When processing the VisibleObjectList, the MeshFeatureProcessor will first sort the visible objects into their respective buckets, then sort within those buckets so that the objectId's are sorted by instance group and then by depth. Once the buckets are sorted, the data is combined to create a per-view instance buffer filled with visible objectId's.

# High Level Performance Considerations
The processing of the visible object lists has an overhead, which is greatly offset by the reduction in draw count for scenes with lots of visible objects. However, the initial implementation has an all-or-nothing approach to instancing, so even objects where there is only one instance in the scene will be part of this process. Thus, instancing is disabled by default. The bucket sort does a reasonable job of handling the case of mostly unique objects, but not well enough to be enabled all the time. There is a debug cvar that can be used to more easily verify this: r_meshInstancingForceOneObjectPerDrawCall.
