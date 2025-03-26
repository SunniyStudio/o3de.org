---
title: "GPU-Memory-Profiling"
description: ""
toc: false
---

_NOTE: This page pertains to the development branch of O3DE only_

GPU Memory Profiling is currently provided from the _Dear ImGui_ interface. To access the memory profiler, press the `Ctrl+Home` hotkey with the viewport active and select the following menu option:

![image](https://user-images.githubusercontent.com/87345238/158007193-42cdba7c-728b-445e-ab2f-ec52dc1f1054.png)

In the subsequently opened capture dialog, select the _Enable GpuMemoryView_ checkbox to bring up the _Gpu Memory Profiler_ window as seen below (a more recent screenshot will also show buttons to save the memory capture to CSV or load an existing capture from CSV):

![image](https://user-images.githubusercontent.com/87345238/158007252-975fb809-e8ba-4496-90ab-5194f3c59d8b.png)

The _Capture_ button produces a memory snapshot that results in tabulated and treemap-organized data to analyze memory allocations in the renderer. In this empty scene, hitting _Capture_ will change the window to look as follows (after resizing the window):

![image](https://user-images.githubusercontent.com/87345238/158007324-a6aab195-361a-46ba-966b-98e1b0728b02.png)

The top section has a few options to enable the host-local (e.g. CPU) and device-local (e.g. GPU) memory treemap views, and checkboxes to filter individual entries based on memory type. Below there are two tables corresponding to aggregated per-pool statistics, and individual allocations below. The fragmentation percentage column reports (as a percentage), 1 minus the ratio of the largest free block to the total amount of free memory in the parent buffer or pool.

The individual allocation table at the bottom shows more granular information per allocation, the parent pool the allocation was obtained from, and the bind flags used.

For a more visual depiction of the memory usage, you may enable the _Show host memory treemap_ or _Show device memory treemap_ checkboxes to view the host or gpu memory distributions accordingly. For example, enabling the device memory treemap displays the following window:

![image](https://user-images.githubusercontent.com/87345238/158007447-d547fd6b-6029-4172-bba9-39f9b366f600.png)

To interpret the treemap, each allocation is represented by a single rectangle, which are grouped with sibling allocations from the same memory pool in larger subdivisions. The allocations may be hovered with a cursor to display statistics about the allocation in question or the parent pool. This view supports highlighting treemap entries by allocation type, and statistics for individual allocations may be pinned to the top using left-click selection. The following view demonstrates a selected entry and all texture memory highlighted to visualize the proportion of used memory occupied by texture data.

![image](https://user-images.githubusercontent.com/87345238/158007521-280d851f-df90-49d6-b9f2-0bb2fa6391d9.png)
