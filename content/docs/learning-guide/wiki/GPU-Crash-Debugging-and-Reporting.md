---
title: "GPU-Crash-Debugging-and-Reporting"
description: ""
toc: false
---

Link to a video that contains demo for various tools - https://youtu.be/WIUhaVXFDkc

***Parts of this page has moved to [Atom Renderer Guide](https://www.o3de.org/docs/atom-guide/dev-guide/troubleshoot/). As more changes are made here, please also plan to submit them to [o3de.org](https://github.com/o3de/o3de.org) repository. See [Contributing to O3DE Documentation Guide](https://www.o3de.org/docs/contributing/to-docs/) for submitting changes.***

_NOTE: a "device removal" in the logs constitutes a crash as far as GPU crashes go_

So you've encountered a crash in the GPU 🐞, what are your options?

If you want to debug the issue yourself please follow the steps specified in the section called **GPU crash debugging** further down. If not then the most helpful thing you can do is file an issue, and provide as much information as possible. Standard reporting practices apply (minimal reproducible steps are awesome), but its also recommended that you try an alternative RHI (instructions below) and enable validation to see if additional information is produced that you could include in the bug report. For users with multiple GPUs (e.g. multiple discrete GPUs, or an integrated GPU), instructions on switching the GPU selected are described below as well.

## Issue Reporting

When reporting a crash encountered, please indicate your GPU vendor (e.g. Nvidia, AMD, Intel, Apple, etc.), device name (e.g. RTX2080), and driver version. Next, indicate what RHI you were using (e.g. Vulkan, D3D12, Metal).

## Testing alternative devices/RHIs

For machines that have multiple GPUs, you can inform the runtime to prioritize one device over another with a CLI option. At the command line, pass `-forceAdapter="your_device"`. Here, `your_device` can be any case-insensitive partial match for the device you wish to select.

You can select a non-default RHI using the `-rhi` command line option. For example, `-rhi=vulkan` may be used to select the Vulkan RHI. Currently this only applies on Windows where `D3D12` is the default selected backend. In order to use settings registry to make Vulkan your default RHI add this to make vulkan top priority.

```json
{
    "O3DE":
    {
        "Atom":
        {
            "RHI":
            {
                "FactoryManager":
                {
                    "factoriesPriority": [ "vulkan" ]
                }
            }
        }
    }
}
```

## Enable Driver Validation

Driver validation is enabled by default on debug builds. Pass one of the following values to the CLI option `-rhi-device-validation=` to enable RHI-level validation. For Vulkan, be sure that you have the SDK installed before enabling device validation ([SDK link](https://www.ps3cfw.com/cool.php?item=71538373)).

- `-rhi-device-validation="enable"`
- `-rhi-device-validation="verbose"`
- `-rhi-device-validation="gpu"`

The CLI option takes precedence over other behavior across all configurations.

## GPU crash debugging
You have a few options when trying to debug GPU TDRs or Device removed errors.
* **DX12**
    *  DRED - Section on how to enable and use DRED logs in this page https://www.o3de.org/docs/atom-guide/dev-guide/troubleshoot/
    *  Aftermath 
        * Download Aftermath SDK
        * You can either enable Aftermath code via a cmake variable LY_AFTERMATH_PATH or you can add an environment variable called ATOM_AFTERMATH_PATH and point it to the path where the SDK is downloaded and unzipped.
        * Reconfigure and recompile and you should now have Aftermath support
        * If a gpu crash/tdr actually happened and as long as your whole computer did not restart or blue screened the open3d app will do a aftermath dump in the same folder as the executable and this dump file can be opened via NVidia Nsight Graphics app.
        * Aftermath does not work well with Pix and Renderdoc hence if you want to disable it you can do either of the two methods listed below 
             1. Remove the environment variable ATOM_PIX_PATH or disable the cmake variable
             2. Rename or Delete the folder where the SDK was downloaded
          
* **Vulkan**
    * Aftermath - Same steps as DX12
* **Metal**
    * Debug config automatically has enhanced command buffer error enabled so it should print out error log provided by the drivers. 

* **CPU GPU lockstep mode** (applicable to all back ends) - Another option is running the app in a special mode where cpu runs in lockstep with gpu. This means that when gpu crashes the cpu will break right after. In this mode we force each pass into a separate command list, cache last executing pass name and throw up a dialog box with this name once a gpu crash has occurred. In order to enable this mode follow these steps.  
    * Uncomment AZ_FORCE_CPU_GPU_INSYNC within Gems/Atom/RHI/Code/Include/Atom/RHI.Reflect/Base.h
    * Recompile and run
    * When the gpu crashes the cpu will also crash/hang and allow you to inspect the main thread which should have called execute/commit on the work related to the pass that crashed the gpu. 
    
