---
linkTitle: In-App Purchases
title: In-App Purchases Gem
description: In-App Purchases Gem为 Open 3D Engine （O3DE） 项目中的应用程序内购买提供功能。
toc: true
---

In-App Purchases Gem 为 **Open 3D Engine （O3DE）** 项目中的应用程序内购买提供功能。特定于平台的信息由提供的 API 处理，从而允许您对 Android 和 iOS 使用应用程序内购买 Gem 的单个实现。

{{< note >}}
对于 Android，您必须从 Android SDK 管理器安装 Google Play 账单库。您可以在 extras 部分找到此选项。
{{< /note >}}

## 处理查询和购买请求

您可以通过连接到事件总线 （EBus） 通过代码访问 API：

`EBUS_EVENT(InAppPurchases::InAppPurchasesRequestBus, Initialize);`

要使用 API，请包含 `InAppPurchasesBus.h` 头文件。

您还可以通过 Script Canvas 访问 API。

|方法 |用法 |参数 |
| --- | --- | ---|
| `Initialize` | 所有平台首先调用该方法。此方法在使用 API 之前处理所有必要的设置。 | None |
| `QueryProductInfo` | 当产品 ID 随游戏一起提供时，请使用此方法。此方法查找产品 ID 文件(在Android上为`product_ids.json` 或 在iOS上为`product_ids.plist`)，并使用文件中指定的 ID 检索产品详细信息。 | None |
| `QueryProductInfoByIds` | 如果在运行时提供了产品 ID，例如，如果在运行时从服务器检索产品 ID，则使用此方法可检索产品详细信息。 | `AZStd::vector<AZStd::string>& productIds` |
| `QueryProductInfoById` | 如果在运行时提供了产品 ID，例如，如果在运行时从服务器检索产品 ID，则使用此方法可检索产品详细信息。使用此方法仅检索单个产品的详细信息。 | `AZStd::string& productId` |
| `QueryPurchasedProducts` | 使用此方法可在 Google Play 商店中查询已登录用户已购买的产品。在 iOS 上，此方法读取存储在设备上的收据，并列出已登录用户购买的商品。 | None |
| `RestorePurchasedProducts` | 使用此方法可恢复用户进行的购买。这适用于在其他设备上进行的购买，其中当前设备没有本地存储的收据。此方法仅在 iOS 上受支持。 | None |
| `ConsumePurchase` | 对所有消耗性购买项（如虚拟货币、生命值和弹药）调用此方法。此方法需要 Google Play 商店在购买产品时提供的购买令牌。此方法是必需的，并且仅在 Android 上受支持。 | `AZStd::string& purchaseToken` |
| `FinishTransaction` | 在事务结束时调用该方法。此方法需要 iOS App Store 在购买产品时提供的交易 ID。它还接受一个布尔参数，以指示是否下载 Apple 服务器上托管的内容。如果不调用该方法，则每次重启游戏时都会上报事务。此方法是必需的，并且仅在 iOS 上受支持。 | `AZStd::string& transactionId | bool downloadHostedContent` |
| `PurchaseProduct` | 使用此方法请求从 Google Play Store 或 iOS App Store 购买产品。此方法需要所购买产品的产品 ID。 | `AZStd::string& productId` |
| `PurchaseProductWithDeveloperPayload` | 使用此方法请求从 Google Play Store 或 iOS App Store 购买产品。此方法需要所购买产品的产品 ID。它接受开发人员负载的附加参数，Android 使用该参数将购买与用户帐户相关联。当您请求购买的产品时，您可以使用开发者负载来确定是否已登录的用户进行了购买。在 iOS 上，用户帐户用于欺诈检测。 | `AZStd::string& productId | AZStd::string& developerPayload` |
| `GetCachedProductInfo` | 使用此方法可在每次查询信息时返回存储在缓存中的产品详细信息。缓存仅存储在上次调用 `QueryProductInfo/QueryProductInfoByIds/QueryProductInfoById`期间检索到的产品详细信息。 | None |
| `GetCachedPurchasedProductInfo` | 使用此方法可返回存储在缓存中的已购买产品的详细信息。缓存仅存储在上次调用`QueryPurchasedProducts`期间检索到的已购买产品的详细信息。 | None |
| `ClearCachedProductDetails` | 使用此方法可清除上一次查询产品详细信息调用缓存的产品详细信息。 | None |
| `ClearCachedPurchasedProductDetails` | 使用此方法可清除上一次调用缓存的已购买产品的详细信息，以查询已购买产品的详细信息。 | None |

## 处理对 queries 和 purchases 的响应

当用户进行查询或购买时，API 会将请求发送到 Apple 或 Google 服务器。收到响应后，API 会在 `InAppPurchasesResponse` 总线上广播响应。

要处理对查询或购买的响应，请重载 `InAppPurchasesResponseBus.h` 文件中类中提供的函数。

|方法 |用法 |参数 |
| --- | --- | ---|
| `ProductInfoRetrieved` | 当检索所有请求的产品的产品信息时，将调用此方法。产品信息包括产品 ID、名称、描述、价格等。根据平台的不同，必须将提供的指针强制转换为适当的类型(`ProductDetailsAndroid` 或 `ProductDetailsApple`). | `const AZStd::vector<AZStd::unique_ptr<ProductDetails const> >& productDetails` |
| `PurchasedProductsRetrieved` | 当检索所有已购买产品的详细信息时，将调用此方法。已购买产品的详细信息包括产品 ID、交易 ID、交易时间等。根据平台的不同，必须将提供的指针强制转换为适当的类型 (`PurchasedProductDetailsAndroid` 或 `PurchasedProductDetailsApple`). | `const AZStd::vector<AZStd::unique_ptr<PurchasedProductDetails const> >& purchasedProductDetails` |
| `NewProductPurchased` | 当新产品购买成功时调用该方法。 | `const PurchasedProductDetails* purchasedProductDetails` |
| `PurchaseCancelled` | 取消购买时调用此方法。 | `const PurchasedProductDetails* purchasedProductDetails` |
| `PurchaseRefunded` | 当购买退款时，将调用此方法。 | `const PurchasedProductDetails* purchasedProductDetails` |
| `PurchaseFailed` | 当购买失败时，将调用该方法。 | `const PurchasedProductDetails* purchasedProductDetails` |
| `HostedContentDownloadComplete` | 当 Apple 服务器上托管的内容下载成功时，将调用此方法。提供了交易 ID 和下载路径。 | `const AZStd::string& transactionId | AZStd::string& downloadedFileLocation` |
| `HostedContentDownloadFailed` | 当 Apple 服务器上托管的内容下载失败时，将调用该方法。提供失败内容下载的事务 ID 和内容 ID。 | `const AZStd::string& transactionId | const AZStd::string& contentId ` |
