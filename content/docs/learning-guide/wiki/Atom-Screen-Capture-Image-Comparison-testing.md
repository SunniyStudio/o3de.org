---
title: "[Atom] Screen Capture Image Comparison 测试"
description: ""
toc: false
---

O3DE 能够通过脚本自动化 Gem 编写脚本加载关卡，捕获渲染输出，然后将捕获的图像与之前捕获的已知“良好图像”进行比较，以验证渲染是否正常工作。此功能已由 smoke 单元测试套件中的新单元测试公开。
本文介绍测试的工作原理以及如何添加其他屏幕截图测试。

### 单元测试设置
单元测试功能在 python 中实现 - o3de/AutomatedTesting/Gem/PythonTests/Atom/TestSuite_Periodic_GPU.py。

该文件声明了 2 个单元测试集：一个用于 DX12，一个用于 Vulkan。在非 Windows 平台上跳过 DX12 测试。此外，这两组的单元测试在测试级别列表（和相关设置）和渲染管道列表中进行参数化。

注意：每个测试都是 AutomatedTesting.GameLauncher 的单独启动。考虑到当前启动 GameLauncher 的成本（大约 30 秒）和每个测试多次运行（测试的 API 数量 * 渲染管道数量），测试的总时间可以快速扩展。

单元测试创建了一种方法来描述将要执行的一组测试，并使其易于执行。让我们以 DX12 设置为例。

在下方，SUITE、project 和 launcher_platform 标记可以作为样板忽略。这些测试需要 GPU。标记 Requires_gpu 对于通知 Jenkins 需要 GPU 是必需的。
```python
    @pytest.mark.SUITE_smoke
    @pytest.mark.REQUIRES_gpu
    @pytest.mark.skipif(not WINDOWS, reason="DX12 is only supported on windows")
    @pytest.mark.parametrize("project", ["AutomatedTesting"])
    @pytest.mark.parametrize("launcher_platform", ["windows"])
    @pytest.mark.parametrize("render_pipeline", atom_render_pipeline_list)
    class TestPeriodicSuite_DX12_GPU(object):

        @pytest.mark.parametrize("test_name, screenshot_name, test_script, level_path, compare_tolerance, camera_name", atom_feature_test_list)
        def test_Atom_FeatureTests_DX12(
                self, workspace, launcher_platform, test_name, screenshot_name, test_script, level_path, compare_tolerance, camera_name, render_pipeline):
            """
            Run Atom on DX12 and screen capture tests on parameterised levels
            """
            run_test(workspace, "dx12", test_name, screenshot_name, test_script, level_path, compare_tolerance, camera_name, render_pipeline)
```

**`atom_render_pipeline_list`** 包含用于测试的渲染管道列表。 
可以通过将新的渲染管线附加到此列表来添加新的渲染管线。
```python
    atom_render_pipeline_list = [
        pytest.param("passes/MainRenderPipeline.azasset"),
        pytest.param("passes/LowEndRenderPipeline.azasset")
    ]
```

**`atom_feature_test_list`** 是要运行的测试列表。它包含每个测试的多个属性。
1. 测试名称。
2. 屏幕截图文件名。逗号分隔列表。
3. ScriptAutomation 脚本来控制测试。
4. 要加载的级别。
5. 图像比较灵敏度级别。逗号分隔列表。
6. 测试的 Camera 实体的名称。逗号分隔列表。
 
下面是运行多个屏幕截图的测试示例。
```python
    pytest.param(
        "Atom_AreaLights", # test name
        "sphere_screenshot.png,disk_screenshot.png", # screenshot filename. Comma separated list
        "@gemroot:ScriptAutomation@/Assets/AutomationScripts/GenericRenderScreenshotTest.lua", # ScriptAutomation script
        "levels/atomscreenshottests/feature/arealight/arealight.spawnable", # level to load
        "Level E,Level E", # image comparison tolerance level. Comma separated list
        "TestCameraPointLights,TestCameraDiskLights" # camera names for the screenshots. Comma separated list
    )
```

### 添加新测试
添加新测试有两种主要方法：
1. 添加包含测试的新级别。
2. 在现有关卡中添加一个新的测试区。

选择哪个设置取决于您是否需要使用与现有测试不兼容的设置。这些通常是 Postprocess 或 skybox 设置。

##### 选项 1
使用命名的 camera 实体创建一个新关卡，以显示您要测试的内容。这方面的一个示例是 DepthOfField 测试。该测试的关卡在这里 - o3de/AutomatedTesting/Levels/AtomScreenshotTests/Feature/DepthOfField  
这是测试捕获的内容。
<img src=https://user-images.githubusercontent.com/82187279/236353035-5ddb878a-fd96-4e53-8fed-a7fe062fbd6f.png width=480 height=270>


这是关卡在编辑器中的样子。 
<img src=https://user-images.githubusercontent.com/82187279/236355534-8f3f72ba-9395-4222-852a-6e9a5aa85388.png width=796 height=409>


创建关卡后，需要将其设置添加到 python 单元测试文件中 - o3de/AutomatedTesting/Gem/PythonTests/Atom/TestSuite_Periodic_GPU.py.

##### 选项 2
加载现有测试级别，并在该级别中创建新的测试区域。这方面的一个例子是 AreaLight 测试，其中每个区域测试一种区域光类型，并且区域是间隔的，因此它们不会相互影响。区域光测试的级别在这里 - o3de/AutomatedTesting/Levels/AtomScreenshotTests/Feature/AreaLight

以下是 AreaLight 测试捕获的内容示例。  
<img src=https://user-images.githubusercontent.com/82187279/236353061-52d0205b-5844-4f7f-b4a6-c2ee51bc0d1f.png width=480 height=270>
<img src=https://user-images.githubusercontent.com/82187279/236353085-6cfd7116-9e89-424f-8284-f8455d14d0d5.png width=480 height=270>

这是 AreaLight 测试关卡的外观。 
<img src=https://user-images.githubusercontent.com/82187279/236355515-024a1984-0a87-4d86-ac84-0e19187f09ae.png width=634 height=387>

更新关卡后，将摄像机实体的名称、比较关卡和屏幕截图文件名添加到 python 单元测试文件中的测试信息中 - o3de/AutomatedTesting/Gem/PythonTests/Atom/TestSuite_Periodic_GPU.py.

##### 其他方法
ScriptAutomation 系统还能够在脚本中加载多个关卡。单个 python 测试实例化也可以以这种方式处理多个测试。系统中当前使用的通用脚本不支持此方法，但如果需要，可以使用此方法。
