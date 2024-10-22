---
title: Material Canvas
description: 了解如何使用 “材质画布 ”从 Atom 中的材质图形创建材质类型和着色器。
toc: true
---

在**Open 3D Engine (O3DE)** 中创建材质类型和着色器通常是一个耗时的过程，需要熟悉引擎、渲染器、着色器、数据类型、文件格式以及所有内容的组合方式。它涉及手动编辑和管理多个文件、编写**Amazon Shading Language (AZSL)**着色器代码、在 JSON 中编辑、启动资产处理器编译它们、处理任何报告的错误，然后使用 O3DE 编辑器或材质编辑器预览和自定义资产。

**Material Canvas** 通过提供一个可视化脚本编辑器，以及熟悉的工具、工作流程、状态报告和实时预览，大大简化、加快并自动创建自定义着色器和材质类型。

通过拖放、连接和配置节点来构建材质图形，这些图形会自动转换为着色器、材质类型和材质的标准源文件。资产处理器会自动识别并处理生成的源文件，因此其他 O3DE 系统和组件也能使用这些材质。默认情况下，只要打开、编辑和保存图形，Material Canvas 就会重新生成并覆盖文件。视口会在更改和处理完成后迅速更新以显示结果。

Material Canvas 建立在与 Script Canvas 和 Material Editor 等其他 O3DE 工具相同的基础之上。它以数据为驱动、可定制、可扩展，并可通过设置注册表、Python 和 C++ 编写脚本。当前所有的材质图节点都定义在包含 AZSL 片段的 JSON 文件中。您可以在 Material Canvas 中编辑和创建新的材质图形节点。

有关 Atom 工具常用功能、菜单、可停靠面板和处理文档的更多信息，请参阅 [Atom 工具常用功能](/docs/atom-guide/look-dev/tools/atom-tools-common-features/)。 

## 快速启动
### 启动 Material Canvas
启动 “材质画布 ”有多种方法。
- 从O3DE编辑器，选择 **主菜单 > Tools > Material Canvas**
- 从Material Component上下文菜单，选择**Open Material Canvas...**
- 从Asset Browser，**双击**一个材质图表或其它与Material Canvas兼容的文件类型。
- 从Asset Browser，**右击**一个材质图表或其它与Material Canvas兼容的文件类型。然后选择 **Open in Material Canvas...** 
- Material Canvas 也是一个独立的可执行文件，可以直接从文件浏览器或命令控制台启动。
    - 启动可执行程序 `<build>\bin\profile\MaterialCanvas.exe`。 指定`--project-path`，跟上你的项目的路径，就像是命令行参数一样。例如：
  ```shell
   .\<build>\bin\profile\MaterialCanvas.exe --project-path MyProject
  ```

### 创建材质图形
您有多个选项可以开始编辑材质图形。
- 默认情况下，“材质画布 ”以一个无标题的空白材质图文档开始，您可以使用该文档创建新的材质图。
    {{< note >}}
  在保存图形之前，生成的文件会暂时输出到项目中的`Assets/Materials/Generated` 文件夹。保存图形后，生成的文件将输出到与图形相同的文件夹。
    {{< /note >}}
- 根据模板创建新的材料图形。
  - 选择 **Main Menu > File > New > New Material Graph Document...**。这将打开 **Create Material Graph Document** 对话框。
  - 选择一个模板作为新材料图形的基础。
    - 模板是以特殊的`.materialgraphtemplate`扩展名保存的材质图形，指定它们作为新图形的起点。
  - 为新材料图形选择路径和文件名。
- 打开现有材料图形
  - 选择 **Main Menu > File > Open > Open Material Graph Document...**.
- 从 “资产浏览器 ”打开或创建材质图形。

### 创建节点
您可以通过将节点从节点调板拖动到图形视图或使用图形视图上下文菜单来创建节点。
1. 将一个输出节点（如基础 PBR）从节点调板拖动到图形视图。您可以从任何节点开始，但如果没有输出节点驱动，则不会进行任何处理。
2. 2. 注意状态栏显示正在生成和处理文件。
3. 注意视口更新以在模型上显示生成的材质。

### 配置节点
您可以直接在图形视图或检查器中的节点上更改节点属性。如果选择基础 PBR 或标准 PBR 作为输出节点，材质最初是白色的。

