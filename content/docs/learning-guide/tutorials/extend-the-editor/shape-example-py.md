---
linkTitle: 在 Python 中创建自定义工具 Gem
title: 在 Python 中创建自定义工具 Gem 以扩展 Open 3D Engine Editor
description: 了解如何通过在 Python 中创建自定义工具 Gem 来扩展 Open 3D Engine （O3DE） 编辑器。
weight: 300
toc: true
---

在本教程中，您将学习如何使用“`PythonToolGem`”模板扩展 **Open 3D Engine （O3DE） 编辑器**，以创建名为 **MyPyShapeExample** 的自定义工具 Gem。此自定义工具允许您使用 Shape 组件创建实体并配置其组件属性。Gem 是使用 [Qt](https://wiki.qt.io/Main)、O3DE 工具 UI API 和其他 O3DE API 用 Python 编写的。

**PyShapeExample** Gem 是 [`o3de/sample-code-gems` repository](https://github.com/o3de/sample-code-gems/tree/main/py_gems/PyShapeExample) 中的示例 Gem，演示了您在本教程中创建的成品 Gem。在学习本教程时，您可以参考 PyShapeExample Gem 示例。

在本教程结束时，您将能够通过创建自己的 Python 编写的自定义工具来扩展 Editor。

下图是您创建的自定义工具的预览。

{{< image-width "/images/learning-guide/tutorials/extend-the-editor/shape-example/python-shape-example-demo.png" "1080" "An image of the Shape Example tool and some entities created by it." >}}


## 先决条件

在开始本教程之前，请确保您具备以下条件：

- 搭建 O3DE 开发环境。有关说明，请参阅 [设置 Open 3D Engine](/docs/welcome-guide/setup/).


## 教程细节

本教程在示例中使用以下 Windows 目录名称和位置。您可以在磁盘上选择不同的文件夹名称和位置。

- **O3DE 引擎目录**: `C:\o3de`  
    包含引擎的源目录。

- **`PythonToolGem` 模板**: `C:\o3de\Templates\PythonToolGem`  
    自定义 Gem 所基于的 Gem 模板。

- **MyPyShapeExample Gem**: `C:\o3de-gems\MyPyShapeExample`  
    您的自定义 Gem。您可以在磁盘中选择不同的文件夹名称和位置。

## 从 `PythonToolGem` 模板创建 Gem

首先，从`PythonToolGem`模板创建一个 Gem，该模板包含一个基本的 Python 框架，用于创建扩展 Editor 的对话框窗口。

要基于 `PythonToolGem` 模板创建 Gem，请完成以下步骤：

1. 使用引擎目录中的 **O3DE CLI** (`o3de`) 脚本创建 Gem。

    ```cmd
    scripts\o3de create-gem --gem-name MyPyShapeExample --gem-path C:\o3de-gems\MyPyShapeExample --template-name PythonToolGem
    ```

    该命令指定以下选项：
    - `--gem-name`, `-gn`: 新 Gem 的名称。
    - `--gem-path`, `-gp`: 创建新 Gem 的路径。
    - `--template-name`, `-tn`: 要从中创建新 Gem 的模板的路径。

    此 Gem 基于`PythonToolGem`模板。在此示例中，将 Gem 命名为 `MyPyShapeExample`并在 `C:\o3de-gems\MyPyShapeExample` 中创建它。

   根据 Gem 路径，此命令会自动将 Gem 注册到其中一个清单文件： `.o3de\o3de_manifest.json`, `<engine>\engine.json`, 和 `<project>\project.json`.

2. （可选）将 Gem 注册到您的项目中。此步骤是可选的，因为当您在上一步中使用 O3DE CLI 创建 Gem 时，它会自动注册 Gem。

    ```cmd
    scripts\o3de register -gp C:\o3de-gems\MyPyShapeExample -espp <project-path>
    ```

3. 使用`o3de`脚本将 Gem 添加到您的项目中。

    ```cmd
    scripts\o3de enable-gem -gn MyPyShapeExample -pp <project-path>
    ```

    或者，使用 Project Manager 启用 Gem（请参阅 [在项目中添加和删除 Gem](/docs/user-guide/project-config/add-remove-gems.md)).

4. 使用 `o3de` 脚本构建项目（请参阅 [构建项目](/docs/user-guide/build/configure-and-build/#build-a-project))。或，使用Project Manager (请参阅[Project Manager](/docs/user-guide/project-config/project-manager/)中的**Build**操作)页面。

5. 打开项目的 Editor。

6. 通过从文件菜单中选择 **Tools > Examples > MyPyShapeExample** 打开该工具。（请参阅下图中的 A。

    或者，通过单击 **Edit Mode Toolbar**（编辑模式工具栏）中的工具图标直接打开该工具。（见 B.）

现在，您可以访问 Shape Example 工具了！默认情况下，此工具包含一个简单的用户界面 （UI）。在接下来的步骤中，我们将设计该工具的 UI 并对其功能进行编码。（见 C.）

{{< image-width "/images/learning-guide/tutorials/extend-the-editor/shape-example/python-tool-gem-template-in-editor.png" "1080" "Editor with a tool created using the PythonToolGem template." >}}


## 代码目录

本节介绍了 MyPyShapeExample Gem 的代码结构。熟悉 Gem 的代码结构非常重要，因为这是您将对工具的自定义功能进行编程的入口点。在此示例中，您的 Gem 目录位于： `C:\o3de-gems\MyPyShapeExample`。这是您在创建 Gem 时指定的路径。Gem 的代码位于子目录`Code\Source` 和 `Editor\Scripts`中，其中包含构成自定义工具的以下类和框架。

`Code\Source` 目录示例:

{{< image-width "/images/learning-guide/tutorials/extend-the-editor/shape-example/python-tool-gem-template-directory.png" "720" "Editor with a tool created using the PythonToolGem template." >}}

`Editor\Scripts` 目录示例:

{{< image-width "/images/learning-guide/tutorials/extend-the-editor/shape-example/python-tool-gem-template-directory-2.png" "620" "Editor with a tool created using the PythonToolGem template." >}}

### 模块和系统组件

所有 Gem 都有用 C++ 编写的模块和系统组件类，这些模块和组件类将工具连接到 O3DE 并允许它与其他系统通信。`PythonToolGem`模板已包含在 Editor 中运行该工具所需的所有代码。您可以在 `Code\Source` 中找到这些 C++ 文件。

有关 Gem 模块和系统组件的更多信息，请参阅 [Open 3D Engine Gem 模块系统概述](/docs/user-guide/programming/gems/overview/) 页面。

### O3DE 和 Qt 框架

O3DE 扩展了 **Qt** 框架，因此您将使用 [PySide2](https://pypi.org/project/PySide2/) ---Qt 的 Python 绑定，[Qt for Python](https://doc.qt.io/qtforpython/)---来创建图形用户界面 （GUI）。您还将使用 O3DE 的事件总线与其他 O3DE 接口（如实体和组件）进行通信，并将它们连接到 Qt 元素。

您将在位于`Editor\Scripts\pyshapeexample_dialog.py`中的 `PyShapeExampleDialog` 类中编写工具的大部分功能和 UI 元素。

### Qt Resources

[Qt Resource System](https://doc.qt.io/qt-5/resources.html)允许 Gem 通过`.qrc`文件存储和加载图像文件。这样就无需从绝对路径加载图像文件，从而简化了 Gem 的分发。稍后，您将存储图像文件以为您的工具创建图标。

## 依赖模块

在`Editor\Scripts\pyshapeexample_dialog.py`文件中导入以下依赖模块。您将创建的`PyShapeExampleDialog`类使用这些模块中的对象。

```py
import azlmbr.bus as bus
import azlmbr.components as components
import azlmbr.editor as editor
import azlmbr.entity as entity
import azlmbr.math as math

from PySide2.QtCore import Qt
from PySide2.QtGui import QDoubleValidator
from PySide2.QtWidgets import QCheckBox, QComboBox, QDialog, QFormLayout, QGridLayout, QGroupBox, QLabel, QLineEdit, QPushButton, QVBoxLayout, QWidget
```

## 对话框、小组件和布局

使用 Qt for Python，您可以创建 *dialogs* 或顶级窗口。对话框中有 *widgets*（UI 元素的容器）和 *layouts*（定义这些 UI 元素的排列方式）。（相比之下，C++ 中的 Qt 创建的是主小部件而不是对话框。自定义工具是一个对话框，用于限制 UI 元素的子窗口小部件。每个 Widget 都可以有自己的布局和其他子 Widget。嵌套的 Widget 和布局结构允许您组织 UI 元素组。

`PyShapeExampleDialog`类继承自`QDialog`，后者创建对话框窗口。以下说明将引导您完成如何设置对话框的布局。请注意，某些指令可能已经由 `PythonToolGem` 模板完成。

1. 在构造函数的顶部，实例化一个名为 `QVBoxLayout` 的 `main_layout`。

2. 稍后，您将创建各种 UI 元素并将它们添加到`main_layout`。

3. （可选）您可以使用`setSpacing(...)`为 `main_layout` 添加间距`.

4. 我们建议您在调整窗口大小时，使用`addStretch()` 在 `main_layout` 的底部添加一个拉伸，以填充任何扩展的空间。

5. 最后，使用 `self.setLayout(...)` 将 `main_layout` 设置为此对话框的布局`.

```py
class PyShapeExampleDialog(QDialog):
    
    def __init__(self, parent=None):

        main_layout = QVBoxLayout()
        main_layout.setSpacing(20)

        // Add various UI elements to main_layout

        main_layout.addStretch()

        self.setLayout(main_layout)
```


## 输入字段和复选框

在此步骤中，为实体名称创建一个输入字段，并为一个选项创建一个复选框，用于将后缀---组件的名称---附加到实体的名称。例如，假设您将实体的名称设置为 “MyEntity” 并启用该复选框。然后，当您创建具有 **Box Shape** 组件的实体和另一个具有 **Sphere Shape** 组件的实体时，它们将分别命名为“MyEntity_BoxShape”和“MyEntity_SphereShape”。

在此步骤结束时，您的输入字段和复选框应如下所示：

{{< image-width "/images/learning-guide/tutorials/extend-the-editor/shape-example/input-field-check-box.png" "500" "Shows UI for an input field and checkbox" >}}

实例化`mainLayout`后，将这些 UI 元素包装在它们自己的子 widget 中，设置布局，并将其添加到主 widget。

1. 首先，实例化一个名为`entity_name_widget`的`QGroupBox`和一个名为`form_layout`的`QFormLayout`。

2. 稍后，您将创建输入字段和复选框并将它们添加到此小部件中。

3. 最后，将 `entity_name_widget` 的布局设置为 `form_layout`，并将 `entity_name_widget` 添加到 `main_layout`。

```py
        entity_name_widget = QGroupBox("Name your entity (Line Edit)", self)    # 1
        form_layout = QFormLayout()

        # ...                                                                   # 2

        entity_name_widget.setLayout(form_layout)                               # 3
        main_layout.addWidget(entity_name_widget)
```

### 创建输入字段

输入字段从用户那里获取文本输入。使用 Qt，您可以使用 `QLineEdit`  对象创建输入字段。在此示例中，input 字段用于命名生成的实体。稍后将定义此行为。

1. 通过实例化`QLineEdit` 创建输入字段。在此示例中，将其命名为`name_input`。

2. 通过调用`setPlaceholderText(...)`设置`name_input`中的占位符文本`.

3. 启用一个按钮以使用 `setClearButtonEnabled(True)` 清除文本。

```py
        self.name_input = QLineEdit(self)
        self.name_input.setPlaceholderText("Optional")
        self.name_input.setClearButtonEnabled(True)
```

### 创建复选框

复选框是一个选项按钮，用户可以启用或禁用该按钮以触发用户定义的行为。在 Qt 中，您可以使用`QCheckBox` 对象创建一个复选框。在此示例中，该复选框控制是否将后缀附加到实体的名称。在运行时，它将开始禁用，并在用户将实体名称输入到输入字段时启用 enable。稍后将定义此行为。

1. 通过实例化`QCheckBox`创建一个复选框。在此示例中，将其命名为 `add_shape_name_suffix`。

2. 使用`setDisabled(True)`设置复选框以禁用状态启动。

```py
        self.add_shape_name_suffix = QCheckBox(self)
        self.add_shape_name_suffix.setDisabled(True)
        self.add_shape_name_suffix.setToolTip("e.g. Entity2_BoxShape")
```

### 添加带有插槽处理程序的信号侦听器

Qt使用信号和插槽在对象之间进行通信（参考Qt文档中的[信号和插槽](https://doc.qt.io/qt-5/signalsandslots.html)）。设置信号侦听器，以便当用户在运行时将文本输入到输入字段中时，该复选框将自动启用。

在此示例中，信号侦听器使用插槽处理程序，该处理程序本质上是信号可以连接到的 Python 函数。

1. 调用`connect(...)`在输入字段(`name_input`)与信号(`textChanged`)和插槽 (`on_name_input_text_changed`)之间创建连接。

    ```py
            self.name_input.textChanged.connect(self.on_name_input_text_changed)
    ```

2. 在构造函数之外，像对标准 Python 函数一样定义一个槽。在此示例中，将槽命名为`on_name_input_text_changed`。

    ```py
        def on_name_input_text_changed(self, text):

            self.add_shape_name_suffix.setEnabled(len(text))
            
    ```


### 将 UI 元素添加到布局

创建 UI 元素---输入字段和复选框---并将信号侦听器连接到它们后，将它们添加到 UI 中。要确保它们属于子小部件`entity_name_widget`，请通过调用`addRow(...)` 将它们添加到`form_layout`中。

```py
        form_layout.addRow("Entity name", self.name_input)
        form_layout.addRow("Add shape name suffix", self.add_shape_name_suffix)
```

## 组合框

在此步骤中，您将创建一个组合框，其中包含可用于缩放实体大小的值列表。

在此步骤结束时，您的组合框应如下所示：

{{< image-width "/images/learning-guide/tutorials/extend-the-editor/shape-example/combo-box.png" "500" "Shows UI for combobox" >}}

首先，将这些 UI 元素包装在它们自己的子 widget 中，设置布局，并将其添加到主 widget。

1. 首先，实例化一个名为 `combobox_group` 的`QGroupBox`和一个名为 `combobox_layout` 的  `QVBoxLayout`。

2. 稍后，您将创建一个组合框并定义一个比例值列表。

3. 最后，将 `combobox_group` 的布局设置为 `combobox_layout`，并将 `combobox_group` 添加到 `mainLayout`。

```py
        combobox_group = QGroupBox("Choose your scale (combobox)", self)   # 1
        combobox_layout = QVBoxLayout()

        # ...                                                               # 2

        combobox_group.setLayout(combobox_layout)                           # 3
        main_layout.addWidget(combobox_group)
```

### 创建组合框

使用组合框，用户可以从弹出的项目列表中选择一个项目。在 Qt 中，您可以使用 `QComboBox`对象创建输入字段。

1. 定义刻度值列表。在此示例中，将其命名为 `scale_values`。

2. 通过实例化`QComboBox`创建一个组合框。在此示例中，将其命名为 `scale_combobox`。

3. 允许用户通过调用 `setEditable(True)` 在组合框中输入自定义选项。

4. 如果用户输入自定义选项，请通过调用`setValidator(...)`来验证他们输入的值是否是特定范围和小数位内的数值。在此示例中，将下限设置为`0.0`，将上限设置为`100.0`，将小数位数设置为 `3`。

5. 通过调用`addItems(...)`将值列表添加到组合框.

6. 通过调用`addWidget(...)`将组合框添加到`combobox_layout` 中.

```py
        scale_values = [                                                            # 1
            "1.0",
            "1.5",
            "2.0",
            "5.0",
            "10.0"
        ]
        self.scale_combobox = QComboBox(self)                                       # 2
        self.scale_combobox.setEditable(True)                                       # 3
        self.scale_combobox.setValidator(QDoubleValidator(0.0, 100.0, 3, self))     # 4
        self.scale_combobox.addItems(scale_values)                                  # 5

        combobox_layout.addWidget(self.scale_combobox)                              # 6
```


## 按钮

在此步骤中，您将创建一个按钮集合，这些按钮创建具有不同 Shape 组件（例如，使用 Box Shape、Sphere Shape、Cone Shape 等）的实体。

在此步骤结束时，您的按钮应如下所示：

{{< image-width "/images/learning-guide/tutorials/extend-the-editor/shape-example/buttons.png" "500" "Shows UI for buttons" >}}


首先，将这些 UI 元素包装在它们自己的子 widget 中，设置布局，并将其添加到主 widget。

1. 首先，实例化一个名为`shape_buttons` 的`QGroupBox`和一个名为 `grid_layout`的 `QGridLayout`。

2. 稍后，您将查询向引擎注册的所有类型的形状组件，向此 widget 添加按钮，并向按钮添加信号侦听器。

3. 最后，将 `shape_buttons` 的布局设置为`grid_layout`，并将 `shape_buttons` 添加到`main_layout` 中。

```py
        shape_buttons = QGroupBox("Choose your shape (Button)", self)   # 1
        grid_layout = QGridLayout()

        # ...                                                           # 2

        shape_buttons.setLayout(grid_layout)                            # 3
        main_layout.addWidget(shape_buttons)
```

### 查询 Shape 组件

1. 查询向引擎注册的 Shape 组件的类型。在 O3DE 中，所有组件都有一个已提供服务的列表，您可以从中进行查询。在此示例中，您正在寻找 Shape 组件，这些组件都提供 “ShapeService”。

   - 创建名为 `provided_services` 的组件服务列表并添加 “ShapeService”。

   - 获取提供 “ShapeService” 的组件类型列表。使用 `editor.EditorComponentAPIBus(...)` 调用 `bus.Broadcast` 和 发送`FindComponentTypeIdsByService`，它接受要包含的服务列表（`provided_services`）和要排除的服务（空列表），并将组件类型存储到`typeIds`。

   ```py
           shape_service = math.Crc32_CreateCrc32("ShapeService")
           provided_services = [shape_service.value]
           type_ids = editor.EditorComponentAPIBus(bus.Broadcast, 'FindComponentTypeIdsByService', provided_services, [])
   ```

2. 查询 Shape 组件的名称。稍后，您将这些名称添加到按钮中。

    - 获取组件名称列表。使用`editor.EditorComponentAPIBus(...)`调用 `bus.Broadcast` 和 发送 `FindComponentTypeNames`，获取组件类型列表  (`typeIds`) ，并将相应的组件名称存储到`component_names`。
    ```py
            component_names = editor.EditorComponentAPIBus(bus.Broadcast, 'FindComponentTypeNames', type_ids)
    ```

{{< note >}}
您可以通过调用组件的`GetProvidedServices()`方法来查询组件提供的服务列表。
{{< /note >}}


### 添加按钮

按钮是一个 UI 元素，用户可以单击该元素来触发用户定义的行为。在 Qt 中，您可以使用 `QPushButton` 对象创建按钮。在此示例中，当用户单击按钮时，它将创建一个具有 Shape 组件的实体。稍后将定义此行为。

遍历组件名称列表，创建按钮，并将它们添加到`grid_layout`中。对于每个组件：

1. 通过实例化`QPushButton` 创建一个按钮。在此示例中，将其命名为`name_input`。传入 `name` 以将组件的名称添加到按钮上。

2. 稍后，您将`shape_button`连接到信号侦听器。

3. 将 `grid_layout` 拆分为三列，然后添加按钮。

```py
        max_column_count = 3                                    
        for i, name in enumerate(component_names):
            type_id = type_ids[i]

            shape_button = QPushButton(name, self)              # 1

            # ...                                               # 2

            row = i / max_column_count                          # 3
            column = i % max_column_count
            grid_layout.addWidget(shape_button, row, column)
```

### 添加具有 lambda 处理程序的信号侦听器

接下来，创建一个信号侦听器，这样当用户单击该按钮时，将在场景中创建一个实体，并将相应的 Shape 组件添加到该实体中。

在此示例中，信号侦听器使用 lambda 处理程序，因此您可以定义函数，而无需定义槽。

1. 声明一个函数`create_entity_with_shape_component`以连接到信号侦听器。此函数与 O3DE 的事件总线通信，您将在后面定义。

    ```py
        def create_entity_with_shape_component(self, type_id):
    ```

2. 在您之前创建的循环中，创建从 `shape_button` 中的 `clicked` 信号到 lambda 函数的连接。lambda 函数调用 `create_entity_with_shape_component(...)` 并传入 `type_id` 中。

    ```py
            shape_button.clicked.connect(lambda checked=False, type_id=type_id: self.create_entity_with_shape_component(type_id))
    ```

 ### 与 EBuse 通信

定义`CreateEntityWithShapeComponent(...)`，它与 O3DE 事件总线通信，以使用由`typeId`参数指定的组件创建新实体。

1. 发送请求，使用`editor.EditorRequestBus`调用`bus.Broadcast`并调度 `IsLevelDocumentOpen`，如果关卡已经加载，它将返回 true/false。
    - 尝试在未加载关卡的情况下创建 Entity 将导致崩溃，因此如果未加载关卡，我们将打印警告消息，然后返回。

2. 使用`editor.ToolsApplicationRequestBus`调用 `bus.Broadcast` 并调度 `CreateNewEntity`，后者创建一个新实体并返回其`AZ::EntityId`。新实体的`AZ::EntityId` 存储在`new_entity_id` 中。

3. 如果用户在输入字段中输入了名称，请更新实体的名称。

   - 通过调用`text()` 获取实体的名称，并将其存储在`entity_name`中。

   - 如果用户启用了复选框以将组件名称的后缀应用于实体名称，请查询组件的名称。使用 `editor.EditorComponentAPIBus`调用`bus.Broadcast`并调度 `FindComponentTypeNames`事件，该事件查找所提供列表的组件名称并将它们存储在 `component_names` 中。 

   - 设置名称和后缀的格式（如果有）。

   - 使用 `editor.EditorEntityAPIBus` 调用 `bus.Event` 并调度`"SetName"` 将 `new_entity_id` 的名称设置为`entity_name`。

4. 设置实体的缩放。
   
   - 通过调用`currentText()`获取组合框中的值，并将其存储在`scale_text` 中。

   - 使用`components.TransformBus`调用 `bus.Event` 和 dispatch `"SetLocalUniformScale"`，它设置`new_entity_id` 的比例。请注意，您必须将 `scale_text`转换为 float。

5. 将 Shape 组件添加到实体。使用 `editor.EditorComponentAPIBus` 调用 `bus.Broadcast` 并调度 `"AddComponentsOfType"`，这会将所提供列表中的组件添加到`new_entity_id`。

```py
     def create_entity_with_shape_component(self, type_id):

        # 1
        is_level_loaded = editor.EditorRequestBus(bus.Broadcast, 'IsLevelDocumentOpen')
        if not is_level_loaded:
            print("Make sure a level is loaded before choosing your shape")
            return

        # 2
        new_entity_id = editor.ToolsApplicationRequestBus(bus.Broadcast, 'CreateNewEntity', entity.EntityId())

        # 3
        if self.name_input.text():
            entity_name = self.name_input.text()

            if self.add_shape_name_suffix.isChecked():
                component_names = editor.EditorComponentAPIBus(bus.Broadcast, 'FindComponentTypeNames', [type_id])

                shape_name = component_names[0]
                shape_name = shape_name.replace(" ", "")

                entity_name = f"{entity_name}_{shape_name}"

            editor.EditorEntityAPIBus(bus.Event, "SetName", new_entity_id, entity_name)

        # 4
        try:
            scale_text = self.scale_combobox.currentText()
            components.TransformBus(bus.Event, "SetLocalUniformScale", new_entity_id, float(scale_text))
        except:
            pass

        # 5
        editor.EditorComponentAPIBus(bus.Broadcast, "AddComponentsOfType", new_entity_id, [type_id])
```

## 图标

图标是用于在 Editor 中表示工具的图像文件。该图标显示在 Editor 的 Edit Mode 工具栏中（请参阅下图）。

{{< image-width "/images/learning-guide/tutorials/extend-the-editor/shape-example/icon.png" "500" "Add an icon for your tool in the Editor" >}}

### 添加图标

以下说明将指导您了解如何使用 Qt 资源系统存储图标并从 Gem 模块加载它。请注意，某些指令可能已经由`PythonToolGem`模板完成。

1. 将图像文件添加到`Code\Source`目录以用作图标。在此示例中，图标名为 `toolbar.svg`。我们建议您的图标遵循 [UI 开发最佳实践](https://o3de.org/docs/tools-ui/uidev-component-development-guidelines/#ui-development-best-practices)中的准则。

2. 通过使用新图标的文件名更新`Code\Source\ShapeExample.qrc`，将您的图标添加到 MyPyShapeExample Gem 的资源中。

    ```xml
    <!DOCTYPE RCC><RCC version="1.0">
        <qresource prefix="/MyPyShapeExample">
            <file alias="toolbar_icon.svg">toolbar_icon.svg</file>
        </qresource>
    </RCC>
    ```

3. 通过在`Code\Source\EditorModule.cpp` 中添加以下代码，将 MyPyShapeExample Gem 的资源注册到 Qt 资源系统。

    - 定义函数 `InitShapeExampleResource()`并调用`Q_INIT_RESOURCE(...)` 注册`ShapeExample.qrc`中列出的Qt资源。

        ```cpp
        void InitShapeExampleResources()
        {
            Q_INIT_RESOURCE(ShapeExample);
        }
        ```

    - 在`EditorModule`类的构造函数中调用 `InitShapeExampleResource()`。


## 构建和测试你的工具

您可以通过从 Editor 的 **Python Scripts** 面板运行自定义工具来测试它。

1. 为您的项目打开 Editor。

2. 从文件菜单中选择 **Tools > Other > Python Scripts** 打开 Python Scripts（Python 脚本）面板。

3. 在 Python Scripts 面板中打开 `pyshapeexample_dialog`。

祝贺！您创建了一个用 Python 编写的自定义工具 Gem，构建了它，并将其加载到 Editor 中。您的 Shape Example 工具应如下所示：

{{< image-width "/images/learning-guide/tutorials/extend-the-editor/shape-example/python-shape-example-ui.png" "500" "An image of the Shape Example tool." >}}

## 下载 PyShapeExample Gem 示例

现在，您已经完成了本教程，您可以将 MyPyShapeExample Gem 与示例 PyShapeExample Gem 进行比较，在 [`o3de/sample-code-gems` repository](https://github.com/o3de/sample-code-gems/tree/main/cpp_gems/ShapeExample)中。

要下载此示例并将其加载到 Editor 中，请执行以下操作：

1. 下载或克隆存储库。PyShapeExample Gem 位于`<repo>\sample-code-gems\py_gems\PyShapeExample`.

    ```cmd
    git clone https://github.com/o3de/sample-code-gems.git
    ```

2. 在打开“形状示例”工具之前，请执行以下操作：

   - 注册您的 Gem。

   - 在您的项目中启用它。

   - 重新构建您的项目。

   - 在 Editor 中打开该工具。

   本教程前面将介绍这些步骤。指[从 `PythonToolGem` 模板创建 Gem](#create-a-gem-from-the-pythontoolgem-template).
