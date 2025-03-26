---
title: "Xcode"
description: ""
toc: false
---

[Xcode](https://developer.apple.com/xcode/) is an IDE that runs on Mac and is primarily needed for Mac and iOS development.

Before configuring please be sure to install O3DE and create a project by following the guide (https://o3de.org/docs/welcome-guide/create/creating-projects-using-cli/)

# Mac
### 1. Install Xcode from the Apple App Store
* Open the Apple App Store on your Mac, search for Xcode and install it

### 2. Install Command line tools
If Xcode does not prompt you to download and install the command line tools when installing you may need to manually install them.
* In a terminal window run
```
xcode-select --install
```

### 3. Open and configure your project in Xcode
1. Open Xcode and select File > Open and browse to the location of your project's (or engine's) .xcodeproj file, which should be in your `build` folder
2. After Xcode loads the project, change the scheme to Editor using the Product > Scheme > Editor menu
3. Configure the Editor scheme to build a profile build by opening the scheme editor using the Product > Scheme > Edit Scheme... menu
4. Select the `Run` scheme and in the `Info` tab set the `Build Configuration` to `profile`
5. If you are using an engine-centric build, which is common for the `AutomatedTesting` project, in the `Arguments` section set the project path by adding an argument like `--project-path <path to project>`  e.g. `--project-path /Users/username/o3de/AutomatedTesting`

### 4. Build and run the Editor
* With the `Editor` scheme active, press the `Play/Run` button or select Product > Run from the main menu.

## Troubleshooting
#### The Editor crashes on startup
There are many reasons a crash may occur, but common reasons include:
1. Path issues - On a Mac the Editor executable is inside a .app bundle so it may have problems finding code libraries outside the bundle like Editor plugins, or applications like the project manager (o3de.app) or AssetProcessor.app.  If you suspect this is the issue you should see relevant error messages in the debug log window that should help you track down where the problem is and fix the path problems.
2. Asset loading issues - If required assets cannot be loading or found Xcode should stop when the assert or exception is thrown and you should be able to use the debug windows to determine the missing asset and why it wasn't found or loading.  If the AssetProcessor crashed it could be the assets were never generated and you may need to run the AssetProcessor again to continue processing the assets.
3. Graphics related crashes - If there are issues with your device's GPU Xcode will usually stop on an assert in the Atom RHI code with a validation error or memory access error.  These issues are harder to troubleshoot and will generally require code fixes related to the Metal RHI.
4. Metal validation asserts - If the asserts are thrown when running the Editor regarding Metal validation, if you don't know how to fix the issue, you can turn off Metal validation by editing the active schema in Xcode and turn off `API Validation` under `Diagnostics` > `Metal`.  You may also need to turn set `Options` > `GPU Frame Capture` to `Disabled`