例如，更改材料的颜色：
1. 将base color更改为红色 (1,0,0,1)。
2. 注意状态栏显示文件正在再生和重新处理。 
3. 注意视口更新后，材质和模型不再是红色。

### 连接节点
连接是一个节点的输出值与另一个节点的输入值之间的链接。

下面的指令演示了连接节点的常见情况：
1. 在图形的任意位置创建 Float4 或 Color 输入节点。这些节点可以互换，但在用户界面中代表不同控件的值。
2. 将连接线从输出插槽**拖放**到输出节点上的基色插槽。连接完成后，节点上的可编辑值将消失，并在 “检查器 ”中显示为灰色。
3. 请注意，视口模型的颜色已从之前的颜色变为输入节点的颜色。更改输入节点的属性值，注意视口中颜色的更新。

对于**输入**节点，您也可以在 “材质编辑器 ”中打开生成的材质或材质类型，并在其中配置属性。

通过在功能、纹理采样、时间、变换和其他节点之间添加和嵌套连接，您可以创建更有趣、更高级的图形。在 `Gems/Atom/Tools/MaterialCanvas/Assets/MaterialCanvas` 文件夹中可找到多个示例。

## Navigating Material Canvas
When you launch Material Canvas, you will see the following main window.

![Material Canvas](/images/atom-guide/tools/material-canvas.jpg)

有关 Atom 工具常用功能、菜单、可停靠面板和处理文档的更多信息，请参阅 [Atom 工具常用功能](/docs/atom-guide/look-dev/tools/atom-tools-common-features/)。 

### 文档类型和视图
Material Canvas 支持创建、打开和编辑多种类型的文档。

#### 材质图文档
材质图文档（`.materialgraph`）是 Material Canvas 中的主要文档类型。每个打开的材质图形都有一个相应的二维网格图形视图，大部分编辑工作都在该视图中进行。所有与节点、连接、槽值、定位、选择相关的数据以及与图形相关的任何其他元数据都存储在材质图形文件中。

#### 材质图节点文档
Material Graph Node 文档（`.materialgraphnode`）是 JSON 文件，用于定义可在 Material Canvas 中创建的每种类型的节点。它们包含节点及其插槽的唯一 ID、唯一名称、显示名称、描述、数据类型和默认值的设置。此外，它们还声明了 AZSL 着色器代码片段和其他元数据，Material Canvas 会使用这些代码和元数据来组装完整的着色器。当前所有的 Material Canvas 节点都是通过材质图节点配置文件指定的。也可以使用 C++ 或 Python 以编程方式创建和注册节点。

#### 着色器源数据配置文档
着色器源数据配置文档（`.shader`）用于编辑着色器配置文件。它反映了所有支持的数据类型，因此你可以在检查器中或使用 Python 编辑着色器源数据文件。在检查器中，会列出所有选项。这对于编辑着色器源数据文件和为材质图形输出节点创建新模板非常有用。

### 主菜单
主菜单包含所有 Atom 工具通用的子菜单和操作，以及 Material Canvas 的特定操作。

有关 Atom 工具常用功能、菜单、可停靠面板和处理文档的更多信息，请参阅 [Atom 工具常用功能](/docs/atom-guide/look-dev/tools/atom-tools-common-features/)。 

### Edit 菜单
除了 Atom 工具常用的选项外，当材质图形文档打开并处于活动状态时，还有管理图形及其元素的选项。 

与脚本画布和横向画布类似，您可以选择、对齐、删除、复制、剪切、拷贝和粘贴节点。

### View 菜单
除了 Atom 工具常用的选项外，还有导航和管理图表视图的选项，以及打开编辑器配置注释节点和节点组预设的选项。

### Tools 菜单
通过 “工具 ”菜单，可以切换 “材质画布 ”特有的可停靠窗口。

### 停靠窗口
Material Canvas 提供了几个与图形相关的附加窗口。与 Material Editor 不同，视口也是可停靠的。 

#### Node palette
节点调色板包含一棵树，树上有所有可用的材质图形节点和其他实用节点，您可以将它们添加到图形中。这些节点按类别排列，并以颜色标示。

