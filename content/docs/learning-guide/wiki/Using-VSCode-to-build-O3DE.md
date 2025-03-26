---
title: "Using-VSCode-to-build-O3DE"
description: ""
toc: false
---

[Visual Studio Code (VS Code)](https://code.visualstudio.com/) is an [open source](https://github.com/Microsoft/vscode/) IDE that runs on Windows, Linux and Mac.  By activating extensions you can set it up as a powerful editor and debugger for O3DE.

Before configuring please be sure to install O3DE and create a project by following the guide (https://o3de.org/docs/welcome-guide/create/creating-projects-using-cli/)

## Platform Specific Instructions
* [Linux](#linux) 
* [Mac](#mac) 
* [Windows](#windows)
* [Android](#android)
* [Troubleshooting](#troubleshooting)

# Linux

### 1. Install recommended extensions
* C/C++ Extension Pack (by Microsoft) includes:
    * C/C++ (by Microsoft)
    * C/C++ Themes (by Microsoft)
    * CMake Tools (by Microsoft)
* CMake (by twxs)
* CodeLLDB (by Vadim Chugunov)
* Python (by Microsoft)

### 2. Create a `.vscode/settings.json` configuration file for your project

If you use the CMake extension build commands you can customize some of the settings, for example, the build directory, and the path to use for 3rdparty packages and path to cmake.  In this example I'm providing the path to the Snap cmake binary.

```json
{
    "cmake.buildDirectory": "${workspaceFolder}/build/linux_ninja",
    "cmake.cmakePath": "/snap/cmake/current/bin/cmake",
    "cmake.configureSettings": {
        "LY_3RDPARTY_PATH": "~/o3de-packages"
    }
}
```

If you're using the engine folder as your CMake source folder you can specify the `LY_PROJECTS` path so CMake uses the correct project and you can also provide debug args for the executable you're running/debugging like so:
```json
    "cmake.configureSettings": {
        "LY_PROJECTS": "AutomatedTesting"
    },
    "cmake.debugConfig": {
        "args": [
            "--project-path=~/o3de/AutomatedTesting"
        ],
        "MIMode": "lldb",
        "miDebuggerPath": "",
        "cwd": "${workspaceFolder}"
    }
```

### 3. Create a `<project root>/.vscode/launch.json` configuration file for your project

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "lldb",
            "request": "launch",
            "name": "Editor",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/Editor",
            "args": ["--project-path=AutomatedTesting"],
            "cwd": "${workspaceFolder}"
        },
        {
            "type": "lldb",
            "request": "launch",
            "name": "MaterialEditor",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/MaterialEditor",
            "args": ["--project-path=AutomatedTesting"],
            "cwd": "${workspaceFolder}"
        },
        {
            "type": "cppdbg",
            "request": "launch",
            "name": "SerializeContextTools",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/SerializeContextTools",
            "args": ["dumptypes", "--project-path=AutomatedTesting"],
            "cwd": "${workspaceFolder}",
            "visualizerFile": "${workspaceFolder}/Code/Framework/AzCore/Platform/Common/VisualStudio/AzCore/Natvis/azcore.natvis"
        },
        {
            "type": "lldb",
            "request": "launch",
            "name": "EditorCore.Tests",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/AzTestRunner",
            "args": ["libEditorCore.Tests", "AzRunUnitTests", "--gtest_filter=*"],
            "cwd": "${workspaceFolder}"
        },
        {
            "type": "lldb",
            "request": "launch",
            "name": "AzCore.Tests",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/AzTestRunner",
            "args": ["libAzCore.Tests", "AzRunUnitTests", "--gtest_filter=*", "--gtest_repeat=1"],
            "cwd": "${workspaceFolder}"
        },
        {
            "type": "lldb",
            "request": "launch",
            "name": "AzFramework.Tests",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/AzTestRunner",
            "args": ["libAzFramework.Tests", "AzRunUnitTests", "--gtest_filter=*"],
            "cwd": "${workspaceFolder}"
        },
        {
            "type": "lldb",
            "request": "launch",
            "name": "AzToolsFramework.Tests",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/AzTestRunner",
            "args": ["libAzToolsFramework.Tests", "AzRunUnitTests", "--gtest_filter=*"],
            "cwd": "${workspaceFolder}"
        },
        {
            "type": "lldb",
            "request": "launch",
            "name": "AssetProcessor.Tests",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/AzTestRunner",
            "args": ["libAssetProcessor.Tests", "AzRunUnitTests", "--gtest_filter=*"],
            "cwd": "${workspaceFolder}"
        },
        {
            "type": "lldb",
            "request": "launch",
            "name": "AWSMetrics.Tests",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/AzTestRunner",
            "args": ["libAWSMetrics.Tests", "AzRunUnitTests", "--gtest_filter=*"],
            "cwd": "${workspaceFolder}"
        },
        {
            "type": "lldb",
            "request": "launch",
            "name": "EMotionFX.Tests",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/AzTestRunner",
            "args": ["libEMotionFX.Tests", "AzRunUnitTests", "--gtest_filter=*"],
            "cwd": "${workspaceFolder}"
        },
        {
            "type": "lldb",
            "request": "launch",
            "name": "ScriptCanvasTesting.Editor.Tests",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/AzTestRunner",
            "args": ["libScriptCanvasTesting.Editor.Tests", "AzRunUnitTests", "--gtest_filter=*"],
            "cwd": "${workspaceFolder}"
        }
    ]
}
```

### 4. Open your project or engine folder in Visual Studio Code

In Visual Studio Code, use the `File > Open Folder` menu option and browse to the location of your project or engine, and press `Select Folder`.

Alternately, run the following command from a terminal, replacing `<project or engine path>` with the path to your project or engine.
```bash
code <project or engine path>
```

The CMake extension may automatically detect your settings and prompt you to configure the project.

CMake Presets can be used to configure your project for Linux or Android (if the NDK is installed).
![image](https://github.com/o3de/o3de/assets/56135373/c1a5300e-4a82-4770-ac3e-c53ba2bce129)


### 5. Build and run your project

The CMake build presets can be used to build specific set of targets within VS Code
Alternatively the CMake extension can build individual targets

# Mac

The Mac instructions are similar to [Linux](#linux)

### 1. Install recommended extensions
* C/C++ Extension Pack (by Microsoft) includes:
    * C/C++ (by Microsoft)
    * C/C++ Themes (by Microsoft)
    * CMake Tools (by Microsoft)
* CMake (by twxs)
* CodeLLDB (by Vadim Chugunov)
* Python (by Microsoft)

### 2. Type `Command+Shift+P` and type launch.json.  
       That will open the launch.json file which can be used to configure the C++ and python applications to launch

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Editor (cppdbg)",
            "type": "cppdbg",
            "request": "launch",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/Editor",
            "args": ["--project-path=AutomatedTesting"],
            "cwd": "${workspaceFolder}",
            "visualizerFile": "${workspaceFolder}/Code/Framework/AzCore/Platform/Common/VisualStudio/AzCore/Natvis/azcore.natvis"
        },
        {
            "name": "AzCore.Tests (cppdbg)",
            "type": "cppdbg",
            "request": "launch",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/AzTestRunner",
            "args": ["libAzCore.Tests.dylib", "AzRunUnitTests", "--gtest_filter=*", "--gtest_repeat=1"],
            "cwd": "${workspaceFolder}",
            "MIMode": "lldb",
            "visualizerFile": "${workspaceFolder}/Code/Framework/AzCore/Platform/Common/VisualStudio/AzCore/Natvis/azcore.natvis"
        },
        {
            "name": "SerializeContextTools (cppdbg)",
            "type": "cppdbg",
            "request": "launch",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/SerializeContextTools",
            "args": ["dumptypes"],
            "cwd": "${workspaceFolder}",
            "MIMode": "lldb",
            "visualizerFile": "${workspaceFolder}/Code/Framework/AzCore/Platform/Common/VisualStudio/AzCore/Natvis/azcore.natvis"
        },
        {
            "name": "Archive.Editor.Tests (cppdbg)",
            "type": "cppdbg",
            "request": "launch",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/AzTestRunner",
            "args": ["libArchive.Editor.Tests.dylib", "AzRunUnitTests", "--gtest_filter=*"],
            "cwd": "${workspaceFolder}",
            "MIMode": "lldb",
            "visualizerFile": "${workspaceFolder}/Code/Framework/AzCore/Platform/Common/VisualStudio/AzCore/Natvis/azcore.natvis"
        },
        {
            "type": "python",
            "name": "Pytest: Debug Current File",
            "request": "launch",
            "module": "pytest",
            "args": ["${file}"]
        },
        {
            "type": "python",
            "name": "o3de.py: Debug Create Gem",
            "request": "launch",
            "program": "${workspaceFolder}/scripts/o3de.py",
            "args": ["create-gem", "-gp", "Gems/Compression", "-kl", "-f"]
        }
    ]
}
```

### 3. Open your project's folder in Visual Studio Code

In Visual Studio Code, use the `File > Open Folder` menu option and browse to the location of your project, and press `Select Folder`.

The CMake extension may automatically detect your settings and prompt you to configure the project.

On Mac, the CMake Presets supports Android, iOS and MacOS for configuring, building and testing o3de.
<img width="597" alt="Screen Shot 2023-06-14 at 12 36 23 PM" src="https://github.com/o3de/o3de/assets/56135373/558dec68-717b-43c2-929c-028d75d3ae0b">


### 4. Build and run your project

The CMake build presets can be used to build specific set of targets within VS Code.
Alternatively the CMake extension can build individual targets
<img width="1680" alt="Screen Shot 2023-06-14 at 12 32 19 PM" src="https://github.com/o3de/o3de/assets/56135373/b6713e65-edd2-4d95-a44f-2d580367faae">


# Windows

### 1. Install recommended extensions
* C/C++ Extension Pack (by Microsoft) includes:
    * C/C++ (by Microsoft)
    * C/C++ Themes (by Microsoft)
    * CMake Tools (by Microsoft)
* CMake (by twxs)
* Python (by Microsoft)

### 2. Create a `.vscode/settings.json` configuration file for your project

Create or update `<project root>/.vscode/settings.json` and customize the below settings.
```json
{
    "cmake.buildDirectory": "${workspaceFolder}/build/windows",
    "cmake.parallelJobs": 5,
    "cmake.configureSettings": {
        "LY_3RDPARTY_PATH": "C:\\o3de-packages"
    },
    "cmake.defaultVariants": {
        "buildType": {
            "default": "profile",
            "description": "The build type.",
            "choices": {
                "debug": {
                    "short": "Debug",
                    "long": "Disable optimizations - include debug information.",
                    "buildType": "Debug"
                },
                "release": {
                    "short": "Release",
                    "long": "Optimize for speed - exclude debug information.",
                    "buildType": "Release"
                },
                "profile": {
                    "short": "Profile",
                    "long": "Optimize for debug and run.",
                    "buildType": "Debug"
                }
            }
        }
    }
}
```
The cmake.parallelJobs is optional.

### 3. Create a `<project root>/.vscode/launch.json` configuration file for your project

Create or update `<project root>/.vscode/launch.json` and customize the below settings. Replace `MyProject` with the name of your project.
```json
{
    "version": "0.2.0",
    "configurations": [
         {
            "name": "Launcher",
            "type": "cppvsdbg",
            "request": "launch",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/MyProject.GameLauncher.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${fileDirname}",
            "environment": [],
            "console": "externalTerminal"
        },
        {
            "name": "Editor",
            "type": "cppvsdbg",
            "request": "launch",
            "program": "c:/O3DE/23.05.0/bin/Windows/profile/Default/Editor.exe",
            "args": [
                 "--project-path",
                 "${workspaceFolder}"
             ],
            "stopAtEntry": false,
            "cwd": "${fileDirname}",
            "environment": [],
            "console": "externalTerminal"
        },
        {
            "name": "(MSVC) Attach",
            "type": "cppvsdbg",
            "request": "attach",
            "processId": "",
            "visualizerFile": "${workspaceFolder}/Code/Framework/AzCore/Platform/Common/VisualStudio/AzCore/Natvis/azcore.natvis"
        },
        {
            "name": "Python: AutomatedTesting_DynamicVegetationTest_Main_Optimized",
            "type": "python",
            "request": "launch",
            "justMyCode": true,
            "python": "${command:python.interpreterPath}",
            "module": "pytest",
            "cwd": "${command:cmake.buildDirectory}",
            "args": [
                "-v",
                "--show-capture=stdout",
                "-c",
                "${workspaceFolder}/pytest.ini",
                "--build-directory",
                "${command:cmake.getLaunchTargetDirectory}",
                "--output-path",
                "${command:cmake.buildDirectory}/Testing/LyTestTools/AutomatedTesting_DynamicVegetationTests_Main_Optimized",
                "${workspaceFolder}/AutomatedTesting/Gem/PythonTests/largeworlds/dyn_veg/TestSuite_Main.py::TestAutomation::test_PrefabInstanceSpawner_External_E2E_Editor[windows-AutomatedTesting-windows_editor]"
            ]
        },
        {
            "name": "(Windows) Launch AzCore.Tests",
            "type": "cppvsdbg",
            "request": "launch",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/AzTestRunner.exe",
            "args": [
                "AzCore.Tests.dll",
                "AzRunUnitTests",
                "--gtest_filter=TimeTests.QueryCPUThreadTime_ReturnsNonZero"
            ],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "visualizerFile": "${workspaceFolder}/Code/Framework/AzCore/Platform/Common/VisualStudio/AzCore/Natvis/azcore.natvis"
        },
        {
            "name": "(Windows) Launch AzFramework.Tests",
            "type": "cppvsdbg",
            "request": "launch",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/AzTestRunner.exe",
            "args": [
                "AzFramework.Tests.dll",
                "AzRunUnitTests",
                "--gtest_filter=*",
            ],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "visualizerFile": "${workspaceFolder}/Code/Framework/AzCore/Platform/Common/VisualStudio/AzCore/Natvis/azcore.natvis"
        },
        {
            "name": "(Windows) Launch AzToolsFramework.Tests",
            "type": "cppvsdbg",
            "request": "launch",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/AzTestRunner.exe",
            "args": [
                "AzToolsFramework.Tests.dll",
                "AzRunUnitTests",
                "--gtest_filter=*",
            ],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "visualizerFile": "${workspaceFolder}/Code/Framework/AzCore/Platform/Common/VisualStudio/AzCore/Natvis/azcore.natvis"
        },
        {
            "name": "(Windows) Launch AssetBundler.Tests",
            "type": "cppvsdbg",
            "request": "launch",
            "program": "${command:cmake.buildDirectory}/bin/${command:cmake.buildType}/AssetBundler.Tests.exe",
            "args": [
                "--unittest",
                "--gtest_filter=*"
            ],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "visualizerFile": "${workspaceFolder}/Code/Framework/AzCore/Platform/Common/VisualStudio/AzCore/Natvis/azcore.natvis"
        }
    ]
}
```

### 4. Open your project's folder in Visual Studio Code

In Visual Studio Code, use the `File > Open Folder` menu option and browse to the location of your project, and press `Select Folder`

The CMake extension may automatically detect your settings and prompt you to configure the project.

### 5. Build and run your project

First, use the CMake extension to configure your project if it didn't already.

To build properly you need to select a proper target by clicking the target name in the status bar (next to Build button.) 
Make it `Editor`.

Finally, to launch, open the `Run and Debug` panel, select the `Editor` entry from the drop down and press the `Play` button.

# Android

The instructions on the [Generating Android project on Windows](https://docs.o3de.org/docs/user-guide/platforms/android/generating_android_project_windows/) can be used to setup the first 3 steps below

### 1. Install Android Studio and make sure the JAVA_HOME environment variable is set your installed JRE directory
* Use the Android Studio SDK Manager to install the NDK 

### 2. Install the following tools to a local location on your machine
 * [Gradle](https://gradle.org/) - This the Android Studio build tool
 * [Ninja](https://ninja-build.org/) - Build system used by the CMake to build the native C++ code (NDK)

### 3. (Optional) Generate an Android KeyStore file to allow APK signing

If on Windows local CMD variables can be setup to configure the keystore signing options
Adjust variable setting for use with `sh` on Linux/Mac if attempting to generate an android project on those platforms
```
SET O3DE_ENGINE_PATH=<o3de-engine-root>
SET O3DE_PROJECT_PATH=<your-project-root>
SET O3DE_ANDROID_KEYSTORE_FOLDER=<persistent-folder-for-keystore-file>
SET O3DE_ANDROID_SIGNCONFIG_FILE=%O3DE_ANDROID_KEYSTORE_FOLDER%\o3de.keystore
SET O3DE_ANDROID_SIGNCONFIG_STORE_PASSWORD=o3de_android
SET O3DE_ANDROID_SIGNCONFIG_KEY_ALIAS=o3de_key
SET O3DE_ANDROID_SIGNCONFIG_KEY_PASSWORD=o3de_android
SET O3DE_ANDROID_SIGNCONFIG_KEY_SIZE=2048
SET O3DE_ANDROID_SIGNCONFIG_VALIDITY_DAYS=10000
SET O3DE_ANDROID_DN="cn=atom-sample-viewer, ou=o3de, o=LF, c=US"
```

Afterwards run the Java `keytool` command to generate a KeyStore file.  
It should be located in the `<JAVA-HOME>/bin` directory
```
keytool -genkey -keystore %O3DE_ANDROID_SIGNCONFIG_FILE% -storepass %O3DE_ANDROID_SIGNCONFIG_STORE_PASSWORD% -alias %O3DE_ANDROID_SIGNCONFIG_KEY_ALIAS% -keypass %O3DE_ANDROID_SIGNCONFIG_KEY_PASSWORD% -keyalg RSA -keysize %O3DE_ANDROID_SIGNCONFIG_KEY_SIZE% -validity %O3DE_ANDROID_SIGNCONFIG_VALIDITY_DAYS% -dname %O3DE_ANDROID_DN%
```

### 4. Install recommended extensions
* Python (by Microsoft)

### 5. Update the `<project root>/.vscode/launch.json` configuration file for your project
by adding an entry that runs the `generate_android_project.py` script.  
The "generate_android_project" script is responsible for generating a gradle project that can build native C++ code for android, but also copy the O3DE processed assets to an APK. 
The gradle project can be opened in Android Studio
```json
 {
            "name": "Python: Generate Android Project",
            "type": "python",
            "request": "launch",
            "justMyCode": true,
            "python": "${command:python.interpreterPath}",
            "program": "${workspaceFolder}/cmake/Tools/Platform/Android/generate_android_project.py",
            "cwd": "${workspaceFolder}",
            "args": [
                "--project-path=<your-project-root>",
                "--engine-root=${workspaceFolder}",
                "--ninja-install-path=<path-to-your-ninja-build-folder",
                "--gradle-install-path=<path-to-your-gradle-bin-folder>",
                "--build-dir=${workspaceFolder}/build/android_gradle",
                "--android-sdk-path=<android-sdk-path>",
                "--android-ndk-version=25*",
                "--include-apk-assets",
                "--asset-mode=<LOOSE|PAK|VFS>",
                "--asset-type=android",
                "--signconfig-store-file=<O3DE_ANDROID_SIGNCONFIG_FILE>",
                "--signconfig-store-password=<O3DE_ANDROID_SIGNCONFIG_STORE_PASSWORD>",
                "--signconfig-key-alias=<O3DE_ANDROID_SIGNCONFIG_KEY_ALIAS>",
                "--signconfig-key-password=<O3DE_ANDROID_SIGNCONFIG_KEY_PASSWORD>"
            ]
        }
```

### 6. Run the "Python: Generate Android Project" command through VS Code

The "Python: Generate Android Project" should complete successfully and generate folder containing a `build.gradle` in it that can be used to build O3DE for Android

## Building an APK for Android

To build an APK for android the steps listed in the [Generating Android Project on Windows](https://docs.o3de.org/docs/user-guide/platforms/android/generating_android_project_windows/#generating-android-projects-on-windows) page can followed

In short to build an APK for the O3DE supported build configuations can be done as follows
```bash
cd ${workspaceFolder}/build/android_gradle
### To build profile config, run the followinng
gradlew assembleProfile
### To build debug config, run the following
gradlew assembleDebug
### To build release config, run the following
gradlew assembleRelease
```
# Troubleshooting

## **Issue**: Memory runs out on Linux when attempting to run the with or without Debugging.
O3DE debug symbols are quite large. This is due to linking in the AzCore/AzFramework to most Client Gem modules, along with AzToolsFramework in most Tool and Builder Gem modules.
On Linux the debug symbols have been detaced from the DSO `lib<name>.so` file that builds into a `lib<name>.so.dbg` file.  

Furthermore this issue is exacerbated by a limitation in VS Code, that even when using the "Run-> Run Without Debugging" option, it still launches the debugger and attempts to load the debug symbols.  
![image](https://github.com/o3de/o3de/assets/56135373/7e11182a-8ca2-4e7e-afa0-8f4da35879b8)  
See the VSCode C++ tools extension Github Issue at https://github.com/microsoft/vscode-cpptools/issues/1201 for more information.

**Workaround**: Because O3DE detaches the debug symbols from the shared libraries and executable files on Linux into `<lib-or-exe-name>.dbg` files, the debug symbols for libraries files can be skipped from loading by renaming those files.  

![image](https://github.com/o3de/o3de/assets/56135373/8a9c3f82-50fa-44c2-828e-f34ca3483442)  
For example renaming or moving `libO3DEKernel.so.dbg` to a name such as `libO3DEKernel.so.dbg.bak` will prevent the lldb/gdb debugger from attempting to load those debug symbols when the `libO3DEKernel.so` file is loaded by those debuggers.

This can be used to reduce the memory footprint of the debugger and allow O3DE applications such as the Editor, AssetProcessor, GameLaunchers, etc... to be debugged without running out of OS memory.

Below is a quick GNU `find` command that can be adjusted for users needs to rename the Debug symbol `.dbg` files that the user desires to NOT debug

### Ex. Backup all `.dbg` files except for the EditorLib and ROS2 Gems debug symbols
This allows only loading the debug symbols for the `libEditorLib.so` and `libROS2.Editor.so` DSOs which reduces the memory footprint
```bash
find . -name "*.dbg" -not -name "libROS2*.so.dbg" -and -not -name "libEditorLib.so.dbg" -exec bash -c 'bak=${0}; bak+=.bak; mv ${0} ${bak}' {} \;
```

To rename the `.dbg.bak` files back to their original location, the following GNU find command can be used.
```bash
find . -name "*.dbg.bak" -exec bash -c 'orig=${0//.bak}; mv ${0} ${orig}' {} \;
```

## **Issue**: When using the `cppvsdbg` type for debugging using the MSVC debugger, long string values are truncated in the debugger.  
There is a currently an Open issue with the [vscode-cpptools](https://github.com/Microsoft/vscode-cpptools/issues/1786) about strings being truncated when outputing to the debug console/variables window with no mitigation for the problem.  
There is currently no reliable workaround at the time.

**Workaround**: None yet
