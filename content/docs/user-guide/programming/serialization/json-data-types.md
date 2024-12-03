---
linkTitle: O3DE数据类型的JSON序列化
title: O3DE数据类型的JSON序列化
description: 了解 JSON 类型如何转换为内部 Open 3D Engine (O3DE) 数据类型。
weight: 400
---

除了直接映射到 JSON 类型的原始 C++ 类型外， **Open 3D Engine (O3DE)** 还支持许多 `AZStd` 库对象的序列化。JSON 输出和反序列化对象完全根据相应的 C++ 类型确定。有关如何注册成员以及如何通过反射系统确定其类型的信息，请参阅**序列化和反序列化 JSON 对象**主题中的 [序列化](json-serialize-deserialize/#serialization)。

本主题参考了 O3DE 序列化和反序列化支持的类型、序列化器默认如何映射这些类型，以及如何将 JSON 类型强制转换回 C++ 类型的信息。

## 基元类型

序列化中使用的基元类型是**布尔**、**整数**、**浮点数**和**字符串**。这些 C++ 类型可自然映射到本地 JSON 布尔型、数字型和字符串型。唯一支持序列化的字符串类型是 `AZstd::string` 和 `OSString`。

从任何原始 JSON 类型到这些 C++ 类型的反序列化过程如下： 


**基础类型**

| C++ 类型 | JSON boolean | JSON number | JSON string |
| --- | --- | --- | --- |
| bool | 直接映射。 | 0 映射为false，所有其他值均为true。 | 与 “true ”和 “false ”进行不区分大小写的比较，以映射到相应的 C++ 值。 |
| 整数类型 | True 映射为 1， 映射为 0。 | 直接映射，浮点数值被截断。 | 尝试提取 64 位整数并转换为目标整数类型。  |
| 浮点数类型 | True 映射为 1， 映射为 0。 | 直接映射。 | 尝试提取 64 位浮点并将其转换为目标类型。 |
| 字符串 (仅AZstd::string 和 OSString) | True 被转化为 "True"， 和 False 被转化为 "False"。 | 数字的字符串表示。 | 直接映射。 |

## 指针 

指针和智能指针类型会被序列化为它们所指向的类型的 JSON 值，并遵循该类型的所有规则。对于指向 C++ 对象的指针，这意味着被指向的对象必须在所使用的序列化上下文中注册。虽然大多数类型不需要任何元数据就能安全地序列化，但对于派生类来说，一个额外的 `$type` 键将与类名一起序列化到 JSON 对象中。如果类名可能存在冲突，则可使用 O3DE 运行时中的全命名名称或类的 UUID。

 反序列化时，`$type`值将作为一个提示，以确定在必要时重建哪个对象。否则，只会检查成员的 C++ 类型。反序列化按成员进行，使用序列化上下文注册的字段映射将 JSON 对象键映射到 C++ 成员。

## 枚举 

 要对枚举进行序列化，需要在与被序列化类相同的序列化上下文中对枚举进行 [注册](register-objects/#register-enums)。如何序列化和反序列化枚举值取决于枚举注册的细节。
+ **枚举值与已注册字段值匹配** - 值将序列化为包含字段名称的字符串。
+ **枚举值不能用 ORed 注册值表示** - 数值序列化为整数。
+  **枚举值可以用 ORed 注册值来表示** - 该值被序列化为一个数组，其中包含被 OR 在一起的字段名字符串。
+  **枚举值可表示为已注册值和未注册值的混合体** - 数值被序列化为一个数组，数组中包含相应字段名称的字符串和一个整数，将这两个字符串或整数组合在一起，就产生了序列化数值。

**示例 序列化一个枚举**
这些规则最好用一个简单的示例来说明：枚举定义、枚举在序列化上下文中的注册以及各种值的序列化输出。

```
enum ExampleEnum : uint8_t
{
    Flag1 = 1,
    Flag2 = 2,
    Flag3 = 4,
    Flag4 = 8,
    Flag5 = Flag2 | Flag3
}

//------

// Registration with SerializedContext
serializeContext->Enum<ExampleEnum>()
    ->Value("Flag1", ExampleEnum::Flag1)
    ->Value("Flag2", ExampleEnum::Flag2)
    ->Value("Flag4", ExampleEnum::Flag4)
    ->Value("Flag2Flag3Combo", ExampleEnum::Flag5)

//------

// ExampleEnum initialization with int
ExampleEnum(0) // Serializes as 0
ExampleEnum(1) // Serializes as "Flag1"
ExampleEnum(-1) // Serializes as 255
ExampleEnum(10) // Serializes as ["Flag2", "Flag4"]

// Declared ExampleEnum values
ExampleEnum::Flag1 // Serializes as "Flag1"
ExampleEnum::Flag3 // Serializes as 4
ExampleEnum::Flag5 // Serializes as "Flag2Flag3Combo"

// ORed together values
ExampleEnum::Flag1 | ExampleEnum::Flag4 // Serializes as ["Flag1", "Flag4"]
ExampleEnum::Flag2 | ExampleEnum::Flag3 // Serializes as "Flag2Flag3Combo"
ExampleEnum::Flag1 | ExampleEnum::Flag3 // Serializes as ["Flag1", 4]
ExampleEnum::Flag2 | ExampleEnum::Flag3 | ExampleEnum::Flag1 // Serializes as ["Flag1", "Flag2Flag3Combo"]
ExampleEnum::Flag4 | 16 // Serializes as ["Flag4", 16]
```

根据这些序列化输出，反序列化的行为符合预期。首先尝试将字符串映射到字段名，然后为枚举分配字段值。如果字符串包含字段名以外的内容，则会尝试将其类型转换为整数。数组中的每个元素都要进行评估，以转换为枚举字段或整数值，然后将评估结果进行 OR，以生成最终的反序列化值。

## 向量 

序列化支持二维、三维和四维向量。一个 N 维向量会按 X、Y、Z 和 W 坐标的顺序序列化为一个包含 N 个浮点数的 JSON 数组。

对于反序列化，可以从任意长度的数组中读取向量，但最多只能读取目标类型中的元素数。如果数组中的元素少于矢量类型，这些矢量组件将被分配默认值 0。数组元素遵循矢量元素 C++ 浮点类型的反序列化规则。向量也可以从 JSON 对象反序列化，其中对象键以大小写不敏感的比较方式映射到向量组件名称。其他键将被忽略，缺少键的组件将使用默认值。

## 容器 

 序列化支持来自 `AZStd`的大量容器。其中包括 **数组**、**vector**、**列表**、**集合**、**map** 和 **元组** 类型。

### 数组 

以下类型会序列化为 JSON 数组：
+ `AZStd::array`
+ `AZStd::fixed_vector`
+ `AZStd::forward_list`
+ `AZStd::list`
+ `AZStd::vector`
+ `AZStd::pair`
+ `AZStd::tuple`
+ `AZStd::set` - 对序列化数组中的值进行排序
+ `AZStd::unordered_set` - 对序列化数组中的值进行排序

将 JSON 数组反序列化为 C++ 数组、列表、vector、集合、pair或元组类型是一种直接的逐元素转换。每个数组元素的类型都会根据其他 JSON 反序列化规则转换为目标容器的值类型。缺失的元素会映射到容器值类型的默认值，其他元素会被忽略。当尝试反序列化为这些 C++ 类型之一时，JSON 数组以外的类型将导致转换错误。

### Map类型 

map类型\(`AZStd::map`, `AZStd::unordered_map`, `AZStd::unordered_multimap`\)的序列化取决于map的键类型。以可序列化的字符串类型\(`AZStd::string` 或 `OSString`\) 作为键的映射会直接序列化为具有等价键/值对的 JSON 对象。使用任何其他键类型的映射都会序列化为 JSON 数组，其中每个元素都是键/值对。例如，映射 `AZStd::map<uint8_t,uint8_t> = {0:1, 2:3}` 序列化为 JSON 数组 `[{“Key”: 0, “Value”: 1}, {“Key”: 2, “Value”: 3}]`。

## 其他类型

JSON 序列化支持的其他内部类型有 **UUID** 和 **color**。

### Uuid

`AZ::Uuid`通过`AZ::Uuid::ToString()`映射为 JSON 字符串，`AZ::Uuid::CreateString()`将字符串转换回 UUID。

### 颜色值

颜色值可以多种方式表示为 JSON，用户可以灵活使用最适合自己需要的方式。

#### 从数组反序列化颜色

颜色类型序列化为 JSON 数组，RGB 颜色包含 3 个浮点数值，RGBA 包含 4 个数值。包含 3 或 4 个值的 JSON 数组可直接反序列化为 RGB 或 RGBA。

| 键 | 值类型 | 示例 |
| --- | --- | --- |
| RGB | 由 3 个浮点元素组成的数组。 | \[1.0, 0.3, 0.2\] |
| RGBA | 由 4 个浮点元素组成的数组。 | \[1.0, 0.3, 0.2, 0.8\] |

#### 从 JSON 对象反序列化颜色

JSON 对象也可以反序列化为颜色，从而允许对颜色值进行其他常用编码。要将一个对象反序列化为颜色，该对象必须正好有以下名称中的一个键，并具有等同的值类型。

| 键 | 值类型 | 示例 |
| --- | --- | --- |
| RGB | 由 3 个浮点元素组成的数组。 | \{ "RGB" : \[1.0, 0.3, 0.2\] \} |
| RGBA | 由 4 个浮点元素组成的数组。 | \{ "RGBA" : \[1.0, 0.3, 0.2, 0.8\] \} |
| RGB8 | 由 3 个整数元素组成的数组，每个值的范围为 \[0, 255\]。 | \{ "RGB8" : \[255, 77, 51\] \} |
| RGBA8 | 由 4 个整数元素组成的数组，每个值的范围为 \[0, 255\]。 | \{ "RGBA8" : \[255, 77, 51, 204\] \} |
| HEX | 字符串，包含代表颜色的 3 个 8 位十六进制值。 | \{ "HEX" : "FF4D33" \} |
| HEXA | 字符串，包含 4 个 8 位十六进制值，分别代表颜色和 alpha 通道。 | \{ "HEXA" : "FF4D33CC" \} |
