---
linktitle: AWS Core
title: AWS Core Gem
description: Open 3D Engine （O3DE） AWS 核心 Gem 简介。
weight: 100
toc: true
---

借助 **AWS Core** Gem，您可以在 O3DE 中使用 AWS 服务。其他 AWS Gem 通常依赖于此 Gem，因为它提供了执行以下操作的常见机制：
* 设置所需的适用于 C++ 的 AWS 开发工具包。
* 配置平台客户端。
* 进行 HTTPS RESTful 调用。
* 处理通用响应和错误
* 设置和使用 AWS 凭证。

## 特征

AWS Core Gem 具有以下主要功能：

* 初始化、[配置](https://docs.aws.amazon.com/sdk-for-cpp/v1/developer-guide/configuring.html)，并终止 [AWS SDK for C++](https://docs.aws.amazon.com/sdk-for-cpp/v1/developer-guide/welcome.html).
    * 包括适用于 C++ 的 SDK 的平台扩展。
    * 提供可用于 O3DE 的通用客户端配置。请参阅[AWS Client 配置](https://docs.aws.amazon.com/sdk-for-cpp/v1/developer-guide/client-config.html)了解详情。

* 处理对 AWS 服务的 HTTPS 调用，包括响应和错误。
* 允许您使用 AWS 凭证，包括现有的 AWS 命令行界面 （AWS CLI） 配置文件和角色，以及控制台变量 （CVAR），以便更轻松地进行配置。请参阅 [配置 AWS 凭据](./configuring-credentials/) 了解详情。
* 支持 AWS 资源共享，帮助您识别可从 O3DE 使用的 AWS 资源。
* 提供实用程序功能，让您能够更轻松地使用 AWS 服务和资源。

## 启用 AWS 核心 Gem

要启用 AWS Core Gem，请执行以下操作：

1. 使用 **O3DE Project Manager** 或命令行将 AWS Core Gem 添加到您的项目中。

2. 使用 Project Manager、Visual Studio 或 CMake 构建您的项目。

3. 要配置您的环境和项目以使用 AWS 服务，请按照 [AWS Gems 入门](./getting-started/) 中的说明进行操作。
