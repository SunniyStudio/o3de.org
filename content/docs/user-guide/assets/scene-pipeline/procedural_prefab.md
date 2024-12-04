---
linkTitle: Procedural Prefab
title: Procedural Prefab
description: 有了程序化Prefab，您就可以使用 Python 脚本从场景源文件中为 Open 3D Engine (O3DE) 创建Prefab资产。
weight: 400
toc: true
---

有了程序化Prefab，您就可以从源场景文件中以Prefab资产的形式写出**Open 3D Engine (O3DE)** 本地实体组件场景描述。您可以编写 Python 脚本，解析导入的源场景并写出任意数量的Prefab资产。有了程序化Prefab，构建Prefab的部分流程就可以转回到数字内容创建 (DCC) 工具（如 Maya 和 Blender）中，从而简化艺术家和设计师的资产创建工作流程。

该工作流程可使用场景管道内的 Python 脚本或预制板 gem 生成默认程序预制板时生成程序预制板。

![High level view of how procedural prefabs work with Python scripts](/images/user-guide/assets/scene-pipeline/proc_prefab_workflow.png)

## 程序Prefab的特征

Prefab是对场景实体、组件和属性的预制描述。Prefab的作用是轻松构建和修改大型复杂的游戏世界。用户可以在 O3DE 编辑器中创建预制板。

关于 [Prefab](docs/user-guide/interactivity/prefabs) 功能的更多信息。

也可以定义一个程序来生成产品Prefab资产。Python 脚本是定义程序化预制产品资产的最典型方式。此功能可将预制模板保存在源美术场景文件（如 FBX 或 glTF）中。内容创建者可以自由地在关卡中实例化这些程序化Prefab，当源场景更新时，它们也会自动更新。

这些程序化预制板资产在编辑器中被视为只读预制板，因为它们是根据原始源场景文件的定义和生成预制板的脚本创建的。事实上，程序化预制板被放置在资产缓存中，因此不应直接对其进行更改。

## 制作程序预制板

项目需要启用 Prefab gem 才能获得程序预制功能。启用该 gem 后，所有源场景文件都会生成默认的程序预制板，Python 脚本也能生成预制板。

{{< note >}}
可以通过将注册表键值"/O3DE/Preferences/Prefabs/UseProceduralPrefabs"设置为False来关闭默认程序预置。
{{< /note >}}

### 使用 Python 创建程序化预制板

可以使用 Python 脚本创建程序化预制板，并将其挂接到场景管道中。该脚本将在场景清单中添加一个预制组规则，该规则描述了它将使用的实体和组件。事实上，如果场景描述不止一个，脚本可以在场景清单中添加多个 Prefab 组。脚本可以从源文件中分割出复杂的场景，并用游戏组件填充实体。

{{< note >}}
还应注意的是，如果开发团队觉得编写 C++ 场景构建器更得心应手，也可以用 C++ 更新场景流水线。
{{< /note >}}

### 构建默认的程序预制板

预制 gem 会将所有网格数据节点分离为网格组，使用实体和网格组件组装预制场景描述，并可选择为网格组件分配材质。该逻辑会为遇到的"transform only"节点创建空实体，但为了达到最佳效果，建议将网格数据存储到预计会在程序预制板中出现的每个节点中。

{{< note >}}
这里有使用 [o3de_default_material](/blog/posts/blog-udp) 用户定义属性用 Atom 材质标记网格数据节点的说明。
{{< /note >}}

## 在 O3DE 编辑器中使用程序预制板

在大多数情况下，程序预制板的使用方式与 O3DE 编辑器中使用普通自制预制板的方式相同。它们可以多次实例化到一个关卡中，如果源场景被修改，可以一次性更新，并且可以放置在实体大纲器中的自制预制板中。

### 在编辑器中实例化程序预制板

一旦源场景文件生成了程序预制资产（扩展名为`.procprefab`），它们就会在 “资产浏览器 ”中显示为`(Procedural Prefab)`类型。

![Asset Browser](/images/user-guide/assets/scene-pipeline/procprefab_ug_ab.png)

#### 在场景中实例化程序Prefab

O3DE 编辑器用户可以通过右键单击关卡 3D 场景并选择"Instantiate Procedural Prefab..."选项来实例化程序Prefab。

