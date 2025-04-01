---
linkTitle: 引言
title: 引言
description: 引言
---
# 引言
## 版权

为了说明如何使用 Open 3D Engine，本书复制了属于 Open 3D 基金会 和 Linux 基金会 并受其版权保护的屏幕截图、图像、模型、纹理和源代码的简短片段。Open 3D Engine （O3DE） 是 Apache 2.0 许可的多平台 3D 引擎。


{{< note >}}
您应该参考 Open 3D Engine 网站和 Linux 基金会网站，了解有关其许可证的详细信息：
* https://o3de.org/
* https://github.com/o3de/o3de/blob/development/LICENSE.txt
* https://o3d.foundation/
{{< /note >}}

## 我对这本书的心态
那些教授他们熟悉的主题的人经常犯一个诚实的错误。他们忘记了为达到对该主题的知识深度而必须采取的所有步骤。任何复杂的主题都涉及相互关联的专业技能和知识。对专家来说显而易见的事情对新手来说却是一个谜。然而，专家应该能够退后一步，考虑给定主题的整个背景，并找到最简单的方法将其呈现给新手。

在写这本书时，我总是小心翼翼地从自己的知识中退后几步，并考虑我必须知道什么才能适应 O3DE 的任务或子系统。每当我认为某个想法对理解至关重要时，我就会退后一步，看看是否有其他重要的先前概念和细节是我认为理所当然的，或者隐含地假设在我的脑海中。然后我会按照适当的顺序仔细地呈现每个想法。

我选择缓慢移动并提供太多信息，而不是省略一个会阻止某人从头开始学习 O3DE 的细节。

## 目标受众
如果您是 O3DE 的新手并希望学习引擎的基础知识，那么这本书适合您。

如果您已经是 C++ 开发人员，这将有很大帮助。O3DE 主要用 C++ 编写。它支持脚本语言，例如 Lua 和可视化语言 Script Canvas。但是，了解 O3DE 在 C++ 中的基本层会获得最大的好处。

没有一本书可以使一个人成为 O3DE 的专家。如果我要列举达到那个水平所需的所有细节和知识，阅读所有材料会感到无聊。人类的思维不是这样运作的。相反，本书适度深入地介绍了引擎的基本子系统，并展示了如何使用它们。什么是组件？什么是行为上下文？如何添加音效？如何在 O3DE 中构建服务器授权的多人游戏？

本书的最终目标是提供引擎的广泛概述。之后，您将能够在本书的范围之外自学 O3DE。

## 本书的源代码
您将在本书本身中找到很多 C++ 源代码。我在书中包含了足够的源代码，我相信您将来在实现各种游戏功能和子系统时，既能掌握材料，又能将本书用作参考。我使编码示例尽可能小，同时仍然显示足够的内容来解释材料。

一些编程书籍提供了代码示例，其中很大一部分代码隐藏在其自定义帮助程序框架后面。在本书中，您将找不到这样的 helper 函数。我认为最好显示整个代码，避免将细节隐藏在方法后面，这些方法的唯一目的是使示例更小。

如果本书中打印的源代码还不够，可以在 GitHub 上找到随附的源代码和完整的资产：
* https://github.com/AMZN-Olex/O3DEBookCode2111

本书的每一章都分配了一个唯一的 Git 分支。main 分支是空的，除了一个设计性的介绍性文件。以下每个分支都基于前一个分支。每章都包含一个指向上述存储库中正确 GitHub 分支的链接。

例如，以下 GitHub 链接：
• https://github.com/AMZN-Olex/O3DEBookCode2111/tree/ch15_unittests
• https://github.com/AMZN-Olex/O3DEBookCode2111/tree/ch16_unittests_mock

对应于第 15 章 “为组件编写单元测试”和第 16 章 “使用模拟组件进行单元测试”。

分支名称的格式为：**ch{chapter number}_{chapter name}**。

## 比较 GitHub 分支
将两个分支放在一起以查看任何给定章节中进行了哪些新更改，这将对您很有用。如果您不熟悉 GitHub 界面，以下是如何生成两个 Git 分支之间的差异。

1. 转到与您正在阅读的章节关联的分支。例如：

* https://github.com/AMZN-Olex/O3DEBookCode2111/tree/ch04_creating_components

2. 选择 Contribute → Open pull request：

![](/images/learning-guide/tutorials/o3de-book/Introduction/o3de_book_0_1.PNG)

3. 这将带您进入 Open a pull request page。

![](/images/learning-guide/tutorials/o3de-book/Introduction/o3de_book_0_4.PNG)

4. 选择较早的章节作为基础分支。

![](/images/learning-guide/tutorials/o3de-book/Introduction/o3de_book_0_3.PNG)

5. 这将创建一个拉取请求配置，该配置将显示两个分支之间的所有差异。

![](/images/learning-guide/tutorials/o3de-book/Introduction/o3de_book_0_2.PNG)

6. 向下滚动一点，您将看到更改。

![](/images/learning-guide/tutorials/o3de-book/Introduction/o3de_book_0_5.PNG)

## 本书中 PowerShell 的使用
我发现编写脚本很有用，这样可以省去输入繁琐命令的工作量。我个人在 Windows 上编写脚本的选择是 PowerShell。如果您也选择使用它，那么要从关联的 GitHub 存储库运行各种脚本，您需要配置您的系统以允许执行 PowerShell 脚本。为此，您需要更改执行策略以允许执行自定义 PowerShell 脚本。
```shell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```

{{< note >}}
如果你好奇这个命令的作用，这里是它的官方参考资料：

https:/go.microsoft.com/fwlink/?LinkID=135170

如果您完全不确定这会对您的系统造成什么影响，请不要运行它。您不需要它来理解本书的内容。你只是无法从各个章节中运行一些有用的脚本，但你总是可以用你最喜欢的 cripting 语言制作自己的脚本。您可以在此处找到有关 PowerShell 的更多文档：

https://docs.microsoft.com/en-us/powershell/
{{< /note >}}


{{< note >}}
您始终可以通过执行以下命令来恢复对执行策略的更改：
```shell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Default
```
{{< /note >}}

## 让 PowerShell 成为您的开发控制台
我使用 PowerShell 控制台作为我的构建和配置工具。下面介绍如何将 PowerShell 设置为在工作目录中自动启动并加载您选择的帮助程序脚本。
```shell
notepad $PROFILE
```

$PROFILE 是您的个人 PowerShell 配置文件，每次打开新的 PowerShell 控制台时，该文件都会执行您选择的命令。因此，我们可以修改此脚本，通过添加有用的控制台快捷方式来帮助我们进行 O3DE 开发。
```shell
Set-Location "C:\git\book"
function Run-AP() {
  & "C:\O3DE\21.11.2\bin\Windows\profile\Default\AssetProcessor.exe"
}
function Run-Editor() {
  & "C:\O3DE\21.11.2\bin\Windows\profile\Default\Editor.exe"
}
Write-Output "Custom O3DE Tools loaded!"
```

下次打开 PowerShell 控制台时，您将从 work 文件夹开始，并加载您在 $profile 中编写的任何自定义脚本。
```shell
C:\git\book> Run-Editor
```
现在开始学习 Open 3D Engine！Chicken Ball 游戏等待着您！

![](/images/learning-guide/tutorials/o3de-book/Introduction/o3de_book_0_6.PNG)
