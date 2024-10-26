---
description: ' 在 O3DE 动画编辑器中自定义 EMotionFX 对象。'
title: 自定义 EMotionFX 对象
---

EMotion FX API 支持注册自定义对象类型，包括状态机节点、混合树节点、转换和条件。您可以在游戏代码或自定义 gem 中定义自定义对象类型。这样，您就可以对 O3DE 动画系统进行细粒度控制。

## 注册自定义对象 

在注册自定义对象之前，先激活EMotion FX的`SystemComponent`，以确保EMotion FX运行时正确初始化。然后使用EBus调用`EMotionFXRequestBus::Events::RegisterAnimGraphObjectType`方法。您可以通过从依赖于`EmotionFXAnimationService`的组件中注册自定义节点来确保激活 EMotion FX 运行时。您无需手动实例化 EMotion FX `SystemComponent` 并调用 `Activate` ；组件依赖关系会处理这些任务。

**注册自定义节点**

1. 在你的自定义Gem或游戏项目代码中，定义`EMotionFX::AnimGraphObject`的子类。

1. 创建`AZ::Component`的子类。

1. 在你的组件的 `GetDependentServices()` 方法中, 在`EmotionFXAnimationService`中添加依赖：

   ```
   dependent.push_back(AZ_CRC("EmotionFXAnimationService", 0x3f8a6369));
   ```

1. 在你的组件的`Activate()`方法中，注册你的节点类型：

   ```
   EmotionFXAnimation::EMotionFXRequestBus::Broadcast(
     &EmotionFXAnimation::EMotionFXRequestBus::Events::RegisterAnimGraphNodeType,
       MyCustomNode::Create(nullptr)
   );
   ```

## 实现 AnimGraphObject 子类 

`AnimGraphObject`是动画图形中所有对象的基类。基类的构造函数是受保护的；相反，对象是通过 `Create()` 方法实例化的。O3DE 动画系统（EMotion FX）使用 `AnimGraphObject`实例，通过调用 `Clone()`方法创建其他实例。

每个`AnimGraphObject`子类都有一个唯一的类型 ID，用于将对象序列化到`.animgraph`文件或从该文件中解码对象。您可以使用带有 `TYPE_ID` 成员的公共匿名枚举来声明对象的类型 ID。

在实现 `AnimGraphObject` 子类时，必须定义以下方法：


| 方法 | 说明 |
| --- | --- |
| uint32 GetBaseType() const | 定义对象的基本类型。有三种基本类型：节点、转换和条件。 |
| const char\* GetTypeString() const | 定义对象类型名称的字符串版本。 |
| const char\* GetPaletteName() const | 定义用户界面中显示的名称。 |
| AnimGraphObject::ECategory GetPaletteCategory() const | 定义对象在用户界面调色板或选项卡中的显示位置。 |
| AnimGraphObject\* Clone(AnimGraph\* animGraph) | 创建同一对象类型的新实例，并将克隆对象的动画图形设置为 animGraph。 |
| AnimGraphObjectData\* CreateObjectData() | 创建一个 AnimGraphObjectData 实例的新实例。这代表了 AnimGraphInstance 中每种类型的节点所独有的数据。 |
