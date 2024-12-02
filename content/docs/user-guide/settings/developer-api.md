---
linkTitle: API 示例
title: Settings Registry API 示例
description: 提供有关可用设置注册表 API 的信息和示例
weight: 800
---

详细说明 Settings Registry 提供的可用 API 集。 提供了使用查询 API 和访客 API 查询设置的功能的示例。如何使用 setter API 定义和更新设置。

可以使用合并 API 从文件或内存中的 JSON 文档中合并设置。 最后，可以使用通知 API 获取有关何时修改或删除设置的通知。

### Query API

[Query API](https://github.com/o3de/o3de/blob/02846cf44347cbf4fae0faacc4a2ba74284908ff/Code/Framework/AzCore/AzCore/Settings/SettingsRegistry.h#L224-L260)  支持直接查询 `bool`, `int64_t`, `double`, `AZStd::string`, `AZStd::fixed_string` 的类型，以及反映到 `SerializeContext` 的任何对象。从 `SerializeContext` 查询对象的 getter 方法可以安全使用，但应将其作为 Settings Registry 的实现细节来遵守。`SettingsRegistryInterface::GetObject`接口将保持稳定，但不应对对象的序列化方式做出任何假设。

以下示例演示了内置设置类型的查询 API (`bool`, `AZ::s64`, `AZ::u64` `double`, `(fixed_)string`):

```c++
// Setting FileIOBase alias based on values within the Settings Registry
using FixedValueString = AZ::SettingsRegistryInterface::FixedValueString
if (FixedValueString pathAlias;
     m_settingsRegistry->Get(pathAlias, AZ::SettingsRegistryMergeUtils::FilePathKey_CacheRootFolder))
{
    fileIoBase->SetAlias("@products@", pathAlias.c_str());
}
if (AZStd::string pathAlias;
     m_settingsRegistry->Get(pathAlias, AZ::SettingsRegistryMergeUtils::FilePathKey_ProjectPath))
{
    fileIoBase->SetAlias("@projectroot@", pathAlias.c_str());
}

if (AZ::s64 intValue;
    m_settingsRegistry->Get(pathAlias, "/O3DE/Int64Value"))
{
    // Do stuff with int value
}

```

#### 使用查询 API 加载 C++ 类对象

要使用 Settings Registry 加载 C++ 类对象，类型本身必须反映到 `SerializeContext` 才能使用查询 API。使用`SettingsRegistry::GetObject`函数，而不是调用`SettingsRegistryInterface::Get`方法来查询内置 JSON 类型。

以下示例演示如何使用`SettingsRegistryInterface::GetObject`函数将 SettingsRegistry 加载到`AzPhysics::SceneConfiguration`中。

```c++
// Make sure the AzPhysics::SceneConfiguration class is reflected
void SceneConfiguration::Reflect(AZ::ReflectContext* context)
{
    Physics::WorldConfiguration::Reflect(context);

    if (auto* serializeContext = azrtti_cast<AZ::SerializeContext*>(context))
    {
        serializeContext->Class<SceneConfiguration>()
            ->Version(1)
            ->Field("LegacyConfig", &SceneConfiguration::m_legacyConfiguration)
            ->Field("LegacyId", &SceneConfiguration::m_legacyId)
            ->Field("Name", &SceneConfiguration::m_sceneName)
            ;
    }
}

// ... Later on the SceneConfiguration class can be loaded from the SettingsRegistry using the `GetObject` function
AzPhysics::SceneConfiguration sceneConfig;
bool configurationRead = false;

AZ::SettingsRegistryInterface* settingsRegistry = AZ::SettingsRegistry::Get();
if (settingsRegistry)
{
    configurationRead = settingsRegistry->GetObject(sceneConfig, "/Amazon/Gems/PhysX/DefaultSceneConfiguration");
}
```

### Visitor API

[Visitor API](https://github.com/o3de/o3de/blob/02846cf44347cbf4fae0faacc4a2ba74284908ff/Code/Framework/AzCore/AzCore/Settings/SettingsRegistry.h#L185-L194) 支持从指定的 JSON 指针开始递归访问 JSON 对象和数组子项。访客 API 允许处理每个子字段，这在使用`GetObject` API 时不可用。它还在处理 JSON 数据时提供了类型灵活性。设置注册表的访问者必须实现`AZ::SettingsRegistryInterface::Visitor`类。该类的实例可以提供给 `AZ::SettingsRegistryInterface::Visit()` 方法。

建议在序列化没有任何 `SerializeContext` 反射的复杂对象时，访问者。由于它未绑定到 `SerializeContext`，因此可以使用相同的逻辑从设置注册表中读取对象，使用其他编程语言（如 Python、JavaScript 或 C#）中提供的 JSON 工具。

以下示例演示了如何使用访客 API 收集在“/O3DE/Gems”的 settings 对象下填充的活动 Gem 信息。 这是由 [GetGemsInfo](https://github.com/o3de/o3de/blob/adb37ad54d69bcef23c5b7f70c77e669f8202194/Code/Framework/AzFramework/AzFramework/Gem/GemInfo.cpp#L27-L64) 函数完成的。

```c++
// Queries the Settings Registry to get the list active gems targets and source paths
auto GemSettingsVisitor = [&gemInfoList](const AZ::SettingsRegistryInterface::VisitArgs& gemVisitArgs)
{
    auto FindGemInfoByName = [&gemVisitArgs](const GemInfo& gemInfo)
    {
        return gemVisitArgs.m_fieldName == gemInfo.m_gemName;
    };
    auto gemInfoFoundIter = AZStd::ranges::find_if(gemInfoList, FindGemInfoByName);
    GemInfo& gemInfo = gemInfoFoundIter != gemInfoList.end() ? *gemInfoFoundIter : gemInfoList.emplace_back(gemVisitArgs.m_fieldName);

    // Read the Gem Target Name into target Name field
    auto VisitGemTargets = [&gemInfo](const AZ::SettingsRegistryInterface::VisitArgs& visitArgs)
    {
        // Assume the fieldName is the name of the target in this case
        gemInfo.m_gemTargetNames.emplace_back(visitArgs.m_fieldName);
        return AZ::SettingsRegistryInterface::VisitResponse::Skip;
    };
    AZ::SettingsRegistryVisitorUtils::VisitObject(gemVisitArgs.m_registry, VisitGemTargets,
        FixedValueString::format("%.*s/Targets", AZ_STRING_ARG(gemVisitArgs.m_jsonKeyPath)));

    // Visit the "SourcePath" array fields of the gem to populate the Gem Absolute Source Paths array
    const auto gemPathKey = FixedValueString::format("%s/%.*s/Path",
        AZ::SettingsRegistryMergeUtils::ManifestGemsRootKey, AZ_STRING_ARG(gemVisitArgs.m_fieldName));
    if (AZ::IO::Path gemRootPath; gemVisitArgs.m_registry.Get(gemRootPath.Native(), gemPathKey))
    {
        gemInfo.m_absoluteSourcePaths.emplace_back(gemRootPath);
    }

    return AZ::SettingsRegistryInterface::VisitResponse::Skip;
};

// Visit the "/O3DE/Gems" setting object to query the data for all active gems
AZ::SettingsRegistryVisitorUtils::VisitObject(settingsRegistry, GemSettingsVisitor,
    AZ::SettingsRegistryMergeUtils::ActiveGemsRootKey);

```

[visitor utilities API](https://github.com/o3de/o3de/blob/02846cf44347cbf4fae0faacc4a2ba74284908ff/Code/Framework/AzCore/AzCore/Settings/SettingsRegistryVisitorUtils.h#L16-L82) 可以简化对 JSON 对象的直接子字段的访问。这些类和函数位于`SettingsRegistryVisitorUtils.h`中。

例如，请考虑以下 JSON 数组：

```json
{
    "O3DE": {
        "Array" : [
            1,
            2
        ]
    }
}
```

可以使用`SettingsRegistryVisitorUtils` 访问数组的字段，如以下示例所示：

```c++
struct AppendArrayVisitor
    : AZ::SettingsRegistryVisitorUtils::ArrayVisitor
{
    CustomArrayVisitor() = default;

    AZ::SettingsRegistryInterface::VisitResponse Visit(const AZ::SettingsRegistryInterface::VisitArgs& visitArgs)
    {
        if (int value; visitArgs.m_registry.Get(value, visitArgs.m_jsonKeyPath))
        {
            m_array.push_back(value);
        }

        return AZ::SettingsRegistryInterface::VisitResponse::Skip;
    }

    AZStd::vector<int> m_array;
};
// ...
settingsRegistry->Visit(AppendArrayVisitor, "/O3DE/Array");
```

相同的方法可用于访问对象的字段。请考虑以下 JSON 对象：

```json
{
    "O3DE": {
        "Object": {
            "Field1": 1,
            "Field2": 2
        }
    }
}
```

您可以使用帮助程序访问者回调函数，如以下示例所示：

```c++
AZStd::unordered_map<AZStd::string, int> fieldToIntMap;
auto AppendObjectFields = [&fieldToIntMap](const AZ::SettingsRegistryInterface::VisitArgs& visitArgs)
{
    if (int value{}; visitArgs.m_registry.Get(value, visitArgs.m_jsonKeyPath))
    {
        fieldToIntMap.emplace(visitArgs.m_fieldName, value);
    }

    return AZ::SettingsRegistryInterface::VisitResponse::Skip;
};
// ...

if (AZ::SettingsRegistryInterface* settingsRegistry = AZ::SettingsRegistry::Get();
    settingsRegistry != nullptr)
{
    AZ::SettingsRegistryVisitorUtils::VisitObject(*settingsRegistry, AppendObjectFields, "/O3DE/Object");
}
```

## Value setter API

[value setter API](https://github.com/o3de/o3de/blob/02846cf44347cbf4fae0faacc4a2ba74284908ff/Code/Framework/AzCore/AzCore/Settings/SettingsRegistry.h#L262-L309) 用于在设置注册表中为 `bool`, `int64_t`, `double`, 和 `AZStd::string_view` 类型设置值，或者为反映到`SerializeContext`的任何类型设置值。

设置注册表`SettingsRegistryInterface::Set`函数可用于存储支持的类型，例如`bool`, `AZ::s64/AZ::u64`, `double`, 和 `AZStd::string_view`。以下示例显示了如何向 Settings Registry 添加多个目录以及 plain 值：

```c++
// Executable folder - corresponds to the @exefolder@ alias
AZ::IO::FixedMaxPath path = AZ::Utils::GetExecutableDirectory();
registry.Set("/O3DE/Runtime/FilePaths/BinaryFolder", path.Native());

// Engine root folder - corresponds to the @engroot@ alias
AZStd::string engineRoot = AZ::Utils::GetEnginePath();
registry.Set("/O3DE/Runtime/FilePaths/EngineRootFolder", engineRoot);

// Retrieve the "project_path" and set the Runtime FilePaths to the ProjectPath
AZ::SettingsRegistryInterface::FixedValueString projectPath = AZ::Utils::GetProjectPath();
registry.Set("/O3DE/Runtime/FilePaths/ProjectPath", projectPath);

// Add a bool value
registry.Set("/O3DE/BoolValue", true);
// Add a double value
registry.Set("/O3DE/DoubleValue1", 1.0);
registry.Set("/O3DE/DoubleValue2", 2.0f); // float promoted to double
// Add a signed integer value
registry.Set("/O3DE/SignedInt", 1LL);
// Add an unsigned integer value
registry.Set("/O3DE/UnsignedInt", 2ULL);
```

### 使用 value setter API 存储 C++ 类对象

值设置器 API 允许通过`SettingsRegistryInterface:SetObject`将 C++ 对象转换为 JSON 数据。有一个先决条件，即 C++ 对象必须反映到`SerializeContext`，如以下示例所示：

```c++
// Make sure the AzPhysics::SceneConfiguration class is reflected
void SceneConfiguration::Reflect(AZ::ReflectContext* context)
{
    Physics::WorldConfiguration::Reflect(context);

    if (auto* serializeContext = azrtti_cast<AZ::SerializeContext*>(context))
    {
        serializeContext->Class<SceneConfiguration>()
            ->Version(1)
            ->Field("LegacyConfig", &SceneConfiguration::m_legacyConfiguration)
            ->Field("LegacyId", &SceneConfiguration::m_legacyId)
            ->Field("Name", &SceneConfiguration::m_sceneName)
            ;
    }
}

// ... Later, the SceneConfiguration class can be stored in the Settings Registry
AzPhysics::SceneConfiguration sceneConfig;
sceneConfig.m_sceneName = "PhysX Scene 1";
bool configurationStored = false;

AZ::SettingsRegistryInterface* settingsRegistry = AZ::SettingsRegistry::Get();
if (settingsRegistry)
{
    configurationStored = settingsRegistry->SetObject("/Amazon/Gems/PhysX/DefaultSceneConfiguration", sceneConfig);
}

```

## Value notifier API

[value notifier API](https://github.com/o3de/o3de/blob/02846cf44347cbf4fae0faacc4a2ba74284908ff/Code/Framework/AzCore/AzCore/Settings/SettingsRegistry.h#L196-L222) 当 SettingsRegistry 中 JSON 指针处的键被修改时发出通知。`SettingsRegistryInterface::RegisterNotifier`函数允许用户向设置注册表注册回调，该回调在设置 JSON 键的值时调用。

以下示例演示了每次在 Settings Registry 中更新 Asset Cache 目录时的日志记录：

```c++
SettingsRegistry& registry = *AZ::SettingsRegistry::Get();
AZ::SettingsRegistryInterface::NotifyCallback assetCacheChangedCB =
    [&registry](const AZ::SettingsRegistryInterface::NotifyArgs& notifyArgs)
{
    if (notifyArgs.m_jsonKeyPath == "/O3DE/Runtime/FilePaths/CacheRootFolder"
        && notifyArgs.m_type == SettingsRegistryInterface::Type::String)
    {
        AZ::IO::FixedMaxPath cacheRootPath;
        if (registry.Get(cacheRootPath.Native(), notifyArgs.m_jsonKeyPath))
        {
            AZ_TracePrintf("Settings Registry Tracking", "The Asset Cache path has changed to %s", cacheRootPath.c_str());
        }
    }
};

{
    // The SettingsRegistryInterface::RegisterNotifier function returns an AZ event handler that needs to be stored for as long
    // as desired to receive the notification events
    AZ::SettingsRegistryInterface::NotifyEventHandler assetChangedHandler = registry.RegisterNotifier(AZStd::move(assetCacheChangedCB));
    // ...
    registry.Set("/O3DE/Runtime/FilePaths/CacheRootFolder", "/home/testuser/TestCache");
}
 // When the assetChangedHandler scope ends, it will unregister from the Settings Registry Notifier event
 // If the handler is desired to persist, it can be stored in a member variable to extend its lifetime

```

## Merge API

设置注册表中最复杂的部分是 [merge API](https://github.com/o3de/o3de/blob/02846cf44347cbf4fae0faacc4a2ba74284908ff/Code/Framework/AzCore/AzCore/Settings/SettingsRegistry.h#L311-L375)。有三种主要类型的输入可以提供给 Settings Registry 进行合并。

* 使用语法 \<JSONPointerPath>=\<value> 输入 - 这被视为正在合并命令行参数。
* 内存中的 JSON 文档
* 包含`.setreg`/`.setregpatch`文件的文件/目录的路径

将设置合并到注册表的顺序很重要。稍后合并的键优先于之前合并的键。假设有 `streamer.setreg` ，其内容位于键“/O3DE/AzFramework/MyArray”，如以下示例所示：

```json
{
    "O3DE":
    {
        "AzFramework":
        {
            "MyArray":
            [
                "String1",
                "String3",
                "String5",
            ],
            "MyObject":
            {
                "AssetKey": "{80CCDA30-1F70-4982-ADE9-62D3DEE332D4}"
            }
        }
    }
}

```

假设有一个具有相同“/O3DE/AzFramework/MyArray”键的`streamer.editor.setreg`，例如以下示例：

```json
{
    "O3DE":
    {
        "AzFramework":
        {
            "MyArray":
            [
                true,
                false,
                75,
                "StringX",
                "InnerObject":
                {
                    "MyKey": "PathKey"
                }
            ]
        }
    }
}
```

如果合并了前面的`streamer.editor.setreg` ，则 settings 注册表包含以下键和值：

```ini
"/O3DE/AzFramework/MyArray/0" = true
"/O3DE/AzFramework/MyArray/1" = false
"/O3DE/AzFramework/MyArray/2" = 75
"/O3DE/AzFramework/MyArray/3" = "StringX"
"/O3DE/AzFramework/MyArray/4/InnerObject/MyKey" = "PathKey"
"/O3DE/AzFramework/MyObject/AssetKey" = "{80CCDA30-1F70-4982-ADE9-62D3DEE332D4}"
```

与 `streamer.setreg` 文件合并的“/O3DE/AzFramework/MyArray”键下的 JSON 数组关联的值已更新为 `streamer.editor.setreg` 文件的值。

### 合并目录中的 Settings Registry 文件

合并特定的`.setreg` 或 `.setregpatch`文件或合并 JSON 文档只需要指定文件名或 JSON 内容。合并包含 `.setreg` 或 `.setregpatch` 文件的目录不仅需要目录名称，还需要为合并 API 指定一个称为 *specializations* 的标签列表。

以下示例包含`automatedtesting`, `automatedtesting_gamelauncher`, `game` 的专用化以及作为其名称（debug、profile、release）一部分的当前构建配置标记：

```c++
SettingsRegistryInterface::Specializations specializations{"automatedtesting", "automatedtesting_gamelauncher", "game", AZ_BUILD_CONFIGURATION_TYPE };

AZ::IO::FixedMaxPath mergePath = Internal::GetExecutableDirectory();
if (!mergePath.empty())
{
    registry.MergeSettingsFolder((mergePath / SettingsRegistryInterface::RegistryFolder).Native(),
        specializations, AZ_TRAIT_OS_PLATFORM_CODENAME);
}

// Look within the Cache Root directory for target build dependency .setreg files
AZ::SettingsRegistryInterface::FixedValueString cacheRootPath;
if (registry.Get(cacheRootPath, FilePathKey_CacheRootFolder))
{
    mergePath = AZStd::move(cacheRootPath);
    mergePath /= SettingsRegistryInterface::RegistryFolder;
    registry.MergeSettingsFolder(mergePath.Native(), specializations, platform);
}
```

以下有关辅助 API 的章节介绍了专业化的详细信息。

## Auxiliary APIs

设置注册表合并实用程序是位于`AzCore/Settings/SettingsRegistryMergeUtils.h`中的一组函数，其中包含通用文件、目录位置和命令行合并的实现。它还包含用于在“/Amazon/AzCore/Settings/Specializations”键下的设置注册表中查询和设置专业化的函数。专用化可用于筛选在调用`SettingsInterface::MergeSettingsFolder`时要合并的 `*.setreg` 文件。

Settings Registry Merge Utilities 还包含一个函数，用于将特定 JSON 指针路径中的 JSON 值转储到`AZ::IO::GenericStream`中。

以下列表提供了指向`AzCore/Settings/SettingsRegistryMergeUtils.h`中各种辅助 API 的链接：

* [Merge to Settings Registry API](https://github.com/o3de/o3de/blob/02846cf44347cbf4fae0faacc4a2ba74284908ff/Code/Framework/AzCore/AzCore/Settings/SettingsRegistryMergeUtils.h#L133-L263)
* [Query Specialization Tag API](https://github.com/o3de/o3de/blob/02846cf44347cbf4fae0faacc4a2ba74284908ff/Code/Framework/AzCore/AzCore/Settings/SettingsRegistryMergeUtils.h#L125-L131)
* [Settings Registry Section Dumping API](https://github.com/o3de/o3de/blob/02846cf44347cbf4fae0faacc4a2ba74284908ff/Code/Framework/AzCore/AzCore/Settings/SettingsRegistryMergeUtils.h#L276-L304)

## O3DE Manifest utils API

Settings Registry Merge Utilities 还提供 [O3DE Manifest Utils API](https://github.com/o3de/o3de/blob/02846cf44347cbf4fae0faacc4a2ba74284908ff/Code/Framework/AzCore/AzCore/Settings/SettingsRegistryMergeUtils.h#L349-L404)，这是一个帮助程序函数的集合，用于访问活动 Gem 的集合，以及递归访问 O3DE 清单“external_subdirectories”以确定所有已注册 Gem 的集合。
