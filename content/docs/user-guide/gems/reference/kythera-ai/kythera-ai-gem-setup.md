---
linkTitle: 设置
title: Kythera AI Gem 设置
description: 设置 Kythera AI Gem 的说明
weight: 200
toc: true
---

[Download the Kythera AI Gem .zip file from Kythera AI’s website](https://www.kythera.ai/kythera-for-o3de).

## 构建 Kythera AI 演示项目

KytheraAIDemo 项目是下载的 zip 文件的一部分。

确保您从安装程序安装了有效的引擎，或者按照 [O3DE 安装指南](/docs/welcome-guide/setup/) 中的说明安装了 SDK。

1. 将下载的 zip 文件的所有内容解压缩到一个文件夹中。
1. 打开 **O3DE Project Manager** (`o3de.exe`).
1. 转到 **New Project...** 并选择 **Add existing Project**.
1. 在解压的zip文件夹中选择 `KytheraAIDemo` 文件夹。
1. 在项目菜单中，选择 **Build**。

    ![Project Menu Build](/images/user-guide/gems/kythera-ai/project-manager-project-menu-build.png)

1. 返回 O3DE 启动器，单击**Open Editor**。
1. 启动 Editor 后，请确保已在 Asset Processor 中处理所有资产（Status （状态） 为 Idle），否则打开演示关卡将引发错误。

## 将 Kythera AI Gem 添加到项目

以下步骤适用于已安装的 SDK 版本和以引擎为中心的版本：

1. 将 zip 文件解压缩到一个文件夹中。
1. 在引擎安装中打开命令提示符或 PowerShell 窗口。
1. 运行以下命令：
    ```
    .\scripts\o3de.bat register -gp <path to the Kythera Gem> --external-subdirectory-project-path <path to your game directory>`
    ```
    Kythera Gem位于解压的zip文件夹中的`Kythera`子目录中。
    此命令将添加 `external_subdirectories` 键到 `project.json` 文件中，并使 Gem 可用于您的项目。
1. 打开 **O3DE Project Manager** (`o3de.exe`)。
1. 在项目菜单中，点击 **Edit Project Settings** ，然后点击右上角的 **Configure Gems** 按钮。
1. Kythera AI 现在应该可以选择为 Gem。选择它并保存项目设置。

必须先重建该项目，然后才能使用 Kythera AI 组件。
