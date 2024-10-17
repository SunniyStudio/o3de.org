---
title: "CMake 基础系列 - 第 1 部分"
date: 2022-10-05
slug: cmake-essentials-series-part-1
author: Tom Hulton-Harrop
blog_img: "/images/blog/announcement_thumbnail.jpg"
full_img: ""
---
本系列旨在概述 CMake 中一些最有用的功能，以及如何在 O3DE 中应用这些功能。

## 动机

CMake 是 C++ 社区使用最广泛的构建系统，了解如何最好地使用它，就能在 O3DE 中进行 C++ 开发。项目设置变得轻而易举，与开源项目的集成也变得更加简单。

## 示例

作为本系列的开头，我们将展示启动并运行 `‘Hello, World!’` 应用程序所需的最少代码量。首先创建一个新文件夹，并在其中创建一个空的 `main.cpp` 和 `CMakeLists.txt` 文件。

```bash
mkdir cmake-essentials-part1 && cd cmake-essentials-part1
touch main.cpp && touch CMakeLists.txt
```

`CMakeLists.txt`文件看起来像这样：

```bash
cmake_minimum_required(VERSION 3.15) # 1
project(cmake-essentials-part1 LANGUAGES CXX) # 2
add_executable(${PROJECT_NAME}) # 3
target_sources(${PROJECT_NAME} PRIVATE main.cpp) # 4
target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_17) # 5
```

首先，我们设置 CMake 版本 (`#1`)。在 CMakeLists.txt 文件的顶层，这一行必须放在第一位（`3.15`是一个安全的选择，但也可以选择更新的版本）。

接下来是 `project` 命令（`#2`），它应紧跟在 `cmake_minimum_required` 之后（`LANGUAGES` 是可选项，但也是很好的做法）。

然后，我们创建要编译的目标（`#3`），这里是一个可执行文件（`${PROJECT_NAME}`映射到`cmake-essentials-part1`，即`project`命令中设置的名称）。

然后，我们设置要编译的源代码（`#4`）。与所有 <sup>1</sup> `target_` 命令一样，我们必须指定项目的范围，这里我们使用 `PRIVATE`，因为这些文件不会被下游依赖项依赖。

最后，我们设置 C++ 版本 (`#5`)，这同样不是严格要求的，但却是良好的做法（注意，我们在此不涉及编译器标志，CMake 会为我们处理）。

<sup>1</sup> 从技术上讲，并非**所有**目标命令都需要作用域，但最好的做法是始终提供作用域，即使传统命令在没有作用域的情况下也能运行。

`main.cpp` 看起来应该很熟悉：

```c++
#include <iostream>
int main(int argc, char argv[]) {
    std::cout << "Hello, World!\n";
    return 0;
}
```

完成上述设置后，最后要做的就是运行 CMake。

```bash
> cmake -S . -B build # 1
> cmake --build build # 2
```

第一行 (`#1`) 将运行 CMake 配置步骤来生成主机构建文件。在 Windows 上，这可能默认为 Visual Studio，可以用 `-G`指定。

下面一行（`#2`）将调用配置步骤生成的构建系统，并将应用程序留在`build`文件夹中。在 Visual Studio 中，这将是 `build/Debug/cmake-essentials-part1.exe`。

## 讨论

与打开 Visual Studio 并在其中创建一个项目相比，这样做似乎要费点事，但这种方法的巨大优势在于，你现在创建了一个完全可移植的 C++ 程序，可以在任何支持 CMake 的操作系统（大多数都支持！）上从源代码开始构建。您可以轻松地与他人分享，快速构建一个演示程序，更妙的是，只需再下几条命令，就可以开始集成第三方库。(无需再在 Visual Studio 配置窗口中搜索包含路径和链接器设置）。

## 更多阅读

关于 CMake 最广泛推荐的讲座之一是 Daniel Pfeifer 所做的题为 “有效的 CMake ”的精彩演讲：

> [C++ Now 2017: Daniel Pfeifer “Effective CMake"](https://youtu.be/bsXLMQ6WgIk) by Daniel Pfeifer

它涵盖了大量内容，全面介绍了当前的 CMake 环境和最佳实践。

## 未完待续...

在本系列的下一篇文章中，我们将介绍如何为项目引入第三方依赖关系。

_免责声明：本文仅代表作者个人观点，不代表Open 3D基金会、Open 3D Engine或作者所在公司的观点。_

### 查看该系列的其他部分：

* [CMake 基础系列 - 第 1 部分](/blog/posts/cmake-essentials-series-part-1/)
* [CMake 基础系列 - 第 2 部分](/blog/posts/cmake-essentials-series-part-2/)
* [CMake 基础系列 - 第 3 部分](/blog/posts/cmake-essentials-series-part-3/)
* [CMake 基础系列 - 第 4 部分](/blog/posts/cmake-essentials-series-part-4/)