您可以执行以下操作：
- 将鼠标**悬停**在树状图中的每个节点上，以阅读该节点的详细信息。
- 将调色板中的节点**拖**到活动图形视图中，在图形上的下拉位置创建该节点的实例。

#### Bookmarks
使用书签面板管理活动图表上的所有书签。您可以配置书签说明和颜色 书签面板和检查器。

You can do the following actions: 
- Place bookmarks on the graph, like pins on a map, as a point of reference for any important positions. 
- **Double-click** on a bookmark to center the graph view on that position.

#### Mini Map
The mini map window displays a zoomed-out overview of the nodes and graph. **Click-and-drag** the mini map to quickly navigate to different parts of the graph.

#### Viewport
The viewport renders a scene containing a model, with the current material applied to it, under configurable lighting conditions. The viewport window has a toolbar with controls for setting different options related to the grid, shadow catcher, and tone mapping mode. There are also drop downs for selecting the viewport model, lighting preset, and render pipeline. Lighting presets contain all the settings for the skybox, image-based lighting, directional lights, and other settings used in the viewport. Since the introduction of the material pipeline system, render pipelines can also be changed at runtime.

For more information about the viewport and how to interact with it, see [Atom Tools Viewport](/docs/atom-guide/look-dev/tools/atom-tools-viewport/). 

#### Viewport Settings
Use the viewport settings panel to edit the active lighting preset. The changes are reflected in the viewport. Select, create, and save lighting presets from this panel. You can also manage model presets, which are sidecar files for identifying which models are available in the viewport. The viewport settings panel does not currently support undo and redo.

For more information about the viewport settings, see [Atom Tools Viewport](/docs/atom-guide/look-dev/tools/atom-tools-viewport/). 

## Editing material graphs
### Node creation and placement
Nodes are the building blocks of every material graph. Every node serves a purpose, providing data or a distinct piece of functionality that you can add to the graph. 

To create and place nodes, do either one of the following:
- **Drag** nodes from the node palette onto the material graph view. 
- **Right-click** on the graph view and choose a node from the embedded node palette. 
When you create a node, the node appears at the drop or click position. The node is selected and its properties display in the Inspector for editing.

Depending on system constraints, you can repeat this process add more nodes to a graph. Some operations may take longer to perform based on the number of nodes on a graph or the number of selected nodes.

After you add nodes, move and reorganize them by dragging them to new positions. Access actions in the toolbar and in the Edit menu to change the placement and alignment of the selected nodes.

### Node slots and connections
Every Material Canvas node has some number of slots. _Slots_ define properties, inputs, and outputs for a node.

_Properties slots_ have no incoming or outgoing connections. They are often used as constant values or to describe other details about the node.

_Input slots_ have default values that are consumed by and assigned to variables used inside the node. Input slots can only have one incoming connection.

_Output slots_ represent return values from some operation performed by the node. Output slots can have connections to multiple input slots on other nodes. When an output slot is connected to an input slot on another node, the output slot value replaces the input slot value.

Connections are made between input slots and output slots by dragging a connection wire between them. Material Canvas prevents invalid, recursive or cyclic, connections between nodes.

### Node types
Material Canvas has different node types that are categorized into logical groups and color-coded based on the function of the node.

#### Output nodes (main nodes)
The most important node type for a material graph is the main output node. These nodes provide all of the templates and meta data that instruct Material Canvas on what to generate. Material Canvas begins to generate data once an output node, with properly configured template data, is added to a material graph.

At this time, Material Canvas has two output nodes. The Base PBR and Standard PBR nodes contain input slots, options, and templates that you can use to create custom shaders and materials with lighting models and features similar to the core material type counterpart. Not all features have been exposed to the Standard PBR node.

Experiment by dragging one of these nodes onto a material graph. If the automatic graph compilation settings are enabled, which they are by default, several shader files, the material type, and the default material will be generated in the folder containing the material graph file.

#### Constant nodes
Constant nodes represent constant variables defined in line in shader code. There are several constant nodes corresponding to different data types supported by the AZSL. These nodes should be used to setup any variables that do not need to be exposed to outside of the shader or configurable through materials.

