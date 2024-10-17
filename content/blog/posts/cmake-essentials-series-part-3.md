---
title: "CMake 基础系列 - 第 3 部分"
date: 2022-11-02
slug: cmake-essentials-series-part-3
author: Tom Hulton-Harrop
blog_img: "/images/blog/announcement_thumbnail.jpg"
full_img: ""
---

如何在 CMake 项目中集成更大的第三方依赖项，并在多个项目中重复使用它们。

## 动机

虽然 `FetchContent` 对某些事情很有用，但它并不总是正确的工具。当需要集成较大的依赖（如完整的框架或软件包）时，与其将其作为同一构建的一部分，不如单独构建和安装依赖，然后使用 `find_package` 将其集成到主构建中。

## 示例

许多 C++ 库都支持一个名为 “安装 ”的概念。所谓安装，就是将所有相关的构建工件复制到一个特定位置（例如 .h 文件、.lib/.dll/.a/.so 文件和 CMake 配置文件），以便另一个应用程序或库可以使用它们。默认情况下，安装程序会将文件复制到系统默认位置，但也可以自由配置。

假设我们要为一个小原型下载、构建并安装 Google Test。命令如下：

```bash
> git clone https://github.com/google/googletest.git #1
> cd googletest #2
> cmake -S . -B build -DCMAKE_INSTALL_PREFIX=../gtest-install -Dgtest_force_shared_crt=ON #3
> cmake --build build --target install #4
```

首先，我们只需克隆 Google Test 代码库，然后导航到该文件夹（`#1` 和 `#2`）。

然后，我们运行 CMake configure 命令 (`#3`)，并提供一个带有 `-DCMAKE_INSTALL_PREFIX`的安装文件夹（否则在 Windows 下会被放在 `C:\Program Files` 中，在 Linux/macOS 下会被放在 `usr/local/lib` 和 `usr/local/include` 中）。

在 Windows 上，我们还需要在构建 Google Test 时提供 `-Dgtest_force_shared_crt`，以确保它动态链接到 C 运行时，否则会出现大量链接错误（这只是 Google Test 的一个怪癖）。

最后，我们构建并安装库 (`#4`)，将相关文件复制到 `gtest-install` 文件夹。

如果我们想在示例应用程序中使用 Google Test，可以在 `CMakeLists.txt` 中使用 `find_package` 将其引入。我们将在 `googletest/` 的同级新建一个名为 `testing` 的文件夹，并添加此文件：

```bash
# CMakeLists.txt
cmake_minimum_required(VERSION 3.15)
project(testing)
find_package(GTest CONFIG REQUIRED) #1
add_executable(${PROJECT_NAME} main.cpp)
target_link_libraries(${PROJECT_NAME} PRIVATE GTest::gtest) #2
```

`find_package` (`#1`) 会尝试将 Google Test 库作为导入目标。`CONFIG` 让 CMake 知道我们使用的是配置文件（而不是格式为 `FindXXX.cmake` 的模块文件），如果找不到 Google Test，`REQUIRED` 关键字将提前终止配置步骤。

我们在链接时使用 `GTest` 命名空间 (`#2`) 来表示 Google Test 是一个导入目标。我们的 `main.cpp` 如下：

```c++
#include "gtest/gtest.h"

// lots of tests...

int main(int argc, char** argv) {
    ::testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}
```

在配置项目时，我们只需告诉 CMake 在哪里可以找到已安装的 Google Test 库。我们可以使用 `-DCMAKE_PREFIX_PATH` (`#1`)。然后，我们就可以简单地构建 (`#2`) 和运行 (`#3`)应用程序了。

```bash
> cmake -S . -B build -DCMAKE_PREFIX_PATH=../gtest-install #1
> cmake --build build #2
> ./build/Debug/testing.exe #3
```

## 讨论

安装库并使用 `find_package` 的最大好处在于：首先，我们可以在多个项目中共享该库；其次，每次重建代码时，我们都无需重建依赖关系。对于像 Google Test 这样的库，我们很少会对其进行修改，因此将其安装并保持独立可以大大缩短迭代时间。整个过程可以通过使用 `ExternalProject_Add` 自动完成，它省去了一些手动步骤，如克隆和构建，尽管它在引擎盖下做着完全相同的事情。

## 进一步阅读

不久前，我对 CMake 产生了浓厚的兴趣，并希望更好地了解安装等主题。因此，我创建了一个 GitHub 仓库，其中包含一些简单的项目，以帮助人们更好地学习和了解 CMake。这可能会对一些刚刚接触 CMake 的人有所帮助。

* [CMake 示例](https://github.com/pr0g/cmake-examples)

还有这个小演示程序，它使用 `ExternalProject_Add` 下载和配置 SDL、bgfx 和 Dear ImGui（请参阅 `third-party/` 文件夹）：

* [SDL-bgfx-ImGui Starter](https://github.com/pr0g/sdl-bgfx-imgui-starter)

## 待续

在本系列的最后一篇中，我们将介绍如何编写一个可以安装的程序库。

_免责声明：本文仅代表作者个人观点，不代表Open 3D基金会、Open 3D Engine或作者所在公司的观点。_

### 查看该系列的其他部分：

* [CMake 基础系列 - 第 1 部分](/blog/posts/cmake-essentials-series-part-1/)
* [CMake 基础系列 - 第 2 部分](/blog/posts/cmake-essentials-series-part-2/)
* [CMake 基础系列 - 第 3 部分](/blog/posts/cmake-essentials-series-part-3/)
* [CMake 基础系列 - 第 4 部分](/blog/posts/cmake-essentials-series-part-4/)
