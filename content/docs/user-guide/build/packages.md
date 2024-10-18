---
title: O3DE 软件包
description: 使用 CMake 构建系统，将依赖项作为软件包集成到 Open 3D Engine (O3DE) Gem 或项目中。
weight: 300
---

**Open 3D Engine (O3DE)** 提供了一个打包系统，允许发送预编译库或其他外部源。有了打包系统，您可以更轻松地添加外部二进制文件作为您的 Gem 或项目的依赖项，而无需在源控制中维护它们。如果你计划添加一个不能以源代码形式发布的静态依赖项，除了获取资源外，打包系统还提供一致性检查和目标生成功能。

{{< note >}}
定义软件包系统的 CMake 文件位于 O3DE 源代码中的 `cmake/3rdParty.cmake` 和 `cmake/3rdPartyPackages.cmake`。
{{< /note >}}

## 概述

O3DE 软件包系统用于从外部来源提供源代码和二进制依赖项。Gem、项目甚至 O3DE 引擎本身都需要这些依赖包。依赖系统能正确检测依赖和版本要求，确保在编译时系统上有所需的软件包。

在调用时，依赖系统会执行以下步骤：

1. 它会在 `LY_3RDPARTY_PATH` 值处检查软件包的有效位置。任何目录都可以用来存储软件包，但建议在设置了 `LY_3RDPARTY_PATH` 值之后避免更改。在 O3DE 编译系统将内容下载到一个目录后，会在该目录下创建一个缓存文件，以方便将来的依赖性检查，因此更改软件包位置会重新下载所有软件包。

1. 联编系统会加载可用的软件包源。这是一个以分号（`;`）分隔的软件包源列表，存储在 `LY_PACKAGE_SERVER_URLS` CMake 缓存值中。如果设置了 `LY_PACKAGE_SERVER_URLS` 环境变量，则会将其**加入**缓存值。

1. 对于每一个软件包：
 
   1. 检查 `LY_PACKAGE_SERVER_URLS` 中的下一个源。如果在源代码中找到匹配的软件包，则将该软件包下载到 `LY_PACKAGE_DOWNLOAD_CACHE_LOCATION`。
    
   1. 从源代码下载软件包压缩包，并与请求 CMake 文件中包含的校验和进行核对。如果校验和不匹配，则删除压缩包，并检查下一个可用的软件包源。
   
   1. 软件包被解压到`LY_PACKAGE_UNPACK_LOCATION/<full-package-name>`中。默认情况下，`LY_PACKAGE_UNPACK_LOCATION` 是 `LY_3RDPARTY_PATH/packages`。
   
   1. 软件包中的每个文件都要与软件包中的校验和文件进行核对。如果校验失败，软件包压缩包和解压缩内容将被删除，然后检查下一个软件包源。

您还可以添加 Python 模块需求，这样 `pip` 就会自动检索工具或其他库所需的软件包。

{{< note >}}
软件包下载和目标创建仅作为 CMake 配置和生成的一部分运行。如果 Gem 或项目中添加了新的依赖关系，则需要重新配置 CMake 缓存并重新生成。
{{< /note >}}

有关影响软件包系统的可用设置的完整列表，请参阅 [CMake 设置参考](reference/)。

## 创建软件包

要创建一个供 O3DE 使用的软件包，您需要提供以下内容：

* 软件包内容本身 - 源代码、二进制文件、资产和任何其他组件。
* 描述软件包的清单（`PackageInfo.json`）。
* 包含软件包软件许可证的 `LICENSE` 文件。
* 软件包中每个文件的校验和（`SHA256SUMS`）。
* 当软件包在另一个 CMake 文件中使用时运行的 CMake 文件（`Find<Package>.cmake`）。

软件包必须以 XZ 或 LZMA 压缩文件（`.tar.xz`）的形式发布。

### O3DE 软件包结构

O3DE 软件包必须具有以下结构：

```cmd
.
├── PackageInfo.json          # A manifest file describing the package.
├── Find<Package>.cmake       # CMake file defining targets for this package.
├── <Package content dir>     # Directory containing package contents.
└── SHA256SUMS                # SHA256 checksums for each file in package. This includes the manifest and Find CMake files.
```

