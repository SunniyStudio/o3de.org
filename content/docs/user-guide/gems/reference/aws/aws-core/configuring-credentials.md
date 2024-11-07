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

You can control which profile is used by default in the AWS CLI either by setting a ```[default]``` or through the use of the [AWS_PROFILE](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html#using-profiles) environment variable.

For more information on using AWS CLI configure commands, see [Configuration and credential file settings](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) in the AWS CLI User Guide. 

You can also utilize IAM roles by defining role based profiles. Refer to [Using an IAM role in the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-role.html) for information.

### 选项 2：使用配置文件配置本地凭证
如果您有自动化流程或其他预置机制，则可以将预配置的用户凭证放在标准 AWS 配置文件中。

Manually create or edit the `~/.aws/config` and `~/.aws/credentials` files (on macOS or Linux) or `%USERPROFILE%\.aws\config` and `%USERPROFILE%\.aws\credentials` files (on Windows) to include the credentials and a default region. 

First, in `~/.aws/config` or `%USERPROFILE%\.aws\config` set your default region:

```
[default]
region=us-west-2
```

Second, in `~/.aws/credentials` or `%USERPROFILE%\.aws\credentials` set up [named profiles](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html) as needed. You can configure a ```default``` profile that is used when no profile is explicitly referenced in commands. 

```
[default]
aws_access_key_id=AKXXXXXXXXXXX
aws_secret_access_key=xxXXXXXXBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY
```

### Option 3: Use environment variables to provide credentials
You can also provide AWS credentials using environmental variables:

| Variable | Description |
| --- | --- |
| `AWS_ACCESS_KEY_ID` | The AWS access key id to use. |
| `AWS_SECRET_ACCESS_KEY` | The AWS secret key id to use. |
| `AWS_SESSION_TOKEN` | The AWS session token to use (optional). If you are working with short-term credentials you will need to include the session token. |
| `AWS_DEFAULT_REGION` | The default AWS region. |

For more information on using environment variables for credentials, see [Environment variables to configure the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html) in the AWS CLI User Guide.

### Option 4: Use O3DE console variables to provide credentials
This method will only work when using an O3DE runtime binary (Launcher and Editor). Credentials can be provided directly by setting the `cl_awsAccessKey` and `cl_awsSecretKey` Console Variables (CVARs), as follows:

```cmd
Editor.exe +cl_awsAccessKey AKXXXXXXXXXXX +cl_awsSecretKey xxXXXXXXBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY
```

Note: Because of a console variable string size limitation, providing a session token is not currently supported.

| CVar | Description |
| --- | --- |
| `cl_awsAccessKey` | The AWS access key id to use. |
| `cl_awsSecretKey` | The AWS secret key id to use. |


## Setting a default profile to use in O3DE
If your development machine is configured with named profiles in your local AWS credentials file, you can set a profile to be automatically used with O3DE on a per-project basis. This profile should be set in the AWS Core [configuration settings](./getting-started/#project-settings) file and will then be used by default each time the O3DE Editor starts.

You can use the following commands to list your defaults and all named profiles (requires AWS CLI version 2):

```cmd
// Show the current defaults.
aws configure list

// Show all the named profiles.
aws configure list-profiles
```

## Controlling user permissions
All AWS IAM users are given permissions through the attachment of managed or inline IAM polices. See [Changing permissions for an IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html).

We recommend that you adhere to the following guidelines:
* Only use an admin user when absolutely necessary, such as when deploying and provisioning AWS resources. An _admin user_ is defined here as a user with full permissions on an AWS account.
* Grant [least privileges](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) on your AWS users, so they only have the minimum set of permissions on specific resources they require. This is controlled through setting of managed IAM policies and other mechanisms.
* Update managed policies to grant or revoke the appropriate permissions as you add and remove AWS resources.

### Creating IAM user groups
To make managing permissions easier, we recommend that you create [IAM user groups](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html). User groups let you specify permissions for multiple users at a time, which can make it easier to manage the permissions for groups of users.

Create user groups using the AWS Console, the AWS CLI, AWS CloudFormation templates or using the AWS Cloud Development Kit (AWS CDK). We recommend you create at least two user groups to work with AWS in O3DE:
* **Admins** - These are the administrators who own and manage AWS resources. Typically, they will perform updates and manage key resources.
* **Users** - These are the users who can take action on the resources, as part of normal gameplay or a simulation.

#### To create a user group using the AWS Console
1. Open the IAM console from [https://console.aws.amazon.com/iamv2](https://console.aws.amazon.com/iamv2).
2. In the IAM console's navigation pane, choose **Groups**.
3. Choose **Create New Group**.
4. On the **Set Group Name** page, for Group Name, enter a name for the new group. 
5. Choose **Next Step**.
6. On the **Attach Policy** page, either choose **Next Step** without attaching any policies, or attach any current policies that are relevant to the group.
7. Choose **Create Group**. 

#### To create a user group using the AWS CLI
1. Install and configure the AWS CLI as above, see [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).
2. Generate a user group.
```cmd
aws iam create-group --group-name MyUserGroup
```

#### To create user groups using the AWS CDK examples
The AWS CDK stacks defined in the AWS Core Gem will autogenerate sample `Admins` and `Users` user groups for you. By default, these groups are named ```O3DE-AWS-PROJECT-Admins``` and ```O3DE-AWS-PROJECT-Users```.

See the [AWS CDK setup instructions](cdk-application) for more details.

### Adding users to a user group
You can add users to IAM User Groups using either the AWS Console or the AWS CLI. See [Adding and removing users in an IAM user group ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_manage_add-remove-users.html) for full details.

You can quickly add users via the CLI as follows:

```
aws iam add-user-to-group --group-name MyGroup --user-name MyNewUser
```

### Attaching permissions to a user group
You can attach IAM policies to a user group to control the permissions they have access to. This can be done using either the AWS Console, the AWS CLI, AWS CloudFormation template or with the AWS CDK. See [Attaching a policy to an IAM user group ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_manage_attach-policy.html) to attach permissions using the AWS Console or AWS CLI.

#### Working with O3DE AWS CDK examples
If you deploy both the Core and example stack from the AWS Core Gem you can see examples of user permissions that were automatically generated by the ```generate_user_policy``` and ```generate_admin_policy``` functions. The user permissions are automatically attached to the user groups by a call to ```__grant_access```.

See the AWS CDK [Permissions](https://docs.aws.amazon.com/cdk/v2/guide/permissions.html) documentation for more details.

The AWS feature gems, such as AWS Metrics Gem, during deployment automatically create managed policies for users and admins that can then be attached manually to the appropriate user group. 

#### To attach managed policies to user groups using the AWS Console
1. Open the AWS Console at [https://console.aws.amazon.com/cloudformation](https://console.aws.amazon.com/cloudformation).
2. Navigate to the desired stack in a region.
3. Choose the **Resources** tab.
4. Look for resources with the type ```AWS::IAM::ManagedPolicy``` in the stack and record the policy name.
5. Open the IAM console at [https://console.aws.amazon.com/iam](https://console.aws.amazon.com/iam).
6. Select **user groups**.
7. Choose a user group.
8. Select **permissions**, then choose **add permissions**, then select **attach polices**.
9. Filter for policy to attach and select it.
10. Choose **Attach Policies**.

#### To attach managed policies to user groups using the AWS CLI
1. [Describe and list your stacks](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-describing-stacks.html) in your AWS account.
2. [List required stack resources](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-listing-stack-resources.html) in a specific stack.
3. Search for the ```AWS::IAM::ManagedPolicy``` resources and record the physical ID of the resource, which will be in the form of the policy ARN.
4. [Attach the relevant policy](https://docs.aws.amazon.com/cli/latest/reference/iam/attach-group-policy.html) to the user groups as desired.

Example CLI commands:
```cmd
aws cloudformation describe-stacks --region <region>
aws cloudformation list-stack-resources --stack-name <feature stack> --region <region>
aws iam  attach-group-policy --group-name <group name> --policy-arn <policy arn>
```

## Setting up credentials for a team

{{< note >}}
You can use [AWS Single Sign-On (SSO)](https://aws.amazon.com/single-sign-on/) instead of IAM to enable multiple users within a single AWS account to work with O3DE. In this usage pattern, the single AWS account serves as the management account for an organization in AWS Organizations, and that organization has no member accounts. To use AWS SSO, follow the [Getting Started](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html) guide and the instructions in [Integrating AWS CLI with AWS SSO](https://docs.aws.amazon.com/singlesignon/latest/userguide/integrating-aws-cli.html). For related information, see the [What is AWS Organizations](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html) and [What is AWS Single Sign-On](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html) guides.
{{< /note >}}

To set up a team, repeat the instructions for individual users above to:
1. Create relevant IAM user groups. See the [Creating IAM user groups](#creating-iam-user-groups) instructions in this topic.
2. Provide any [permissions required](#attaching-permissions-to-a-user-group) to access AWS resources to those user groups. 
3. Create any IAM users and distribute credentials using the instructions above for [individual users](#setting-up-aws-credentials-as-an-individual) as a guide.
4. [Add users](#adding-users-to-a-user-group) to the relevant user groups to grant them permissions they require.

Please read [Working with AWS credentials](#working-with-aws-credentials) to decide the right method for providing AWS IAM credentials for your O3DE project.

## Running your O3DE project on Amazon EC2

If you are running your project on Amazon EC2, you can utilize the [instance profile](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#ec2-instance-profile) to authenticate calls made to AWS. Using the IAM role from the Amazon EC2 instance profile lets you avoid configuring AWS credentials on the machine through less secure methods like supplying them as user data at deploy time or remotely accessing the machine to manually set up a profile.

To use Amazon EC2 instance role credentials with your project:
1. Create an Amazon EC2 [instance profile](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) through the [Amazon web console](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html#instance-profiles-manage-console) or AWS CLI. An instance profile is essentially a container for an IAM role that your Amazon EC2 instance can assume to make calls to AWS.
1. Provide the associated IAM role with any [permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) required to access the AWS resources that your O3DE project needs.
1. Attach the instance profile to the Amazon EC2 instance(s) running your O3DE project. You can attach it to a new instance [at launch time](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/LaunchingAndUsingInstances.html) or you can [attach it to a running instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role). 

The AWS Core Gem also requires that the `AllowAWSMetadataCredentials` setting be set to `true` before it will query the [Amazon EC2 Instance Metadata Service (IMDS)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html) for credentials. By default, it is set to `false` to prevent unwanted calls to the Amazon EC2 IMDS endpoint when running your O3DE project locally or on non-Amazon EC2 compute.

{{< note >}}
If the environment variable `AWS_EC2_METADATA_DISABLED` is set to `true`, this will override the `AllowAWSMetadataCredentials` setting and you will be unable to use credentials from the instance profile. For more information about this environment variable, refer to the [HttpRequestor Gem](/docs/user-guide/gems/reference/network/http-requestor/#turn-off-amazon-ec2-instance-metadata-service-calls).
{{< /note>}}

To turn on `AllowAWSMetadataCredentials` in AWSCore:

1. Add it to your `awscoreconfiguration.setreg` [project settings file](./getting-started/#project-settings) at the following path:

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

2. **OR** set it to true via the command line when directly invoking the launcher:

    ```
    ./MyGame.ServerLauncher.exe --regset="/Amazon/AWSCore/AllowAWSMetadataCredentials=true"
    ```


## Additional resources

* For general help with AWS CLI configuration commands, see [Configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html).
* For help with configuring credentials for the **AWS C++ SDK**, see [Providing AWS credentials](https://docs.aws.amazon.com/sdk-for-cpp/v1/developer-guide/credentials.html).
* For help with managing permissions for AWS resources, see [Policies and permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html).
* See the IAM documentation for help with [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) and [Using IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use.html).
