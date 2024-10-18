---
title: CMake 设置参考
description: Open 3D Engine特定 CMake 设置的参考资料。
weight: 500
---

Open 3D Engine 使用自定义 CMake 配置值来检测有效部署平台、活动项目和下载软件包位置等设置。本文档参考了 O3DE 使用的用户可用 CMake 设置。Gem 的特定设置在 [Gem 参考](/docs/user-guide/gems/reference)。有关 CMake 的一般选项，请参阅 [cmake-变量文档](https://cmake.org/cmake/help/v3.18/manual/cmake-variables.7.html)。

请记住，每次更改配置值时，都需要重新生成项目文件，以便在下一次构建时接收和应用更改。

{{< caution >}}
CMake 选项如 `CMAKE_CXX_STANDARD` 是 O3DE 在配置过程中自动设置的。更改这些值可能会破坏您编译 O3DE 代码的能力，因此请谨慎编辑。
{{< /caution >}}

## 缓存值

### 必要设置

这些选项是用户提供的配置 O3DE 构建所需的设置。请确保在首次运行 configure 之前设置好这些值，并在必要时进行更改。

* **`LY_3RDPARTY_PATH`** - 软件包目录的文件系统路径。更改此值需要重新配置，并会提示再次安装软件包。请参阅 [软件包](./packages/) 获取更多信息。
  
  *Type*: `PATH`

### 构建配置

* **`LY_UNITY_BUILD`** - 控制 [unity build](https://cmake.org/cmake/help/latest/prop_tgt/UNITY_BUILD.html) 文件的生成。Unity 联编可将多个 `.cpp` 文件合并为一个编译单元，从而加快联编速度。

  {{< note >}}  
  如果您的项目、O3DE 引擎或 O3DE 工具的构建时间较慢，请确保将此选项打开 `ON` 。对于可用内存较多、可用内核较少或磁盘吞吐量较低的系统，这种影响最为显著。
  {{< /note >}}  

  *Type*: `BOOL`  
  *Default*: `ON`
* **`LY_MONOLITHIC_GAME`** - 控制项目库链接。将此值设置为`ON`时，编译器会提示尽可能使用静态库。某些库（如 PhysX）只能作为共享库使用，无法进行静态链接。某些平台可能会完全禁用静态链接。

  *Type*: `BOOL`  
  *Default*: `OFF`
* **`LY_LINKER`** - 设置构建过程中使用的链接器程序。如果未设置该值，将根据主机平台上安装的可用链接程序选择默认链接程序。目前仅支持 Linux x86 Clang 配置。

  *Type*: `STRING`
  *Default*: `(Empty string)`

### 资产配置

这些选项可控制构建的资产类型，以及项目在运行时从哪里加载资产。

* **`LY_ASSET_DEPLOY_TYPE`** - 资产处理器[Asset Processor](/docs/user-guide/assets/asset-processor/)构建的**默认**资产类型。有效的平台有
  * `pc` - Windows PC
  * `linux` - Linux
  * `mac` - MacOS
  * `ios` - iOS 和 iPad OS
  * `android` - Android
  
  *Type*: `STRING`  
  *Default*: 当前主机平台的资产类型。

* **`LY_ASSET_DEPLOY_MODE`** - 控制项目在运行时加载资产的方式。允许的值有
  * `LOOSE` - 在资产处理器处理来源后，按需从资产缓存加载资产。此设置适用于开发。
  * `PAK` - 只能从资产捆绑程序创建的 `.pak` 资产捆绑包中加载资产。从哪些目录加载资产包由 `LY_OVERRIDE_PAK_FOLDER_ROOT` 设置控制。
  * `VFS` - 从虚拟文件系统服务器（VFS）加载数据。
  
  *Type*: `STRING`  
  *Default*: `LOOSE`

* **`LY_ARCHIVE_FILE_SEARCH_MODE`**  
  定义在存档系统中查找非 Pak 文件的默认文件搜索模式
  *  `0` = S首先搜索文件系统，然后再搜索挂载的 `.pak` 文件。
  *  `1` = 在搜索文件系统之前，先搜索挂载的 `.pak` 文件。
  *  `2` = 只搜索已加载的 `.pak` 文件。

  *Type*: `STRING`  
  *Default*: `0` = (debug/profile 配置), `2` = (release 配置)

### 软件包系统设置

这些设置控制软件包下载系统的功能。

* **`LY_PACKAGE_DEBUG`** - 当`ON`时，产生关于软件包下载和安装过程的冗长信息。在提交任何针对 O3DE 软件包系统的错误报告之前，请设置此缓存值，以确保所有必要信息都能解决问题。

  *Type*: `BOOL`  
  *Default*: `OFF`

* **`LY_PACKAGE_DOWNLOAD_CACHE_LOCATION`** - 从远程服务器下载软件包的下载缓存。如果 `LY_PACKAGE_KEEP_AFTER_DOWNLOADING` 为 `ON`，该缓存将永不清空。

  *Type*: `PATH`  
  *Default*: `${LY_3RDPARTY_PATH}/downloaded_packages`

* **`LY_PACKAGE_KEEP_AFTER_DOWNLOADING`** - 是否将下载的软件包保留在缓存中，即使在安装后也是如此。

  *Type*: `BOOL`  
  *Default*: `ON`

* **`LY_PACKAGE_SERVER_URLS`** - 从服务器提取软件包的 URL，以分号（`;`）分隔。这些 URL 可以是 `http`、`https`、`file` 或 `s3` URL。这些值将_prepended_到任何 `LY_PACKAGE_SERVER_URLS`环境变量。

  *Type*: `STRING`  
  *Default*: `(Empty string)`

* **`LY_PACKAGE_UNPACK_LOCATION`** - 下载的软件包在重新定位到软件包文件夹之前被解压缩到的位置。

  *Type*: `PATH`  
  *Default*: `${LY_3RDPARTY_PATH}/packages`

* **`LY_PACKAGE_VALIDATE_PACKAGE`** - 根据请求 CMake 文件中的校验和验证软件包，必要时从源代码重新下载软件包。

  *Type*: `BOOL`
  *Default*: `OFF`

* **`LY_PACKAGE_VALIDATE_CONTENTS`** - 根据软件包的 `SHA256SUMS` 中包含的哈希值检查每个文件。此值为`OFF`时，仅在第一次下载软件包时验证校验和。打开此设置可检查对软件包的本地修改，但会降低配置速度。

  *Type*: `BOOL`
  *Default*: `OFF`

* **`LY_PACKAGE_DOWNLOAD_RETRY_COUNT`** - 如果传输过程中出现错误，尝试从软件包源检索的次数。

  *Type*: Integer
  *Default*: 3

### 构建/调试工具

* **`LY_BUILD_WITH_ADDRESS_SANITIZER`** - 启用 [Address Sanitizer](https://en.wikipedia.org/wiki/AddressSanitizer) (ASan)。

  {{< note >}}
  目前仅支持 Windows 和 "Visual Studio" 生成器。文档可在 [此处](https://docs.microsoft.com/en-us/cpp/sanitizers/asan?view=msvc-160)找到
  {{< /note >}}

  *Type*: `BOOL`
  *Default*: `OFF`

* **`O3DE_BUILD_WITH_DEBUG_SYMBOLS_RELEASE`** - 在发行版配置中生成符号文件 (`.pdb`)，并关闭优化，从而更容易在发行版构建中排除故障。

  {{< note >}}
  目前仅支持 Windows 和 "Visual Studio" 生成器。
  {{< /note >}}

  *Type*: `BOOL`
  *Default*: `OFF`

<!-- 
  TODO: 平台特定设置--是放在这里、平台页面上，还是完全放在其他地方（如参考附录中？）
-->

## CMake 函数参考

O3DE CMake 文件提供对 Visual Studio 和 Visual Studio Code 的 Intellisense 支持，这些文件位于 O3DE 源代码的 `cmake` 目录中。
