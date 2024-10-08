---
linkTitle: 将自定义组件暴露给Track View视图
title: 将自定义组件暴露给Track View视图
description: 了解如何将自定义组件及其属性公开给Track View编辑器，以便在 Open 3D Engine 中制作动画。
toc: true
weight: 800
---

要在电影剪辑场景和渲染到磁盘的影片中加入自定义组件，必须在 O3DE 的 Track View和**Entity Inspector**中公开可动画组件的属性。要公开自定义组件及其属性，必须执行三个步骤：

1. 在组件的一个请求事件总线上为 可以制作动画的 属性创建获取和设置方法。

1. 在组件中实施 getter 和 setter 请求处理程序。

1. 将组件反射到编辑上下文和行为上下文中。编辑上下文反射会在**Entity Inspector**中显示组件，而行为上下文反射会在 Track View中显示组件。

## 公开自定义组件：示例

下面的示例假设创建了一个名为 `ImaginaryTargetComponent` 的自定义组件。该组件有一个名为`ImaginaryPosition`的 `Vector3` 属性，您希望在**Track View**中对其进行动画处理。还为该组件创建了名为 `ImaginaryTargetComponentBus` 的请求总线。本示例假定您熟悉事件总线和组件处理程序的编程。有关详细信息，请参阅 [使用事件总线 (EBus) 系统](/docs/user-guide/programming/messaging/ebus/)  和 [创建组件](/docs/user-guide/programming/components/create-component/)。

**要将自定义组件公开到 Track View**

1. **创建getter和setter方法**

   每个属性都必须提供一个方法来设置其值和获取其当前值。要实现这一点，可在组件的一个请求总线上创建 setter 和 getter 方法。然后将这些方法反射到行为上下文中，作为组件类反映的一部分。

   下面的示例在 `ImaginaryTargetComponentRequestBus` 上创建了 setter 和 getter 请求。

    ```
   /*!
   * ImaginaryTargetComponentRequests EBus Interface
   * Messages serviced by ImaginaryTargetComponents.
   */
   class ImaginaryTargetComponentRequests
       : public AZ::ComponentBus
   {
   public:

       // EBusTraits overrides - Application is a singleton.
       // Only one component on an entity can implement the events.
       static const AZ::EBusHandlerPolicy HandlerPolicy = AZ::EBusHandlerPolicy::Single;

       // Getter/Setter methods for ImaginaryTargetPosition.
       virtual AZ::Vector3 GetImaginaryTargetPosition() = 0;
       virtual void SetImaginaryTargetPosition(const AZ::Vector3& newPosition) = 0;
   };
   using ImaginaryTargetComponentRequestBus = AZ::EBus<ImaginaryTargetComponentRequests>;
   ```

1. **在组件中实现处理程序**

   在组件中为第一步中声明的 setter 和 getter 请求执行处理程序，如下面的示例。

   ```
   class ImaginaryTargetComponent
       : public AzToolsFramework::Components::EditorComponentBase
       , public LmbrCentral::ImaginaryTargetComponentRequestBus::Handler
   {

   public:
       AZ_EDITOR_COMPONENT(ImaginaryTargetComponent, "{4491D282-C120-4B2E-BC63-AC86296956A2}");

       ImaginaryTargetComponent() : m_imaginaryPosition(.0f) {};

       // ImaginaryTargetComponentRequestBus::Handler implementation.

       // Implementations for Getter/Setter methods for ImaginaryTargetPosition.
       // Presumably these would be used for something useful; this example just
       // stores and returns the value.
       AZ::Vector3 GetImaginaryTargetPosition() override { return m_imaginaryPosition; }
       void SetImaginaryTargetPosition(const AZ::Vector3& newPosition) override { m_imaginaryPosition = newPosition; }

   protected:
       // Required Reflect function.
       static void Reflect(AZ::ReflectContext* context);

   private:
       AZ::Vector3 m_imaginaryPosition;
   };
   ```

