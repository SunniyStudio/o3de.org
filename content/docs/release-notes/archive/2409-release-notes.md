# O3DE 24.09.0 发行说明

准备好迎接 O3DE 24.09 版本的颠覆性更新。此最新版本包含由您的反馈驱动的出色增强功能，专注于增强性能和提升您的体验。现在，使用安装程序版本，您可以仅使用 Script Canvas 和 Lua 构建项目，无需编译器\！但这只是开始。我们为充满资源的大型项目提供了高达 90% 的惊人提升，从而加快了编辑器的启动时间。移动开发者们，欢呼吧\！在 iOS 和 Android 上享受惊人的 400% 性能提升，同时大幅减少运行时内存使用量。对于模拟用例，我们添加了 Georeference 组件、解析 Gazebo 数据的能力和 ROS2FrameComponent，以及许多模拟性能改进。此版本包含数百项生活质量改进和错误修复。深入了解 24.09 的一些令人兴奋的亮点。

请参阅模拟、渲染和 AR/VR 部分，了解这些区域的具体更改。

## 24.09.0 中的常规更改和改进

**主要改进：**

* 快速入门“纯脚本”项目 \- 可以使用 O3DE 安装程序版本使用 Script Canvas 和 Lua 构建项目，而无需 C++ 编译器。 
* 对移动设备和 AR/VR 设备上的内存使用进行了大量优化 
* 移动性能 \- 在 iOS、Android 和 AR/VR/XR 设备上提升高达 400% 
* 将 PhysX 4 和 5 拆分为单独的 Gem，使用户能够轻松地在 PhysX 版本之间切换，即使在使用安装程序版本时也是如此。 
* 添加了项目导出 UI，并完全支持 iOS、Android、Linux 和 Windows。用户不再需要使用命令行。 
* 添加了将无头服务器的大小减少多达 90% 的选项，具体取决于使用的资产。  
* 添加了特定于移动设备的渲染管道，允许用户根据需要轻松启用/禁用功能。 
* 添加了控制每个设备的质量设置的功能。根据 CPU、GPU 和内存的设备规格，默认有 3 个性能级别（低、中、高）。 
* 添加了着色器变体。设置后，这允许渲染器自动利用性能最高的着色器来满足给定的渲染需求。  
* 添加了实体轮廓功能 
* 添加了允许用户与他们选择的 LLM（AI 模型）交互的框架。 
* 在 CPU 使用率较高时发送网络心跳数据包;避免因超时而导致网络断开。


**改进：**

* 添加了对 PGM 图像格式的支持 
* 添加了对模板关卡创建的支持 
* 解决了网络游戏可能因加载大型关卡而被阻塞而导致网络游戏“超时”的几个问题。 
* 允许在 Lua 编辑器中使用非抗锯齿字体 
* 改进了 Lua 编辑器语法高亮显示和样式更新 
* 许多Prefab生活质量改进
* Prefab容器实体现在可以设置为 Editor Only （仅编辑器）。这样做会导致它们在运行时被跳过，只有Prefab的内容会生成到关卡中。 
* 迁移 CTrackViewTrack 的 AZ::Vector3 的 OffsetKeyPosition 
* 改进的 Asset Processor 资产分析性能 
* Linux 控制台日志记录
* 能够为每个平台设置不同的最大路径长度（解决了使用深路径安装 O3DE 时遇到的常见问题）。 
* 改进了从外部源导入模型和对象的能力 
* 项目管理器将项目对话框添加到目标project.json而不是任何目录 
* 添加 r\_fogLayerSupport 和 r\_fogTurbulenceSupport CVARS 以提高质量 
* 添加指数和指数平方雾模式 
* ScriptCanvas 支持四元数 CreateFromValues
* 改进的 HDRiSkybox 脚本 API 
* 解决了许多 Script Canvas 生活质量问题 
* 改进了具有大量实体的编辑器性能 
* 在 SpecularReflectionsFeatureProcessor 中添加了对光线追踪反射的支持 
* 许多 Asset Browser 生活质量改进 
* 地形 |将帮助 URL 添加到具有文档页面的组件  
* MiniAudio：为播放组件添加了更多音量控制和暂停方法 
* 将质量设置添加到默认项目模板 
* 针对所有支持的平台进行了许多 Atom 性能改进 
* 添加了对重命名 Gem 的支持  
* 添加了对针对蒙皮网格的光线跟踪的支持 
* 添加了材质管道文件以指定自定义对象 SRG 成员的功能 
* 为所有光源类型添加了对照明通道的支持 
* 添加了对通过 CVars 配置抗锯齿和多采样计数的支持 
* 添加了对 script canvas 中节点搜索的支持
* 添加了对打开/关闭定向光源阴影的支持 
* 添加了对模拟客户端无头运行客户端的支持 
* MultiplayerSample 游戏手柄玩家输入支持 
* 为 BasePBR、StandardPBR、EnhancedPBR 材质类型以及材质画布添加了顶点颜色支持。
* 添加了三种新的色调映射模式：AcesFitted、AcesFilmic 和 Filmic
* 添加了对 SimpleSpotLights 的阴影支持
* 添加了对简单聚光灯的图案轮（即纹理投影）支持。
* 在启用网格实例化时添加了每个网格着色器选项支持。 
* 改进的 RenderDoc 和 Pix 集成

