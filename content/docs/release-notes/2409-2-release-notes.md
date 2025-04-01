---
linktitle: 24.09.2 发行说明
title: Open 3D Engine 2409.2的发行说明
description: 了解 O3DE 2409.2 中提供的内容
weight: 899
toc: true
---

## 24.09.2 发行说明

24.09.2 是一个维护版本，用于修复 24.09.0 版本中发现的问题。主要侧重于性能优化和错误修复。

**常规**

* GameLift 服务器将仅引入多人游戏 Gem 服务器，解决 O3DE 中的 GameLift 问题
* 修复了在发布模式下构建 ScriptCanvas.Editor Gem 时出现的链接器错误，但仅限于关闭 Unity 构建时。解决新项目的构建问题。
* 修复了高密度场景中帮助程序图标渲染缓慢的问题（FPS 从 60 降低到 3）。在编辑器中启用 Icon 帮助程序不再对性能产生明显影响。
* 通过将无效的 MFC 代码替换为 NativeUIRequests 对话框，修复并改进了内置的崩溃报告对话框。MFC 对话框正在编译，但从未显示，因为其资源 IDS 从未注册。新建对话框： ![crash-dialog](https://github.com/user-attachments/assets/12de1db0-112a-4e4d-a587-059b1d3c1150)
* 修复了着色器重新加载错误
* 修复了与 Material::m_shaderVariantReadyEvent 相关的竞争条件
* 修复了加载关卡时的竞争条件和死锁
* 修复了 InstanceDatabase 中的两个争用条件
* 修复了 PreviewRenderer 材质更新崩溃的问题
* 修复了 DX12::StreamingImagePool 相关的死锁
* 修复了某些 ASV 测试中发生的死锁
* 修复了在撤消/重做预制件焦点时发生的断言
* 修复了 Address Sanitizer 发现的许多问题
* 修复了切换关卡后阴影不起作用的问题


**机器人和仿真**

* 解决了使用 ROS2 Camera 渲染星形组件的问题
* 解决了使用 ROS2 摄像机渲染天空组件的问题
* 实验性功能（隐藏在注册表设置后面），允许在渲染到纹理功能（在 ROS2 相机中使用）中进行管道修改
* 修复了 ROS2 Camera 中的地形渲染问题
* 在 FrameConversion 脚本中为 'remove'作添加了 if 语句
* 修复了从 ROS2 模板创建的项目的 Gem 依赖项问题
* 通过将具体组件名称恢复为模板化组件名称，修复了模板化的 DemoLevel 预制件
* 删除了对 Ros2ProjectTemplate 中模板中不存在文件的引用
* RobotImporter：删除了发布模式下的 unused_variable 警告
* 将 Joint Trajectory State 移至 TrajectoryComponent。修复了机器人移动目标的问题
* 修复了 JointPositionEditorComponent 中关节未清除的 bug
* 修复了 ROS2 相机中的 PostProcessFeatureProcessor 视图锯齿
