---
linkTitle: 自动化编辑器
title: 使用Python Editor Bindings Gem自动化编辑器
description: 使用 Python Editor Bindings Gem 的功能，在 O3DE 编辑器内自动执行操作和任务。
weight: 600
---

O3DE 编辑器中的某些任务比较繁琐，或者很容易实现自动化，为了支持这些任务，O3DE 支持通过 Python 绑定底层编辑器实现编辑器脚本。这些绑定通过**PythonEditorBindings** gem启用，并通过嵌入在编辑器中的Python 3库进行交互。你可以通过编辑器内的控制台访问 Python REPL，或在启动编辑器时使用加载和运行 Python 脚本的参数。

## 启用编辑器自动化

为项目选择**PythonEditorBindings** gem，然后重建编辑器，即可启用编辑器自动化。启用 Python 绑定不需要特定的配置（调试、配置文件、发布）。由于绑定是通过您为项目选择的 gem 启用的，因此您需要确保您打算使用自动化的**个项目都启用了该 gem。

## 使用编辑器自动化

开始使用编辑器自动化的最简单方法是使用 O3DE 编辑器中的 REPL，并尝试使用一些命令。选择 **Tools** > **Other** > **Python Console**，打开 REPL。Python 控制台会在新的编辑器视图中打开，您可以访问显示 Python 输出、REPL 输入和可用命令完整参考的控制台。要访问参考，请选择控制台右下角的 **?**

您还可以通过选择**Tools** > **Other** > **Python Scripts**，访问一组可用的脚本，包括编辑器中常见任务的一些示例。这些脚本存储在不同的目录中，具体取决于其范围。仅用于你的项目的脚本存储在 `Editor\Scripts`目录下，而与 gem 一起使用的脚本存储在`Gems\<name>\Editor\Scripts`目录下。

