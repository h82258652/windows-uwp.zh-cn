---
ms.assetid: E9BEB2D2-155F-45F6-95F8-6B36C3E81649
使用 Windows 应用商店收集 API 中的此方法，以面向给定客户将可消费产品报告为已完成。 在用户可以重新购买可消费产品前，你的应用或服务必须面向该用户将可消费产品报告为已完成。
将可消费产品报告为已完成。
---

# 将可消费产品报告为已完成。


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

使用 Windows 应用商店收集 API 中的此方法，以面向给定客户将可消费产品报告为已完成。 在用户可以重新购买可消费产品前，你的应用或服务必须面向该用户将可消费产品报告为已完成。

若要使用此方法来将可消费产品报告为已完成，可采用以下两种方式：

-   提供消费品的项目 ID（如[查询产品](query-for-products.md)的 **itemId** 参数中返回所示）和你提供的唯一跟踪 ID。 如果将同一跟踪 ID 用于多次尝试，那么即使该项目已经使用过，也仍然会返回相同的结果。 如果你不确定消耗请求是否成功，你的服务应重新提交具有同一跟踪 ID 的消耗请求。 跟踪 ID 始终与该消耗请求关联并且可以无限期地重新提交。
-   提供产品 ID（如[查询产品](query-for-products.md)的 **productId** 参数中返回所示）和从以下请求正文部分中的 **transactionId** 参数的描述中所列的源之一获取的事务 ID。

## 先决条件


若要使用此方法，你需要：

-   使用 **https://onestore.microsoft.com** 受众 URI 创建的 Azure AD 访问令牌。
-   一种通过从应用中的客户端代码调用 [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) 方法所生成的Windows 应用商店 ID 密钥。

有关详细信息，请参阅[从服务查看和授予产品](view-and-grant-products-from-a-service.md)。

## 请求


### 请求语法

| 方法 | 请求 URI                                                   |
|--------|---------------------------------------------------------------|
| POST   | https://collections.mp.microsoft.com/v6.0/collections/consume |

 

### 请求标头

| 标头         | 类型   | 描述                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| 授权  | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** <*token*>。                           |
| Host           | 字符串 | 必须设置为值 **collections.mp.microsoft.com**。                                            |
| Content-Length | 数字 | 请求正文的长度。                                                                       |
| Content-Type   | 字符串 | 指定请求和响应类型。 当前，唯一受支持的值为 **application/json**。 |

 

### 请求正文

| 参数     | 类型         | 描述         | 必需 |
|---------------|--------------|---------------------|----------|
| 受益人   | UserIdentity | 正在使用此项目的用户。                                                                                                                                                                                                                                                                 | 是      |
| ItemID        | 字符串       | [查询产品](query-for-products.md)返回的 itemID 值。 将此参数与 trackingId 一起使用                                                                                                                                                                                                  | 否       |
| trackingId    | Guid         | 由开发人员提供的唯一跟踪 ID。 将此参数与 itemId 一起使用。                                                                                                                                                                                                                                     | 否       |
| productId     | 字符串       | [查询产品](query-for-products.md)返回的 productId 值。 将此参数与 transactionId 一起使用                                                                                                                                                                                            | 否       |
| transactionId | Guid         | 从以下源之一获取的事务 ID 值：                                                                                                                                                                                                                                      | 否       | 
|               |              | * [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392) 类的 [TransactionID](https://msdn.microsoft.com/library/windows/apps/dn263396) 属性。   |        | 
|               |              | * 由 [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263381)、[RequestAppPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/hh967813) 或 [GetAppReceiptAsync](https://msdn.microsoft.com/library/windows/apps/hh967811) 返回的应用或产品收据。   |        |
|               |              | * [查询产品](query-for-products.md)返回的 transactionId 参数。   |        |        
|               |              | 将此参数与 productId 一起使用。   |        |
 

UserIdentity 对象包含以下参数。

| 参数            | 类型   | 描述                                                                                                                                 | 必需 |
|----------------------|--------|---------------------------------------------------------------------------------------------------------------------------------------------|----------|
| IdentityType         | 字符串 | 指定字符串值 **b2b**。                                                                                                           | 是      |
| identityValue        | 字符串 | Windows 应用商店 ID 密钥的字符串值。                                                                                                   | 是      |
| localTicketReference | 字符串 | 已返回响应的请求标识符。 我们建议你使用与 Windows 应用商店 ID 密钥中的 *userId* 声明相同的值。 | 是      |

 

### 请求示例

以下示例使用 *itemId* 和 *trackingId*。

```
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1…..
Host: collections.mp.microsoft.com
Content-Length: 2050
Content-Type: application/json

{
    "beneficiary": {
        "localTicketReference": "testreference",
        "identityValue": "eyJ0eXAiOi…..",
        "identityType": "b2b"
    },
    "itemId": "44c26106-4979-457b-af34-609ae97a084f",
    "trackingId": "44db79ca-e31d-49e9-8896-fa5c7f892b40"
}
```

以下示例使用 *productId* 和 *transactionId*。

```
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1……
Content-Length: 1880
Content-Type: application/json
Host: collections.md.mp.microsoft.com

{
    "beneficiary" : {
        "localTicketReference" : "testReference",
        "identityValue" : "eyJ0eXAiOiJ…..",
        "identitytype" : "b2b"
    },
    "productId" : "9NBLGGH5WVP6",
    "transactionId" : "08a14c7c-1892-49fc-9135-190ca4f10490"
}
```

## 响应


如果已成功执行消耗，将不返回任何内容。

### 响应示例

```
HTTP/1.1 204 No Content
Content-Length: 0
MS-CorrelationId: 386f733d-bc66-4bf9-9b6f-a1ad417f97f0
MS-RequestId: e488cd0a-9fb6-4c2c-bb77-e5100d3c15b1
MS-CV: 5.1
MS-ServerId: 030011326
Date: Tue, 22 Sep 2015 20:40:55 GMT
```

## 错误代码


| 代码 | 错误        | 内部错误代码           | 描述                                                                                                                                                                           |
|------|--------------|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 401  | 未授权 | AuthenticationTokenInvalid | Azure AD 访问令牌无效。 在某些情况下，ServiceError 的详细信息包含更多信息，例如令牌到期或 *appid* 声明丢失的时间。 |
| 401  | 未授权 | PartnerAadTicketRequired   | 在授权标头中，Azure AD 访问令牌不会 传递到服务。                                                                                                   |
| 401  | 未授权 | InconsistentClientId       | 请求正文的 Windows 应用商店 ID 密钥中的 *clientId* 声明与授权标头的 Azure AD 访问令牌中的 *appid* 声明不匹配。                     |

 

## 相关主题

* [从服务查看和授予产品](view-and-grant-products-from-a-service.md)
* [查询产品](query-for-products.md)
* [授予免费产品](grant-free-products.md)
* [续订 Windows 应用商店 ID 密钥](renew-a-windows-store-id-key.md)
 

 





<!--HONumber=Mar16_HO1-->


