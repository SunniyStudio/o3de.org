# O3DE 24.09.1 发布日志

24.09.1 是一个维护版本，用于修复在 24.9.0 版本中发现的问题。 

**一般问题**

* 修复重载关卡时Decal纹理泄露的问题  
* 修复由于新版 spirv-cross 导致的 iOS 性能问题
* 修复手机上的阴影带  
* 修复 Windows 安装程序tar问题   
* 双引号 Windows 命令调用，以支持路径中的空格   
* 修复 Windows 上的纯脚本模式   
* 修复两个 AR 测试崩溃
* 针对 Linux 安装程序的 Python 启动限制修复

**机器人与仿真**

* 更新 ros2 模板包 \#769 修正了项目模板的版本号；  
* 修复 ROS2FrameComponent::UpdateNamespaceConfiguration()   
* 扩展 FrameConversion.py 的功能以处理覆盖。 
* 允许 ROS2 Frame组件出现在关卡实体上。 
* 修复 FollowingCameraComponent：允许飞行摄像头   
* 注册表设置管道修改  
* 修复\`一个spawner组件的错误\`

**O3DE二进制文件网站**

* 允许下载当前版本\- 1 的安装程序和 debian 软件包  
* 现在还提供离线安装程序