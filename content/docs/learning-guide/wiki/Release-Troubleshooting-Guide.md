---
title: "Release-Troubleshooting-Guide"
description: ""
toc: false
---

Stepping through a release build's code in an IDE is often necessary when the release build is not behaving the same way as a profile or debug build.

# Prerequisites 
1. You have already created the `.pak` files needed for your game to run
1. You are using a 'project-centric' build.  If you are using 'engine-centric' you will need to provide the appropriate `-DLY_PROJECTS <project path>` path in the CMake configure command.

# Debugging a release game launcher (Windows + Visual Studio)
1. Configure your monolithic release build with the `O3DE_BUILD_WITH_DEBUG_SYMBOLS_RELEASE` flag on so debug symbols are generated.
    ```
    cmake -S . -B build/windows_mono -DO3DE_BUILD_WITH_DEBUG_SYMBOLS_RELEASE=1 -DLY_MONOLITHIC_GAME=1
    ```
1. Open the generated solution file in `build/windows_mono` in Visual Studio, and ensure the `release` build configuration is selected.

    ![image](https://github.com/o3de/o3de/assets/26804013/a00952cb-9706-49cb-9b95-68088f901d00)

1. Set the startup project to `<project-name>.GameLauncher`
1. Right click on `<project-name>.GameLauncher` and select `Properties` to open the Project Properties window.
1. Make sure the active configuration is `Active(release)`, then select `Configuration Properties` > `Debugging` and modify the `Command Arguments` to provide a project path that is the absolute path to where the binaries will go.  Optionally, set a `sys_pakPriority=2` to make sure only `.pak` files are loaded. 
    ```
    --project-path="$(TargetDir)" --sys_pakPriority=2
    ```
    This will ensure that the engine will look in this folder (and `build/windows_mono/release/bin/release/Cache/pc`) for your game .pak files and won't use the `Cache` folder in your project's directory.
1. Copy your `.pak` files into `build/windows_mono/release/bin/release/Cache/pc`

Now you're ready to build, run and attach the debugger to your game launcher.  If you want to set breakpoints in the code you can do that now, or just run the launcher from Visual Studio and pause the program at any time to debug the code.

Use the `Local Windows Debugger` button or `F5` to build and run your game launcher.

**PRO Tip**: Looking for missing .pak files? Set `sys_report_files_not_found_in_paks` to 1 so the launcher outputs a `user\log\PakMissingAssets.log` file that contains a list of missing assets.  This may NOT list all missing assets because 3rd party gems may use their own asset loading code that looks for archives inside you `.pak` files and then opens those proprietary archives and looks for assets inside and that code may not report missing files like the regular engine asset loading code does.
