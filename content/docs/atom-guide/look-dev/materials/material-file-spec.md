---
title: "材质文件规范"
description: "材质文件（`*.material`）以 JSON 格式编写，包含以下元素。"
toc: true
---

## Elements

材质文件（`*.material`）以 JSON 格式编写，包含以下元素。

### **description** (**可选**)  
提供材质作者的说明或评论。

### **materialType**
材质类型的路径。材质必须引用材质类型，该类型提供了可用属性列表和渲染时使用的着色器。该路径必须与资产根目录或材质文件相对。

### **materialTypeVersion** (**可选**)  

表示用于创建此材质的材质类型的版本号。材质类型版本更新功能可以使用它来支持与材质类型旧版本的向后兼容性。请参阅材质类型文件规范中的 [`版本`](material-type-file-spec/#version) 和 [`版本更新`](material-type-file-spec/#versionupdates) 。

### **parentMaterial** (**可选**)  
父材质文件的路径。如果指定，该材质将继承另一个父材质的属性。父材质必须与该材质具有相同的材质类型。路径必须是资产根目录或材质文件的相对路径。如果未指定，材质将直接继承材质类型的默认值。

### **propertyValues**

列出材质属性的名称和设置值。此处未指定的任何属性都将继承父材质的值，如果有的话，或者继承材质类型的默认值。值所使用的 JSON 数据类型应与`.materialtype`文件中指定的属性数据类型相匹配。不过，也支持一些数据类型转换；例如，“1 ”可用于将浮点属性设置为 “1.0”。

图像属性必须包含源图像文件的路径。该路径必须是资产根目录或材质文件的相对路径。

枚举属性必须使用由`.materialtype`文件中的属性定义所指定的可用枚举值之一。

所有其他属性类型必须根据其标准 JSON 序列化来指定。参见 [O3DE 数据类型的 JSON 序列化](/docs/user-guide/programming/serialization/json-data-types)。

下面的示例演示了如何为每种可能的数据类型设置属性。

### **properties** (已弃用)
旧版本的材质文件格式是按嵌套 JSON 对象中的组来组织属性的。现在这种格式已被 `propertyValues`（如前所述）所取代，后者使用的是属性标识符的平面列表。引擎仍可加载旧格式，但新材质文件必须使用新格式。

## 示例

```json
{
    "description":  "This is an example only. The material type, parent, and properties don't exist.",
    "materialType": "Materials/Types/Example.materialtype",
    "materialTypeVersion": 6,
    "parentMaterial":  "Materials/ExampleParent.material",
    "propertyValues": {
        "example.MyBool": true,
        "example.MyInt": -1,
        "example.MyUInt": 1,
        "example.MyFloat": 1.0,
        "example.MyVector2": [1.0, 1.0],
        "example.MyVector3": [1.0, 1.0, 1.0],
        "example.MyVector4": [1.0, 1.0, 1.0, 1.0],
        "example.MyColor": [1.0, 1.0, 1.0, 1.0],
        "example.MyImage": "Textures/Default/default_basecolor.tif",
        "example.MyEnum": "Basic"
    }
}
```