**修复：**

* 对色调映射和眼睛着色器进行了许多修复 
* 修复了 Linux 安装错误，尤其是与 Python 安装相关的错误。 
* 许多 Trackview 错误修复 
* 修复了几个 Linux 启动崩溃问题 
* 解决了版本控制的 Gem 依赖项问题 
* 解决了 Linux 上的几个 python 问题 
* 解决了物理材质的几个问题 
* 解决了一些 Script Canvas 调试问题 
* 解决了 Lua 编辑器的几个问题 
* 解决了多个 Script Canvas 崩溃问题 
* 更新了 O3DE 以支持最新版本的 Visual Studio 
* 许多 Vulkan 崩溃修复 
* 跨各种 Android 芯片组和设备的许多修复 
* 跨 iOS 版本和设备的许多修复 
* Prefab \- 减少了大型深度嵌套关卡的内存消耗
* 解决了许多编辑器崩溃问题 
* 解决了许多Prefab覆盖问题 
* 解决了无头服务器的许多问题 
* 解决了多个 Mac 编译问题。注意：Mac 不是官方支持的平台，主要是 iOS 的构建器。但是，已经针对 Mac 编辑器完成了一些工作。请随意使用（风险自负）并报告您遇到的问题。  
* 解决了使用适用于 Windows 的安装程序版本、适用于 Linux 的 Debian 软件包和适用于 Linux 的 Snap 软件包时的多个问题。 
* 解决了几个 Material canvas 问题 
* 解决了地形生成和运行时的几个问题。 
* 更新了对 Visual Studio 2022 兼容性的 WWise 支持 
* 修复因数据包损坏导致流错误导致多人游戏断开连接的问题 
* 修复 O3DE-Extras 多人游戏模板项目 
* 修复音频线程占用 90% 帧时间的问题

**已删除旧代码：**

* 删除了未使用的旧切片代码 
* 删除了旧版 perlin 噪声 
* 删除了 ObjectManager 的 SendEvent 用法 
* 删除了 tools/styleui 
* 删除了 iRenderAuxGeom 
* 从 ObjectManager 中删除未使用的 Viewport cvar 
* 从 CryCommon 中删除未使用的接口 
* 删除未使用的 CTemplateObjectClassDesc 
* 删除对象管理器 python 反射和测试 
* 从 CTrackViewAnimNode 中删除 ITransformDelegate 
* EMFX |删除 baseObject 并将 MemoryObject 重命名为 RefCounted 
* 删除旧版 GameExporter（从未使用或调用） 
* 切片级别 UI 删除 |删除旧版 Slice Editor Ownership Service 和 Preemptive Undo Cache 类 
* 切片级别 UI 删除 |Inspector 和 Asset Browser 清理 
* 切片级别 UI 删除 |从 Editor Preferences 中删除与切片相关的设置。 
* 编辑器首选项 |删除未使用的深度选择设置

## 24.09.0 版本的模拟 O3DE Gem 更改：

**主要改进**

* ROS 2 Gem：添加了地理配准组件 
* ROS 2 Gem：在 ROS2SpawnerComponent 中添加了使用 WGS-84 数据格式生成资产的可能性 
* ROS 2 Gem：在 Editor 中扩展了 ROS2FrameComponent（分为 Game 和 Editor 组件） 
* ROS 2 RobotImporter：为摄像头、GNSS、IMU 和激光雷达传感器添加了 Gazebo 数据解析 
* ROS 2 RobotImporter：为滑移转向和 Ackermann 驱动模型添加了 Gazebo 数据解析 
* ROS 2 RobotImporter：为关节姿势轨迹和关节状态发布者配置添加了 Gazebo 数据解析

