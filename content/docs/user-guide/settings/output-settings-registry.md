---
title: 使用 C++ 将 Settings Registry 输出到 Stream
linkTitle: 输出 Settings Registry
description: 了解如何转储内存中的设置注册表，以便在 Open 3D Engine （O3DE） 中使用 C++ 进行流式传输。
weight: 600
---

您可能需要将设置注册表或设置注册表的一部分存储到磁盘以供以后访问。有时，您可能需要将 Settings Registry 存储在正在运行的应用程序中的字符串中，以便进一步处理。本主题提供了一些示例，说明如何在维护上级 JSON 密钥层次结构或排除任何上级密钥层次结构的同时，将设置注册表的某些部分转储到特定密钥的磁盘上。

## 使用 `DumpSettingsRegistryToStream` 函数

在此示例中， `SettingsRegistryMergeUtils::DumpSettingsRegistryToStream` 函数用于将设置注册表的一部分存储到实现`AZ::IO::GenericStream` 接口的类中。

```c++
//! Structure for configuring how values should be dumped from the Settings Registry
struct DumperSettings
{
    //! Determines if a PrettyWriter should be used when dumping the Settings Registry
    bool m_prettifyOutput{};
    //! Include filter that is used to indicate which paths of the Settings Registry
    //! should be traversed.
    //! If the include filter is empty, then all paths underneath the JSON pointer path are included
    //! Otherwise, the include filter is invoked, and if it returns true, traversal continues down the path
    AZStd::function<bool(AZStd::string_view path)> m_includeFilter;
};

//! Dumps the supplied Settings Registry at the path specified by key if it exist to the AZ::IO::GenericStream
//! @param key is a JSON pointer path to dumping settings recursively from
//! @param stream is an AZ::IO::GenericStream that supports writing
//! @param dumperSettings are used to determine how to format the dumped output
bool DumpSettingsRegistryToStream(SettingsRegistryInterface& registry, AZStd::string_view key,
    AZ::IO::GenericStream& stream, const DumperSettings& dumperSettings);
```

{{< important >}}
`DumpSettingsRegistryToStream`的结果相对于`key`参数进行转储。
{{< /important >}}

为了更好地解释，请考虑以下示例内存中 Settings Registry 实例：

**Settings Registry View**

```json
{
    "Amazon":
    {
        "Editor":
        {
            "Preferences":
            {
                "EnablePrefabSystemUi": false,
                "QtSearchPaths":
                [
                    "Qt/bin",
                    "/usr/local/Qt/bin",
                    "C:/Program Files/Qt"
                ]
            }
        },
        "Engine":
        {
            "OtherSettings": 16
        }
    }
}
```

使用关键参数`"/Amazon/Editor/Preferences"`调用`SettingsRegistryMergeUtils::DumpSettingsRegistryToStream`函数，如下所示：

```c++
AZ::SettingsRegistryMergeUtils::DumperSettings dumperSettings;
dumperSettings.m_prettifyOutput = true;

auto registry = AZ::SettingsRegistry::Get();
AZStd::string stringBuffer;
AZ::IO::ByteContainerStream stringStream(&stringBuffer);
if (!AZ::SettingsRegistryMergeUtils::DumpSettingsRegistryToStream(*registry, "/Amazon/Editor/Preferences", stringStream, dumperSettings))
{
    AZ_Warning("SEditorSettings", false, R"(Unable to dump the Editor Preferences settings to from the Settings Registry)");
    return;
}

```

结果:

```json
{
    "EnablePrefabSystemUi": false,
    "QtSearchPaths":
    [
        "Qt/bin",
        "/usr/local/Qt/bin",
        "C:/Program Files/Qt"
    ]
}
```

这表明 Amazon、Editor、Preferences的祖先对象没有输出到转储的文本数据。如果需要将祖先 JSON 对象写出到编辑器首选项设置，请在 `DumperSettings` 中设置 `m_jsonPointerPrefix`变量。

## 使用键和锚点存储 Editor Preferences

以下示例使用键参数“/Amazon/Editor/Preferences”调用`SettingsRegistryMergeUtils::DumpSettingsRegistryToStream`函数，并将`DumperSettings`中的 JSON 指针前缀参数设置为“/Amazon/Editor/Preferences”：

```c++
AZ::SettingsRegistryMergeUtils::DumperSettings dumperSettings;
dumperSettings.m_prettifyOutput = true;
dumperSettings.m_jsonPointerPrefix = "/Amazon/Editor/Preferences";

auto registry = AZ::SettingsRegistry::Get();
AZStd::string stringBuffer;
AZ::IO::ByteContainerStream stringStream(&stringBuffer);
if (!AZ::SettingsRegistryMergeUtils::DumpSettingsRegistryToStream(*registry, "/Amazon/Editor/Preferences", stringStream, dumperSettings))
{
    AZ_Warning("SEditorSettings", false, R"(Unable to dump the Editor Preferences settings to from the Settings Registry)");
    return;
}
```

前面的示例生成以下结果：

