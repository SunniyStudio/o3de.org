---
title: "[Atom]-Screen-Capture-Image-Comparison-testing"
description: ""
toc: false
---

O3DE has the ability, via the script automation gem, to script loading a level, capture the rendered output, and then compare the captured image to a previously captured known "good image" to validate rendering is working correctly. This capability has been exposed by a new unit-test in the smoke unit-test suite.
This article describes how the test works and how to add additional screenshot tests.

### Unit Test Settings
The unit test functionality is implemented in python in - o3de/AutomatedTesting/Gem/PythonTests/Atom/TestSuite_Periodic_GPU.py.

The file declares 2 unit test sets: one for DX12 and one for Vulkan. The DX12 tests are skipped on non-Windows platforms. Additionally, the unit tests for both sets are parametrized on a list of test levels (and associated settings), and a list of Render Pipelines.

Note: each test is a separate launch of the AutomatedTesting.GameLauncher. Given the current cost to launch the GameLauncher (approximately 30 seconds) and each test being run multiple times (number of api's tested * number of render pipelines) the total time for the test can scale rapidly.

The unit tests create a way to describe a set of tests that are going to be executed, and make it easy to do so. Let's look at the DX12 setup as an example. 

Below, the SUITE, project, and launcher_platform marks can be ignored as boilerplate. These tests require a GPU. The mark Requires_gpu is necessary to notify Jenkins a GPU is needed. 
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

**`atom_render_pipeline_list`** contains the list of render pipelines to use for the tests.  
A new render pipeline can be added by appending it to this list.
```python
    atom_render_pipeline_list = [
        pytest.param("passes/MainRenderPipeline.azasset"),
        pytest.param("passes/LowEndRenderPipeline.azasset")
    ]
```

**`atom_feature_test_list`** is the list of tests to run through. It contains several atrtributes per test.
1. Test name.
2. Screenshot filename. Comma separated list.
3. ScriptAutomation script to control the test.
4. The level to load.
5. Image comparison sensitivity level. Comma separated list.
6. Name of the Camera entity for the test. Comma separated list.
 
Here's an example of a test that runs multiple screenshots.
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

### Adding A New Test
There are two main ways to add a new test:
1. Add a new level that contains the test.
2. Add a new test zone inside an existing level.

Which you chose will depend on if you need to use a setting that is incompatible with an existing test. These are usually postprocess or skybox settings.

##### Option 1 
Make a new level with a named camera entity that shows what you want to test. An example of this is the DepthOfField test. The level for that test is here - o3de/AutomatedTesting/Levels/AtomScreenshotTests/Feature/DepthOfField  
This is what the test captures.  
<img src=https://user-images.githubusercontent.com/82187279/236353035-5ddb878a-fd96-4e53-8fed-a7fe062fbd6f.png width=480 height=270>


This is what the level looks like in the editor.  
<img src=https://user-images.githubusercontent.com/82187279/236355534-8f3f72ba-9395-4222-852a-6e9a5aa85388.png width=796 height=409>


Once the level is created, its settings need to be added to the python unit test file - o3de/AutomatedTesting/Gem/PythonTests/Atom/TestSuite_Periodic_GPU.py.

##### Option 2
Load an existing test level and make a new test zone within the level. An example of this is the AreaLight test, where each zone tests an area light type and the zones are spaced so they don't affect each other. The level for the area light tests is here - o3de/AutomatedTesting/Levels/AtomScreenshotTests/Feature/AreaLight

These are examples of what the AreaLight test captures.  
<img src=https://user-images.githubusercontent.com/82187279/236353061-52d0205b-5844-4f7f-b4a6-c2ee51bc0d1f.png width=480 height=270>
<img src=https://user-images.githubusercontent.com/82187279/236353085-6cfd7116-9e89-424f-8284-f8455d14d0d5.png width=480 height=270>

This is what the AreaLight test level looks like.  
<img src=https://user-images.githubusercontent.com/82187279/236355515-024a1984-0a87-4d86-ac84-0e19187f09ae.png width=634 height=387>

Once the level is updated, add the name of the camera entity, the comparison level, and the screenshot file name to the test info in the python unit test file - o3de/AutomatedTesting/Gem/PythonTests/Atom/TestSuite_Periodic_GPU.py.

##### Other Approaches
The ScriptAutomation system is also capable of loading multiple levels within a script. A single python test instantiation can handle multiple tests that way as well. The generic scripts currently used in the system don't support this approach, but it is available to use if needed.