**改进**

* ROS 2 Gem：增加了关节的位置控制（可以设置初始配置;添加了 ROS 2 订阅和处理程序） 
* ROS 2 Gem：增加了相机稳定功能（防止倾斜） 
* ROS 2 Gem：添加了使用各种图像编码传输 ROS 2 相机图像的选项 
* ROS 2 Gem：修改后的时钟设计 
* ROS 2 RobotImporter：为主题、框架和命名空间添加了 Gazebo 数据解析 
* ROS 2 RobotImporter：重新设计的向导

**修复**

* ROS 2 Gem：修复了 ROS2SpawnerComponent 
* ROS 2 Gem：修复激光雷达数据发布 
* ROS 2 相关 Gem 和项目模板：清理了版本控制，更新了与 ROS 2 Jazzy 一起使用 
* ROS 2 相关 Gem 和项目模板：更新了Prefab以匹配 O3DE \>= 2310.0

**已删除代码**

* ROS 2 Gem：从 Gem 中删除过时和重复的文档文件

**其他值得注意的宝石：**

* Pointcloud https://github.com/RobotecAI/robotec-o3de-tools/tree/main/Gems/Pointcloud  
* ROS2ScriptIntegration https://github.com/RobotecAI/robotec-o3de-tools/tree/main/Gems/ROS2ScriptIntegration  
* VehicleDynamics https://github.com/RobotecAI/o3de-vehicle-dynamics-gem

## 渲染 24.09.0 版的特定更改

**移动渲染管道 \-** 添加了对为平铺 GPU 构建的高性能移动渲染管道的支持。

* 为移动设备优化了 BRDF \- 为移动设备添加了新的优化 BRDF 并支持分析环境 IBL 近似。 
* 添加了对简单聚光灯的图案轮（即纹理投影）支持。
* 为贴花添加了 CPU 剔除
* 添加了对 SimpleSpotLights 的阴影支持
* 添加了对贴花颜色和贴花颜色因子的支持，以帮助调整最终计算的贴花颜色。
* 为简单的点光源/聚光灯和贴花添加了着色器选项
* 添加了对简单点和简单准时光源的 CPU 剔除的支持
* 在 Mobile Pipeline 中添加了对延迟雾的支持。 
* 添加了指数和指数平方雾模式。  
* 添加了对定向光阴影的 PCF 过滤中的着色器选项的支持
* 为移动渲染管道添加了 Silhouette 支持。有关 Shadow 功能的更多详细信息，请参阅有关 Rendering additions 的部分。 
* 向移动渲染管道添加了粒子支持。 
* 在管道中添加了对 postFX 效果的支持 
* 针对移动渲染管道的各种半浮点优化。 
* 在各种移动设备上提供与渲染比例相关的支持和修复。


**渲染添加/改进 \-** 所有渲染管道的各种渲染改进和一般的 Atom 改进。

* 引擎的 RHI 和 RPI 层中多 GPU 支持的初始基架。其中很多工作包括多 GPU 对象的初始管道，以及在设备之间传输资源时对基本 GPU-GPU 同步的初始支持。 
* 剪影功能 \- 添加对剪影材质类型的支持，当应用于网格时，以各种模式绘制轮廓和填充的剪影：始终、XRay、可见和从不。 
* 添加了对 BasePBR、StandardPBR、EnhancedPBR 材质类型以及材质画布的顶点颜色支持。
* 添加了三种新的色调映射模式：AcesFitted、AcesFilmic 和 Filmic。移动设备渲染管线默认为 Filmic 模式。 
* 为图形质量的四层（低、中、高、非常高）添加了 android 和 ios 注册表设置。 
* 在 DX12 和 Vulkan RHI 后端中添加了对几何着色器的支持。 
* 增加了对 DX12 和 Vulkan RHI 后端中的交集着色器的支持。
* 外部视锥体的阴影剔除 \- 通过跳过所有不可见阴影的不必要工作来提高性能。这是通过对光源进行 CPU 剔除来实现的，以限制需要处理的光源数量。
* r\_antiAliasing \- 添加对通过 CVar r\ 切换抗锯齿模式的支持_antiAliasing 
* 添加对照明通道的支持 \- 此功能为每种光线和可照亮对象引入了 3 个通道。同一通道中的对象和灯光被分组，因此组中的灯光会影响同一组中的对象。默认行为是通道 0 中的所有光源和对象，因此它不应更改当前使用光照的方式，但它将提供一个进一步的选项，让艺术家能够灵活地控制哪些光照照射哪些对象。 
* 启用和禁用 ContrastAdaptiveSharpening 通道以及时间抗锯齿（即 TAA）。
* 添加了支持，允许将自定义导数传递给纹理采样函数。 
* 添加了 GPU 型号的 device 属性。这使我们能够为图形质量层提供更好的设备筛选。 
* 在启用网格实例化时添加了每个网格着色器选项支持。这允许我们为具有网格实例化的场景构建优化的着色器变体。 
* 在 Vulkan RHI 中添加了对时间轴信号量的支持，以帮助进行低级同步。 
* 添加了禁用为空渲染器的游戏启动器创建本机窗口的功能。

