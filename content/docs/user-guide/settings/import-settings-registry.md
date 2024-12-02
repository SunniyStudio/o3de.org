---
title: Settings Registry Chaining
linkTitle: Settings Registry Chaining
description: Learn how the Settings Registry can use the $import directive to merge additional settings registry files in Open 3D Engine (O3DE).
weight: 1000
---

设置注册表支持通过 [JSON 导入器框架](https://github.com/o3de/o3de/blob/development/Code/Framework/AzCore/AzCore/Serialization/Json/JsonImporter.h)合并其他设置注册表文件`*.setreg(patch)`的功能。 
设置注册表文件中名为 `$import` 的任何字段都可用于指定要合并到设置注册表的其他`*.setreg`, `*.setregpatch`或任何其他 JSON 格式文件。

当使用 [Merge API](./developer-api#merge-api)合并设置注册表文件时，它会触发逻辑以在设置文件中查找`$import`指令以合并其他 JSON 文件。
附加文件的合并顺序由`$import`指令的位置决定。 
* 在`$import`指令之前出现的任何设置都可以被导入文件的 JSON 内容覆盖。 
* 在`$import`指令之后显示的任何设置都可以覆盖导入文件的 JSON 内容。

## 使用 `$import`指令字符串的链导入 setreg 文件

以下示例演示如何使用 `$import` 指令通过字符串值合并另一个 Settings Registry 文件。

#### `test.apple.setreg`
```json
{
    "pre_field": { "first": 1, "second": 2 },
    "$import": "test.ios.setreg",
    "post_field": { "1": 11, "2": 12 }
}
```

#### `test.ios.setreg`
```json
{
    "pre_field": { "second": 202 },
    "post_field": { "2": 120 }
}
```

合并`test.ios.setreg`后，`test.apple.setreg`文件会生成以下最终 JSON 输出：

### Import Result
```json
{
    "pre_field": { "first": 1, "second": 202 },
    "post_field": { "1": 11, "2": 12 }
}
```

请注意，出现在 `$import`指令之前的 `"pre_field"`的`"second"`字段值(`202`)被导入的`test.ios.setreg`文件覆盖。 
将差异与 `$import`指令后面出现的`"post_field"`进行对比`"2"`字段值 (`12`)来自原始的`test.apple.setreg` 文件。

## 使用`$import`指令对象链导入 setreg 文件

下面显示了如何使用`$import`指令来合并另一个使用带有 `filename` 键的 JSON 对象的 Settings Registry 文件。 
在`$import`指令字符串上使用`$import` 指令对象的原因是，对象语法支持额外的 `"patch"` 字段。
`"patch"` 字段是一个 JSON 对象，其内容将直接合并到导入的内容数据上，然后再合并到初始 Settings Registry 文件数据上。

#### `test.mobile.setreg`
```json
{
    "device_abis": [
        "arm64-v8a"
    ]
}
```

在将导入的内容读入内存之后，但在将内容应用于其余的`test.android.setreg`数据之前，`test.android.setreg`会修补`test.mobile.setreg`的`"device_abis"`数组。

#### `test.android.setreg`
```json
{
    "$import": {
        "filename": "test.mobile.setreg",
        "patch": {
            "device_abis": [
                "arm64-v8a",
                "x86_64"
            ]
        }
    }
}
```

## 使用 `$import`  指令导入多个 setreg 文件

下面显示了如何使用同一`$import` 键的多次使用来合并多个 Settings Registry 文件。

给定后面的两个 Settings Registry 文件 `number.setreg` 和 `string.setreg`，合并的结果取决于文件在导入文件中的显示顺序。

### 示例数据

#### `number.setreg`
```json
{
    "1": 7,
    "2": 14
}
```

#### `string.setreg`
```json
{
    "1": "Hello",
    "3": "World"
}
```

### 示例 1
将聚合 setreg 文件与以下内容合并将合并 `string.setreg` 和 `number.setreg`.。

#### `aggregate.setreg`
```json
{
    "$import": "number.setreg",
    "$import": "string.setreg"
}
```

键 `"1"`的值来自`string.setreg`文件。

#### `aggregate.setreg` (Post Import)
```json
{
    "1": "Hello",
    "2": 14,
    "3": "World"
}
```

### 示例 2
交换 `$import` 指令的顺序会更改 Settings Registry 中值的结果。

#### `aggregate2.setreg`
```json
{
    "$import": "string.setreg",
    "$import": "number.setreg"
}
```


键 `"1"`的值来自`number.setreg`文件。

#### `aggregate2.setreg` (Post Import)
```json
{
    "1": 7,
    "2": 14,
    "3": "World"
}
```

使用前面示例中所示的方法，您可以使用 `MergeSettingsFolder` 或 `MergeSettingsFile`  将一系列文件合并到设置注册表中，[Merge APIs](./developer-api#merge-api).