```json
{
    "Amazon":
    {
        "Editor":
        {
            "Preferences":
            {
                "EnablePrefabSystemUi": false,
                "QtSearchPaths":
                [
                    "Qt/bin",
                    "/usr/local/Qt/bin",
                    "C:/Program Files/Qt"
                ]
            }
        }
    }
}
```

## 使用根键和过滤器存储 Editor Preferences

使用 include 过滤器转储 Settings Registry 的整个根目录会写入锚定到指定键的设置的祖先对象。以下示例使用键参数 “”（根）调用`SettingsRegistryMergeUtils::DumpSettingsRegistryToStream`函数，并包含一个筛选条件，该筛选条件维护具有 `"/Amazon/Editor/Preferences"`前缀的 JSON 指针的任何 JSON 对象：

```c++
AZ::SettingsRegistryMergeUtils::DumperSettings dumperSettings;
dumperSettings.m_prettifyOutput = true;
//! Make sure that any JSON paths that are a prefix of the "/Amazon/Editor/Preferences" path are included in the dumped contents
dumperSettings.m_includeFilter = [](AZStd::string_view path)
{
    AZStd::string_view prefixPath("/Amazon/Editor/Preferences");
    return prefixPath.starts_with(path.substr(0, prefixPath.size()));
};

auto registry = AZ::SettingsRegistry::Get();
AZStd::string stringBuffer;
AZ::IO::ByteContainerStream stringStream(&stringBuffer);
if (!AZ::SettingsRegistryMergeUtils::DumpSettingsRegistryToStream(*registry, "", stringStream, dumperSettings))
{
    AZ_Warning("SEditorSettings", false, R"(Unable to dump the Editor Preferences settings to from the Settings Registry)");
    return;
}

```

前面的示例生成以下结果：

```json
{
    "Amazon":
    {
        "Editor":
        {
            "Preferences":
            {
                "EnablePrefabSystemUi": false,
                "QtSearchPaths":
                [
                    "Qt/bin",
                    "/usr/local/Qt/bin",
                    "C:/Program Files/Qt"
                ]
            }
        }
    }
}
```

通过使用包含过滤器，将保留前往 Editor Preferences 部分的 JSON 键的层次结构，并将其转储到输出字符串中。

## 将 Settings Registry 的一部分保存到文件中

以下示例显示了如何使用`SettingsRegistryMergeUtils::DumpSettingsRegistryToStream`函数将编辑器首选项保存到用户本地注册表位置(`<project-root>/User/Registry`)的文件中：

```c++
//! Recurses over the entire Settings Registry to the Editor Preferences settings
//! The dumper settings is supplied an include filter to only the keys on the way to the editor preferences key of "/Amazon/Editor/Preferences"
//! as well as all settings below the"/Amazon/Editor/Preferences"
AZ::SettingsRegistryMergeUtils::DumperSettings dumperSettings;
dumperSettings.m_prettifyOutput = true;
dumperSettings.m_includeFilter = [](AZStd::string_view path)
{
    AZStd::string_view prefixPath("/Amazon/Editor/Preferences");
    return prefixPath.starts_with(path.substr(0, prefixPath.size()));
};
AZStd::string stringBuffer;
if (auto registry = AZ::SettingsRegistry::Get(); registry != nullptr)
{
    AZ::IO::ByteContainerStream stringStream(&stringBuffer);
    if (!AZ::SettingsRegistryMergeUtils::DumpSettingsRegistryToStream(*registry, "", stringStream, dumperSettings))
    {
        AZ_Warning("SEditorSettings", false, R"(Unable to dump the Editor Preferences settings to from the Settings Registry)");
        return;
    }
}

//! Resolve path to editorpreferences.setreg
auto fileIo = AZ::IO::FileIOBase::GetInstance();
AZ::IO::FixedMaxPath editorPreferencesFilePath = "user/Registry/editorpreferences.setreg";
if (fileIo == nullptr || !fileIo->ResolvePath(editorPreferencesFilePath, "@projectroot@/user/Registry/editorpreferences.setreg"))
{
    AZ_Warning("SEditorSettings", false, R"(Unable to resolve path "%s" to the Editor Preferences registry file\n)",
        editorPreferencesFilePath.c_str());
    return;
}

//! Save settings to the file on the editorpreferences.setreg file
bool saved{};
constexpr auto configurationMode = AZ::IO::SystemFile::SF_OPEN_CREATE
    | AZ::IO::SystemFile::SF_OPEN_CREATE_PATH
    | AZ::IO::SystemFile::SF_OPEN_WRITE_ONLY;
if (AZ::IO::SystemFile outputFile; outputFile.Open(editorPreferencesFilePath.c_str(), configurationMode))
{
    saved = outputFile.Write(stringBuffer.data(), stringBuffer.size()) == stringBuffer.size();
}

AZ_Warning("SEditorSettings", saved, R"(Unable to save Editor Preferences registry file to path "%s"\n)",
    editorPreferencesFilePath.c_str());
```