**Atom （O3DE 渲染器） 内存优化**

* 添加了对转储有关已加载资产的信息的支持。 
* 各种与 Metal RHI 相关的内存优化。 
* 支持释放 ModelAssets 拥有的 BufferAssets。这允许我们在 CPU 上释放与网格相关的内存，一旦它被传输到 gpu。 
* RayTracing 缓冲区的环形缓冲区支持。

## 24.09.0 版本的 VR/OpenXR 改进。

**改进**

* \[OpenXR\] 新的数据驱动型 API（Lua 和 C++），用于管理所有 OpenXR 兼容硬件中的操作 （I/O） 和空间（世界转换锚点）。 
* \[Android \+ OpenXR\] 新的 Android 项目生成器 UI，集成到 ProjectManager 中。有助于生成在 Android 上编译和部署项目所需的所有文件。（可选）可以为 Meta Quest 系列设备生成 Android 项目。开发人员在设备上的迭代时间从 \~1m：30s 缩短到不到 10 秒。

**修复**

* 修复了 Quest 设备的许多崩溃问题 
* \[OpenXR\] MSAA 解析可以直接在 Swapchain 附件中完成。

## 已知问题

* 音频 WWise 在删除并重新添加 Gem 之前无法正常工作
* 启用 PhysX 4 和 PhysX 5 将导致编辑器崩溃。请注意，ROS2 Gem 会自动启用 PhysX 5，请确保在启用 ROS2 Gem 之前禁用 PhysX 4。
* 首次加载项目时，默认情况下，资源处理器将使用所有可用的 CPU 内核，从而导致大量内存使用。这可能会导致大型项目的编辑器中的初始性能下降，直到处理完资源为止。这在 Linux 上尤其明显。如果这是一个问题，建议等到处理完资产。

## 感谢我们所有的贡献者使 24.09.0 版本成为可能\！

