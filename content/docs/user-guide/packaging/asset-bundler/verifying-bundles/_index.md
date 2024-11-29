---
description: ' 验证 O3DE 项目中的资产是否已正确捆绑和引用。'
linktitle: 验证捆绑包
title: 验证 Bundle 是否包含必需的资源
weight: 300
---

当您的团队生成包含捆绑游戏资源的发布版本时，您需要验证这些捆绑包是否包含发布游戏所需的一切。在整个游戏开发过程中使用 O3DE 资产捆绑系统的工具，以验证您的资产包是否包含所需的资产。

作为最佳实践，请在所有游戏开发阶段验证您的捆绑包是否包含其所需的资产：

* 开始时
* 当您开发时
* 正在开发版本中
* 在发布版本中

## 当你开始时

在开始构建游戏时，请确保创建对产品在运行时所需的资产的引用。

## 当你开发你的游戏时

在开发过程的后期捆绑资产时，可能会发生错误。为避免错误，请考虑尽早捆绑。但是，即使在定义种子列表或构建捆绑包之前，您也可以采取措施避免以后出现不必要的意外。

### 在你有一个种子列表之后

在构建种子列表之后，但在捆绑资产之前，您可以使用 Asset Validation Gem 来验证资产加载是否映射回种子。有关详细信息，请参阅 [使用资产验证 Gem 验证种子](/docs/user-guide/gems/reference/assets/asset-validation).

## In Development Builds

为游戏生成捆绑包后，请使用 Editor 或 Launcher 的开发版本在 Bundle 模式下测试您的游戏。在捆绑包模式处于活动状态时，使用 `sys_report_files_not_found_in_paks` 控制台变量查找游戏 `.pak` 文件中缺少的任何资产文件。有关详细信息，请参阅 [使用 Bundle Mode 测试 Bundle](bundle-mode/).

## 发布版本中

与开发版本一样，您可以使用捆绑包模式和`sys_report_files_not_found_in_paks`控制台变量来查找发布版本中缺少的资源文件。有关详细信息，请参阅 [使用 Bundle Mode 测试捆绑包](bundle-mode/).
