---
title: 开始使用 AWS Gems
description: 了解如何设置 AWS Core Gem 以从 Open 3D Engine （O3DE） 项目访问 AWS 服务。
toc: true
weight: 100
---

要开始将 AWS Gems 与 O3DE 项目中的 AWS 服务结合使用，请完成以下步骤。

1. [创建一个AWS账号](https://portal.aws.amazon.com/gp/aws/developer/registration/index.html)，如果你还没有。

2. 按照 [为 O3DE 配置 AWS 凭证](./configuring-credentials/) 中的说明配置 **AWS 凭证**。

    a. 使用`aws configure list`命令配置确认您有凭据。

3. 安装[AWS 云开发套件 (CDK)](https://docs.aws.amazon.com/cdk/v2/guide/getting_started.html#getting_started_install)。

    a. 使用 `cdk --version` 命令确认AWS CDK的安装情况。

4. 在启用 AWS Core Gem（和您需要的其他 AWS Gem）的情况下**构建** O3DE 项目。

5. 为您启用的 AWS Gem 部署 **AWS CDK 应用程序**。查看 [部署AWS CDK应用程序](./cdk-application/)了解详细指南。

6. 使用[Resource Mapping Tool](./resource-mapping-tool/)配置[资源映射文件](./resource-mapping-files/)。

7. 将资源映射文件与项目关联。请参阅下一节标题为 [Project Settings（项目设置）](#project-settings)。

您现在应该能够在 Lua 脚本、Script Canvas 或 C++ 中使用 AWS 函数与您的 AWS 资源进行通信。查看[AWS Core脚本编程](./scripting/)了解脚本示例。

## 阻止调用 Amazon EC2 实例元数据服务

AWS Gem 使用 [AWS C++ 开发工具包](https://github.com/aws/aws-sdk-cpp) 调用 AWS 资源。除非您的项目在 Amazon EC2 计算上运行，否则建议您通过将 [AWS_EC2_METADATA_DISABLED](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html) 环境变量设置为`true`来关闭 Amazon EC2 实例元数据服务 （IMDS） 查询。

AWS Gem 使用 [AWS C++ 开发工具包](https://github.com/aws/aws-sdk-cpp)调用 AWS 资源。除非您的项目在 Amazon EC2 计算上运行，否则建议您通过将 [AWS_EC2_METADATA_DISABLED](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)  环境变量设置为`true`来关闭 Amazon EC2 实例元数据服务 （IMDS） 查询。

设置此环境变量将防止 SDK 资源不必要地尝试联系 Amazon EC2 IMDS 以获取配置、区域和凭证信息，这可能会导致延迟和网络资源浪费。

```
# macOS / Linux
export AWS_EC2_METADATA_DISABLED=true

# Windows (for all sessions)
setx AWS_EC2_METADATA_DISABLED true

# Windows (for just this session)
set AWS_EC2_METADATA_DISABLED=true
```

## 项目设置

启动时，AWS Core Gem 将在项目的注册表目录中查找`awscoreconfiguration.setreg`文件： `<ProjectName>\Registry`.

如果要设置以下任何选项，则需要创建此文件。使用示例中所示的格式。
2
| 设置 | 说明 |
| --- | --- |
| **ProfileName** | \[可选\] 项目将使用`./aws/credentials` (在 macOS 和 Linux) 或 `%USERPROFILE%\.aws\credentials` (在 Windows上)中**default** 配置。. 使用此变量覆盖 **default** 配置文件或任何环境变量设置。必须是在`credentials`文件中的一个有名称的设置。 |
| **ResourceMappingConfigFileName** | \[可选\] 启动时要加载的资源映射文件的名称。资源映射文件应位于`<ProjectName>\Config`。查看[Resource Mapping 文件](./resource-mapping-files/) 了解更多信息。 |
| **AllowAWSMetadataCredentials** | \[可选\] 在查找凭证时，AWS 核心 Gem 是否应查询 AWS 环境终端节点，如 [Amazon EC2 Instance Metadata Service （IMDS）](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)。默认为`false`。有关更多信息，请参阅 [在 Amazon EC2 上运行 O3DE 项目](./configuring-credentials#running-your-o3de-project-on-amazon-ec2)。 |

{{< note >}}
如果对此文件进行更改，则需要重新启动 O3DE 编辑器。
{{< /note >}}

注册表设置文件示例：

```json
  {
    "Amazon":
    {
      "AWSCore": {
          "ProfileName": "testprofile",
          "ResourceMappingConfigFileName": "default_aws_resource_mappings.json",
          "AllowAWSMetadataCredentials": false
      }
    }
  }
```