#### Input nodes
Input nodes represent named variables that will be added to the Material SRG structure with properties and connections exposed in the material type. These properties will be displayed and configurable in the Material Editor and material component. You can also control them through script. Input nodes have additional property slots for you to specify their name, description, and other data to make them easily identifiable. As with constant nodes, there are several different nodes corresponding to data types supported by AZSL and the material system.

#### Vertex nodes
The vertex node category contains nodes that correspond to different vertex attributes like positions, normals, texture coordinates, and so on. There are nodes for the same attribute in local and world space.

#### Function nodes
The function node category contains several math related and utility function nodes that process property and input values and return a result. Currently, the majority of the function nodes are wrappers for AZSL intrinsic functions.

#### Texturing nodes
This category contains nodes related to texture sampling. Note that some texture sampling nodes sample from a constant vector by default. You may require input from a UV node or other varying vertex attribute node to sample using texture coordinates covering a surface or model.

#### Scene nodes
This category is intended to contain nodes for elements of the Scene SRG. At this time, the only exposed node is Time.

#### Utility nodes
The utility node category contains standard comment and group nodes used by all of the O3DE, graph editing tools. Comment nodes can be placed throughout the graph to leave notes and descriptions about a particular part of the graph. You can use group nodes as a container for other nodes on the graph. For groups that contain other nodes, you can expand, collapse, and treat them as a single node.

## Creating new material graph nodes
Material Canvas nodes, with the exception of utility nodes, are completely defined in JSON configuration files. As mentioned earlier, these files describe the nodes UUID, name, description, category, as well as the layout and details for each slot on the node. Node configurations may have additional settings or meta data to drive the code and data generation process.

The inspector for material graph node documents allows you to create unique node UUIDs, add and remove slots, select data types, configure default values, and manage custom settings for nodes and slots. The inspector also has custom controls for making selections and editing AZSL.

It is possible to create nodes from scratch using tools provided by Material Canvas. However, some of the nodes are simple enough that it might be more convenient to copy an existing material graph node file, update the UUID, and make changes using the tool or directly in JSON.

### Material graph node configuration example
Below is a material graph node configuration for a floating-point constant node with one property and one output slot.

```
{
    "Type": "JsonSerialization",
    "Version": 1,
    "ClassName": "DynamicNodeConfig",
    "ClassData": {
        "id": "{5E2A378E-D27D-43C0-B708-3586FA2293F3}",
        "category": "Constants",
        "title": "Float Constant",
        "titlePaletteName": "ConstantNodeTitlePalette",
        "description": "Create a shader constant with the type and value defined by this node.",
        "slotDataTypeGroups": [
            "inValue|outValue"
        ],
        "propertySlots": [
            {
                "name": "inValue",
                "displayName": "Value",
                "description": "Value",
                "supportedDataTypeRegex": "float", 
                "defaultDataType": "float",
                "settings": {
                    "instructions": [
                        "SLOTTYPE SLOTNAME = SLOTVALUE;"
                    ]
                }
            }
        ],
        "outputSlots": [
            {
                "name": "outValue",
                "displayName": "Value",
                "description": "Value",
                "supportedDataTypeRegex": "float",
                "defaultDataType": "float",
                "settings": {
                    "instructions": [
                        "SLOTTYPE SLOTNAME = inValue;"
                    ]
                }
            }
        ]
    }
}
```

#### Material graph node configuration attributes
Every node configuration must have a unique ID. This ensures that they are uniquely identifiable, regardless of location on disk, project, name collisions, or other factors.

The node category determines how nodes are grouped together in the node palette tree.

The node title is the name displayed in the node palette and on the top of the node in the graph view. It's also used to create unique symbol names and variable names in shader code.

The node title palette name is an optional field specifying which style sheet palette to use for the title bar on the node. Style sheets configure styling, coloring, fonts, and other attributes that control how elements in the node palette and graph view are displayed. Style sheets are defined in a separate, application wide file.

The node description is an optional field that's presented as a tool tip when you hover over a node in the palette.

Slot data type groups contains a delimited list of slots names. The Material Canvas graph traversal and code generation process will enforce that all slot names listed in this field are promoted to the same data type, if they are compatible. Currently, if all of the listed slots reference scalar or vector values than all of the slot values will be promoted to the largest data type. For example, if all of the slots on the node are scalar values but an incoming connection is a three-dimensional vector then all of the other slots will be up converted to three dimensional vectors. This is necessary, in addition to other forms of casting, so that variables generated from different incoming types will be compatible with code and function calls defined in the node.

