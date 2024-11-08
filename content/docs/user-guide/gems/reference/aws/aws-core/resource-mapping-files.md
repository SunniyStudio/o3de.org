---
title: Resource Mapping 文件
description: 了解 Open 3D 引擎 （O3DE） 中的 AWS Gems 使用的资源映射文件。
weight: 300
toc: true
---

**资源映射文件** 是一个 JSON 文件，它提供 **查找名称** 到有关 AWS 资源的 **属性** 集合的映射。O3DE 运行时可以查询映射文件，以便轻松查找和利用 AWS 资源。通常，正在开发的项目将使用与项目的已发布版本不同的部署，因此使用不同的映射文件。

映射文件包含一个或多个资源条目和一组全局属性。

**资源条目格式**

每个条目定义一个查找名称（资源的逻辑名称）和一个属性集合，格式如下：

```json
"<lookup name>":  {
    "Type": string, (required)
    "Name/ID": string, (required)
    "Region": string, (optional)
    "AccountId": string (optional)
}
```

***支持的属性***
* Type - AWS资源类型，例如AWS::Lambda::Function, AWS::S3::Bucket等，应该映射到一个[CloudFormation资源类型](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)。
* Name/ID - AWS 资源名称或id。
* Region - 如果此资源部署到其他 AWS 区域，则用于覆盖 global region 属性。
* AccountId - 用于覆盖全局账户 ID 属性（如果此资源部署在其他 AWS 账户 中）。

**全局属性**

映射文件还支持一个 _可选的_ 和两个 **必须的** 全局属性：

* AccountId (可选的) - 用于搜索 O3DE 应用程序代码中引用的 AWS 资源的默认 AWS 账户。当未以其他方式显式引用账户 ID 时使用（例如在特定资源映射上）。
* Region - 要用于此映射文件的默认区域。
* Version - 本文档的 json 架构版本。

**完整映射文件的示例**

```json
{
    "AWSResourceMappings": {
        "MyKey": {
            "Name/ID": "ExampleLambda",
            "Type": "AWS::Lambda::Function"
        }
    },
    "AccountId": "123456789123",
    "Region": "us-west-2",
    "Version": "1.0.0"
}
```

此映射文件具有单个映射：**MyKey**，它是一个名为“ExampleLambda”的 Lambda 函数。它部署在账户 ID 为“123456789123”的 AWS 账户中的 us-west-2 中。

{{< important >}}
我们强烈建议不要在分发到不受信任的环境的构建中包含您的 AWS 账户 ID。根据您的使用案例，匿名 [client authentication](https://o3de.org/docs/user-guide/gems/reference/aws/aws-client-auth/) 或使用 [自定义域](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-custom-domains.html) 可能会提供与 AWS 资源交互的方法，而无需指定账户 ID。
{{< /important >}}

## 生成资源映射文件

映射文件具有定义明确的 [JSON 格式](/docs/user-guide/gems/reference/aws/aws-core/resource-mapping-schema/)。它们可以使用自定义工具生成，也可以手动生成。但是，生成或编辑这些文件的最简单方法是使用 O3DE 提供的可视化 [资源映射工具](/docs/user-guide/gems/reference/aws/aws-core/resource-mapping-tool/)。

所有映射文件都应保存到项目的 `Config` 目录中。

## 使用资源映射

在位于工程的 `Registry`  目录中的 `awscoreconfiguration.setreg`文件的`ResourceMappingConfigFileName`设置中定义要在加载时使用的资源映射文件。有关此文件的更多信息，请参阅 [项目设置](./getting-started/#project-settings) 。AWS Core Gem 在初始化时加载定义的映射文件，并支持对映射的访问。

使用请求总线 (`AWSResourceMappingRequestBus`) 与配置的资源映射文件进行交互。有关总线的类 API 参考，请参阅 [AWSResourceMappingRequests](https://o3de.org/docs/api/gems/awscore/class_a_w_s_core_1_1_a_w_s_resource_mapping_requests.html)。

**示例：读取条目**

```cpp
// 读取名为 “MyTableKey” 的映射条目的设置。
AZStd::string requestTableName = "";
AWSResourceMappingRequestBus::BroadcastResult(requestTableName, &AWSResourceMappingRequests::GetResourceNameId, "MyTableKey");

AZStd::string requestRegion = "";
AWSResourceMappingRequestBus::BroadcastResult(requestRegion, &AWSResourceMappingRequests::GetResourceRegion, "MyTableKey");
```
