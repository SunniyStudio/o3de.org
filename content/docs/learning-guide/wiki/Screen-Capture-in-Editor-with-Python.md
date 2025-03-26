---
title: "Screen-Capture-in-Editor-with-Python"
description: ""
toc: false
---

You can take a screenshot in Editor using the Python console.

1. Open the Python console in the Editor and run
```
import azlmbr.bus
import azlmbr.atom
```
2. In the Editor viewport menu set the resolution to `Custom` and enter the width and height you want (e.g. 4k or higher)
3. In the Python console run 
```
azlmbr.atom.FrameCaptureRequestBus(azlmbr.bus.Broadcast, "CaptureScreenshot", "Capture1.png")
```

This will write a PNG file named `Capture1.png` to your project's Cache folder.
