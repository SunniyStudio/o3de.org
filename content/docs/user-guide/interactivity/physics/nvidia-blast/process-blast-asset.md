---
description: ' 处理 NVIDIA Blast 的资产。'
title: 处理 NVIDIA Blast 的资产
weight: 300
draft: true
---


从 Houdini 导出的 Blast 资产必须由 Asset Processor 处理才能生成运行时资产。有两种方法可以处理 Blast 资产以在 O3DE 中使用：一种是使用 Python Asset Builder 的自动方法，另一种是选择要处理的网格的手动方法。

{{< important >}}
与手动设置要处理的爆破网格相比，自动处理网格的时间更少。但是，了解这两个过程很重要。您可以使用自动过程进行初始导出，然后手动编辑网格资源，以便为自定义法线或顶点颜色流添加修改器。

在 **FBX Settings** 中手动编辑`.fbx`资产后，将为`.fbx`资产创建一个`.assetinfo`文件。`.assetinfo`文件会阻止 **Python Asset Builder**自动处理网格资产。对源 `.fbx`文件所做的任何进一步更改，例如添加或删除破裂级别或块，都必须在 **FBX Settings** 中手动编辑。
{{< /important >}}

**内容**
+ [自动处理 Blast 资产](#process-blast-assets-automatically)
+ [手动处理 Blast 资产](#process-blast-mesh-assets-manually)

## 自动处理 Blast 资产

当自动处理爆炸资产时，将创建一个爆炸切片资产，该资产将爆炸网格块添加到 **Blast Family Mesh Data** 组件。如果您的 BLAST 资产包含数十个数据块，则使用适用于 NVIDIA Blast 的 Python 资产生成器进行自动处理可以节省一些时间。

{{< note >}}
自动处理 NVIDIA Blast 的资源要求您的项目已在启用 **Python Asset Builder** 和 **EditorPythonBindings** gem 的情况下构建。有关更多信息，请参阅 [Python Asset Builder gem](/docs/user-guide/assets/builder).
{{< /note >}}

**自动处理 Blast 资源**

1. 将 Blast 资产的 `.blast` 和 `.fbx`文件复制到项目的资产目录中。

1. 启动 O3DE 编辑器。**Asset Processor** 检测`.blast` 和 `.fbx`文件，并生成运行时网格资产、爆炸资产和爆炸切片资产。

1. 您可以在 **Asset Processor** 的 **Jobs** 选项卡中验证资产是否已成功处理。如果您需要重新处理资产，请执行以下操作：

   1. 在 **Asset Processor**中，选择 **Assets** 选项卡。

   1. 右键单击资产列表中的资产以打开上下文菜单。

   1. 从上下文菜单中选择 **Reprocess File** 。

   ![Automatic process of Blast assets.](/images/user-guide/physx/blast/ui-blast-process-automatic.png)

## 手动处理 Blast 网格资产

手动处理爆炸资产需要您为每个块网格添加一个网格组，以便 **Asset Processor** 可以生成运行时资产。手动处理还要求您将每个运行时网格添加到 **Blast Family Mesh Data** 组件。如果需要向爆炸网格块添加修改器，例如指定顶点颜色流，则必须使用此手动过程。

**手动处理 Blast 资源**

1. 将 Blast 资产的`.blast` 和 `.fbx`文件复制到项目的资产目录中。

1. 启动 O3DE 编辑器。

1. 在 **Asset Browser** 中找到`.fbx`资源，然后双击该资源以打开 **FBX Settings**。

1. 选择 **Meshes** 选项卡。

1. 在`.fbx`中为每个网格创建一个 **Mesh group** 。

   1. 如果存在现有的 **Mesh group**，请确保它仅具有从 **Select Meshes** 列表中选择的第一个网格。

   1. 通过选择 **Add another mesh** 添加新的 **Mesh group**。

   1. 通过选择 **Select meshes** 右侧的 **Hierarchy** 按钮并从列表中选择下一个网格，将下一个网格添加到新的 **Mesh group** 中。

   1. 重复步骤 **b** 和 **c**，直到网格列表中的每个网格都分配给其自己的 **Mesh group**。

      ![Create mesh groups for Blast assets.](/images/user-guide/physx/blast/ui-blast-asset-mesh-groups.png)

1. **可选:** 如果网格需要特殊处理，例如**Mesh (Advanced)**修改器提供的顶点颜色流，请根据需要向每个网格组添加修改器。

1. 选择 **FBX Settings** 右下角的 **Update** 按钮。**Asset Processor** 生成运行时资产。此时将出现 **File progress** 窗口，并显示有关该过程的反馈。