![Procedural Prefab Context Menu](/images/user-guide/assets/scene-pipeline/procprefab_ug_pp_context.png)

右键菜单"Pick Procedural Prefab"将打开，允许用户选择要实例化的程序Prefab资产。

![Pick Procedural Prefab](/images/user-guide/assets/scene-pipeline/procprefab_ug_pp_pick.png)

也可以简单地将程序Prefab从 “资产浏览器 ”拖放到关卡 3D 场景中。

![Procedural Prefab in the Entity Outliner](/images/user-guide/assets/scene-pipeline/procprefab_ug_pp_eo.png)

#### 在 “实体大纲视图 ”中实例化程序预制板

使用右键菜单的"Instantiate Procedural Prefab..."选项，并使用"Pick Procedural Prefab"菜单选择程序预制板，可以在实体大纲视图中直接实例化程序预制板。

![Entity Outliner Procedural Prefab](/images/user-guide/assets/scene-pipeline/procprefab_ug_pp_eo_menu.png)

{{< note >}}

在实体检查器中，Procedural Prefab 资产组件和属性显示为 “只读”，因为属性是从组件和属性的脚本定义中生成的。程序预制板内的实体也是不可变的。

![Entity Inspector Procedural Prefab Read Only](/images/user-guide/assets/scene-pipeline/procprefab_ug_pp_ei_readonly.png)

{{< /note >}}

### 程序预制板被用于自动生成的预制板中

在 “实体大纲视图”（Entity Outliner）中，可以将程序预制板添加到已创建的预制板实例中。为此，用户需要进入Prefab的焦点模式。

![Entity Outliner Authored Prefab](/images/user-guide/assets/scene-pipeline/procprefab_ug_pp_eo_authored.png)

用户可使用右键菜单**Instantiate Procedural Prefab...**选项将程序预制板补丁添加到已创建的预制板中。

![Entity Outliner Procedural Prefab Inside](/images/user-guide/assets/scene-pipeline/procprefab_ug_pp_eo_inst.png)

用户不能在 “实体大纲视图”（Entity Outliner）中添加实体，也不能在 “实体检查器”（Entity Inspector）中将组件添加到任何程序Prefab实例中。资产处理器可能会随时重新导出程序预制模板，这将在编辑器之外重新写入`.procprefab`文件。程序预制模板由源代码场景文件支持。

### 将程序化Prefab保存为自制Prefab

编辑器还可以将程序化预制板资产保存为自制预制板源文件。这样编辑器用户就可以对程序预制板资产进行调整。`(Procedural Prefab)`资产的上下文菜单项**Save as Prefab...** 将提示输入文件名，以保存为自制预制板文件。

这将允许设计人员调整预制模板的实体布局、组件和属性

![Save As Prefab](/images/user-guide/assets/scene-pipeline/procprefab_ug_pp_ab_saveas.png)

{{< note >}}
自动生成的预制板一旦保存为自动生成的预制板，就不会从程序预制板资产中获得更新。
{{< /note >}}

### 切换每个源资产的程序预置

有时，团队有理由关闭源场景文件的默认程序预置，例如当脚本被指定创建预置组时。

使用**Scene Settings**对话框可以防止生成默认的程序预制板。在 “资产浏览器 ”中选择源场景文件，使用上下文菜单并选择**Edit Scene Settings...** 选项。

![Edit Settings](/images/user-guide/assets/scene-pipeline/procprefab_ug_pp_es_toggle.png)

要禁用程序Prefab生成，请将**Create default procedural prefab?** 的切换设置为关闭值。

## 默认程序预制的基本准则

默认程序预制板逻辑是通过 “最佳猜测 ”来描述场景中的实体、网格和材质。本节旨在帮助指导艺术家从他们所选择的 DCC 工具中导出最简洁的默认程序预制版本。

正如[场景格式支持](docs/user-guide/assets/scene-settings/scene-format-support)用户指南中详细说明的那样，O3DE 场景资产导入使用：

- 右旋坐标系
- +Z 为向上轴
- -Y为观察轴
- 基本测量单位为每世界单位一立方米

程序Prefab不支持设置细节级别，也不支持为动画设置Actor或动作。

用户定义的 `o3de_default_material` 属性，默认程序Prefab会使用产品资产路径为 `.azmaterial` 资产分配 AZ 材质。

