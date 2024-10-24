---
description: ' 使用 EventData 类型为 Open 3D Engine 中的动作事件创建自定义参数。'
title: 创建 EventData 类型
---

您可以创建派生自 [`EMotionFX::EventData`](/docs/api/gems/emotionfx/) 接口的对象，为单个事件附加任意数量的数据对象。

**要创建EventData类型**

1. 使用以下标准，子类化 `EventData` 类或 `EventDataSyncable` 类：
   + 对于通用参数，可对 `EventData` 类型进行子类化。
   + 对于同步轨道的参数，请使用 `EventDataSyncable` 类。这些参数用于同步混合动作。

   下面的代码片段子类化了 `EventDataSyncable` 类，为左脚步声创建了一个 `LeftFootEvent` 事件。

   ```
   class LeftFootEvent final
       : public EMotionFX::EventDataSyncable
   {
   public:
       AZ_RTTI(LeftFootEvent, "{117454DC-0675-483E-843E-841C57A4354D}", EventDataSyncable);

       LeftFootEvent() = default;
   ...
   ```

1. 在子类中实现 `Equal` 函数。`Equal` 函数用于测试两个 `EventData` 实例是否相等，并用于重复删除 `EventData` 实例。

   下面的示例会检查传入的 `EventData` 是否为`LeftFootEvent`。

   ```
   bool Equal(const EventData& rhs, const bool /*ignoreEmptyFields*/) const override
   {
       // All LeftFootEvents are equal.
       const LeftFootEvent* rhsEvent = azrtti_cast<const LeftFootEvent*>(&rhs);
       return rhsEvent != nullptr;
   }
   ```

