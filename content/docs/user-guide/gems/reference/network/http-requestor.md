---
linkTitle: HTTP Requestor
title: HTTPRequestor Gem
description: HTTPRequestor Gem 提供通过用户提供的回调函数发出异步 HTTP/HTTPS 请求和返回数据的功能。
toc: true
---

HTTPRequestor Gem提供通过用户提供的回调函数发出异步 HTTP/HTTPS 请求和返回数据的功能。

{{< note >}}
虽然此功能目前仅在 Windows 上受支持，但可以扩展到其他平台。
{{< /note >}}

## 开始

要使用 HttpRequestor Gem，必须在项目中启用它。有关更多信息，请参阅 [在项目中添加和删除 Gem](/docs/user-guide/project-config/add-remove-gems/).

`HttpRequestor` Gem使用 AWS C++ 开发工具包。要获取这些依赖项，请将以下内容添加到项目的 CMake 文件中：

```cmake
BUILD_DEPENDENCIES
        PRIVATE
            ...
            Gem::HttpRequestor
            3rdParty::AWSNativeSDK::Core
```

### 在 Gem 上创建依赖项

如果您需要确保在组件中使用 HttpRequestor Gem 之前已启用它，请在组件的`GetRequiredServices`方法中添加要求。例如：

```cpp
void AutomatedTestingSystemComponent::GetRequiredServices(AZ::ComponentDescriptor::DependencyArrayType& required)
{
   ...
   required.push_back(AZ_CRC_CE("HttpRequestorService"));
}
```

### 关闭 Amazon EC2 实例元数据服务调用

