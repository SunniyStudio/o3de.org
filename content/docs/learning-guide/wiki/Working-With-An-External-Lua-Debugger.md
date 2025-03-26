---
title: "Working-With-An-External-Lua-Debugger"
description: ""
toc: false
---

O3DE developers can choose which IDE to use when editing/debugging Lua scripts.  

The default behavior of the **Editor**, when the user clicks the `Open in Lua Editor`(
![image](https://user-images.githubusercontent.com/66021303/232074723-d2d57322-2294-45cc-878b-45bc4e440b31.png)
) button of a **Lua Script** component is to open the Lua script with **LuaIDE**.  
  
To override this default behavior, the user can define the registry key `O3DE/Lua/Debugger/Uri` with a URL representing an application capable of editing and debugging Lua scripts. For example, if you wish to use this Visual Studio Code debugger extension for Lua: https://marketplace.visualstudio.com/items?itemName=lumbermixalot.o3de-lua-debug, you can do something like this in a `*.setreg` file:  
```
{
    "O3DE": {
        "Lua": {
            "Debugger" : {
                "Uri": "vscode://lumbermixalot.o3de-lua-debug/debug?" 
            }
        }
    }
}
```

### For developers of Lua Editor/Debuggers:    
The `Editor` will execute the following URL Query as a system call to open a Lua script in an external debugger:
```  
<App URI>?projectPath=<Game project path>&enginePath=<O3DE path>&files[]=<Lua Script path>
```
* `<App URI>`: The value of `O3DE/Lua/Debugger/Uri`, e.g. `vscode://lumbermixalot.o3de-lua-debug/debug?`.  
* `<Game project path>`: Absolute path to the current game project, e.g. `C:\GIT\MyGame`.
* `<O3DE path>`: Absolute path to the root of O3DE, e.g. `C:\GIT\o3de`.  
* `<Lua Script path>`: Absolute path to the Lua script that needs to be edited/debugged, e.g. `C:\GIT\MyGame\Assets\score_counter.lua`.  