编辑器自动化主要通过事件总线（EBus）系统驱动。在使用编辑器绑定之前，你应该熟悉 [使用事件总线（EBus）系统](/docs/user-guide/programming/messaging/ebus/)中的 EBus 基础知识。要了解编辑器自动化系统使用的一些特定总线，请参阅 [Python 编辑器绑定 Gem 示例](#python-editor-bindings-gem-examples)。

## Python Editor Bindings Gem 示例

Python Editor Bindings Gem 提供了一个 API，它定义了与编辑器 C++ 实现的连接，使用 O3DE 事件总线 (Ebus) 在 Python 脚本和编辑器之间发送消息。本参考资料涵盖了绑定 API 的使用，以执行与组件、实体和属性交互等任务。
 
 ## 关卡管理

使用这些函数加载、创建和保存关卡。要使用其他绑定 API，需要在 O3DE 编辑器中加载关卡。
 
 ```python
 # 用用户提示打开关卡
 azlmbr.legacy.general.open_level(strLevelName)
 
 # 在不提示用户的情况下打开关卡（更适合自动化操作）
 azlmbr.legacy.general.open_level_no_prompt(strLevelName)
 
 # creates a level with the parameters of 'levelName', 'resolution', 'unitSize' and 'bUseTerrain'
 azlmbr.legacy.general.create_level(levelName, resolution, unitSize, bUseTerrain)
 
 # same as create_level() but no prompts
 azlmbr.legacy.general.create_level_no_prompt(levelName, resolution, unitSize, bUseTerrain)
 
 # saves the current level
 azlmbr.legacy.general.save_level()
 ```
 
 ## 编辑器定时

有时，脚本需要在编辑器中执行另一个操作（如加载关卡）时引入延迟。与其使用内置的 Python 延迟方法，不如使用这些编辑器绑定 API。
 
 ```python
 # enables/disables idle processing for the Editor
 azlmbr.legacy.general.idle_enable(boolValue)
 
 # Returns whether or not idle processing is enabled for the Editor
 azlmbr.legacy.general.is_idle_enabled()
 
 # idles for specified number of seconds
 azlmbr.legacy.general.idle_wait(floatSeconds)
 ```
 
 ## 实体

该应用程序接口可让您在一个层的根实体中添加和删除实体、检索和比较实体 ID 以及搜索实体。
 
 ### Entity IDs

使用 `azlmbr.entity.EntityId` 类来引用实体实例、属性和实体树。
 
 ```python
 # returns True if the entity ID is valid
 entityId.IsValid()
 
 # returns string representation of an entity ID
 entityId.ToString()
 
 # returns True if both entity IDs
 entityId.Equal(otherEntityId)
 ```
 
 ### 实体操作和 Ebus 接口

用于管理编辑器实体的 EBus 接口主要有三个：
 +  `azlmbr.editor.ToolsApplicationRequestBus`: 用于创建和删除编辑器实体
 +  `azlmbr.editor.EditorEntityInfoRequestBus`: 用于访问实体值
 +  `azlmbr.editor.EditorEntityAPIBus`: 用于更改实体值
 
 **使用示例**:
 
 ```python
 # Create a new entity at the root level
 rootEntityId = azlmbr.editor.ToolsApplicationRequestBus(azlmbr.bus.Broadcast, 'CreateNewEntity', EntityId())
 
 # Create a new entity parented to the parent entity
 childEntityId = azlmbr.editor.ToolsApplicationRequestBus(azlmbr.bus.Broadcast, 'CreateNewEntity', rootEntityId)
 
 # Delete the entity
 azlmbr.editor.ToolsApplicationRequestBus(azlmbr.bus.Broadcast, 'DeleteEntityById', childEntityId)
 
 # Delete the root entity we created and all its children
 azlmbr.editor.ToolsApplicationRequestBus(azlmbr.bus.Broadcast, 'DeleteEntityAndAllDescendants', rootEntityId)
 
 # Get current name
 name = azlmbr.editor.EditorEntityInfoRequestBus(azlmbr.bus.Event, 'GetName', entityId);
 
 # Set a new name
 azlmbr.editor.EditorEntityAPIBus(azlmbr.bus.Event, 'SetName', entityId, "MyName")
 
 # Get the parent ID of this entity ID
 getId = azlmbr.editor.EditorEntityInfoRequestBus(azlmbr.bus.Event, 'GetParent', childId);
 ```
 
 ### 实体搜索

实体搜索 API 基于使用 `azlmbr.entity.SearchFilter` 设置过滤器来设置搜索参数，然后通过 `azlmbr.entity.SearchBus` 所代表的 Ebus 进行搜索。
 
 **`azlmbr.entity.SearchFilter` 用法**:
 
 ```python
 searchFilter = azlmbr.entity.SearchFilter()
 searchFilter.names = [] # List of names (matches if any match); can contain wildcards in the name
 searchFilter.names_case_sensitive = False # Determines if the name matching should be case-sensitive
 searchFilter.components = {} # Dictionary keyed on component type IDs (matches if any match)
 searchFilter.components_match_all = False # Determines if the filter should match all component type IDs
 searchFilter.roots = [] # Specifies the entity IDs that act as roots of the search
 searchFilter.names_are_root_based = False # Determines if the names are relative to the root or should be searched in children too
 ```
 
 **`azlmbr.entity.SearchBus` 用法**:
 
 ```python
 # The SearchBus interface
 busType = azlmbr.bus.Broadcast
 
 # Iterates through all entities in the current level, and returns a list of the ones that match the conditions
 entityIdList = azlmbr.entity.SearchBus(busType, 'SearchEntities', searchFilter)
 
 # Returns a list of all Editor entities at the root level in the current level
 entityIdList = azlmbr.entity.SearchBus(busType, 'GetRootEditorEntities', searchFilter)
 ```

**使用通配符搜索**：

实体可通过 `名称路径 `寻址，使用由管道字符 `|` 分隔的字符串，如`root name|my entity|my child`作为名称路径。实体搜索还支持使用`?`和`*`通配符。
 
 **使用示例**:
 
 ```python
 import azlmbr.bus as bus
 import azlmbr.entity as entity
 
 searchFilter = entity.SearchFilter()
 searchFilter.names = ['TestName']
 
 # Search by name
 entityIdList = entity.SearchBus(bus.Broadcast, 'SearchEntities', searchFilter)
 
 # Search by name path (a directed acyclic graph (DAG))
 searchFilter = entity.SearchFilter()
 searchFilter.names = ['TestParent|TestChild']
 entityIdList = entity.SearchBus(bus.Broadcast, 'SearchEntities', searchFilter)
 
 # Search using wildcard
 searchFilter = azlmbr.entity.SearchFilter()
 searchFilter.names = ['Test*|?estChild']
 entityIdList = entity.SearchBus(bus.Broadcast, 'SearchEntities', searchFilter)
 ```
 
 **强制搜索从根实体开始**:
 
 ```python
 import azlmbr.bus as bus
 import azlmbr.entity as entity
 
 # Filter with roots
 searchFilter = entity.SearchFilter()
 searchFilter.names = ["TestChild"]
 searchFilter.roots = [rootId]
 searchFilter.names_are_root_based = False  # default
 entityIdList = entity.SearchBus(bus.Broadcast, 'SearchEntities', searchFilter)
 
 # Filter with roots using the names, only get the children relative from the root nodes
 searchFilter = entity.SearchFilter()
 searchFilter.names = ["TestParent|TestChild"]
 searchFilter.roots = [rootId]
 searchFilter.names_are_root_based = True # Search from roots for these names
 entityIdList = entity.SearchBus(bus.Broadcast, 'SearchEntities', searchFilter)
 ```
 
 ### 实体通知

您可以使用`EditorEntityContextNotificationBus`处理程序捕获编辑器实体事件。回调可分配给实体管理事件名称：`OnEditorEntityCreated` 和`OnEditorEntityDeleted`，其中回调将使用来自事件的数据元组调用。
  
 ```python
 # The events
 "OnEditorEntityCreated" # returns when an entity is created in the Editor
 "OnEditorEntityDeleted" # returns when an entity is destroyed in the Editor
 ```
 
 **使用示例**:
 
 ```python
 # assumes a level has been opened or created
 import azlmbr.bus as bus
 import azlmbr.editor as editor
 from azlmbr.entity import EntityId
 
 createdEntityIds = [] # to capture created entities
 
 def onEditorEntityCreated(parameters):
    global createdEntityIds
    entityId = parameters[0]
    createdEntityIds.append(entityId)
 
 def onEditorEntityDeleted(parameters):
    global createdEntityIds
    deletedEntityId = parameters[0]
    for entityId in createdEntityIds:
        if (entityId.Equal(deletedEntityId)):
            createdEntityIds.remove(entityId)
            break
 
 # Listen for notifications when entities are created/deleted
 handler = editor.EditorEntityContextNotificationBusHandler()
 handler.connect() # connects to a singleton bus handler
 handler.add_callback('OnEditorEntityCreated', onEditorEntityCreated)
 handler.add_callback('OnEditorEntityDeleted', onEditorEntityDeleted)
 
 # Create new Editor entity
 editor.ToolsApplicationRequestBus(bus.Broadcast, 'CreateNewEntity', EntityId())
 ```
 
 ## 组件管理

使用组件系统，通过 `azlmbr.editor.EditorComponentAPIBus` 总线向现有实体添加和删除组件。
 
 {{< note >}}
 组件在编辑模式下不会激活。只有在编辑器中进行游戏时，它们才会激活。
 {{< /note >}}
 
 ### 组件类型事件

API 需要 ID 才能创建、使用或控制组件实例。要获取组件 ID，请使用以下 Ebus 事件：
 
 ```python
 # azlmbr.editor.EditorComponentAPIBus Broadcast events
 
 # Finds the component IDs from their type names
 # input: list of strings of type names
 # output: (list of component type IDs)
 'FindComponentTypeIds'
 
 # Finds the component names from their type IDs
 # input: list of component type IDs
 # output: (list of strings) of type names
 'FindComponentTypeNames'
 
 # Returns the full list of names for all game components that can be created with the EditorComponent API
 # input: entity.EntityType().Game
 # output: (list of strings) of the known component type names
 'BuildComponentTypeNameListByEntityType'

 # Returns the full list of names for all level components that can be created with the EditorComponent API
 # input: entity.EntityType().Level
 # output: (list of strings) of the known component type names
 'BuildComponentTypeNameListByEntityType'
 ```
 
 **使用示例**:
 
 ```python
 import azlmbr.bus as bus
 
 # Generate list of game component type names
 componentList = editor.EditorComponentAPIBus(bus.Broadcast, 'BuildComponentTypeNameListByEntityType', entity.EntityType().Game)
 
 # Generate list of level component type names
 componentList = editor.EditorComponentAPIBus(bus.Broadcast, 'BuildComponentTypeNameListByEntityType', entity.EntityType().Level)

 # Get component types for 'Mesh' and 'Comment'
 typeIdList = azlmbr.editor.EditorComponentAPIBus(bus.Broadcast, 'FindComponentTypeIds', ["Mesh", "Comment"])
 
 # Get component type names from component type IDs
 typeNameList = azlmbr.editor.EditorComponentAPIBus(bus.Broadcast, 'FindComponentTypeNames', typeIdsList)
 ```
 
 ### 组件使用事件

应用程序接口可以向现有实体添加组件、测试组件是否存在、按类型统计组件以及枚举实体上的组件。
 
 ```python
 # azlmbr.editor.EditorComponentAPIBus Broadcast events
 
 # Add components of the given types to an entity.
 # input: entity ID
 # input: list of component type IDs
 # output: (Outcome<list of component IDs>)
 'AddComponentsOfType'
 
 # Tests a component of type can be found on entity
 # input: component type ID
 # output: (bool) True if a component of type provided can be found on entity, False otherwise
 'HasComponentOfType'
 
 # Count components of type provided on the entity
 # input: entity ID
 # input: component type ID
 # output: (number) of component instances on an entity
 'CountComponentsOfType'
 
 # Get component of type from entity
 # Only returns first component of type if found (early out).
 # input: entity ID
 # input: component type ID
 # output: (Outcome<component ID>)
 'GetComponentOfType'
 
 # Get all components of type from entity
 # Returns list of component IDs, or an empty list if components could not be found
 # input: entity ID
 # input: component type ID
 # output: (Outcome<list of component IDs>)
 'GetComponentsOfType'
 ```
 
 **使用示例:**
 
 ```python
 import azlmbr.bus as bus
 
 # adding a Mesh component
 meshComponentOutcome = azlmbr.editor.EditorComponentAPIBus(bus.Broadcast,'AddComponentsOfType', entityId, [meshComponentTypeId])
 
 if (meshComponentOutcome.IsSuccess()):
    print("Mesh component added to entity.")
 
 meshComponents = meshComponentOutcome.GetValue()
 meshComponent = meshComponents[0]
 
 # test for a Mesh component exists on the enity
 hasComponent = azlmbr.editor.EditorComponentAPIBus(bus.Broadcast, 'HasComponentOfType', entityId, meshComponentTypeId)
 
 if (hasComponent):
    print("Entity has a Mesh component.")
 
 # find the number of Mesh components on the entity
 commentsCount = azlmbr.editor.EditorComponentAPIBus(bus.Broadcast, 'CountComponentsOfType', entityId, meshComponentTypeId)
 
 if(commentsCount == 1):
    print("Entity has one Mesh component")
 
 # returns the first Mesh component ID, if any
 meshSingleComponentOutcome = azlmbr.editor.EditorComponentAPIBus(bus.Broadcast, 'GetComponentOfType', entityId, meshComponentTypeId)
 
 if (meshSingleComponentOutcome.IsSuccess()):
    print("GetComponentOfType mesh works.")
 
 firstMeshComponentId = meshSingleComponentOutcome.GetValue()
 
 # returns a list of component IDs for a component type
 meshMultipleComponentOutcome = azlmbr.editor.EditorComponentAPIBus(bus.Broadcast, 'GetComponentsOfType', entityId, meshComponentTypeId)
 
 if (meshMultipleComponentOutcome.IsSuccess()):
    print("GetComponentsOfType mesh works.")
 
 firstMeshComponentId = meshMultipleComponentOutcome.GetValue()[0]
 ```
 
 ### 组件控制事件

API 提供用于验证、启用或禁用以及移除组件的事件。
 
 ```python
 # azlmbr.editor.EditorComponentAPIBus Broadcast events
 
 # Verifies a component instance is valid
 # input: component type ID
 # output: (bool) Returns True if the component is valid
 'IsValid'
 
 # Tests if a component is active
 # input: component type ID
 # output: (bool) Returns True if the component is active
 'IsComponentEnabled'
 
 # Enable Components on an entity using a list of component IDs
 # input: list of component type IDs
 # output: (bool) Returns True if the operation was successful, False otherwise
 'EnableComponents'
 
 # Disable Components on an entity using a list of component IDs
 # input: list of component type IDs
 # output: (bool) Returns True if the operation was successful, False otherwise
 'DisableComponents'
 
 # Remove components from an entity using a list of component IDs
 # input: list of component type IDs
 # output: (bool) Returns True if the operation was successful, False otherwise
 'RemoveComponents'
 ```
 
 **使用示例**:
 
 ```python
 import azlmbr.bus as bus
 
 # test a component is valid
 isValid = azlmbr.editor.EditorComponentAPIBus(bus.Broadcast, 'IsValid', meshComponent)
 if (isValid is True):
    print("Mesh component is valid.")
 
 # test if the component is enabled
 isEnabled = azlmbr.editor.EditorComponentAPIBus(bus.Broadcast, 'IsComponentEnabled', meshComponent)
 if (isEnabled is True):
    print("Mesh component is enabled.")
 
 # enable this Mesh component
 isEnabled = azlmbr.editor.EditorComponentAPIBus(bus.Broadcast, 'EnableComponents', [meshComponent])
 if (isEnabled is True):
    print("Mesh component set to enabled.")
 
 # disable this Mesh component
 didDisable = azlmbr.editor.EditorComponentAPIBus(bus.Broadcast, 'DisableComponents', [meshComponent])
 if (didDisable is True):
    print("Mesh component set to disabled.")
 
 # remove only this Mesh component
 didRemove = azlmbr.editor.EditorComponentAPIBus(bus.Broadcast, 'RemoveComponents', [meshComponent])
 if (didRemove is True):
    print("Mesh component has been removed.")
 ```
 
 ## 组件属性事件

组件属性可以使用一个字符串来访问和修改，该字符串表示通向属性值的直接路径。在属性路径元素之间使用管道字符 `|`作为分隔符。
 
 `azlmbr.editor.EditorComponentAPIBus` 总线用于访问或修改组件属性值。

 ```python
 # azlmbr.editor.EditorComponentAPIBus Broadcast events
 
 # Get value of a property on a component
 # input: component ID
 # input: property path
 # output: (Outcome<object>) the current value of the property
 'GetComponentProperty'
 
 # Set value of a property on a component
 # input: component ID
 # input: property path
 # input: object value
 # output: (Outcome<object>) the new value of the property
 'SetComponentProperty'
 
 # Get a full list of properties in a component
 # input: component ID
 # output: (list of strings) property paths
 'BuildComponentPropertyList'
 ```
 
 **使用示例:**
 
 ```python
 import azlmbr.bus as bus
 
 # Get current value of the mesh asset property of the MeshComponentRenderNode
 propertyPath =  "MeshComponentRenderNode|Mesh asset"
 valueOutcome = azlmbr.editor.EditorComponentAPIBus(bus.Broadcast, 'GetComponentProperty', componentId, propertyPath)
 
 if (valueOutcome.IsSuccess()):
    meshAssetId = valueOutcome.GetValue()
    print ('Old mesh asset is {}'.format(meshAssetId))
 
 # Set the mesh asset
 outcome = None
 if (meshAssetId is not None):
    outcome = azlmbr.editor.EditorComponentAPIBus(bus.Broadcast, 'SetComponentProperty', componentId, propertyPath, meshAssetId)
 
 if(outcome.IsSuccess()):
    result = outcome.GetValue()
    print ('New mesh asset is {}'.format(result))
 
 # Read the properties of this MeshComponentRenderNode
 propertyPaths = azlmbr.editor.EditorComponentAPIBus(bus.Broadcast, 'BuildComponentPropertyList', componentId)
 
 for path in propertyPaths:
    print ('ComponentId path has {}'.format(path))
 ```
 
 ## 编辑属性

要访问此 API，脚本需要访问一个属性树编辑器实例。该对象以编辑器内部属性编辑视图的样式访问组件上的属性。属性的访问从组件的根开始，并沿着标签链进行，直到遇到属性值为止。

创建属性树编辑器实例的常见方法是在内容创建过程中，通过 `EditorComponentAPIBus.AddComponentsOfType` 事件创建组件。
 
 ```python
 componentOutcome = editor.EditorComponentAPIBus(bus.Broadcast, 'AddComponentsOfType', entityId, typeIdsList)
 if (!componentOutcome.IsSuccess()):
    raise Exception('FAILURE FATAL: AddComponentsOfType')
 
 components = componentOutcome.GetValue()
 pteObj = editor.EditorComponentAPIBus(bus.Broadcast, 'BuildComponentPropertyTreeEditor', components[0])
 if(pteObj.IsSuccess()):
   pte = pteObj.GetValue()
 ```
 
 `azlmbr.property.PropertyTreeEditor` API:
 
 ```python
 # type: azlmbr.property.PropertyTreeEditor
 #  - method: build_paths_list() -> string List
 #            Get a complete list of all property paths in the tree.
 #  - method: build_paths_list_with_types() -> string List
 #            Get a complete list of all property paths in the tree with (typenames).
 #  - method: set_visible_enforcement() -> string List
 #            Limits the properties using the visibility flags such as ShowChildrenOnly.
 #  - method: has_attribute(str: path, str: attribute) -> bool
 #            Detects if a property has an attribute.
 #  - method: get_value(str: path) -> Object
 #            Gets a property value.
 #  - method: set_value(str: path, object: value)
 #            Sets a property value.
 #  - method: compare_value(str: path, object: value) -> Boolean
 #            Compares a property value.
 #  - method: is_container(str: path) -> Boolean
 #            True if property path points to a container.
 #  - method: get_container_count(str: path) -> Outcome Integer
 #            Returns the size of the container.
 #  - method: reset_container(str: path) -> Outcome Boolean
 #            Clears the items in a container.
 #  - method: add_container_item(str: path, object key, object value) -> Outcome Boolean
 #            Add an item in a container.
 #  - method: append_container_item(str: path, object value) -> Outcome Boolean
 #            Appends an item in an non-associative container.
 #  - method: remove_container_item(str: path, object key) -> Outcome Boolean
 #            Removes a single item from a container.
 #  - method: update_container_item(str: path, object key, object value) -> Outcome Boolean
 #            Updates an existing the item's value in a container.
 #  - method: get_container_item(str: path, object: key) -> Outcome Object
 #            Retrieves an item value from a container.
 ```
 
 ### 属性容器

编辑器自动化 API 提供了许多特殊方法来处理容器组件属性类型。如果属性树编辑器指向具有容器属性的组件，这些方法就可以访问容器中的项目。

要确定属性是否属于容器类型，请使用 `azlmbr.PropertyTreeEditor.is_container()` 方法。
 
 **使用示例**:
 
 ```python
 # the path to the 'Extended Tags' property
 tagListPropertyPath = 'm_template|Extended Tags'
 
 # get current item count of the container
 outcome = pte.get_container_count(path)
 if(outcome.IsSuccess()):
   count = outcome.GetValue()
 
 # clear the container
 outcome = pte.reset_container(path)
 if(outcome.IsSuccess()):
   print('cleared item')
 
 # if this is a Dictionary type make sure to have a valid key
 key = 0
 value = 'tag_1'
 outcome = pte.add_container_item(path, key, value)
 if(outcome.IsSuccess()):
   print('added item')
 
 # an update needs a key such as an index or a Dictionary key
 value = 'tag_2'
 outcome = pte.update_container_item(path, key, value)
 if(outcome.IsSuccess()):
   print('updated an item')
 
 # the 'append' can be used for properties that are Lists
 value = 'tag_3'
 outcome = pte.append_container_item(path, value)
 if(outcome.IsSuccess()):
   print('appended an item')
 
 # get an item using a key such as an index or a Dictionary key
 key = 0
 outcome = pte.get_container_item(path, key)
 if(outcome.IsSuccess()):
   print('got the value {} from index 0'.format(outcome.GetValue()))
 
 # remove an item using a key,
 # even in List types give an index for the key
 key = 0
 outcome = pte.remove_container_item(path, key)
 if(outcome.IsSuccess()):
   print('removed an item')
 ```
 
 ## 资产管理

编辑器自动化 API 通过 `azlmbr.asset.AssetCatalogRequestBus` 总线提供了一些管理资产的方法。
  
 ```python
 # type: azlmbr.asset.AssetId
 #   - method: IsValid()
 
 # Retrieves an asset-root-relative path by ID.
 # input: asset ID (azlmbr.asset.AssetId)
 # output: (string) relative file path if it's in the catalog, otherwise an empty string
 'GetAssetPathById'
 
 # Retrieves an asset ID given a full or asset root-relative path.
 # input: asset path (string) asset full or asset root-relative path
 # input: typeToRegister (azlmbr.math.Uuid) if autoRegisterIfNotFound is set and the asset isn't already registered, it will be registered as this type
 # input: autoRegisterIfNotFound (bool) registers the asset if not already in the catalog
 # output: (azlmbr.asset.AssetId) valid asset ID if it's in the registry, otherwise an empty AssetId
 'GetAssetIdByPath'
 ```
 
 **使用示例**:
 
 ```python
 import azlmbr.bus as bus
 import azlmbr.math
 
 emptyTypeId = azlmbr.math.Uuid()
 
 # get the cube asset ID
 bRegisterType = False
 cubeAssetId =  azlmbr.asset.AssetCatalogRequestBus(bus.Broadcast, 'GetAssetIdByPath', 'objects/default/primitive_cube.cgf', emptyTypeId, bRegisterType)
 print ('cube asset ID validity is {}'.format(cubeAssetId.IsValid()))
 
 # get the cube path name (relative in project)
 cubePath =  azlmbr.asset.AssetCatalogRequestBus(bus.Broadcast, 'GetAssetPathById', cubeAssetId)
 print ('cube asset path is {}'.format(cubePath))
 ```
