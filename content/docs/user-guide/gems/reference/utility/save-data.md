---
linkTitle: Save Data
title: Save Data Gem
description: Save Data Gem 提供用于将运行时数据保存在 Open 3D Engine （O3DE） 项目中的 API。
toc: true
---

**Save Data** Gem将所有功能封装在 **Open 3D Engine （O3DE）** 中，用于将游戏和个人用户数据保存在一组与平台无关的 API 操作下。

Save Data Gem 使用 O3DE 的通用通信系统 [事件总线 （EBus）](/docs/user-guide/programming/messaging/ebus/)来调度通知和接收请求。要发出与保存或加载持久性用户数据相关的请求，请使用 Save Data Gem 的`SaveDataRequests`总线。要侦听与保存持久性用户数据相关的通知，请使用`SaveDataNotifications`总线。

## 请求保存数据

使用 `SaveDataRequestBus` 发出保存或加载数据的请求时，请记住以下几点：

* Save Data Gem 仅负责保存和加载通用数据缓冲区。您的游戏必须使用数据格式 （如 JSON 或 XML） 序列化或反序列化数据。但是，Gem 提供了方便的函数，用于保存或加载已使用 [序列化上下文](/docs/user-guide/programming/components/reflection/serialization-context/).

* 每个保存数据缓冲区必须由字符串唯一标识。在大多数操作系统和设备上，此字符串是数据缓冲区写入的文件的名称。

### 保存本地用户 ID 的数据

保存 处理本地用户配置文件的数据通信取决于在本地设备上唯一标识用户的本地用户 ID。使用本地 User ID 时，请记住以下几点：

* 可以选择将保存数据缓冲区与本地用户 ID 相关联。

* 可以为每个本地用户 ID 单独存储具有相同名称的数据缓冲区。

* 调用保存数据函数时，必须指定本地用户 ID 或传入值`AzFramework::LocalUserIdNone`。此要求可帮助您考虑是否应将要保存的数据与用户关联。

* 与本地用户 ID 关联的数据将保存到本地用户 ID 和应用程序唯一的容器或目录中。

* 未与本地用户 ID 关联的数据将保存到应用程序唯一的 **全局** 容器或目录中。

有关 `SaveDataRequests`总线的更多信息，请参阅`\Gems\SaveData\Code\Include\SaveData\SaveDataRequestBus.h`.

## 获取保存数据通知

Save Data 执行的所有保存和加载操作都是异步的。因此，您必须订阅以接收 Save Data 通知，或者提供一个回调函数，以便在 Save 或 Load 操作完成时通知您。此操作始终在主线程中执行。

有关 `SaveDataNotifications` 总线的更多信息，请参阅`\Gems\SaveData\Code\Include\SaveData\SaveDataNotificationBus.h`.

## Save Data 代码示例

以下示例代码使用 Save Data Gem 将缓冲区和对象保存到持久性存储或从持久存储加载缓冲区和对象。

```c++
const AZ::u64 testSaveDataSize = 9;
const char* testSaveDataName = "TestSaveData";
char testSaveData[testSaveDataSize] = {'a', 'b', 'c', '1', '2', '3', 'x', 'y', 'z'};

void SaveBufferToPersistentStorage()
{
    SaveData::SaveDataRequests::SaveDataBufferParams params;
    params.dataBuffer.reset(testSaveData);
    params.dataBufferSize = testSaveDataSize;
    params.dataBufferName = testSaveDataName;
    params.callback = [](const SaveData::SaveDataNotifications::DataBufferSavedParams& onSavedParams)
    {
        if (onSavedParams.result != SaveData::SaveDataNotifications::Result::Success)
        {
            // Error handling
        }
    };
    SaveData::SaveDataRequestBus::Broadcast(&SaveData::SaveDataRequests::SaveDataBuffer, params);
}

void LoadBufferFromPersistentStorage()
{
    SaveData::SaveDataRequests::LoadDataBufferParams params;
    params.dataBufferName = testSaveDataName;
    params.callback = [](const SaveData::SaveDataNotifications::DataBufferLoadedParams& onLoadedParams)
    {
        if (onLoadedParams.result == SaveData::SaveDataNotifications::Result::Success)
        {
            // SaveDataNotifications::DataBuffer is a shared_ptr, so you can choose to either preserve the
            // buffer (by keeping a reference to it), or just let it go out of scope so it will be deleted.
                        SaveDataNotifications::DataBuffer loadedDataBuffer = onLoadedParams.dataBuffer;
            // Use the loaded data buffer...
        }
        else
        {
            // Error handling
        }
    };
    SaveData::SaveDataRequestBus::Broadcast(&SaveData::SaveDataRequests::LoadDataBuffer, params);
}

class TestObject
{
public:
    AZ_RTTI(TestObject, "{9CE29971-8FE2-41FF-AD5B-CB15F1B92834}");
    static void Reflect(AZ::SerializeContext& sc)
    {
        sc.Class<TestObject>()
            ->Version(1)
            ->Field("testString", &TestObject::testString)
            ->Field("testFloat", &TestObject::testFloat)
            ->Field("testInt", &TestObject::testInt)
            ->Field("testBool", &TestObject::testBool)
        ;
    }
    AZStd::string testString;
    float testFloat = 0.0f;
    int testInt = 0;
    bool testBool = false;
};

void SaveObjectToPersistentStorage()
{
    // Reflect the test object class (if not already done).
    AZ::SerializeContext serializeContext;
    TestObject::Reflect(serializeContext);

    // Create a test object instance to save.
    AZStd::shared_ptr<TestObject> testObject = AZStd::make_shared<TestObject>();

    // Setup the save data params
    SaveData::SaveDataRequests::SaveOrLoadObjectParams<TestObject> params;
    params.serializableObject = testObject;
    params.serializeContext = &serializeContext; // Omit to use the global AZ::SerializeContext instance
    params.dataBufferName = "TestSaveObject";
    params.callback = [](const SaveData::SaveDataRequests::SaveOrLoadObjectParams<TestObject>& callbackParams,
                         SaveData::SaveDataNotifications::Result callbackResult)
    {
        if (callbackResult != SaveData::SaveDataNotifications::Result::Success)
        {
            // Error handling
        }
    };
    SaveData::SaveDataRequests::SaveObject(params);
}

void LoadObjectFromPersistentStorage(const AzFramework::LocalUserId& localUserId = AzFramework::LocalUserIdNone)
{
    // Reflect the test object class (if not already done).
    AZ::SerializeContext serializeContext;
    TestObject::Reflect(serializeContext);

    // Create a test object to load.
    AZStd::shared_ptr<TestObject> testObject = AZStd::make_shared<TestObject>();

    // Setup the load data params
    SaveData::SaveDataRequests::SaveOrLoadObjectParams<TestObject> params;
    params.serializableObject = testObject;
    params.serializeContext = &serializeContext; // Omit to use the global AZ::SerializeContext instance
    params.dataBufferName = "TestSaveObject";
    params.callback = [](const SaveData::SaveDataRequests::SaveOrLoadObjectParams<TestObject>& callbackParams,
                         SaveData::SaveDataNotifications::Result callbackResult)
    {
        if (onLoadedParams.result == SaveData::SaveDataNotifications::Result::Success)
        {
            // Use the loaded data buffer...
            callbackParams.serializableObject;
        }
        else
        {
            // Error handling
        }
    };
    SaveData::SaveDataRequests::LoadObject(params);
}
```
