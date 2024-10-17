---
title: "CMake 基础系列 - 第 4 部分"
date: 2022-11-16
slug: cmake-essentials-series-part-4
author: Tom Hulton-Harrop
blog_img: "/images/blog/announcement_thumbnail.jpg"
full_img: ""
---

在最后一部分中，我们将介绍为其他项目安装程序库所需的命令。

## 动机

在本系列文章中，我们已经学习了如何使用 CMake 和利用现有库，但还没有学习到如何编写新库以便他人轻松集成。通过遵循这些最佳实践，我们可以使我们的库易于使用，更有可能被采用。

## 示例

CMake 提供的各种安装命令至少可以说有点令人困惑。造成混乱的主要原因是 `install` 命令本身（在 `CMakeLists.txt` 文件中使用的命令）有许多变体，每个变体所做的事情略有不同。我们将依次介绍每种变体，看看安装静态库以便在其他应用程序中使用需要做些什么。

`CMakeLists.txt` 文件以一些熟悉的命令开始。

```cmake
cmake_minimum_required(VERSION 3.15)
project(calculator)
add_library(${PROJECT_NAME}) #1
target_sources(${PROJECT_NAME} PRIVATE src/calculator.cpp) #2
```

我们不创建可执行文件/应用程序，而是创建一个库 (`#1`)，并添加相关源代码 (`#2`)。

```cmake
include(GNUInstallDirs) #3
```

然后，我们必须引入一些标准的安装位置路径，以确保安装的可移植性，这些路径可通过包含 `GNUInstallDirs` (`#3`) 提供。

```cmake
target_include_directories(
    ${PROJECT_NAME}
    PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include> #4
           $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)      #5
```

接下来我们设置库的包含路径（`#4`）。请注意，我们使用了一个生成器表达式（也许将来会成为另一篇博文的主题）来配置在何处查找包含。

这可以是安装后使用的导入目标 (`#5`)，也可以是正常编译的一部分（由 `BUILD_INTERFACE` 和 `INSTALL_INTERFACE`区分）。

```cmake
install( #6
    TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}-config
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})

install( #7
    EXPORT ${PROJECT_NAME}-config
    NAMESPACE ${PROJECT_NAME}::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME})

install(
    DIRECTORY ${CMAKE_CURRENT_LIST_DIR}/include/${PROJECT_NAME}/ #8
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/${PROJECT_NAME})
```

最后三个步骤是虚幻的 `install` 命令。我们首先指定要安装的目标 (`#6`)。我们设置导出名称（不需要与目标名称匹配，但通常需要），这是 CMake 在使用 `find_package` 时查找的名称。我们还要指定联编库文件的安装位置（`${CMAKE_INSTALL_LIBDIR}` 由 `GNUInstallDirs` 提供）。

接下来，我们必须将导出文件安装（复制）到安装位置（`#7`）。在此设置命名空间是个好做法，因此客户端必须使用 `calculator::calculator` 将其标识为导入目标，我们还必须设置导出文件的安装位置。

最后调用 `install` (`#8`) 只是将界面/头文件复制到预期位置。

完成所有设置后，现在就可以配置并运行 `cmake --build build --target install` 来构建并安装库了。要设置自定义的安装位置，请记住向 CMake configure 命令提供 `-DCMAKE_INSTALL_PREFIX=<path>`。

## 讨论

安装并不是你经常会用到的东西，但了解了这些命令的作用，阅读现有的 `CMakeLists.txt` 文件就会容易得多。我们还有很多没讲到的内容，比如版本控制和自定义导出/配置文件等更高级的主题，但这些应该已经给了你足够的信息来开始学习。

希望这次对 CMake 及其一些鲜为人知的功能的介绍对你有所帮助。这些内容现在可能还不太有用，但毫无疑问，将来会越来越有用。如果您有任何问题或需要跟进未涉及的内容，请通过 O3DE Discord 与我们联系。

## 进一步阅读

要了解有关安装的更多信息，我推荐您观看克雷格-斯科特（Craig Scott）的精彩演讲：

* [面向程序库作者的深度 CMake](https://youtu.be/m0DwB4OvDXk)

上周提到的 repo 上还有更多信息和大量示例：

* [CMake 示例](https://github.com/pr0g/cmake-examples)

这里还有一套 CMake 安装辅助工具，可以省去安装新库时的一些繁琐步骤：

* [CMake 助手](https://github.com/pr0g/cmake-helpers)

_免责声明：本文仅代表作者个人观点，不代表Open 3D基金会、Open 3D Engine或作者所在公司的观点。_

### 查看该系列的其他部分：

* [CMake 基础系列 - 第 1 部分](/blog/posts/cmake-essentials-series-part-1/)
* [CMake 基础系列 - 第 2 部分](/blog/posts/cmake-essentials-series-part-2/)
* [CMake 基础系列 - 第 3 部分](/blog/posts/cmake-essentials-series-part-3/)
* [CMake 基础系列 - 第 4 部分](/blog/posts/cmake-essentials-series-part-4/)
