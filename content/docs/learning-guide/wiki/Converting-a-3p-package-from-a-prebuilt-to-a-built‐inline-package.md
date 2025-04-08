---
title: "将第三方软件包从预构建包转换为内置内联包"
description: ""
toc: false
---

本页是实际执行从使用下载的第三方包转换为使用内置内联第三方包的实用指南。

有关所涉及的内容的完整概述，请参阅 [此 wiki 页面](https://github.com/o3de/o3de/wiki/O3DE-3p-Package-System-vs-FetchContent-vs-ExternalProject-investigation)

## 什么时候适合这样做？

通过 fetchcontent 使用内联库可以带来很多好处（请参阅上面的 wiki 页面了解优缺点），但请限制满足以下条件的库
* 已通过 3P 批准程序（TSC、O3DF）批准。 现有的 3p 套餐已获得批准。
* 他们编译得非常快（这是一个很难遵循的指标，但对我来说，一个例子是编译 O3DE 大约需要 2 小时，增加 30 秒，这也不是不可能的）。 仅头文件库会自动满足此条件，因为它们的编译时间为 0 秒。
* 他们下载得很快（请不要使用包含 GB 级“示例数据”的存储库）
* 它们可以使用现有的 CMake 脚本进行编译，无需（或少量）在我们的引擎中进行修补。 如果你必须为他们构建一个全新的构建系统，他们属于预构建的 [第三方软件包系统](http://github.com/o3de/3p-package-source).
* 在编译期间，他们不需要太多的平台抽象层内容在每个平台上执行完全不同的作。
* 3p Package System 中没有任何内容依赖于它们的二进制文件。 例如，zlib 满足上述所有标准，但 Qt、Python 和其他几个并不都依赖于它。 因此，它保留在 3P Package 系统中，以便其他包的编译脚本可以依赖于它，然后引擎可以依赖于完全相同的包。

## 什么时候必须这样做？
* 当库接口执行使其对编译器或链接器标志极其敏感的事情时，它们必须绝对完全匹配，否则会发生坏事。 一个例子是 OpenMesh，它允许一些 STL 对象（向量）从其接口中泄漏出来，从而导致它们在库中初始化并在库的用户中销毁。 这会导致 Address Sanitizer 等内存工具出现问题。

## 如何？

首先，您将删除注册现有 3p 的任何现有代码，因此请搜索它并删除任何尝试运行脚本的 `ly_associate_package` 调用或其他 `include` 调用。 保持对 `3rdParty::xxxx` 的依赖项不变。

参见 [Whitebox Gem](https://github.com/o3de/o3de/tree/development/Gems/WhiteBox) 它使用 OpenMesh 3p 作为内联编译，以获得这个示例

基本文件夹结构对于名为 XXXX 的 3p 是这样的。

```
+ CMakeLists.txt (cmake root file)
+ 3rdParty (folder)
    + FindXXXX.cmake (file)
    + o3de-XXXX.patch (optional patch file)
    + Installer (folder)
        * FindXXXX.CMake (file)  # this one runs when in a pre-built installer
```

**所有内容都区分大小写！ 如果事情依赖于名为 `3rdParty::XxXxX` 的库，那么你的 Find 文件必须命名为 `FindXxXxX.cmake`，你的目标也必须以类似的名称命名。**

### 根CMakeLists.txt

当有人声明对 `3rdParty::XXXX` 的依赖关系时，O3DE 会自动调用 `find_package(XXXX)`，这是一个内置的 CMake 函数（它首先减去 3rdParty 位）。  
如果有人调用 `find_package(XXXX)`，CMake 会自动搜索 FindXXXX.cmake 的注册位置列表（`CMAKE_MODULE_PATH`），如果找到该文件，则执行该文件。 它期望该文件导入或声明构建目标。  它还希望设置变量 `XXXX_FOUND`。
因此，CMake Root 文件只是将 3rdParty 文件夹添加到`CMAKE_MODULE_PATH`中，以便在有人声明对 FindXXXX.cmake 文件的依赖关系时运行它。

```cmake
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/3rdParty)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH})
```

### 第三方中的 FindXXXX.cmake
这是大多数作发生的地方。
当调用它时，它的工作是以任何必要的方式声明 `3rdParty::XXXX`，然后设置 `XXXX_FOUND`。
CMake 并不真正关心它是如何实现这一点的 - `IMPORTED`库，或指定要编译的源代码。
请参阅 [OpenMesh FindXXXX.cmake](https://github.com/o3de/o3de/blob/development/Gems/WhiteBox/3rdParty/FindOpenMesh.cmake)  以获取完整示例。 您必须根据软件包对其进行高度自定义。

上面的文件给出了一个非常清晰的演练，但我将在这里介绍各个阶段

#### 第 0 阶段：如果您已经申报，则提前退出
```
if (TARGET 3rdParty::XXXX)
    return()
endif()
```

这就像一个 include 守卫，但它也允许用户替换自己的 3rdParty::XXXX，如果他们愿意，可以更早地在更高的级别上使用它。

#### 第 1 阶段：FetchContent_Declare
告诉它从哪里获取源，或者 zip 文件，或者其他什么。 它适用于 git 存储库、zip 文件等。
始终将其固定到特定的标签或哈希值。

如果您需要制作补丁或使用补丁，请使用 `PATCH_COMMAND cmake -P "${LY_ROOT_FOLDER}/cmake/PatchIfNotAlreadyPatched.cmake" o3de-XXXX.patch`，因为 CMake 倾向于尝试重新修补已经修补的东西并失败。

请勿在此处使用 CMAKE_ARGS，它们不起作用。 FetchContent 不会启动 cmake 的新实例，因此不会重新解析命令行参数。

#### 第 2 阶段:  FetchContent_MakeAvailable
命令 `FetchContent_MakeAvailable` 实际上获取源代码（通过 git 或解压缩 zip 等），然后以内联运行的方式运行其 CMake 文件（类似于 `add_subdirectory` 或 `include()`。 如果它没有 cmake 文件，则不执行任何作。 你也可以在 `FetchContent_Declare`中给它一个伪造的 CMake 目录，让它故意不做任何事情（`SOURCE_SUBDIR bogus`），或者你可以将它指向你放在 3rdParty 中的自定义 CMakeLists.txt 文件，使用 `SOURCE_SUBDIR` 指向它。

由于它不会启动 CMake 的新实例来执行 subbuld，因此设置 CMAKE_ARGS 不会执行任何作。 在调用 `FetchContent_MakeAvailable` 之前，你必须 `set()` 变量才能在这里应用它们。 这就是为什么 `openmesh` 示例在函数中执行此作的原因 - 以限制设置这些变量的范围。 如果库不允许，则不必这样做 - 只需考虑保存全局变量，然后在 `FetchContent_MakeAvailable` 调用返回后重新设置值。

`FetchContent_MakeAvailable`返回后，下载源中的 CMakeLists.txt 将运行，并且已将其声明的所有目标添加到 O3DE 环境中。 这些目标将具有与任何其他 O3DE 目标相同的标志，例如在调试模式下具有调试标志等。

理想情况下，如果可能的话，您可以禁用目标的自动安装，因为我们希望仔细控制何时/如何安装。 大多数 3p 库都有关闭安装的选项。 内置的安装文件通常会做一些事情，比如生成 cmake 东西或制作 .pc 文件，而我们不需要这些。 

If all else fails, you can either patch their code, or write your own simpler CMakeLists.txt using the `SOURCE_SUBDIR` variable.

### 第 3 阶段：使目标适应 O3DE（编译器设置、链接器设置、安装等）
这是您从第三方更改目标上任何编译器设置的机会。
有用的工具
```
# target_compile_options APPENDS compile options to the target.
 target_compile_options(XXXX ${O3DE_COMPILE_OPTION_DISABLE_WARNINGS})     # XXXX does not pass Warning Level 4

# target property FOLDER makes it show up in that folder in Visual Studio and other IDEs.
# If the 3p is used by others besides this gem, consider putting it in 3rdParty rather than relative to gem.
# The path is relative to the root of the project folder structure.
get_property(this_gem_root GLOBAL PROPERTY "@GEMROOT:${gem_name}@")
ly_get_engine_relative_source_dir(${this_gem_root} relative_this_gem_root)
set_property(TARGET XXXX PROPERTY FOLDER "${relative_this_gem_root}/External")
```

#### 第 4 阶段：为 O3DE 的目标和安装程序设置别名。
O3DE 需要 `3rdParty::XXXX`，但目标的 CMake 脚本可能生成了类似于 `XXXX` 或 `SomeCompany::XXXXX`的内容或一系列目标。 O3DE 的安装系统还期望有`XXXX`和`3rdParty::XXXX`。

对于非常简单的库，这可能就像将它们别名为 3p 的 cmake 脚本生成的基本 XXXX 库一样简单：
```
add_library(3rdParty::XXXX ALIAS XXXX)
```

对于更复杂的 Target，您可能需要做更多的工作，遍历它们的所有目标，并根据需要调用 add_library。

除非您有信心，否则请避免称呼`ly_create_alias`。 
'ly_create_alias（NAMESPACE 3rdParty NAME XXXX TARGETS YYYY） 此函数执行 3 项作
1. 创建一个名为 `XXXX` 的 INTERFACE 库，它是 O3DE 的名称，它依赖于 `YYYY`，即内部 3p 的名称。
2. 创建名为 `3rdParty::XXXX` 到 `XXXX` 的别名
3. 在安装程序自动生成系统中注册 `3rdParty::XXXX`，该系统将为您生成虚假的导入目标。

如果 `XXXX` 已经存在，即如果软件包 `zlib` 的第三方 CMake 脚本已经创建了一个名为 `zlib` 的目标，则上述第 1 点可能会失败。 所以在这种情况下不要使用 ly_create_alias。 O3DE 之所以这样做，是因为构建脚本通常要求使用 `3rdParty::OpenMesh` 之类的内容，但实际的第三方库定义了 `openmesh::core`, `openmesh::tools` 等内容。 这允许选择 `3rdParty::XXXX` 以及 `XXXX` 作为主要目标（甚至是链接所有其他目标的目标）。

3 可能不需要。 它*可能*有效，因为自动生成会查看生成的包含文件和 lib 文件，如果有，在您的测试中，您不需要创建 3rdParty/Installer/FetchContent。   

#### 安装
如果您禁用了 install on the target（为避免名称冲突，您可能必须这样做），则必须自行处理。
但即使你没有这样做，你可能也想告诉 O3DE 将安装程序 FindXXXX.cmake 等杂项文件安装到安装程序映像中。
目录是相对于安装映像的根目录的。
 
```
ly_install(FILES ${CMAKE_CURRENT_LIST_DIR}/Installer/FindXXXX.cmake DESTINATION cmake/3rdParty)
ly_install(FILES ${CMAKE_CURRENT_LIST_DIR}/XXXX-o3de.patch DESTINATION cmake/3rdParty)
```

请注意，cmake/3rdParty 已经是安装程序中CMAKE_MODULE_PATH的一部分，因此在安装程序中运行时不需要对模块路径添加任何添加。

不过，完成此作后，您可以调用`set(XXXX_FOUND TRUE PARENT_SCOPE)` 来表示成功。 如果你不在作用域函数中，请删除 `PARENT_SCOPE`

### 第三方/安装程序中的 FetchXXXX.cmake

这个版本应该被复制到安装程序中（参见上一节的最后一部分）。
原始 cmake 文件在安装程序中时不会运行。  相反，O3DE 会自动生成 `IMPORTED`（预构建） 目标。 在我们的例子中，这里发生的事情在很大程度上取决于库的使用方式。

对于 OpenMesh，在运行时或安装程序时实际上根本不需要它，因为整个 3p 库仅由 WhiteBox gem 私有使用，仅在其私有库中，并且我们发布的公共标头实际上不包含 OpenMesh 的任何部分。 这是一种非常常见的模式，在这种情况下，目标只需要存在。

有关示例，请参阅(Open Mesh 安装程序库)[https://github.com/o3de/o3de/blob/development/Gems/WhiteBox/3rdParty/Installer/FindOpenMesh.cmake]，它只是生成预期的 XXXX 和 3rdParty：：XXXX 库，并打印出“using”消息，以便用户了解他们正在使用的第三方库。

请注意，如果您确实需要此处的库，请在安装程序中（它们被公开使用），您将需要进行判断调用。
1. 确保在原始 Cmake 脚本中使用 ly_install（....） 以确保在关闭原始 3p 安装机制时部署 .lib、.a、.so、.h 文件。 考虑使用不同的文件夹进行调试与发布，或者在文件名 （blahblah_d） 或其他名称上添加疣，用于调试与发布，然后将它们放在同一个文件夹中。 检查安装文件夹的布局。
2. 在 FindXXXX.cmake 的“安装程序”版本中，您可以声明它们为已导入
```cmake
set_target_properties(TARGETS XXXX
                        PROPERTIES 
                          IMPORTED_LOCATION ${LY_ROOT_FOLDER}/lib/${CMAKE_STATIC_LIBRARY_PREFIX}xxxx${CMAKE_STATIC_LIBRARY_SUFFIX}
                          IMPORTED_LOCATION_DEBUG ${LY_ROOT_FOLDER}/lib/${CMAKE_STATIC_LIBRARY_PREFIX}xxxx_d${CMAKE_STATIC_LIBRARY_SUFFIX})
```
您将像往常一样添加 include 路径，具体取决于您的安装位置。 请使用 ${LY_ROOT_FOLDER}，因为项目本身可能会用完其他项目文件夹。

## 测试安装程序
配置 O3DE **WITHOUT** 一个已配置的项目。 现在，也请关闭 test modules。
```
windows:
cmake -S . -B build/windows -ULY_PROJECTS -DBUILD_TESTING=OFF -DLY_DISABLE_TEST_MODULES=ON -DO3DE_INSTALL_ENGINE_NAME=o3de-installer
cmake --build build/windows --config debug --target install -- /m
cmake --build build/windows --config profile --target install -- /m

linux
cmake -S . -B build/linux -ULY_PROJECTS -DBUILD_TESTING=OFF -DLY_DISABLE_TEST_MODULES=ON -G"Ninja Multi-Config" -DO3DE_INSTALL_ENGINE_NAME=o3de-installer
cmake --build build/linux --config debug --target install
cmake --build build/linux --config profile --target install
```
这将在当前目录的 `install` 子文件夹中创建一个 **调用** o3de-installer 的安装程序，然后在其中构建调试和配置文件库（如果您只想使用 profile 或 debug 进行快速测试，则可以省略其中一个）。 您构建的每个不同的排列都会以附加方式将这些库放入其中。

一旦你完成了安装程序，你就可以去注册它了
```
windows:

cd install
python\get_python
scripts\o3de register --this-engine

linux:

cd install
./python/get_python.sh
./scripts/o3de.sh register --this-engine
```

然后，您可以从 install 的 bin 文件夹中运行 o3de，并创建项目来测试它。 使用这些项目对其进行测试，确保它们使用您的库构建。

## 迭代补丁文件。

有时您必须修补库。 使用 fetch content 执行此作非常简单。
库被下载到 （buildfolder/_deps/XXXX_src） 中，因此在你执行初始 fetchcontent 后（让它运行并失败），你可以像这样 cd 到该文件夹中：
`cd build\windows\_deps\XXXX_src` 位于该软件的 git 存储库中。 使用您最喜欢的 IDE 进行您想要的任何更改。

进行更改后，创建/更新补丁文件。 从 XXXX_src 文件夹中：
```
git diff > (Full absolute path to your 3rdParty folder in your gem where the FindXXXX.cmake file lives)\o3de-XXXX.patch
```

如果是第一次，请添加`PATCH_COMMAND cmake -P "${LY_ROOT_FOLDER}/cmake/PatchIfNotAlreadyPatched.cmake" o3de-XXXX.patch` to the `FetchContent_Declare`块中，使其使用该补丁。

然后照常重新运行 O3DE 配置和构建。 根据需要重复。 如果补丁是新的，请确保将其包含在带有`ly_install`的发布版本中，以确保每个人都了解他们要遇到的内容，并像 openmesh 示例一样在您的`message(STATUS "`块中披露它。


