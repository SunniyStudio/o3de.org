---
title: 设置注册表脚本 API
linkTitle: 脚本 API
description: 了解设置注册表脚本 API 如何在 Open 3D Engine （O3DE） 中向 Lua、Python 和 Script Canvas 公开设置注册表。
weight: 300
---

设置注册表通过反射到行为上下文绑定到 Open 3D Engine （O3DE） 脚本语言。执行对行为上下文的反射的代码位于 [SettingsRegistryScriptUtils](https://github.com/o3de/o3de/blob/development/Code/Framework/AzCore/AzCore/Settings/SettingsRegistryScriptUtils.cpp#L377-L390).

## API 详情

下表详细介绍了向脚本语言公开的 Settings Registry API 类、属性和方法。

| 名称 | <div style="width:300px">Description</div> | 类型 | 参数 | 返回值 |
| :-- | :-- | :-- | :-- | :-- |
| `SettingsRegistryInterface` | 提供对设置注册表`<key, value>` 设置、查询和删除功能的访问权限的抽象类。 | Class | NA | NA |
| `g_SettingsRegistry` | 提供对 Global Settings Registry 的访问的 Global Property。 | Property | NA | NA |
| `SettingsRegistry` |创建新的 Settings Registry 对象的函数。| Method | None | SettingsRegistryInterface |
| `MergeSetting` | 将提供的 JSON 文档合并到 Settings Registry 中。默认使用[JSON Merge Patch](https://tools.ietf.org/html/rfc7386#section-1) 格式。 | Method | <ul><li>String `json_data` - 要合并到 Settings Registry 的 JSON 字符串文档。</li><li>Enum `format` - 正在合并的 JSON 文件的格式，默认情况下，这是 JsonMergePatch。指定`settingsregistry.JsonPatch` 或 `settingsregistry.JsonMergePatch`(默认值).</li></ul> | Boolean |
| `MergeSettingFile` | 将提供的`.setreg` 文件合并到指定 JSON 指针处的设置注册表中。 默认使用[JSON Merge Patch](https://tools.ietf.org/html/rfc7386#section-1) 格式。 | Method | <ul><li>String `file_path` - 要合并到设置注册表中的 `.setreg` 文件的路径。</li><li>String `json_root_key` - JSON 指针：合并的 JSON 内容应位于其中的根位置。</li><li>Enum `format` - 正在合并的 JSON 文件的格式，默认情况下，这是 JsonMergePatch。指定`settingsregistry.JsonPatch` 或 `settingsregistry.JsonMergePatch`(默认值).</li></ul> | Boolean |
| `MergeSettingFolder` |枚举指定目录中的`.setreg`文件，并将它们合并到 Settings Registry 中。使用[JSON Merge Patch](https://tools.ietf.org/html/rfc7386#section-1)格式。<br><br>特化是针对 `.setreg` 文件名中每对点 （*.*） 之间的文本进行检查的标记。例如，`cmake_dependencies.automatedtesting.automatedtesting_gamelauncher.setreg`文件名包含两个tags *automatedtesting* 和 *automatedtesting_gamelauncher*. 文件名的每个标记必须与专用化列表中指定的标记匹配。<br><br>检查下面的 [MergeSettingFolder specialization table](#mergesettingfolder-specialization-table) 了解更多信息。<br><br>如果提供的文件夹已合并，则返回`True` 。每个特定文件是否合并成功不是返回结果的一部分。 | Method | <ul><li>String `folder_path` - Directory containing `.setreg` files to merge into Settings Registry.</li><li>List `specializations` - 用于筛选`.setreg`文件的标签列表。</li><li>String `platform` - 搜索`.setreg`文件时要遍历的 OS 平台文件夹名称。</li></ul> | Boolean |
| `DumpSettings` | 转储在 JSON 指针处指定的 JSON 值（如果在设置注册表中找到）。转储值的结果存储在 `outputString` 参数中。<br><br>要转储整个设置注册表，必须提供带有空字符串 （“”） 的`json_pointer`。<br><br>如果 JSON 指针处的 JSON 值已成功转储，则返回 `True`。 | Method | <ul><li>String `json_pointer` - JSON 指针 应转储其 JSON 值的指针。</li><li>String `outputString` - 输出变量，如果成功，则使用转储的 JSON 值内容填充。</li></ul> | Boolean |
| `GetBool` | 在设置注册表中查询提供的 JSON 指针处的布尔值。如果找到布尔值，则其值存储在`boolValue`参数中。<br><br>`GetBool`的返回值表示在特定 JSON 指针处找到布尔键。如果返回值为`True`，则布尔值的实际值存储在`boolValue`中。 | Method | <ul><li>String `json_pointer` - 用于查找布尔值的 JSON 指针。</li><li>Boolean `boolValue` - Output 变量，如果成功，则使用布尔值结果进行设置。</li></ul> | Boolean |
| `SetBool` | 将 Settings Registry 中的 JSON 指针设置为指定的布尔值。| Method | <ul><li>String `json_pointer` -JSON 指针指向将设置布尔值的 JSON 键。</li><li>Boolean `boolValue` - 要在 JSON 指针处设置的布尔值。</li></ul> | Boolean |
| `GetInt` | 在设置注册表中查询提供的 JSON 指针处的整数值。如果找到整数，则其值存储在 `intValue` 参数中。 | Method | <ul><li>String `json_pointer` - 用于查找整数值的 JSON 指针。</li><li>Integer `intValue` - Output 变量，如果成功，则使用整数结果设置。</li></ul> | Boolean |
| `SetInt` | 将 Settings Registry 中的 JSON 指针设置为指定的整数值。 | Method | <ul><li>String `json_pointer` - JSON 指针 指向将设置整数值的 JSON 键。</li><li>Integer `intValue` - 要在 JSON 指针处设置的整数值。</li></ul> | Boolean |
| `GetFloat` | 在设置注册表中查询提供的 JSON 指针处的浮点值。如果找到浮点值，则其值存储在`floatValue`参数中。| Method | <ul><li>String `json_pointer` - 用于查找浮点值的 JSON 指针。</li><li>Float `floatValue` - Output 变量，如果成功，则使用浮点结果设置。</li></ul> | Boolean |
| `SetFloat` | 将 Settings Registry 中的 JSON 指针设置为指定的浮点值。| Method | <ul><li>String `json_pointer` - 指向将设置浮点值的 JSON 键的 JSON 指针。</li><li>Float `floatValue` - 要在 JSON 指针处设置的浮点值。</li></ul> | Boolean |
| `GetString` |在设置注册表中查询提供的 JSON 指针处的字符串值。如果找到字符串，则其值存储在`stringValue`参数中。 | Method | <ul><li>String `json_pointer` - 用于查找字符串值的 JSON 指针。</li><li>String `stringValue` - 输出变量，如果成功，则使用字符串值 result 进行设置。</li></ul> | Boolean |
| `SetString` | 将 Settings Registry 中的 JSON 指针设置为指定的字符串值。 | Method | <ul><li>String `json_pointer` - JSON 指针 指向将在其中设置字符串值的 JSON 键。</li><li>String `stringValue` - 要在 JSON 指针处设置的 String 值。</li></ul> | Boolean |
| `RemoveKey` | 删除提供的 JSON 指针处的 JSON 键和值（如果存在）。 | Method | <ul><li>String `json_pointer` - 指向要删除的 JSON 密钥的 JSON 指针。</li></ul> | Boolean |

### `MergeSettingFolder` 专业化表

下表说明了何时通过`MergeSettingFolder`方法合并 `.setreg` 文件。

| 文件名 | Specializations | 将被合并 |
| :-- | :-- | :-- |
| `cmake_dependencies.automatedtesting.automatedtesting_gamelauncher.setreg` | automatedtesting<br>automatedtesting_game_launcher | Yes |
| `cmake_dependencies.automatedtesting.automatedtesting_gamelauncher.setreg` | automatedtesting | No |
| `cmake_dependencies.automatedtesting.automatedtesting_gamelauncher.setreg` | automatedtesting_game_launcher | No |
| `cmake_dependencies.automatedtesting.automatedtesting_gamelauncher.setreg` | <None> | No |
| `cmake_dependencies.setreg` | <Any> | Always |

## 示例

可以通过 O3DE 支持的脚本语言 Lua 和 Python 访问设置注册表。

### Python 示例

要在 Python 中访问 Settings Registry，请确保在项目中启用 **Editor Python Bindings** Gem。以下示例代码演示了如何使用设置注册表 API。

```python
import azlmbr.settingsregistry as SettingsRegistry
import os

ExampleTestFileSetreg = 'AutomatedTesting/Editor/Scripts/SettingsRegistry/example.file.setreg'
ExampleTestFolderSetreg = 'AutomatedTesting/Editor/Scripts/SettingsRegistry'

def test_settings_registry():
    # Access the Global Settings Registry and dump it to a string
    if SettingsRegistry.g_SettingsRegistry.IsValid():
        dumpedSettings = SettingsRegistry.g_SettingsRegistry.DumpSettings("")
        if dumpedSettings:
            print("Full Settings Registry dumped successfully\n{}", dumpedSettings.value())

    # Making a script local Settings Registry
    localSettingsRegistry = SettingsRegistry.SettingsRegistry()
    localSettingsRegistry.MergeSettings('''
    {
        "TestObject": {
            "boolValue": false,
            "intValue": 17,
            "floatValue": 32.0,
            "stringValue": "Hello World"
        }
    }''')

    registryVal = localSettingsRegistry.GetBool('/TestObject/boolValue')
    if registryVal:
        print(f"Bool value '{registryVal.value()}' found")
        registryVal = localSettingsRegistry.GetInt('/TestObject/intValue')

    if registryVal:
        print(f"Int value '{registryVal.value()}' found")
        registryVal = localSettingsRegistry.GetFloat('/TestObject/floatValue')

    if registryVal:
        print(f"Float value '{registryVal.value()}' found")

    registryVal = localSettingsRegistry.GetString('/TestObject/stringValue')
    if registryVal:
        print(f"String value '{registryVal.value()}' found")

    if localSettingsRegistry.SetBool('/TestObject/boolValue', True):
        registryVal = localSettingsRegistry.GetBool('/TestObject/boolValue')
        print(f"Bool value '{registryVal.value()}' set")

    if localSettingsRegistry.SetInt('/TestObject/intValue', 22):
        registryVal = localSettingsRegistry.GetInt('/TestObject/intValue')
        print(f"Int value '{registryVal.value()}' set")

    if localSettingsRegistry.SetFloat('/TestObject/floatValue', 16.0):
        registryVal = localSettingsRegistry.GetFloat('/TestObject/floatValue')
        print(f"Float value '{registryVal.value()}' set")

    if localSettingsRegistry.SetString('/TestObject/stringValue', 'Goodbye World'):
        registryVal = localSettingsRegistry.GetString('/TestObject/stringValue')
        print(f"String value '{registryVal.value()}' found")

    if localSettingsRegistry.RemoveKey('/TestObject/stringValue'):
        print("Key '/TestObject/stringValue' has been successfully removed")

    # Merge a Settings File using the JsonPatch format
    jsonPatchMerged = localSettingsRegistry.MergeSettings('''
        [
            { "op": "add", "path": "/TestObject", "value": {} },
            { "op": "add", "path": "/TestObject/boolValue", "value": false },
            { "op": "add", "path": "/TestObject/intValue", "value": 17 },
            { "op": "add", "path": "/TestObject/floatValue", "value": 32.0 },
            { "op": "add", "path": "/TestObject/stringValue", "value": "Hello World" },
            { "op": "add", "path": "/TestArray", "value": [] },
            { "op": "add", "path": "/TestArray/0", "value": { "intIndex": 3 } },
            { "op": "add", "path": "/TestArray/1", "value": { "intIndex": -55 } }
        ]''', SettingsRegistry.JsonPatch)

    if jsonPatchMerged:
        print("JSON in JSON Patch format has been merged successfully to the local Settings Registry")

    # Example usage of `MergeSettingsFile` and `MergeSettingsFolder`
    if localSettingsRegistry.MergeSettingsFile(ExampleTestFileSetreg):
        print(f"Successfully merged setreg file '{ExampleTestFileSetreg}' to local Settings Registry")
        registryVal = localSettingsRegistry.GetString('/AutomatedTesting/ScriptingTestArray/3')
        if registryVal:
            print(f"Settings Registry contains '/AutomatedTesting/ScriptingTestArray/3'='{registryVal.value()}' merged from the {ExampleTestFileSetreg}")

    # Add the 'folder' to the Settings Registry so that only non-specialized .setreg
    # and .setreg files that only contain a 'folder' tag are merged into the Setting Registry
    filetags = SettingsRegistry.Specializations()
    filetags.Append('folder')
    if localSettingsRegistry.MergeSettingsFolder(ExampleTestFolderSetreg, filetags):
        print(f"Successfully merged setreg folder '{ExampleTestFolderSetreg}' to local Settings Registry")
        registryVal = localSettingsRegistry.GetBool('/AutomatedTesting/Settings/IsFolder')
        if registryVal:
            print(f"Settings Registry contains '/AutomatedTesting/Settings/IsFolder'='{registryVal.value()}' merged from the {ExampleTestFolderSetreg} folder")

# Invoke main function
if __name__ == '__main__':
    test_settings_registry()
```

### Lua 示例

Settings Registry 在 Lua 中可通过 Behavior Context 绑定获得。以下示例使用 Settings Registry 通过 Lua 访问帧捕获设置。

```lua
local OutputProfileData =
{
    Properties =
    {
        FrameDelayCount = 100,
        FrameCaptureCount = 100,
        ProfileName = "LevelFrameTiming",
        QuitOnComplete = true,
    },
    frameCount = 0,
    frameDelayCount = 0,
    frameCaptureCount = 0,
    captureCount = 0,
    quitOnComplete = false,
    profileName = "UninitializedName",
    outputFolder = "UninitializedPath",
    cpuTimingsOutputPath = "UninitializedPath",
    captureInProgress = false,
    active = false,
};

local FrameTimeRecordingActiveRegistryKey <const> = "/O3DE/Performance/FrameTimeRecording/Activate"
local FrameDelayCountRegistryKey <const> = "/O3DE/Performance/FrameTimeRecording/DelayCount"
local FrameCaptureCountRegistryKey <const> = "/O3DE/Performance/FrameTimeRecording/CaptureCount"
local ProfileNameRegistryKey <const> = "/O3DE/Performance/FrameTimeRecording/ProfileName"
local QuitOnCompleteRegistryKey <const> = "/O3DE/Performance/FrameTimeRecording/QuitOnComplete"
local SourceProjectUserPathRegistryKey <const> = "/O3DE/Runtime/FilePaths/SourceProjectUserPath"
local ConsoleCommandQuitRegistryKey <const> = "/Amazon/AzCore/Runtime/ConsoleCommands/quit"


function OutputProfileData:TryQuitOnComplete()
    if (self.quitOnComplete) then
        g_SettingsRegistry:SetString(ConsoleCommandQuitRegistryKey, "")
    end
end



function OutputProfileData:OnActivate()
    if (g_SettingsRegistry:IsValid()) then
        local quitOnCompleteValue = g_SettingsRegistry:GetBool(QuitOnCompleteRegistryKey)
        self.quitOnComplete = quitOnCompleteValue:value_or(self.Properties.QuitOnComplete)

        local frameTimeRecordingActivateValue = g_SettingsRegistry:GetBool(FrameTimeRecordingActiveRegistryKey)
        if (not frameTimeRecordingActivateValue:has_value() or not frameTimeRecordingActivateValue:value()) then
            Debug:Log("OutputProfileData:OnActivate - Missing registry setting to activate frame time recording, aborting data collection")
            self:TryQuitOnComplete()
            return
        end

        -- get path to user folder
        local pathToUserFolder = "InvalidPath/"
        local settingsRegistryResult =  g_SettingsRegistry:GetString(SourceProjectUserPathRegistryKey)
        if (settingsRegistryResult:has_value()) then
            pathToUserFolder = settingsRegistryResult:value()
        else
            Debug:Log("OutputProfileData:OnActivate - Unable to resolve the SourceProjectUserPath, aborting data collection")
            self:TryQuitOnComplete()
            return
        end

        -- get any registry property overrides
        local frameDelayCountValue = g_SettingsRegistry:GetUInt(FrameDelayCountRegistryKey)
        self.frameDelayCount = frameDelayCountValue:value_or(self.Properties.FrameDelayCount)
        local frameCaptureCountValue = g_SettingsRegistry:GetUInt(FrameCaptureCountRegistryKey)
        self.frameCaptureCount = frameCaptureCountValue:value_or(self.Properties.FrameCaptureCount)
        local profileNameValue = g_SettingsRegistry:GetString(ProfileNameRegistryKey)
        self.profileName = profileNameValue:value_or(self.Properties.ProfileName)
    end
end


function OutputProfileData:OnDeactivate()
end

return OutputProfileData
```

## 限制

* 用于向设置注册表注册通知 AZ 事件的 API 未在脚本中公开（这是`SettingsRegistryInterface::RegisterNotifier`API）。
* 不支持设置/查询复杂的 AZ 反射类型（这是`SettingsRegistryInterface::SetObject` 和 `SettingsRegistryInterface::GetObject` API）。

在脚本中访问 AZ 反射类型的一种解决方法是使用`DumpSettings` 和 `MergeSettings`函数。
