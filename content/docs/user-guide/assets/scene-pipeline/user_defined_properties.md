---
title: "用户定义的属性"
date: 2022-03-08
slug: scene-api-udp
author: Allen Jackson
---

艺术家可以将源场景中的元数据存储到场景节点上的用户自定义属性 (UDP)。这种用户定义的属性元数据可以导出到源场景文件（即 FBX）中。这些元数据非常重要，因为它可以显示场景的细节，例如将哪些网格节点用作细节级别（LOD）和/或物理碰撞网格。存储这类元数据的最佳方法是使用 DCC 工具内部使用的属性子系统，然后将这些用户定义的属性导出为 FBX 和 glTF 等可持久保存元数据的文件格式。这种 UDP 元数据在与引擎相关的流水线调整方面有很多用途，但在扩展场景流水线和生成编辑器自定义游戏内容方面也有很多用途。

O3DE 场景流水线会读取这些 UDP 元数据，并将其作为包含 CustomPropertyData 内容的节点添加到场景图中。这些 UDP 元数据本身没有任何作用，因为它们是其他场景构建逻辑单元访问的属性，以便执行构建操作和/或修改。

自定义属性数据（CustomPropertyData）中有一个属性图（PropertyMap），用于存储该节点的字符串到值属性的字典。场景构建器可以使用 PropertyMap 访问这些元数据。这些属性可以存储字符串、布尔、32 位无符号整数、64 位有符号整数、浮点数和双倍值类型。元数据可用于修改场景的规则清单、场景产品资产参数化和下游场景参数设置。每个节点都可以有一个 PropertyMap，其中的键不会重复。PropertyMap 和场景图中的其他元素一样是只读的。

场景构建器可以使用 Python 或 C++ API 访问 CustomPropertyData，方法是处理任何将场景图作为输入的事件。典型的方法是处理更新清单（Update Manifest）事件，这样场景构建器就可以修改场景清单规则。

## 使用 UDP 在默认程序预制中分配材质

Prefab 游戏中的默认程序预制场景生成器有一个如何使用 UDP 元数据自定义场景管道的示例。Prefab gem 会为场景图构建一个默认预制板，它会在网格数据节点中查找 UDP ```o3de_default_material```，以便在预制板中为该网格组定义 O3DE 渲染材质。这样，美工人员就可以在 DCC 工具中分配 O3DE 渲染材质。每次导出源场景文件（即 FBX）时，都会在场景管道中保留此引用。

## 场景管道中的 UDP 访问

场景生成器 API 可以使用 C++ 或 Python 访问用户定义的属性。

### C++ 访问

当 C++ 事件被触发时，它通常会将请求上下文作为输入发送，其中包含可能具有 CustomPropertyData 节点的节点场景图。在 C++ 中，有多种使用节点索引查看节点的方法。要访问节点内容，可使用场景图的 GetNodeContent() 方法返回一个指向 ICustomPropertyData 的智能指针。如果智能指针不为空，则可使用 GetPropertyMap() 方法访问字符串到值的属性字典。

C++ 代码示例：

```c++
const auto customPropertyData = azrtti_cast<const DataTypes::ICustomPropertyData*>(graph.GetNodeContent(propertyDataIndex));
if (!customPropertyData)
{
    return false;
}

const auto propertyMaterialPathIterator = customPropertyData->GetPropertyMap().find("o3de_default_material");
if (propertyMaterialPathIterator == customPropertyData->GetPropertyMap().end())
{
    return false;
}

const AZStd::any& propertyMaterialPath = propertyMaterialPathIterator->second;
if (propertyMaterialPath.empty() || propertyMaterialPath.is<AZStd::string>() == false)
{
    return false;
}

// find asset path via node data
const AZStd::string* materialAssetPath = AZStd::any_cast<AZStd::string>(&propertyMaterialPath);
if (materialAssetPath->empty())
{
    return false;
}
```

### Python 访问

Python 脚本使用回调来访问场景图节点，以查找用户定义的属性。CustomPropertyData 将以 Python 字典的形式返回，供脚本枚举以查找键值对。

Python 代码示例：

```python
def print_properties(scene):
    scene_graph = scene_api.scene_data.SceneGraph(scene.graph)
    node = scene_graph.get_root()
    children = []

    while node.IsValid():
        if scene_graph.has_node_child(node):
            children.append(scene_graph.get_node_child(node))

        node_name = scene_api.scene_data.SceneGraphName(scene_graph.get_node_name(node))
        node_content = scene_graph.get_node_content(node)
        if node_content.CastWithTypeName('CustomPropertyData'):
            if len(node_name.get_path()):
                print(f'CustomPropertyMap {node_name.get_path()}')
                props = node_content.Invoke('GetPropertyMap', [])
                for index, (key, value) in enumerate(props.items()):
                    print(f'property index:{index} key:{key} value:{value}')

        if scene_graph.has_node_sibling(node):
            node = scene_graph.get_node_sibling(node)
        elif children:
            node = children.pop()
        else:
            node = azlmbr.scene.graph.NodeIndex()
```

### 带有 LOD 和碰撞网格的汽车示例

这是一个 Car.fbx 文件的示例，该文件保存了用于 LOD 的 UDP 元数据和 PhsyX 元数据。资产处理器将请求场景生成器处理 Car.fbx 源场景资产。场景生成器（在资产生成器进程中运行）会导入场景，并用包含 CustomPropertyData 内容的场景图节点构建场景图。随后，默认的程序化预制件生成器将读取自定义属性，以建立 LOD 清单规则和物理清单规则。最后，这些清单规则将转化为 LOD 模型和物理网格产品文件。

![Car FBX example with UDPs.](/images/user-guide/assets/pipeline/scene_api_udp.jpg)
