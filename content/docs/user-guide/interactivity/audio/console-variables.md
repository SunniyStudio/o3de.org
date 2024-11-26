---
title: Audio Console Variables
description: 使用控制台变量来控制 Open 3D Engine 音频系统。
weight: 500
---

以下控制台变量可用于 O3DE 音频系统。

| 变量 | 说明 |
| --- | --- |
| **s_ATLMemorySize** | 以 KB 为单位指定音频转换层 （ATL） 要使用的内存池的大小。 <br> 默认值: PC = `8192`, Mac = `8192`, Linux = `8192`, iOS = `8192`, Android = `4096` |
| **s_AudioEventPoolSize** | 设置预分配的音频事件的数量。 <br> 默认值: PC = `512`, Mac = `512`, iOS = `128`, Android = `128` |
| **s_AudioObjectsDebugFilter** | 将调试绘制过滤为仅名称与字符串匹配的音频对象。 <br> 默认值: "" (all) <br> Example: `s_AudioObjectsDebugFilter=weapon_axe` |
| **s_AudioObjectPoolSize** | 设置预分配的音频对象和相应音频代理的数量。 <br> 默认值: PC = `1024`, Mac = `2048`, iOS = `256`, Android = `256` |
| **s_AudioProxiesInitType** | 可以在全局范围内覆盖。如果设置，它将确定 AudioProxies 是同步初始化还是异步初始化。这是一个性能变量，因为异步初始化 AudioProxies 会大大降低对调用线程的影响。当设置为异步初始化时，音频播放会延迟。 <br> 值: `0` = AudioProxy-specific initialization; `1` = Initialize synchronously; `2` = Initialize asynchronously <br> 默认值: `0` (all platforms) |
| **s_AudioTriggersDebugFilter** | 将调试绘制筛选为仅匹配字符串的音频触发器。 <br> 默认值: "" (all) <br> 例如: `s_AudioTriggersDebugFilter=impact_hit` |
| **s_DrawAudioDebug** | 将 AudioTranslationLayer 相关的调试数据绘制到屏幕上。标志可以组合。 <br> 值: <br><ul><li>`0`: 屏幕上没有音频调试信息</li><li>`a`: 在活动音频对象周围绘制球体</li><li>`b`: 显示活动音频对象的文本标签</li><li>`c`: 显示活动音频对象的触发器名称</li><li>`d`: 显示活动音频对象的当前状态</li><li>`e`: 显示活动音频对象的 RTPC 值</li><li>`f`: 显示活动音频对象的 Environment 量</li><li>`g`: 绘制遮挡光线</li><li>`h`: 显示遮挡光线标签</li><li>`i`: 在活动音频侦听器周围绘制球体</li><li>`v`: 列出活动事件</li><li>`w`: 列出活动的 Audio Object</li><li>`x`: 显示 FileCache Manager 调试信息</li><li>`y`: 显示音频实现的内存池使用信息</li></ul> 默认值: "" (all) <br> 例如: `s_DrawAudioDebug abc` |
| **s_EnableRaycasts** | 设置为`true`/`false`以全局启用/禁用音频遮挡和阻塞的光线投射。 <br> 默认值: `true` |
| **s_FileCacheManagerDebugFilter** | 允许过滤显示不同的 AFCM 条目，例如 Globals、Level Specifics 和 Volatiles。标志可以组合。 <br> 值: `a` = Globals; `b` = Level Specifics; `c` = Game Hints; `d` = Currently Loaded <br> Default: "" (all)|
| **s_FileCacheManagerMemorySize** | 设置 AFCM 在堆上分配的大小（以 KB 为单位）。 <br> 默认值: PC = `393216`, Mac = `393216`, Linux = `393216`, iOS = `2048`, Android = `73728` |
| **s_IgnoreWindowFocus** | 如果设置为 `true`，则当 Editor 或 Game 窗口失去焦点时，声音系统将继续播放。 <br> 默认值: `false` (off) |
| **s_PositionUpdateThreshold** | 音频对象必须至少移动此量才能向音频系统发出位置更新请求。 <br> 默认值: `0.1` (10 cm) |
| **s_RaycastCacheTimeMs** | 物理光线投射结果在被视为脏污并需要重新投射之前会给予此时间。 <br> 默认值: `250.0` |
| **s_RaycastMaxDistance** | 对于与听者的距离大于此值的声音，不会发送声障/声笼的光线投射。 <br> 默认值: `100.0` |
| **s_RaycastMinDistance** | 对于与听者的距离小于此值的声音，不会发送用于声障/声笼的光线投射。 <br> 默认值: `0.5` |
| **s_RaycastSmoothFactor** | 声障/声笼值的平滑到目标的速度应有多慢： `delta / (smoothFactor^2 + 1)`. <br> 较低的值将更快地平滑;较高的值将使平滑速度变慢。 <br> 默认值: `7.0` |
| **s_ShowActiveAudioObjectsOnly** | 确定在启用调试绘制时，是只绘制活动的音频对象还是绘制所有音频对象。 <br> 默认值: `false` (绘制所有音频对象) |
| **s_VelocityTrackingThreshold** | 音频对象必须至少将其速度更改此量，才能向音频系统发出`object_speed` RTPC 更新请求。 <br> 默认值: `0.1` (10 cm/s) |
