---
title: "Remote-Console-HowTo"
description: ""
toc: false
---

The Open 3D Engine includes a [Python module](https://github.com/o3de/o3de/tree/development/Tools/RemoteConsole/ly_remote_console) for communicating with the client or server runtime while it is running.  This is especially useful for controlling servers and development on external devices like mobile, vr and consoles.

## Use the O3DE Python REPL to connect to a game launcher and issue commands
To use the remote console module in the O3DE Python REPL, 
1. Start up your game client or launcher
1. Open a command window or terminal
1. Navigate to where O3DE is installed
1. Run the O3DE `Python.cmd` to start Python and use the `RemoteConsole` object.
```bash
c:\o3de > python\python.cmd
Python 3.10.5 (tags/v3.10.5-dirty:f377153, Jul 23 2022, 05:17:26) [MSC v.1916 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import ly_remote_console.remote_console_commands as remote_console_commands
>>> console = remote_console_commands.RemoteConsole(addr='127.0.0.1',port=4600)
>>> console.start()
>>> console.send_command('loadlevel defaultlevel')
>>> console.send_command('r_displayinfo 2')
>>> console.stop()
```

The `RemoteConsole` object takes an `addr` parameter which should be the IP address of your client or server launcher.  The default `port` is 4600.
Use the `send_command` message to send commands to the server.