有关 `Equal` 函数的更多信息，请参阅 [关于等号函数的更多信息](#more-about-the-equal-function).

1. 实现 `Reflect` 方法，将类型反射到[序列化上下文](/docs/user-guide/programming/components/reflection/serialization-context/)和 [编辑上下文](/docs/user-guide/programming/components/reflection/edit-context/)中。

将事件反映到编辑上下文时，请在 `ClassElement` 中添加 `Creatable` 属性。这将使 `EventData` 类型在**动画编辑器**的**Motion Events**选项卡中可见，以便用户选择。

下面的代码示例将 `LeftFootEvent` 反映到序列化和编辑上下文中。

   ```
   ...
   static void Reflect(AZ::ReflectContext* context)
   {
       AZ::SerializeContext* serializeContext = azrtti_cast<AZ::SerializeContext*>(context);
       if (!serializeContext) return;

       serializeContext->Class<LeftFootEvent, EventDataSyncable>()->Version(1);

       AZ::EditContext* editContext = serializeContext->GetEditContext();
       if (!editContext) return;

       editContext->Class<LeftFootEvent>("LeftFootEvent", "")
           ->ClassElement(AZ::Edit::ClassElements::EditorData, "")
               ->Attribute(AZ::Edit::Attributes::AutoExpand, true)
               ->Attribute(AZ::Edit::Attributes::Visibility, AZ::Edit::PropertyVisibility::ShowChildrenOnly)
               ->Attribute(AZ_CRC("Creatable", 0x47bff8c4), true);
   }
   ...
   ```

1. 要将事件添加到动作中，请使用 `FindOrCreateEventData` 模板，该模板可接受 `EventData` 的任何子类。

   ```
   ...
   auto footstepData = GetEMotionFX().GetEventManager()->FindOrCreateEventData<LeftFootEvent>();
   motion->GetEventTable()->FindTrackByName("Sound")->AddEvent(0.2f, footstepData);
   ...
   ```

## 示例 

下面的示例显示了已完成的 `LeftFootEvent` 示例 `EventData` 子类，以及将事件添加到动作中的代码。

```
class LeftFootEvent final
    : public EMotionFX::EventDataSyncable
{
public:
    AZ_RTTI(LeftFootEvent, "{117454DC-0675-483E-843E-841C57A4354D}", EventDataSyncable);

    LeftFootEvent() = default;

    static void Reflect(AZ::ReflectContext* context)
    {
        AZ::SerializeContext* serializeContext = azrtti_cast<AZ::SerializeContext*>(context);
        if (!serializeContext) return;

        serializeContext->Class<LeftFootEvent, EventDataSyncable>()->Version(1);

        AZ::EditContext* editContext = serializeContext->GetEditContext();
        if (!editContext) return;

        editContext->Class<LeftFootEvent>("LeftFootEvent", "")
            ->ClassElement(AZ::Edit::ClassElements::EditorData, "")
                ->Attribute(AZ::Edit::Attributes::AutoExpand, true)
                ->Attribute(AZ::Edit::Attributes::Visibility, AZ::Edit::PropertyVisibility::ShowChildrenOnly)
                ->Attribute(AZ_CRC("Creatable", 0x47bff8c4), true);
    }

    bool Equal(const EventData& rhs, const bool /*ignoreEmptyFields*/) const override
    {
        // all LeftFootEvents are equal
        const LeftFootEvent* rhsEvent = azrtti_cast<const LeftFootEvent*>(&rhs);
        return rhsEvent != nullptr;
    }

    size_t HashForSyncing(bool isMirror) const override { return isMirror ? 1 : 0; }

protected:
    LeftFootEvent(const size_t /*hash*/) : LeftFootEvent() {}
};

auto footstepData = GetEMotionFX().GetEventManager()->FindOrCreateEventData<LeftFootEvent>();
motion->GetEventTable()->FindTrackByName("Sound")->AddEvent(0.2f, footstepData);
```

## 关于 Equal 函数的更多信息 

`Equal`函数测试两个`EventData`实例是否相等。

**语法**

```
virtual bool Equal(const EventData& rhs, bool ignoreEmptyFields = false) const = 0;
```

EMotion FX 使用`Equal`方法来重复`EventData`子类的实例。`AnimGraphMotionCondition` 类也将其用于动作事件匹配逻辑。

当事件管理器加载`.motion`文件并反序列化事件轨上的动作事件时，`EventManager::FindOrCreateEventData`会处理每个`EventData`实例。

`EventManager`会存储正在使用的`EventData`实例的列表，并尝试查找调用 `Equal(loadedEventData)`返回 true 的`EventData`实例。如果 `EventManager` 找到一个相等的 `EventData` 实例，重复的数据将被丢弃。

调用 `AnimGraphMotionCondition` 测试动作事件时，`AnimGraphMotionCondition::TestCondition` 会调用 `Equal` 方法，并将 `ignoreEmptyFields` 参数设置为 `true`。使用 `ignoreEmptyFields` 参数可以对 `EventData` 实例进行部分匹配。例如，如果其中一个字段是字符串，而条件中的字符串值为空，则该字段中的任何值都会匹配。

## 同步混合的动作

`EMotionFX::EventDataSyncable`类扩展了基础`EventData`类的功能，并启用了驱动动作同步行为的事件。使用 `EventDataSyncable` 类可指定用于同步混合动作的参数。该类会在两个不同动作的同步轨迹上调用 `HashForSynccing`，比较结果，并根据哈希值找出相等的事件。

### 镜像

您可以使用 `EMotionFX` 以编程方式镜像动作。镜像动作时，其同步事件也必须被镜像。为了发出镜像信号，`HashForSyncing`方法会接受一个`isMirror`参数。

例如，假设使用 `EventDataSyncable` 子类来镜像一匹马的步态。您使用一个整数字段来表示马的脚，其约定如下。

```
0=left rear
1=right rear
2=left front
3=right front
```

按以下示例实施 `HashForSyncing`.

```
size_t HashForSyncing(bool isMirror) const
{
    if (!isMirror)
    {
        return m_footIndex;
    }
    // Translate left foot (an even foot index) to right foot, and vice
    // versa
    return (m_footIndex % 2 == 0) ? m_footIndex + 1 : m_footIndex - 1;
}
```

`HashForSyncing` 的默认实现会返回类型 ID 的哈希值，并忽略 `isMirror` 参数。
