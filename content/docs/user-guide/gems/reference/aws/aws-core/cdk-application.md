---
title: 部署 AWS CDK 应用程序
description: 了解如何在 Open 3D Engine 中部署可选的 Python AWS CDK 应用程序。
weight: 200
toc: true
---

[AWS Cloud Development Kit](https://docs.aws.amazon.com/cdk/v2/guide/home.html) (AWS CDK) 是 AWS 的一个软件开发框架，用于为您的项目定义云基础设施并通过 AWS CloudFormation 进行预置。

{{< note >}}
AWS Gem 已更新为支持 AWS CDK v2，即新的长期 AWS CDK 版本。AWS CDK v1 已于 2022 年 6 月 1 日进入维护阶段，现在将仅接收关键错误修复和安全补丁。

对于仍在使用 AWS CDK v1 的用户，示例应用程序的旧版快照位于每个 Gem 内的“`cdkv1`”文件夹中。有关如何将旧版 AWS CDK 应用程序迁移到 AWS CDK v2 的建议，请参阅 [AWS CDK 迁移指南](https://docs.aws.amazon.com/cdk/v2/guide/migrating-v2.html)。
{{< /note >}}

AWS Core Gem 包括一个可选的 Python AWS CDK 应用程序，该应用程序提供两个堆栈：

1.用作项目的 AWS CDK 应用程序基础的核心堆栈。
2. 一个示例堆栈，其中包含可连接到 Core 中的 **ScriptBehavior** 示例的示例资源。

Python 项目的设置与标准 Python 项目类似。初始化过程使用虚拟环境，这些虚拟环境使用 '`virtualenv`' 创建并存储在 '`.venv'` 目录下。

要创建虚拟环境，您的路径中必须有一个“`python3`”可执行文件（或 Windows 的“`python`”），并且可以访问“`venv`”包。如果自动创建 '`virtualenv`' 失败，你可以手动创建 '`virtualenv`'。


## 先决条件

* 已配置 AWS 凭证。请参阅 [配置 AWS 凭证](./configuring-credentials/)中显示的步骤。
* 已安装[AWS Cloud Development Kit](https://docs.aws.amazon.com/cdk/v2/guide/getting_started.html#getting_started_install) 。
* O3DE 项目已构建。

## 使用 AWS CDK 应用程序

有一些一次性设置步骤可以为您的第一个 AWS CDK 部署做准备。以下命令都应从您正在使用的 Gem 的 'cdk' 目录运行。

### 1. 设置Python环境

有关最新的说明，请参阅`<engine root>\Gems\AWSCore\cdk`中的`README.MD`。

1. 打开命令提示符，转到您正在使用的 Gem 的 '`cdk`' 目录。

    ```cmd
    cd <ENGINE_ROOT>\Gems\<AWS_GEM>\cdk
    ```

    例如，在为 AWS Core Gem 部署 AWS CDK 应用程序时
    ```cmd
    cd <ENGINE_ROOT>\Gems\AWSCore\cdk
   ```

2. 创建一个`virtualenv`。

    ```cmd
    # Windows
    python -m venv .venv

    # Mac or Linux
    python3 -m venv .venv
    ```

3. 激活你的`virtualenv`。

    ```cmd
    # Windows
    .venv\Scripts\activate.bat

    # Mac or Linux
    source .venv/bin/activate
    ```

4. 安装所需的依赖项。

   ```cmd
   # Windows
   pip install -r requirements.txt

   # Mac or Linux
   pip3 install -r requirements.txt
   ```

### 2. 设置环境变量或接受默认值
2
| 变量 | 说明 |
| --- | --- |
| `O3DE_AWS_DEPLOY_REGION` | 要将堆栈部署到的区域将默认为 CDK_DEFAULT_REGION。 |
| `O3DE_AWS_DEPLOY_ACCOUNT` | 要将堆栈部署到的账户将默认为 CDK_DEFAULT_ACCOUNT。 |
| `O3DE_AWS_PROJECT_NAME` | 应为其部署堆栈的 O3DE 项目的名称将默认为 AWS-PROJECT。 |

有关更多信息，包括如何传递参数以用于环境变量，请参阅 AWS CDK 开发人员指南 中的 [环境](https://docs.aws.amazon.com/cdk/v2/guide/environments.html)。

### 3. 引导环境

AWS 环境是 AWS 账户和区域的组合，必须引导才能预置用于部署的常见 AWS CDK 资源。这只需要发生一次。

使用 AWS CDK“`bootstrap`”命令引导一个或多个 AWS 环境。

```cmd
cdk bootstrap aws://ACCOUNT-NUMBER-1/REGION-1 aws://ACCOUNT-NUMBER-2/REGION-2 ...
```

例如：

```cmd
cdk bootstrap aws://123456789012/us-east-1
```

有关引导预置过程的更多信息，请参阅《AWS CDK 开发人员指南》的 [引导](https://docs.aws.amazon.com/cdk/v2/guide/bootstrapping.html)部分。

### 4. 合成项目

此时，您现在可以合成 AWS CloudFormation 模板。使用您正在部署其应用程序的 Gem 的“`cdk`”目录中的 AWS CDK“`synth`”命令。

```cmd
cdk synth
```

要添加其他依赖项（例如其他 AWS CDK 库），只需将它们添加到您的“`requirements.txt`”文件中，然后重新运行“`pip install -r requirements.txt`”命令即可。

### 5. 部署项目

要部署 AWS CDK 应用程序，请使用您正在部署其应用程序的 Gem 的“`cdk`”目录中的“`deploy`”命令。

```cmd
cdk deploy
```

部署堆栈时，“`deploy`”命令会显示进度信息。

## 有用的命令

| 命令 | 说明                                           |
| --- |-------------------------------------------------------|
| `cdk ls` | 列出应用程序中的所有堆栈。                         |
| `cdk synth` | 发出合成的 AWS CloudFormation 模板。   |
| `cdk deploy` | 将此堆栈部署到您的默认 AWS 账户/区域。 |
| `cdk diff` | 将部署的堆栈与当前状态进行比较。           |
| `cdk docs` | 打开 AWS CDK 文档。                          |

## 故障排除

有关故障排除的帮助，请参阅《AWS CDK 开发人员指南》中的 [排查常见的 AWS CDK 问题](https://docs.aws.amazon.com/cdk/v2/guide/troubleshooting.html) 。
