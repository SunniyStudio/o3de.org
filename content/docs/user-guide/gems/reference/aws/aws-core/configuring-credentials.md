---
title: 配置 AWS 凭证
description: 了解如何在 Open 3D Engine 中配置 AWS 凭证以实现 AWS 云连接功能。
weight: 150
toc: true
---

这些说明假定您拥有（或将拥有）对 AWS 账户的管理访问权限。有关更多信息，请参阅 [AWS 主页](https://aws.amazon.com/) 了解有关创建账户的说明、[AWS 账户根用户](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) 指南和 [创建您的第一个 IAM 管理员用户和用户组](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html)。如果您已经拥有 AWS 账户，但没有该账户的管理访问权限，请咨询您的 AWS 账户管理员。

预期在本指南及其链接资源结束时，您将：
* 了解如何为 O3DE 设置 AWS 凭证。
* 了解如何通过策略和组控制 AWS 用户的权限。
* 了解团队设置要采取的步骤。
* 了解如何在项目的开发和发布/分发阶段管理凭证。

要设置 [个人用户](#setting-up-aws-credentials-as-an-individual)，您需要：
* 创建所需的 IAM 用户。
* 添加所需的任何 AWS 权限。
* 将凭证导出到本地环境。

要 [为团队设置凭据](#setting-up-credentials-for-a-team)，您需要：
* 设置用户和 IAM 凭证。
* 创建用户组。
* 将 AWS IAM 用户添加到相应的用户组。
* 向用户组添加 AWS 权限。

完成上述任务后，用户可以将其凭证导出到其本地环境。

## 使用 AWS 凭证

您需要为用户提供 AWS 凭证。您可以在短期和长期凭证之间进行选择。长期凭证在开发过程中很方便。它们更易于配置，但您需要小心确保它们的安全。当您将生成分发给外部用户时，通常建议使用短期凭证，因为它们的生命周期有限。有关更多信息，请参阅 AWS 指南中的 [管理 AWS 访问密钥的最佳实践]](https://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)。

* 要预置长期凭证，请创建一个具有编程凭证的 AWS Identity and Access Management （IAM） 用户，然后按照本指南中涵盖 O3DE 的 [将 AWS 凭证设置为个人](#setting-up-aws-credentials-as-an-individual) 该用户的部分进行操作。有关更多信息，请参阅有关 [编程访问](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys)的一般 AWS 指南。

* 要提供短期凭证，请使用 [Amazon Cognito](https://aws.amazon.com/cognito/) 或 [AWS Security Token Service](https://docs.aws.amazon.com/STS/latest/APIReference/welcome.html) 生成 [临时安全凭证](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html)。[AWSClientAuth Gem](/docs/user-guide/gems/reference/aws/aws-client-auth)提供了在 O3DE 中使用 Amazon Cognito 的配置点。

* 要通过 IAM 角色提供访问权限，请参阅 [IAM 角色](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)文档了解更多信息，并按照 [在 AWS CLI 中使用 IAM 角色](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-role.html)了解设置信息。

强烈建议不要将您的 AWS 账户根用户用于日常任务。相反，请在 IAM 中创建具有您的使用案例所需权限的用户或角色。最佳做法是定期更改用户的访问密钥，并遵循最低权限的做法。有关管理访问密钥的更多信息，请参阅《AWS IAM 用户指南》中的 [管理 IAM 用户的访问密钥](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) 。

## 以个人身份设置 AWS 凭证

本部分假设您在 AWS 账户中设置了 [AWS IAM 管理员用户](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html)。

本指南中的步骤介绍如何使用具有长期编程凭证的 IAM 用户在 O3DE 中使用。如果您尚未配置 IAM 访问密钥，请使用 AWS 控制台为 [编程访问](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys)的 AWS 参考指南中显示的步骤为现有或新的 IAM 用户生成和下载新的访问密钥。

如果您想使用短期凭证来使用 AWS，请参阅 [AWSClientAuth Gem](/docs/user-guide/gems/reference/aws/aws-client-auth) 中的设置信息。

从以下选项中进行选择，以设置用户的 AWS 凭证以在 O3DE 中使用。如果您使用的是 [named profiles](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html)，请记住在项目设置中设置配置文件。

### 选项 1：使用 AWS CLI 创建命名配置文件
O3DE 建议使用 [AWS命令行接口](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html) (CLI) (version 2)来管理 AWS 凭证的导入和配置。如果您尚未在计算机上配置凭证或区域，则满足此要求的最简单方法是使用 AWS 的“`configure`”命令：

```cmd
aws configure
```

使用此命令，您可以在出现提示时手动提供 AWS 访问密钥 ID、秘密访问密钥和默认区域。

或者，当您为用户创建新的访问密钥时，您可以选择将密钥下载为 CSV。然后，您可以使用 AWS “`import`” CLI 命令（需要 AWS CLI 版本 2）自动导入它们：

```cmd
aws configure import --csv file://credentials.csv
```

这将根据 “`credentials`” 文件中的 IAM 用户名称创建一个命名配置文件。

您可以通过设置 ```[default]``` 或使用[AWS_PROFILE](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html#using-profiles)环境变量来控制在 AWS CLI 中默认使用的配置文件。

有关使用 AWS CLI configure 命令的更多信息，请参阅《AWS CLI 用户指南》中的 [配置和凭证文件设置](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) 。

您还可以通过定义基于角色的配置文件来利用 IAM 角色。有关信息，请参阅 [在 AWS CLI 中使用 IAM 角色](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-role.html) 。

### 选项 2：使用配置文件配置本地凭证
如果您有自动化流程或其他预置机制，则可以将预配置的用户凭证放在标准 AWS 配置文件中。

手动创建或编辑`~/.aws/config`和`~/.aws/credentials`文件（在 macOS 或 Linux 上）或`%USERPROFILE%\.aws\config`和`%USERPROFILE%\.aws\credentials` 文件（在 Windows 上）以包含凭证和默认区域。

首先，在 `~/.aws/config` 或 `%USERPROFILE%\.aws\config` 中设置您的默认区域：

```
[default]
region=us-west-2
```

其次，在 `~/.aws/credentials` 或 `%USERPROFILE%\.aws\credentials` 中，根据需要设置 [命名配置文件](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html)。您可以配置一个 ```default``` 配置文件，当命令中没有明确引用配置文件时使用。

```
[default]
aws_access_key_id=AKXXXXXXXXXXX
aws_secret_access_key=xxXXXXXXBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY
```

### 选项 3：使用环境变量提供凭据
您还可以使用环境变量提供 AWS 凭证：

| 变量 | 说明 |
| --- | --- |
| `AWS_ACCESS_KEY_ID` | 要使用的 AWS 访问密钥 ID。 |
| `AWS_SECRET_ACCESS_KEY` | 要使用的 AWS 密钥 ID。 |
| `AWS_SESSION_TOKEN` | 要使用的 AWS 会话令牌 （可选）。如果您使用的是短期凭证，则需要包含会话令牌。 |
| `AWS_DEFAULT_REGION` | 默认 AWS 区域。 |

有关将环境变量用于凭证的更多信息，请参阅《AWS CLI 用户指南》中的 [用于配置 AWS CLI 的环境变量](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html) 。

### 选项 4：使用 O3DE 控制台变量提供凭据
仅当使用 O3DE 运行时二进制文件（启动器和编辑器）时，此方法才有效。可以通过设置`cl_awsAccessKey`和`cl_awsSecretKey` 控制台变量 （CVAR） 直接提供凭据，如下所示：

```cmd
Editor.exe +cl_awsAccessKey AKXXXXXXXXXXX +cl_awsSecretKey xxXXXXXXBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY
```

注意：由于控制台变量字符串大小限制，目前不支持提供会话令牌。

| CVar | 说明 |
| --- | --- |
| `cl_awsAccessKey` | 要使用的 AWS 访问密钥 ID。 |
| `cl_awsSecretKey` | 要使用的 AWS 密钥 ID。 |


## 设置要在 O3DE 中使用的默认配置文件
如果您的开发计算机在本地 AWS 凭证文件中配置了命名配置文件，则可以将配置文件设置为按项目自动与 O3DE 一起使用。此配置文件应在 AWS Core [配置设置](./getting-started/#project-settings)  文件中设置，然后每次启动 O3DE 编辑器时都将默认使用。

您可以使用以下命令列出您的默认配置文件和所有命名配置文件（需要 AWS CLI 版本 2）：

```cmd
// 显示当前默认值。
aws configure list

// 显示所有命名的配置文件。
aws configure list-profiles
```

## Controlling user permissions
所有 AWS IAM 用户都通过附加托管或内联 IAM 策略获得权限。请参阅 [更改 IAM 用户的权限](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html)。

我们建议您遵守以下准则：
* 仅在绝对必要时使用管理员用户，例如在部署和预置 AWS 资源时。此处将 _admin user_定义为对 AWS 账户具有完全权限的用户。
* 为您的 AWS 用户授予 [最低权限](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege)，以便他们仅对所需的特定资源拥有最低权限集。这是通过设置托管的 IAM 策略和其他机制来控制的。
* 更新托管策略，以便在添加和删除 AWS 资源时授予或撤销适当的权限。
  
### 创建 IAM 用户组
为了简化权限管理，建议您创建 [IAM 用户组](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html)。用户组允许您一次为多个用户指定权限，这样可以更轻松地管理用户组的权限。

使用 AWS 控制台、AWS CLI、AWS CloudFormation 模板或使用 AWS Cloud Development Kit （AWS CDK） 创建用户组。我们建议您至少创建两个用户组，以便在 O3DE 中使用 AWS：
* **Admins** - 这些管理员是拥有和管理 AWS 资源的管理员。通常，他们将执行更新并管理关键资源。
* **Users** - 这些用户可以对资源执行操作，作为正常游戏或模拟的一部分。
  
#### 使用 AWS 控制台创建用户组
1. 从[https://console.aws.amazon.com/iamv2](https://console.aws.amazon.com/iamv2)打开IAM控制台。
2. 在IAM控制台的导航面板中，选择 **Groups**。
3. 选择 **Create New Group**。
4. 在 **Set Group Name** 页面上，在Group Name中，为新的用户组输入名称。 
5. 选择 **Next Step**。
6. 在 **Attach Policy** 页面上，选择 **Next Step** 不带任何策略，或附加与组相关的任何当前策略。
7. 选择 **Create Group**。

#### 使用 AWS CLI 创建用户组
1. 如上所述安装和配置 AWS CLI，请参阅 [安装或更新最新版本的 AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)。
2. 生成用户组。
```cmd
aws iam create-group --group-name MyUserGroup
```

#### 使用 AWS CDK 示例创建用户组
AWS Core Gem 中定义的 AWS CDK 堆栈将为您自动生成示例`Admins`和 `Users` 用户组。默认情况下，这些组名为 ```O3DE-AWS-PROJECT-Admins``` 和 ```O3DE-AWS-PROJECT-Users```。

请参阅 [AWS CDK 设置指南](cdk-application) 了解详情。

### 将用户添加到用户组
您可以使用 AWS 控制台或 AWS CLI 将用户添加到 IAM 用户组。请参阅 [在 IAM 用户组中添加和删除用户](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_manage_add-remove-users.html)了解详情。

您可以通过 CLI 快速添加用户，如下所示：

```
aws iam add-user-to-group --group-name MyGroup --user-name MyNewUser
```

### 为用户组附加权限
您可以将 IAM 策略附加到用户组，以控制他们有权访问的权限。这可以使用 AWS 控制台、AWS CLI、AWS CloudFormation 模板或 AWS CDK 来完成。请参阅 [将策略附加到 IAM 用户组](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_manage_attach-policy.html) 以使用 AWS 控制台或 AWS CLI 附加权限。

#### 使用 O3DE AWS CDK 示例
如果您同时从 AWS Core Gem 部署 Core 堆栈和示例堆栈，则可以看到由 ```generate_user_policy``` 和```generate_admin_policy```函数自动生成的用户权限示例。用户权限通过调用```__grant_access```自动附加到用户组。

请参阅AWS CDK [Permissions](https://docs.aws.amazon.com/cdk/v2/guide/permissions.html) 文档了解更多详情。

在部署期间，AWS 功能 Gem（如 AWS Metrics Gem）会自动为用户和管理员创建托管策略，然后可以手动将这些策略附加到相应的用户组。

#### 使用 AWS 控制台将托管策略附加到用户组
1. 在 [https://console.aws.amazon.com/cloudformation](https://console.aws.amazon.com/cloudformation) 打开AWS控制台。
2. 导航到区域中所需的堆栈。
3. 选择 **Resources** 标签页。
4. 在堆栈中查找类型为 ```AWS::IAM::ManagedPolicy``` 的资源，并记录策略名称。
5. 在 [https://console.aws.amazon.com/iam](https://console.aws.amazon.com/iam)打开IAM控制台。
6. 选择 **user groups**。
7. 选择一个用户组。
8. 选择 **permissions**，然后选择 **add permissions**，然后选择 **attach polices**。
9. 筛选要附加的策略并选择它。
10. 选择 **Attach Policies**。
    
#### 使用 AWS CLI 将托管策略附加到用户组
1. 在你的AWS账号中[描述和列出您的堆栈](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-describing-stacks.html)。
2. 在指定堆栈中[列出所需的堆栈资源](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-listing-stack-resources.html)。
3. 搜索 ```AWS::IAM::ManagedPolicy``` 资源 并记录资源的物理 ID，该 ID 将采用策略 ARN 的形式。
4. [附加相关策略](https://docs.aws.amazon.com/cli/latest/reference/iam/attach-group-policy.html) 添加到用户组。

CLI 命令示例：
```cmd
aws cloudformation describe-stacks --region <region>
aws cloudformation list-stack-resources --stack-name <feature stack> --region <region>
aws iam  attach-group-policy --group-name <group name> --policy-arn <policy arn>
```

## 为团队设置凭据

{{< note >}}
您可以使用 [AWS Single Sign-On （SSO）](https://aws.amazon.com/single-sign-on/)代替 IAM，使单个 AWS 账户中的多个用户能够使用 O3DE。在这种使用模式中，单个 AWS 账户 充当 AWS Organizations 中组织的管理账户，并且该组织没有成员账户。要使用 AWS SSO，请按照 [入门](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html) 指南和 [将 AWS CLI 与 AWS SSO 集成](https://docs.aws.amazon.com/singlesignon/latest/userguide/integrating-aws-cli.html) 中的说明进行操作。有关相关信息，请参阅 [什么是 AWS Organizations](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html) 和 [什么是 AWS Single Sign-On](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html) 指南。

{{< /note >}}

要设置团队，请重复上述针对单个用户的说明，以：
1. 创建相关的 IAM 用户组。请参阅本主题中的 [创建 IAM 用户组](#creating-iam-user-groups)说明。
2. 向这些用户组提供任何 [所需权限](#attaching-permissions-to-a-user-group) 以访问 AWS 资源。
3. 创建任何 IAM 用户并按照上述针对 [个人用户](#setting-up-aws-credentials-as-an-individual)的说明作为指南分发凭证。
4. [添加用户](#adding-users-to-a-user-group) 到相关用户组，以授予他们所需的权限。

请阅读 [使用 AWS 凭证](#working-with-aws-credentials) 以确定为您的 O3DE 项目提供 AWS IAM 凭证的正确方法。

## 在 Amazon EC2 上运行 O3DE 项目

如果您在 Amazon EC2 上运行项目，则可以使用 [实例配置文件](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#ec2-instance-profile)来验证对 AWS 的调用。使用 Amazon EC2 实例配置文件中的 IAM 角色，您可以避免通过不太安全的方法在计算机上配置 AWS 凭证，例如在部署时将其作为用户数据提供，或远程访问计算机以手动设置配置文件。

要将 Amazon EC2 实例角色凭证用于您的项目：
1. 通过 [Amazon Web 控制台](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) 或 AWS CLI 创建 Amazon EC2 [实例配置文件](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html#instance-profiles-manage-console) 。实例配置文件本质上是 IAM 角色的容器，您的 Amazon EC2 实例可以代入该容器以调用 AWS。
1. 为关联的 IAM 角色提供访问 O3DE 项目所需的 AWS 资源所需的任何 [权限](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html)。
1. 将实例配置文件附加到运行 O3DE 项目的 Amazon EC2 实例。您可以 [在启动时](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/LaunchingAndUsingInstances.html) 将其附加到新实例  或可以 [将其附加到正在运行的实例](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role)。

   AWS 核心 Gem 还要求将`AllowAWSMetadataCredentials`设置设置为`true`，然后才能查询 [Amazon EC2 实例元数据服务 （IMDS）](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)以获取凭证。默认情况下，它设置为`false`，以防止在本地或非 Amazon EC2 计算上运行 O3DE 项目时对 Amazon EC2 IMDS 终端节点进行不必要的调用。

{{< note >}}
如果环境变量`AWS_EC2_METADATA_DISABLED`设置为`true`，这将覆盖`AllowAWSMetadataCredentials`设置，并且您将无法使用实例配置文件中的凭证。有关此环境变量的更多信息，请参阅 [HttpRequestor Gem](/docs/user-guide/gems/reference/network/http-requestor/#turn-off-amazon-ec2-instance-metadata-service-calls)。
{{< /note>}}

在AWSCore中启用 `AllowAWSMetadataCredentials` ：

1. 将它添加到你的 `awscoreconfiguration.setreg` [项目设置文件](./getting-started/#project-settings)：

    ```json
    {
        "Amazon":
        {
            "AWSCore": {
                "ProfileName": "testprofile",
                "ResourceMappingConfigFileName": "default_aws_resource_mappings.json",
                "AllowAWSMetadataCredentials": true, // example of value set to "true"
            }
        }
    }
    ```

2. **或** 在直接调用启动器时，通过命令行将其设置为 true：

    ```
    ./MyGame.ServerLauncher.exe --regset="/Amazon/AWSCore/AllowAWSMetadataCredentials=true"
    ```


## 其他资源

* 有关 AWS CLI 配置命令的一般帮助，请参阅 [配置 AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html).
* 有关为 AWS C++ 开发工具包配置凭证的帮助，请参阅 [提供 AWS 凭证](https://docs.aws.amazon.com/sdk-for-cpp/v1/developer-guide/credentials.html).
* 有关管理 AWS 资源权限的帮助，请参阅 [IAM 中的策略和权限](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html).
* 请参阅 IAM 文档以获取有关 [IAM 角色](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) 和 [使用 IAM 角色](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use.html)的帮助。