1. **反射组件**

   使用编辑上下文和行为上下文，反射组件的类、请求事件总线以及设置器和获取器方法。**Track View**会使用您在此步骤中反映的setter和getter方法来设置和获取动画属性的值。您还必须反映一个 `VirtualProperty` 声明，告诉 **Track View** 您的组件能够被动画化。

   ```
   /*static*/ void ImaginaryTargetComponent::Reflect(AZ::ReflectContext* context)
   {
       AZ::SerializeContext* serializeContext = azrtti_cast<AZ::SerializeContext*>(context);

       if (serializeContext)
       {
           serializeContext->Class<ImaginaryTargetComponent, AzToolsFramework::Components::EditorComponentBase>()
               ->Version(0)
               ->Field("ImaginaryPosition", &ImaginaryTargetComponent::m_imaginaryPosition);

           AZ::EditContext* editContext = serializeContext->GetEditContext();
           if (editContext)
           {
               editContext->Class<ImaginaryTargetComponent>("ImaginaryTarget", "A Code Sample enabling Track View Animation")
                   ->ClassElement(AZ::Edit::ClassElements::EditorData, "")
                       ->Attribute(AZ::Edit::Attributes::Category, "Game")
                       ->Attribute(AZ::Edit::Attributes::AppearsInAddComponentMenu, AZ_CRC("Game", 0x232b318c))
                   ->DataElement(0, &ImaginaryTargetComponent::m_imaginaryPosition, "Imaginary Target Pos", "Imaginary Target Position")
                   ;
           }
       }

       AZ::BehaviorContext* behaviorContext = azrtti_cast<AZ::BehaviorContext*>(context);
       if (behaviorContext)
       {
           // Reflect the setter and getter methods and create a virtual property that refers to them.
           behaviorContext->EBus<ImaginaryTargetComponentRequestBus>("ImaginaryTargetRequestBus")
               ->Event("GetImaginaryTargetPosition", &ImaginaryTargetComponentRequestBus::Events::GetImaginaryTargetPosition)
               ->Event("SetImaginaryTargetPosition", &ImaginaryTargetComponentRequestBus::Events::SetImaginaryTargetPosition)
               ->VirtualProperty("ImaginaryPosition", "GetImaginaryTargetPosition", "SetImaginaryTargetPosition");

           // Attach the "ImaginaryTargetRequestBus" EBus that you reflected to the behavior context of the ImaginaryTargetComponent class.
           behaviorContext->Class<ImaginaryTargetComponent>()->RequestBus("ImaginaryTargetRequestBus");
       }
   }
   ```

1. **(可选）在获取器上放置单位属性**

   **Track View** 的用户界面取决于 getter 和 setter 所使用的数据类型。前述示例使用的数据类型是 `AZ::Vector3`，因此 **Track View** 从属性中创建了一个复合的 `x,y,z` 轨迹。相比之下，如果获取器和设置器使用了`bool`类型，**Track View**就会创建一个布尔轨迹。对于大多数可动画属性来说，使用这种类型就足够了。但在某些情况下，您可能需要为反射属性设置单位。例如，如果属性的 `AZ::Vector3` 表示颜色，则必须在 getter 事件的反射中添加一个属性。该属性指示 **Track View** 为该属性使用颜色选择器。如果您有一个名为`ImaginaryTargetColor`的属性，它调用了一个名为`GetImaginaryTargetColor`的getter事件，请使用如下反射代码：

   ```
   ->Event("GetImaginaryTargetColor", &ImaginaryTargetComponentRequestBus::Events::GetImaginaryTargetColor)
       ->Attribute("Units", AZ::Edit::Attributes:: PropertyUnits8BitColor)
   ```

   如下图所示，**Track View**会为属性使用颜色轨迹。

   ![Color picker in Track View](/images/user-guide/programming/components/component-entity-system-pg-track-view-unit-attributes.png)

   其他单位可以在文件`Code\Framework\AZCore\AZCore\Serialization\EditContextConstants.inl`中找到。这些单元如下。

   ```
   const static AZ::Crc32 PropertyUnitsRadian = AZ_CRC("Radians");
   const static AZ::Crc32 PropertyUnits8BitColor = AZ_CRC("8BitColor");
   ```

   如果您想让 **Track View** 在用户界面中将以弧度为单位的角度参数转换为度，请使用`AZ::Crc32 PropertyUnitsRadian`。

## 查看结果

现在，您可以查看示例组件和属性在**Entity Inspector**和跟踪视图中的显示效果。

在下面的**Entity Inspector**图像中，`EditContext`反射暴露了**ImaginaryTarget**组件及其**Imaginary Target Pos**属性。

![ImaginaryTarget component in Entity Inspector](/images/user-guide/programming/components/exposing-custom-components-to-track-view-for-animation-entity-inspector.jpg)

在下面的**Track View**图片中，`BehaviorContext`反射从相应的虚拟属性中显示了**ImaginaryTarget**组件和**ImaginaryPosition**轨迹。

![ImaginaryTarget component in the Track View](/images/user-guide/programming/components/exposing-custom-components-to-track-view-for-animation-track-view.jpg)
