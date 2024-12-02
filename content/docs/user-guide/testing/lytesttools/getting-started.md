---
description: ' 按照本指南了解开始使用 LyTestTools 的第一步。 '
title: LyTestTools 入门
---

本指南概述了开始使用 LyTestTools 的第一步。在本指南结束时，您将准备好编写和运行测试。

## 先决条件

本指南要求您已完成以下操作：

 * [Open 3D Engine - 入门](/docs/user-guide)
 * [配置和构建](/docs/user-guide/build/configure-and-build)

## 开始测试

完成先决条件后，您应该配置并准备好测试的所有内容。您可以通过运行以下命令来验证这一点。

```shell
~/python/python -m pytest <path_to_test_file> --build-directory <path_to_build_output>
```

Open 3D Engine 附带一个示例测试，演示了`LyTestTools`的使用，位于`Tools/LyTestTools/tests/integ/sanity_tests.py`文件中。编译 O3DE 后，您可以使用以下命令（从 O3DE 目录运行）针对构建输出运行示例测试：

```shell
python -m pytest Tools/LyTestTools/tests/integ/sanity_tests.py --build-directory <cmake-build-directory>
```

## 更多信息

* [PyTest 框架](https://docs.pytest.org/) - 有关 LyTestTools 使用的框架以及它如何帮助编写测试的信息。
