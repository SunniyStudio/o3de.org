---
linktitle: 动态数据槽
title: Script Canvas 中的动态数据槽
description: 使用动态数据槽使单个节点能够在 Script Canvas 中处理各种数据类型，Script Canvas 是 Open 3D Engine （O3DE） 中的可视化脚本系统。
weight: 300
---

在某些情况下，单个节点可以处理多种不同的输入类型（例如，**Lerp Between** 节点可以处理数字和矢量对象）。要减少唯一节点的数量，您可以使用动态数据槽。动态数据槽使单个节点能够处理各种数据类型。同时，它们允许对可以连接的数据类型进行限制。

您可以使用`ScriptCanvas_DynamicDataSlot` 标签将 `DynamicDataSlot` 添加到任何节点，如以下示例所示。

```cpp
ScriptCanvas_DynamicDataSlot(ScriptCanvas::DynamicDataType::Value,
                             ScriptCanvas::ConnectionType::Output,
                             ScriptCanvas_DynamicDataSlot::Name("Step", "The value of the current step of the lerp.")
                             ScriptCanvas_DynamicDataSlot::DynamicGroup("LerpGroup")
                             ScriptCanvas_DynamicDataSlot::RestrictedTypeContractTag({ Data::Type::Number(), Data::Type::Vector2(), Data::Type::Vector3(), Data::Type::Vector4() })
                            )
```

`ScriptCanvas_DynamicDataSlot` 标记包含以下 Code Gen 属性：
2
| 属性 | 说明 |
| --- | --- |
| DynamicDataType |  允许用户指定动态类型信息的宏类别。DynamicDataType 具有以下支持的值：<br><ul><li>**Container &ndash;**任何数据类型的映射或数组。</li><li>**Value &ndash;** 任何非 map 或非数组值。</li><li>**Any &ndash;** 任何 Container 或 Value 类型。</li></ul> |
| DynamicGroup | 统一一组动态类型的槽。当一个槽具有类型时，组中的所有槽共享相同的类型。此属性可用于确保直通值或操作数都共享一个公共类型。 |
| RestrictedTypeContractTag | 限制动态类型槽接受的数据类型。采用一个参数，该参数是支持的数据类型列表。 |

## 有关组的重要说明

出于合同限制的目的，分组的插槽充当单个单元。如果一个槽不允许特定类型，则该组中的任何槽都不允许该类型。此行为是因为连接类型通过组的每个成员以及连接到该组的每个组传播。这也适用于相互连接的动态数据槽以及已连接但未分组的任何动态数据槽。

### 链式动态类型

连接动态类型后，接收槽将采用连接槽的数据类型和限制。

**例**

![Create a dynamically chained node.](/images/user-guide/scripting/script-canvas/script-canvas-chained-dynamic-types.gif)

与动态组一样，链接的任何一对动态类型节点都共享相同的限制。这包括来自节点组的任何限制。此外，插槽通常处于尽可能不受限制的状态。除非显示类型为一组动态类型的插槽提供类型，否则插槽将保持无类型状态。

## 以编程方式添加动态数据槽

您还可以以编程方式添加动态插槽。使用 `DynamicDataSlotConfiguration` 定义槽，然后使用通用 `AddSlot`方法添加槽，如以下示例所示。

```cpp
SlotId MyNode::AddDynamicSlot(AZStd::string_view name, AZStd::string_view toolTip, ConnectionType connectionType)
{
    DynamicDataSlotConfiguration slotConfiguration;

    // Generic Slot Configuration
    slotConfiguration.m_name = name;
    slotConfiguration.m_toolTip = toolTip;
    slotConfiguration.SetConnectionType(connectionType);
    slotConfiguration.m_addUniqueSlotByNameAndType = false;

    // Contract Descs provides a list of contracts that must be satisfied for a connection to be accepted to this slot.
    //slotConfiguration.m_contractDescs.push_back(TypeRestriction);

    // DynamicDataSlot Specific Configurations
    slotConfiguration.m_dynamicGroup = "DynamicDataGroup";
    slotConfiguration.m_dynamicDataType = DynamicDataType::Value;
    ////

    return AddSlot(slotConfiguration);
}
```

有关 `DynamicGroup` 和 `DynamicDataType` 的信息，请参阅本主题前面的 `ScriptCanvas_DynamicDataSlot` 属性表。
