---
linkTitle: 编辑上下文
title: O3DE中的编辑上下文
description: 使用编辑上下文可在 O3DE 编辑器（Open 3D Engine (O3DE) 的内容创建环境）中显示用于编辑的参数。
weight: 200
---

O3DE 编辑上下文是一种依赖于[序列化上下文](/docs/user-guide/programming/components/reflection/serialization-context/)的实用上下文。您可以使用编辑上下文公开序列化数据的参数，以便在**O3DE 编辑器**中进行编辑。不过，编辑上下文是可编辑数据的抽象容器。因此，它并不直接与任何特定的编辑器绑定。任何编辑器都可以查询编辑上下文中的数据，并实现自己的可视化和编辑控件。

以下代码显示了一个 `EditContext` 定义：

```cpp
AZ::EditContext* editContext = serializeContext->GetEditContext();
if (editContext)
{
    editContext->Class<MyEditStruct>("MyEditStruct", "My edit struct class used for ...")
    ;
}
```

`EditContext`由`ClassElements` 和 `DataElements`组成。
+ `ClassElement` - 指定通过 `SerializeContext::Class` 反映的类的属性。您可以用它来对共同元素进行分组。
+ `DataElement` - 指定通过 `SerializeContext::Field` 序列化的字段的显示、行为和可视化。
+ `UIElement` - 指定`UIHandler`的显示、行为和可视化，但不将其与 “SerializeContext::Field ”绑定。

## 属性 

您可以使用 `EditContext` 为类和数据元素添加任意属性。

属性是基于模板的。因此，它们可以是任何类型，包括函数，如下例所示。

```cpp
editContext->Class<EditorLightComponent>(
	"Light", "Attach lighting to an entity.")
	->ClassElement(AZ::Edit::ClassElements::EditorData, "")
		->Attribute(AZ::Edit::Attributes::AutoExpand, true)
		->Attribute(AZ::Edit::Attributes::NameLabelOverride, &EditorLightComponent::GetLightTypeText)
	->DataElement(AZ::Edit::UIHandlers::Default, &EditorLightComponent::m_configuration, "Settings", "Light configuration")
		->Attribute(AZ::Edit::Attributes::Visibility, AZ_CRC("PropertyVisibility_ShowChildrenOnly", 0xef428f20))

    ->ClassElement(AZ::Edit::ClassElements::Group, "Cubemap generation")
	    ->Attribute(AZ::Edit::Attributes::Visibility, &EditorLightComponent::IsProbe)
    	->Attribute(AZ::Edit::Attributes::AutoExpand, true)
```

为方便起见，O3DE在文件`dev\Code\Framework\AzCore\AzCore\Serialization\EditContextConstants.inl`中存储了一个常用属性库。