#### Material graph node slot configuration attributes
Every slot configuration must have a unique name with respect to the node. The slot name is used to identify and address the slot, set up connections between slots, and create a unique variable name in generated shader code and other files. The name is also used as the display name in the UI, if there is no display name specified. Avoid changing the name because it breaks connections and loses any data associated with the slot.

For display name, specify a more user-friendly name to present in the UI. If no display name is provide, then a name inferred from the slot name.

The description provides more detail about what slots do or represent. This is presented as tool tips when you hover over the node and slot in the graph view.

The supporting data types' regular expression field is used to acquire which data types are compatible with a slot. With [regular expressions](https://en.m.wikipedia.org/wiki/Regular_expression), you can query multiple data types that match a specific pattern, or list the data types individually. 

The default value field is used to set a specific default value for a slot. This is optional because the system specifies a standard default value when registering all the data types. No explicit default value is assigned in the node configuration then the registered default value will be used.

#### Material graph node settings
Material graph nodes and slots provide a field for arbitrary settings. Material graph nodes use the settings for data like blocks of AZSL instructions, template file lists, include file lists, and other entries used for material inputs and shader options.

In the previous example, the settings are used to add AZSL instruction blocks and code snippets to create a variable from a property slot and return its value on an output slot.

#### Material graph node settings for AZSL instructions
Material graph node and slot configurations can both contain settings for AZSL instruction blocks. These instructions settings are simply lines of AZSL code that create variables, assign values, call functions, and anything else that can be done in AZSL.

In the above example, unique instruction sets are added to each input and output slot. The input slots have instructions for creating variables with the slot type and value assigned. The output slot instructions create another variable to hold the result of the multiplication.

During the code generation process, the entire graph is traversed in depth order, and instructions are stitched together from each node to fill in the shader program. For each node, instructions are added in the following order: property slot instructions, input slot instructions, node instructions, output slot instructions. This gives a deterministic flow of data from inputs to outputs. In the final shader code, each variable name is prefixed with a unique identifier for the contributing node.

Use the following macros to insert details about the node or slot in instruction settings.
-	SLOTTYPE is substituted with the AZSL data type for the current slot.
-	SLOTTYPE(name) is substituted with the AZSL data type for the slot with the specified name.
-	SLOTNAME is substituted with the unique, decorated variable name for the current slot.
-	SLOTNAME(name) is substituted with the unique, decorated variable name for the slot with the specified name.
-	SLOTVALUE is substituted with the value for the current slot unless it has an incoming connection. If there is an incoming connection, it is replaced with the unique variable name for that connection.
-	SLOTVALUE(name) is substituted with the value for the slot with the specified name unless it has an incoming connection. If there is an incoming connection, it is replaced with the unique variable name for that connection.

## Troubleshooting
### Material Canvas viewport does not update immediately after editing graph
Material Canvas automatically launches the Asset Processor if it is not already running. Some graphics related assets must be processed before the main window opens. Wait until Asset Processor is done processing all assets.

The shader compilation process is expensive, complex, and currently managed by the Shader Asset Builder. Material Canvas relies on the Asset Processor and Shader Asset Builder to process, validate, and preview content generated by material graphs. The Asset Processor reports status, error messages, and other notifications as shader and material assets are built. The viewport updates with shader and material previews as quickly as those assets can be processed.

Building shader assets takes more time on Windows than Linux, or other platforms. This is partially because Windows builds shaders for the null renderer, DX12, and Vulkan by default. Registry settings can be configured to disable unused targets and vastly improve shader compilation and preview times. Use the Material Canvas settings dialog to override these settings.

### Material Canvas fails to launch
Material Canvas initialize all of the O3DE Gems enabled by your game project to access the same rendering features and assets. To reduce start times and system resource utilization, Material Canvas and the other Atom tools include registry setting files that forcibly disable several standard O3DE Gems that are not needed within the tool.

If Material Canvas fails to launch, then it may be because of dependency issues with Gems in the active project. Check `MaterialCanvas.log` for any system entity or module initialization errors. If necessary, change or delete the custom registry settings from the Material Canvas project registry folder.
