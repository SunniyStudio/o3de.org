---
linkTitle: 转换关卡
title: 转换关卡
description: 了解如何将 Lumberyard .slice 和 .ly 文件转换为 O3DE .prefab
weight: 400
toc: true
---

本教程教您如何使用 **SerializeContextTools** 将传统关卡文件（*.ly*、*.slice*）转换为 O3DE Prefab。

|O3DE 体验 |完成时间 | 功能聚焦                                             |最后更新 |
| - | - |--------------------------------------------------| - |
| 中级 |15 分钟 | 将 `.ly` 和 `.slice` 文件转换为 `.prefab` 并在 O3DE 中测试它们 | February 27, 2024 |

{{< note >}}
这是位于 O3DE 项目中的 C++ 工具。您需要自己构建此工具才能使用它。查看 [从 GitHub 设置 O3DE](/docs/welcome-guide/setup/setup-from-github/) 以了解如何制作 O3DE 解决方案。
{{< /note >}}

## 启动 Legacy Asset Converter 应用程序

打开代码解决方案并找到`SerializeContextTools`项目。在使用 Visual Studio 时，您可以右键单击它并 **“Set as Startup Project”**。如果您使用的是其他 IDE，则过程应类似。

![Serialize Context Tools project](/images/learning-guide/tutorials/lumberyard-to-o3de/serialize-context-tools.png)

在 Visual Studio 的同一个右键单击菜单中，您有 **“Properties”** 选项。使用它来更改启动参数。它们如下所示，只需使用正确的路径更新`YOUR_LUMBERYARD_REPO`即可。

```cmd
convert-slice -specializations=editor -project-path=YOUR_LUMBERYARD_REPO\dev\StarterGame -files=YOUR_LUMBERYARD_REPO\dev\StarterGame\Levels\*.ly -slices=YOUR_LUMBERYARD_REPO\dev\Gems\StarterGame\*.slice,YOUR_LUMBERYARD_REPO\dev\StarterGame\slices\*.slice
```

然后将其粘贴到 Debugging 部分下的`Command Arguments`字段中。同样，如果您使用的是其他 IDE，则过程应该类似。

![Launch arguments](/images/learning-guide/tutorials/lumberyard-to-o3de/serialize-context-tools-launch.png)

现在，您还需要通过以下两种方法删除`Instance.cpp`文件中的`AZ_Assert`：

```cpp
void Instance::ClearEntities()
bool Instance::RegisterEntity()
```

最后，您将能够启动该过程。一旦一切都结束了，你将在应用程序拆解期间遇到一个断言（你将在调用堆栈中看到 `SerializeContextTools.exe!AZ::ModuleManager::UnloadModules()`）。只需在程序发生时退出程序，'.prefab' 文件已经在 `.slice` 文件旁边生成。

## 将Prefab文件复制到 O3DE 项目

如 [转换组件](converting-components) 部分所述，您必须 **保持相同的文件层次结构** 才能找到嵌套切片。在此步骤中，您必须手动复制该进程生成的每个`.prefab`文件。

这些是您需要为 StarterGame 关卡复制的文件，只需在复制后擦除 `.slice` 文件，因为您不需要它们。

|Lumberyard 位置 |O3DE 位置 |
| - | - |
| dev\StarterGame\Levels\Game\SinglePlayer\SinglePlayer.prefab | YOUR_PROJECT\Levels\SinglePlayer\SinglePlayer.prefab |
| dev\StarterGame\Slices | YOUR_PROJECT\Slices |
| dev\Gems\StarterGame\Environment\Assets\Slices | YOUR_GEM\Assets\Slices |
| dev\Gems\StarterGame\Environment\Assets\Objects\Props\Diorama0*X*.prefab | YOUR_GEM\Assets\Objects\Props\Diorama0*X*.prefab |
| dev\Gems\StarterGame\Weapons\Assets\SGD1000\Slices | YOUR_GEM\Weapons\Assets\SGD1000\Slices |

## 打开并编辑已转换的关卡

{{< caution >}}
如果在启动 O3DE 之前，如果 Lumberyard 资产处理器仍在运行，请确保从任务栏 **关闭 Lumberyard 资产处理器**。否则，O3DE Asset Processor 将无法启动，您将无法打开嵌套的Prefab
{{< /caution >}}

现在您将能够 **启动 O3DE 并打开 SinglePlayer 关卡**。对于一些实体，您将遇到一些有关不兼容组件或未解析组件的警告。它们应该不多，您可以手动修复它们（如果警告未报告实体名称，请在代码编辑器中打开 .prefab 文件以查找有问题的实体或组件）。

此外，预计一些实体（灌木、橡树和am_air_cockpit_g1）具有灰色材质。此问题将在不久的将来得到修复。

如上一个 [转换组件](converting-components)部分所述，您必须在关卡中 **自己添加照明** 并在编辑器设置中展开 Perspective Far Plane（透视远平面）以获得以下结果。

![StarterGame level prefab](/images/learning-guide/tutorials/lumberyard-to-o3de/starter-game-prefab.png)