Steve Pham  
alexpete  
Guthrie Adams  
moudgils  
Michał Pełka  
Michael McGarrah  
Nicholas Lawson  
Gene Walters  
jhmueller-huawei  
Jan Hanca  
Akio Gaule  
Adam Dąbrowski  
ANT\\daimini  
Luis Sempé  
Mike Chang  
galibzon  
T.J. Kotha  
mbalfour  
Michael Pollind  
Alex Montgomery  
Martin Winter  
lemonade-dm  
guillaume-haerinck  
Akio Gaule  
Junhao Wang  
Danilo Aimini  
Shauna Gordon  
Joe Bryant  
Markus Prettner  
Martin Winter  
colinb\[APMG\]  
antonmic  
Joerg H. Mueller  
antonmic  
Hongshan Li  
Artur Kamieniecki  
siliconvoodoo  
Martin Sattlecker  
Chris Galvan  
Qing Tao  
Pawel Liberadzki  
Nicholas Lawson  
pwalterscarb  
Sarah Anderson  
André L. Alvares  
Reece Hagan  
Adi Bar-Lev  
omacx  
Olex Lozitskiy  
phuchau1989  
Patryk  
Xichen Zhou  
kursad-k  
Paulina Kubera  
carbonated-niko  
stevo26  
Djfos  
TheTopHat26  
Mateusz Żak  
Naomi Washington  
Vincent  
Antoni-Robotec  
0x14307  
Kacper Lewandowski  
o3de-issues-bot  
Patryk Antosz  
ShalokShalom  
jjs-home  
ExMatics HydrogenC  
realJavabot  
Nathan Brooks  
idclosed  
rhuthCarb  
AaronRuizMoraUK  
Umesh Rajesh Ramchandani  
Guy MacGeorges  
Alexander  
Lloyd Tullues  
Huawei-CarlosCarbone  
Shao Biyao  
Antoni Puch  
Andre Mitchell  
solo1967  
LB-MichalRodzos  
Luis Gutierrez  
Wiktor Bajor  
Maciej Aleksandrowicz  
CarbAndrewD  
Roderick Kieley  
konkuk kim  
Caleb Leak  
TheShed412  
Fergal  
utk-hua  
Tobias Alexander Franke  
Yaakuro  
Adam Krawczyk  
alexeykostin  
Joshua Rainbolt  
Erik Jacobs  
kberg-amzn  
yosagi  
TechnoPorg  
arvrschool  
floroeske  
JaviNunsys  
zchengw  
jeremy-kubota  
Mateusz Szweda  
Derikson  
iByte-ua  
Boy-Ma  
Nicholas Van Sickle  
Mahdi Safarmohammadloo  
Ganidhu Abey  
timstullich  
Khaled  
Filip Vavera  
Mike Sanders  
Oreloth  
skleembof  
colinb-accesspointmg  
Umesh Rajesh Ramchandani  
osmaneTKT  
shazhenyu123  
Lloyd T  
NanaYellen  
chanmosq  
Jeremy Pritts  
Aleks Starykh  
Krzysztof Rymski  
Xichen Zhou  
lemonade-dm  
Frede H  
michabr  
IVasilets  
Jyothish Kumar M S  
oym1994  
Piotr Jaroszek  
Mark \[Hilliness Studios\]  
maj-tom  
Kacper Dąbrowski  
yumigenack  
Egor Zelenkov  
Getue  
Archer Allstars  
gituser1024  
gerroon  
Kacper Bieniek  
yunni  
marszhao  
Bartek Boczek  
RicardasSim  
UsmanTariq2  
Greg O  
Marc Bestmann  
Yves Silva  
Samuel Porras  
Vivien Oddou  
alukyanov-tr  
Mirage-c  
Ulkesh Solanki  
bingbingmasdhg  
illiteratesquid  
Tortenschachtel  
Cody St Pierre  
antonkai  
carbfeng  
Gamers-Cafe  
Bartlomiej Styczen  
itachi-73m  
alexeykostin  
Reece Hagan  
Alexander Efimov  
Tailgunnnmer  
Cameron Angus  
Mohammad Nakshbandi  
jonbitzen  
Yufei Jiao  
Matthias Fauconneau  
vmednonogov  
chinepun  
scorp  
Riccardo m. Bonato  
apmontgo  
Gaël Écorchard  
Khalid Hanif   
Eric  
KokkakNiphon  
IndieDev99  
nicholas-rh  
MajesticGames  
Amy Finch  
Michał Borowiecki  
angel eyes  
Mayumi1988  
Jarvis  
Patryk  
VinnyVicious  
feng  
Krzysztof Rymski  
ZielIhu  
Łukasz Mróz  
CBBosman  
Pedro Henrique Andrade da Silva  
Tony Balandiuk  
jjs-home  
BlastBlaster17  
Vladimir Ziablitskii  
pjhusky  
Anshul  
Clayton Walker  
dKumin  
Anna Fąferek  
VladimirLagutin  
Przemyslaw Poljanski  
conradax451  
Nahuel Espinosa  
Gene Walters  
tarboss  
Benjamin Yde  
Blackmaster1988  
Cefatus  
albertdai-rf  
Valentin Bas  
michael-chen2010  
Nathan Brooks  
Robin Johnson  
w1t1  
Przemysław Poljański  
jckand-amzn  
Trung Nguyen  
Eric  
Branden J Brown  
anon-oss  
boberfly  
Xichen(Sichem) Zhou  
Sai Kumar  
Amy Finch  
IllustrisJack  
Tyler Jaacks  
ash7977  
Wiktor Bajor  
Mohamed Ali  
fasfasafaffasfa  
loopaq
