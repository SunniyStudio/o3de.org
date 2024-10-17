---
title: "CMake 基础系列 - 第 2 部分"
date: 2022-10-19
slug: cmake-essentials-series-part-2
author: Tom Hulton-Harrop
blog_img: "/images/blog/announcement_thumbnail.jpg"
full_img: ""
---
如何在 CMake 项目中快速集成第三方库。

## 动机

C++ 中的依赖关系管理一直是个难题。在没有任何软件包管理器的情况下，手动引入一个库的步骤既耗时又容易出错。幸运的是，CMake 最近（3.11-3.14）增加了一个功能，使这一工作变得异常简单。

## 示例

让依赖关系管理变得轻而易举的杀手级功能是`FetchContent`。让我们继续上一部分的内容，为 “Hello, World!”应用程序添加一些有用的功能。假设我们想使用一个迭代器库，但又不想把它复制到源代码树中，然后自己手动配置构建。我们可以这样使用 `FetchContent`（新代码遵循 [Part 1](/blog/posts/cmake-essentials-series-part-1) 中的 `project` 命令）。

```cmake
...
include(FetchContent) #1
FetchContent_Declare( #2  
  EasyIterator #3
  GIT_REPOSITORY https://github.com/TheLartians/EasyIterator.git #4  
  GIT_TAG aab0c0d8fd17708c64522408d9b304729dbc3a3f #5
)
FetchContent_MakeAvailable(EasyIterator) #6
...
target_link_libraries(${PROJECT_NAME} PRIVATE EasyIterator) #7
...
```

首先，我们需要将 CMake `FetchContent` 模块引入我们的 `CMakeLists.txt` 文件 (`#1`)。

然后，我们使用 `FetchContent_Declare` (`#2`)声明我们要使用的库。

我们给依赖库取一个名字 (`#3`)，并将其指向我们要使用的库的 git 仓库 (`#4`)（`GIT_REPOSITORY` 只是众多选项之一，如本地路径的 `SOURCE_DIR`）。

最后，将库的版本固定在特定的提交（或标签，如果存在的话）（`#5`）上是个不错的做法，这样后续的上游更改就不会立即破坏我们的库。

`FetchContent_MakeAvailable` (`#6`) 然后在幕后进行大量工作，但本质上只是让我们的应用程序可以使用目标。

我们需要做的最后一件事就是链接依赖项（`#7`）。这样做的好处是，只要我们所依赖的项目设置正确，就不需要接触 include 或库路径，一切都由上游处理，与我们无关。

这样，我们就可以编写一个改进的 'Hello, \<planet\>'程序，在每次迭代时输出索引。

```c++
#include <easy_iterator.h>
...
std::vector<std::string> planets = {    
    "Mercury", "Venus", "Earth", "Mars", "Jupiter", "Saturn", "Uranus", "Neptune", "Pluto?"};
for (auto [index, planet] : easy_iterator::enumerate(planets)) {    
    std::cout << index << ": Hello," << ' ' << planet << '\n';
}

// outputs:
// 1: Hello, Mercury
// 2: Hello, Venus
// 3: Hello, Earth
// ...
//
```

## 讨论

`FetchContent` 对于小型、简单、快速构建的库来说是非常好的。依赖库会直接与我们的应用程序一起构建（依赖库实际上在 `build/_deps` 中）。(对于较大的依赖库，我们可能需要在较早的阶段单独构建它们，然后使用 `find_package` 命令将其引入（下次再详述......）。`FetchContent` 也只适用于使用 CMake 构建的项目，不过现在绝大多数项目都使用 CMake 构建，即使那些不使用 CMake 构建的项目也通常有社区维护的 CMake 版本。以[bgfx.cmake](https://github.com/bkaradzic/bgfx.cmake) 为例。

## 进一步阅读
 
CMake [`FetchContent` docs](https://cmake.org/cmake/help/latest/module/FetchContent.html) 是获取更多信息的好地方。

还有一个名为['CMake 傻瓜教程'](https://youtu.be/7W4Q-XLnMaA)的 CMake 入门视频。(视频质量不是很好，但它很好地介绍了 CMake 的基本使用方法_）。

_免责声明：本文仅代表作者个人观点，不代表Open 3D基金会、Open 3D Engine或作者所在公司的观点。_

### 查看该系列的其他部分：

* [CMake 基础系列 - 第 1 部分](/blog/posts/cmake-essentials-series-part-1/)
* [CMake 基础系列 - 第 2 部分](/blog/posts/cmake-essentials-series-part-2/)
* [CMake 基础系列 - 第 3 部分](/blog/posts/cmake-essentials-series-part-3/)
* [CMake 基础系列 - 第 4 部分](/blog/posts/cmake-essentials-series-part-4/)
