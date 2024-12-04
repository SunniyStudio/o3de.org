---
linktitle: 组件开发指南
title: O3DE UI 组件开发指南
description: 使用自定义 Qt 小部件库，简要了解 Open 3D Engine （O3DE） UI 组件开发背后的概念。
weight: 200
toc: true
---



对于许多基本组件（例如复选框、按钮和行编辑），您将使用基本的 Qt 小部件（例如`QCheckBox`, `QPushButton`, `QLineEdit`）来开发您的 UI。这些小组件的自定义样式和行为会自动应用。 需要扩展功能的组件，或者是 Open 3D Engine （O3DE） 独有的组件，是可以子类化的自定义类，并且可以包含 Qt 小部件的组合。在这些情况下，类定义位于此文件夹中：`<engine>/Code/Framework/AzQtComponents/AzQtComponents/Components/.`

## 样式表和 StyleManager

O3DE UI 建立在 [Qt 样式表](https://doc.qt.io/qt-5/stylesheet.html) 之上，这是自定义小部件外观的强大机制。其概念和语法类似于级联样式表 （CSS）。

在 Qt 样式表中，`AzQtComponents` 模块有一个 `StyleManager` 类，该类在应用程序级别安装自定义 [QProxyStyle 类(https://doc.qt.io/qt-5/qproxystyle.html) 。类的 `QProxyStyle` 使用自定义样式覆盖所有小部件的样式。这就是如何使用基本的 Qt 小部件（例如  `QPushButton` 或`QCheckBox`）并为其提供自定义样式。

StyleManager 类预加载了一系列基本样式表，从`<engine>/Code/Framework/AzQtComponents/AzQtComponents/Components/Widgets/BaseStyleSheet.qss`开始。

此样式表为每个单独的小部件应用自定义样式，每个小部件都分为 "`CheckBox.qss`" 和 "`PushButton.qss`" 等文件。此样式从顶部应用程序层开始，向下应用于整个 Widget 层次结构。其他自定义样式表可以应用于仅影响该 widget 及其任何子项的较低级别（例如在您自己的自定义工具中）。

## 开始 UI 组件开发

根据您的工具和现有代码，访问新的组件库可能需要一些初始设置。

如果您的工具是核心 O3DE 编辑器的一部分，或者是使用编辑器的 Qt 应用程序的 Gem 的一部分，那么您已经设置好并准备好了。只需从`<engine>/Code/Framework/AzQtComponents/AzQtComponents/Components`文件夹中包含您需要使用的组件的标头。

对于具有自己的 Qt 应用程序的独立工具，您必须采取一些额外的步骤来确保正确应用新样式。

1. 在创建 QApplication 之前，设置属性以正确处理高 DPI 屏幕。

    ```cpp
    #include <QApplication>

    QCoreApplication::setAttribute(Qt::AA_EnableHighDpiScaling);
    QCoreApplication::setAttribute(Qt::AA_UseHighDpiPixmaps);
    QGuiApplication::setHighDpiScaleFactorRoundingPolicy(Qt::HighDpiScaleFactorRoundingPolicy::PassThrough);
    AzQtComponents::Utilities::HandleDpiAwareness(AzQtComponents::Utilities::PerScreenDpiAware);

    QApplication app(argc, argv);
    ```

1. 实例化 StyleManager，它加载新 UI 的样式表和自定义设置。

    ```cpp
    #include <AzQtComponents/Components/StyleManager.h>

    AzQtComponents::StyleManager* styleManager = new AzQtComponents::StyleManager(this);
    const bool useLegacyStyle = false;
    styleManager->initialize(app, useLegacyStyle);
    ```

    StyleManager 会自动加载基本样式表，但您也可以添加其他资源。

## UI 开发最佳实践

编写代码来扩展核心 O3DE 编辑器？以下是符合 UI 准则的一些高级建议：

+ UI 样式使用 O3DE 标准调色板，其中包括灰色和蓝色。创建自定义样式和颜色覆盖将使您的工具看起来与 O3DE 编辑器的其余部分不一致。我们建议尽可能避免自定义样式覆盖，以便 O3DE 显示为一个内聚的应用程序。
+ 编辑器现在支持图标的矢量文件格式，这可以防止它们在高 DPI 显示器上显得模糊或像素化。确保将 SVG 文件用于您的图标，并尽可能在现有工具中用矢量图形图像替换旧的 PNG 和 JPG。
+ 当使用非 O3DE 提供的自定义图标时，图标应为 16 x 16 的倍数。
+ 展望未来，我们希望确保整个 O3DE Editor 的用户体验是连贯的和熟悉的。您应该避免进行一次性的自定义更改;当您添加新功能时，请将它们添加到库中，以便它们可供整个 Editor 使用，而不仅仅是单个工具。
+ 避免从组件库中子类化或封装 widget。如果您需要某些特定行为，请查看我们的资源，通过特定设置验证它是否已在组件库中可用。

## 常见问题

### 我可以直接从 O3DEQtControlGallery 复制代码示例吗？

是的。请注意，此工具并非涵盖所有设置。使用此扩展指南和 [O3DE UI 扩展C++ API 参考](/docs/api/frameworks/azqtcomponents/namespace_az_qt_components.html)  查找其他示例和文档。

### 控件的视觉和颜色修改呢？

我们不支持对布局和视觉设计的自定义修改，因为我们希望支持在所有工具中感觉一致的体验。不过，有时可能需要进行细微的间距和样式调整。

### 为什么我应该使用现有的小部件样式而不是创建自己的样式？

我们听取了来自各种游戏开发者的反馈，一个共同点是，编辑器不同工具的用户体验应该是一致的。组件库在构建时考虑了这些反馈，旨在为整个 Editor 带来统一且连贯的标准。

在开发方面，该解决方案允许更好的模块化和代码重用，目的是减少开发人员创建接口所需的工作。统一 UI 控件还允许轻松共享改进，因为整个 Editor 在应用后会自动使用它们，并且同样减少了在库更新或核心更改期间出现问题的可能性。

### 为什么我不应该创建子类或封装现有的小部件？

组件库旨在简化开发和用户体验。通过对 UI 组件进行子类化或封装以更改其行为或添加功能，可以将特殊情况添加到代码库中。然后，这些情况将需要特殊的文档和测试以避免回归。与将所有 UI 组件放在一个位置相比，子类化或封装 UI 组件的好处非常小。

子类化也不利于可发现性，因为新开发人员很难在不创建新的依赖项或复制代码的情况下查找、导入和重用专门在工具中构建的代码。过去，这导致多个工具创建自己的特殊控件，这些控件执行非常相似的操作，但方式截然不同，这使得难以统一它们并验证它们的行为。