HttpRequestor Gem 使用 [AWS C++ SDK](https://github.com/aws/aws-sdk-cpp) 来提供 Http（s） 客户端。您无需提供 AWS 凭证或账户信息即可使用此 Gem。

除非您的项目在 Amazon EC2 计算上运行，否则建议您通过将 [AWS_EC2_METADATA_DISABLED](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)  环境变量设置为“`true`”来关闭 Amazon EC2 实例元数据服务 （IMDS） 查询。
设置此环境变量将防止 SDK 资源不必要地尝试联系 Amazon EC2 IMDS 以获取配置、区域和凭证信息，这可能会导致延迟和网络资源浪费。

```cmd
set AWS_EC2_METADATA_DISABLED=true
```


## C++ API

{{< note >}}
此内容正在移至 [API 参考](https://o3de.org/docs/api/gems/httprequestor/class_http_requestor_1_1_http_requestor_requests.html).
{{< /note >}}

HttpRequestor Gem 有两组 API：一组用于发出返回 JSON 响应的请求，另一组用于返回字符串（文本）响应。

### AddRequest, AddRequestWithHeaders, AddRequestWithHeadersAndBody

使用`AddRequest`, `AddRequestWithHeaders`, 和 `AddRequestWithHeadersAndBody`向任何网站发送通用 HTTP 请求并以 JSON 格式接收返回数据的 API。这些方法返回在 `callback` 参数中接收的数据。

#### Syntax

```cpp
HttpRequestor::HttpRequestorRequestBus::Broadcast(&HttpRequestor::HttpRequestorRequests::AddRequest, URI, method, callback)
```

```cpp
HttpRequestor::HttpRequestorRequestBus::Broadcast(&HttpRequestor::HttpRequestorRequests::AddRequestWithHeaders, URI, method, headers, callback)
```

```cpp
HttpRequestor::HttpRequestorRequestBus::Broadcast(&HttpRequestor::HttpRequestorRequests::AddRequestWithHeadersAndBody, URI, method, headers, body, callback)
```

每个添加请求方法都需要 URI、方法和回调。

#### 参数

|参数 |类型 |描述 |
| --- | --- | --- | 
| URI | `AZStd::String` | 完全限定的 Web 地址，格式如下： `scheme:[//[user:password@]host[:port]][/]path[?query][#fragment]`.  | 
| method | `Aws::Http::HttpMethod` | 方法类型。支持以下值： `HTTP_GET`, `HTTP_POST`, `HTTP_DELETE`, `HTTP_PUT`, `HTTP_HEAD`, and `HTTP_PATCH`. | 
| callback | [see below](#json-request-callback) | 当 HTTP 请求完成时，将调用此函数。响应正文和代码存在于回调中。 | 
| headers | `HttpRequestor::Headers` | HTTP 请求的标头字段列表。 | 
| body | `AZStd::String` | 与请求一起发送的可选正文。 | 

Return：无返回值。

### JSON 请求回调

该回调针对 `AddRequest`, `AddRequestWithHeaders`, 和 `AddRequestWithHeadersAndBody` 等方法返回。

```cpp
void Callback(const Aws::Utils::Json::JsonValue& json, Aws::Http::HttpResponseCode responseCode);
```

#### 参数

|参数 |类型 |描述 |
| --- | --- | --- | 
| json | `Aws::Utils::Json::JsonValue` | JSON 对象。此对象的生命周期仅在回调范围内有效。 | 
| responseCode | `Aws::Http::HttpResponseCode` | HTTP 响应代码。 | 

#### Returns
`void`

### AddTextRequest, AddTextRequestWithHeaders, AddTextRequestWithHeadersAndBody

使用`AddTextRequest`, `AddTextRequestWithHeaders`, 和 `AddTextRequestWithHeadersAndBody`API 向任何网站发送通用 HTTP 请求，并以文本字符串形式接收返回的数据。 这些方法返回在 `callback` 参数中接收的数据。

#### Syntax

```cpp
HttpRequestor::HttpRequestorRequestBus::Broadcast(&HttpRequestor::HttpRequestorRequests::AddTextRequest, URI, method, callback)
```

```cpp
HttpRequestor::HttpRequestorRequestBus::Broadcast(&HttpRequestor::HttpRequestorRequests::AddTextRequestWithHeaders, URI, method, headers, callback)
```

```cpp
HttpRequestor::HttpRequestorRequestBus::Broadcast(&HttpRequestor::HttpRequestorRequests::AddTextRequestWithHeadersAndBody, URI, method, headers, body, callback)
```

每个添加文本请求方法都需要 URI、方法和回调。

#### 参数

|参数 |类型 | 描述                                                                                                                  |
| --- | --- |---------------------------------------------------------------------------------------------------------------------| 
| URI | `AZStd::String` | 完全限定的 Web 地址，格式如下： `scheme:[//[user:password@]host[:port]][/]path[?query][#fragment]`                               | 
| method | `Aws::Http::HttpMethod` | 方法类型。支持以下值： `HTTP_GET`, `HTTP_POST`, `HTTP_DELETE`, `HTTP_PUT`, `HTTP_HEAD`, 和 `HTTP_PATCH`.                        | 
| callback | [see below](#text-request-callback) | 当 HTTP 请求完成时，将调用此函数。响应正文和代码存在于回调中。 | 
| headers | `HttpRequestor::Headers` |HTTP 请求的标头字段列表。                                                                 | 
| body | `AZStd::String` | 与请求一起发送的可选正文。                                                                            | 

#### Returns
`void`

### Text Request Callback

该回调针对 `AddTextRequest`, `AddTextRequestWithHeaders` 和 `AddTextRequestWithHeadersAndBody` 方法返回。

```cpp
void Callback(const AZStd::string& response, Aws::Http::HttpResponseCode responseCode);
```

#### 参数

|参数 |类型 |描述 |
| --- | --- | --- | 
| response | AZStd::string& | 从服务器返回的文本。此对象的生命周期仅在回调范围内有效。 | 
| responseCode | Aws::Http::HttpResponseCode | HTTP 响应代码。 | 

#### Returns
`void`

## 例

以下示例显示了调用 HttpBin 以获取源 IP 地址：

```cpp
#include <aws/core/http/HttpResponse.h>
#include <HttpRequestor/HttpRequestorBus.h>
#include <HttpRequestor/HttpTypes.h>

...

HttpRequestor::HttpRequestorRequestBus::Broadcast(
    &HttpRequestor::HttpRequestorRequests::AddRequest, "https://httpbin.org/ip", Aws::Http::HttpMethod::HTTP_GET,
    [](const Aws::Utils::Json::JsonView& data, Aws::Http::HttpResponseCode responseCode)
    {
        if (responseCode == Aws::Http::HttpResponseCode::OK)
        {
            AZ_Printf("HttpRequestExample",  "Call succeed with %s %d", data.WriteCompact().c_str(), responseCode);
        }
        else
        {
            AZ_Printf("HttpRequestExample", "Request Failed!");
        }
    });
```