### `PackageInfo.json` 文件

`PackageInfo.json` 是描述软件包的简约清单文件。以下代码块中的任何关键字，如果其描述中不包含 `[optional]`，则无需包含。

```json
{
    "PackageName" : "The full name of the package. This is the name of the generated target and the folder package that contents are extracted to.",
    "URL"         : "URL for the package project. This doesn't need to be the same as the URL the package is stored at. [optional]",
    "License"     : "The SPDX license for the package.",
    "LicenseFile" : "Path to the LICENSE file. This path should start with the package content directory and use the `/` path separator."
}
```

有关 `License` 的允许值，请参阅 [SPDX 许可证列表](https://spdx.org/licenses/)。

### `Find<Package>.cmake` 文件

要创建目标并允许 O3DE 编译系统引用它，软件包需要包含一个 `Find<Package>.cmake` 文件，其中 `<Package>` 必须是软件包内容目录的名称。该 CMake 文件负责在软件包名称空间中识别目标，设置所需的缓存或编译值，并在 O3DE 编译系统中注册目标。

对于具有这种结构的软件包：

```cmd
example-0.1
├── PackageInfo.json
├── FindExample.cmake
└── example
    ├── LICENSE.txt
    ├── lib
    │   └── hello.dll
    └── include
        └── hello.h
```

最小的 `FindExample.cmake` 文件应如下所示。与示例结构特别相关的值包含在角括号 `<...>`中。

```cmake
set(MY_NAME "<example>")
set(TARGET_WITH_NAMESPACE "3rdParty::${MY_NAME}")
if (TARGET ${TARGET_WITH_NAMESPACE})
    return()
endif()

add_library(${TARGET_WITH_NAMESPACE} INTERFACE IMPORTED GLOBAL)

# --- Package-specific starts ---
set(${MY_NAME}_LIB_DIR ${CMAKE_CURRENT_LIST_DIR}/${MY_NAME}/<lib>)
set(${MY_NAME}_INCLUDE_DIR ${CMAKE_CURRENT_LIST_DIR}/${MY_NAME}/<include>)

ly_add_target_files(TARGETS ${TARGET_WITH_NAMESPACE} FILES ${${MY_NAME}_LIB_DIR})
ly_target_include_system_directories(TARGET ${TARGET_WITH_NAMESPACE} INTERFACE ${${MY_NAME}_INCLUDE_DIR}
# --- Package-specific ends ---

set(${MY_NAME}_FOUND True)
```

一般情况下，您需要：

* 如果在构建其定义的目标时调用了 `Find` 文件，则短路。
* 添加一个库作为 `IMPORTED INTERFACE GLOBAL`。该库必须位于 `3rdParty::`命名空间中，以避免命名冲突并协助依赖检测。
* 执行软件包的特定配置。使用 `ly_add_target_files` 向目标软件包添加库或二进制文件，使用 `ly_target_include_system_directories` 添加包含目录。
* 设置 `<package name>_FOUND` 的值为 `True`。

{{< note >}}
本例中使用的 CMake 函数：

* `ly_add_target_files`: 向现有目标添加文件。请参阅 `cmake/LYWrappers.cmake` 中的定义。
* `ly_target_include_system_directories`: 这是[target_include_directories](https://cmake.org/cmake/help/latest/command/target_include_directories.html)的精简封装，在不包含该功能的平台上提供对该功能的支持。

有关 O3DE CMake 文件中声明的函数的详细信息，请参阅 [CMake 参考 - 函数](/docs/user-guide/build/reference/#cmake-functions-reference)。
{{< /note >}}

### 校验和文件

该文件是纯文本文件，每行包含软件包中每个文件的一个 SHA256 校验和。此列表**必须**
包括 Find cmake 文件和 `PackageInfo.json` 的校验和。

该文件使用 Linux `sha256sum` 工具生成的输出格式，每行一个校验和和文件：

```
<hash> *<full path to file>
```

摘录自`cityhash`软件包的一个示例：

```
f6adb0dfdbfd4b404d19e61fa86c4eda3e46aa06cfc98eefa0403fa397dd1db5 *PackageInfo.json
af173c7e6c28551679f6676cd923731b514f02c8a29980cb54987be8479a251e *cityhash/src/citycrc.h
```

## 将软件包添加为依赖项

当软件包被上传到可从 `LY_PACKAGE_SERVER_URLS` 获取的源代码时，你需要在 Gem 或项目构建文件中向构建系统注册该软件包。软件包注册是通过 `ly_associate_package` 函数完成的。

对于使用 `ly_associate_package` 的 `example-0.1` 软件包的 Gem 来说，这将如下所示：

```cmake
ly_associate_package(PACKAGE_NAME <example-0.1> TARGETS <example> PACKAGE_HASH <a0e852b21668bae2f5259a5e4a866db0488b0ef28e4559d4cc7b427f6c61a0b0>)
```

这些都是最基本的参数：

* **PACKAGE_NAME**: 软件包的名称。该名称必须与上传到软件包存储库的`.tar.xz`文件的名称一致。
* **TARGETS**: 与软件包相关联的目标名称。
* **PACKAGE_HASH**: `.tar.xz` 文件的 SHA256 校验和。

{{< note >}}
本例中使用的 CMake 函数：

* `ly_associate_package`: 将软件包名称链接到 CMake 目标。请参阅 `cmake/3rdPartyPackages.cmake` 中的定义。
{{< /note >}}

## Pip 集成

对于依赖 [PyPi](https://pypi.org/) 或其他 `pip` 仓库中的 Python 模块或工具的 Gems、项目或软件包，O3DE 构建系统提供了 `update_pip_requirements` 和 `ly_pip_install_local_package_editable` 函数。

* `update_pip_requirements`: 处理 `requirements.txt` 文件，并将需求与 O3DE 构建系统中的名称关联。
* `ly_pip_install_local_package_editable`: 创建一个符号链接，将非标准位置的 Python 模块链接到用户 "local" 的 `pip` Site-Packages 目录内的路径，使系统 Python 能够像安装了该模块一样运行。 如果链接的 Python 模块有任何需求，**必须**在此函数之前调用 `update_pip_requirements` 以确保正确跟踪 Python 依赖关系。

{{< important >}}
对于 `requirements.txt` 中列出的 Python 模块，您不能为 `pip` 声明另一个下载源。如果您的 Python 依赖项被托管在 PyPi 以外的服务上，请手动配置 O3DE Python 安装的 `pip` 以使用相应的 CDN。
{{< /important >}}

例如，您的 Gem 包含一个名为 `example-py` 的 Python 模块，其设置文件为 `example-py/setup.py`。 要让 O3DE 访问该模块，该 Gem 的 CMake 文件需要以下代码段：

```cmake
update_pip_requirements(${CMAKE_CURRENT_LIST_DIR}/requirements.txt <ExamplePyGem>)
ly_pip_install_local_package_editable(${CMAKE_CURRENT_LIST_DIR}/<example-py> <ExamplePyGem>)
```

`<ExamplePyGem>`可以是 Python 模块的任何唯一标识符，但通常是 Gem 的名称，后面跟`Gem`。

更改 `requirements.txt` 文件或模块的标识符将重新运行注册。

{{< note >}}
本例中使用的 CMake 函数：

* `update_pip_requirements`: 查看`cmake/LYPython.cmake`中的定义。
* `ly_pip_install_local_package_editable`: 查看`cmake/LYPython.cmake`中的定义。

{{< /note >}}

## 许可证文件生成
许可证归属文件（通常称为 NOTICES 文件）可在项目开发过程中生成，以正确归属导入的任何代码或可下载软件包。要扫描项目和软件包目录中的许可证，可以运行位于引擎 `scripts\license_scanner` 子目录下的脚本。该脚本将查找 `PackageInfo.json` 文件，以创建一个包含 SPDX 标记的所有软件包许可证的摘要文件，便于参考。

详情请参阅 [引擎和项目分发](./distributable-engine#license-file-generation)中的说明。
