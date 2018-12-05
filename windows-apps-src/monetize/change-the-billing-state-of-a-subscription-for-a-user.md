---
ms.assetid: F37C2CEC-9ED1-4F9E-883D-9FBB082504D4
description: 在 Microsoft Store 购买 API 中使用此方法来更改用户订阅的计费状态。
title: 更改用户订阅的计费状态
ms.date: 08/01/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 购买 API, 订阅
ms.localizationpriority: medium
ms.openlocfilehash: 9e4cf27331a218c0c0ef06ee1a80c141b889504a
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8750125"
---
# <a name="change-the-billing-state-of-a-subscription-for-a-user"></a>更改用户订阅的计费状态

在 Microsoft Store 购买 API 中使用此方法来更改给定用户订阅加载项的计费状态。 你可以取消、延长、退款或禁用订阅的自动续订。

> [!NOTE]
> 此方法仅供已由 Microsoft 预配能够为通用 Windows 平台 (UWP) 应用创建订阅加载项的开发人员帐户使用。 对于大多数开发人员帐户来说，订阅加载项目前尚不可用。

## <a name="prerequisites"></a>先决条件

若要使用此方法，你需要：

* 受众 URI 值为 `https://onestore.microsoft.com` 的 Azure AD 访问令牌。
* Microsoft Store ID 键代表具有想要更改的订阅权利的用户身份。

有关详细信息，请参阅[管理来自服务的产品授权](view-and-grant-products-from-a-service.md)。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/{recurrenceId}/change``` |


### <a name="request-header"></a>请求标头

| 标头         | 类型   | 描述   |
|----------------|--------|-------------|
| 授权  | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。                           |
| Host           | 字符串 | 必须设置为值 **purchase.mp.microsoft.com**。                                            |
| 内容长度 | 数字 | 请求正文的长度。                                                                       |
| Content-Type   | 字符串 | 指定请求和响应类型。 当前，唯一受支持的值为 **application/json**。 |


### <a name="request-parameters"></a>请求参数

| 名称         | 类型  | 说明   |  必填  |
|----------------|--------|-------------|-----------|
| recurrenceId | 字符串 | 想要更改的订阅 ID。 若要获取此 ID，调用[获取用户订阅](get-subscriptions-for-a-user.md)的方法，标识代表想要更改的订阅加载项的响应正文条目**id**字段的值用于条目。     | 是      |


### <a name="request-body"></a>请求正文

| 字段      | 类型   | 说明   | 必填 |
|----------------|--------|---------------|----------|
| b2bKey         | 字符串 | 代表想要更改其订阅的用户身份的 [Microsoft Store ID 密钥](view-and-grant-products-from-a-service.md#step-4)。     | 是      |
| changeType     | 字符串 |  用于标识想要进行的更改类型的以下字符串之一：<ul><li>**Cancel**：取消订阅。</li><li>**Extend**：延长订阅。 如果指定此值，则你必须还包括 *extensionTimeInDays* 参数。</li><li>**Refund**：将订阅款项退还给客户。</li><li>**ToggleAutoRenew**：禁用订阅的自动续订。 如果当前已禁用订阅的自动续订，则此值不起作用。</li></ul>   | 是      |
| extensionTimeInDays  | 字符串  | 如果 *changeType* 参数的值为 **Extend**，则此参数用于指定订阅的延长天数。 |  是，如果 *changeType* 的值为 **Extend**；否则，为否。  ||


### <a name="request-example"></a>请求示例

以下示例展示了如何使用此方法将订阅期延长 5 天。 将 *b2bKey* 值替换为代表想要更改其订阅的用户身份的 [Microsoft Store ID 密钥](view-and-grant-products-from-a-service.md#step-4)。

```json
POST https://purchase.mp.microsoft.com/v8.0/b2b/recurrences/mdr:0:bc0cb6960acd4515a0e1d638192d77b7:77d5ebee-0310-4d23-b204-83e8613baaac/change HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
Host: https://purchase.mp.microsoft.com

{
  "b2bKey":  "eyJ0eXAiOiJ...",
  "changeType": "Extend",
  "extensionTimeInDays": "5"
}
```


## <a name="response"></a>响应

此方法将会返回包含与已修改的订阅加载项（包括已修改的任何字段）相关信息的 JSON 响应正文。 下面的示例展示了此方法的响应正文。

```json
{
  "items": [
    {
      "autoRenew":true,
      "beneficiary":"pub:gFVuEBiZHPXonkYvtdOi+tLE2h4g2Ss0ZId0RQOwzDg=",
      "expirationTime":"2017-06-16T03:07:49.2552941+00:00",
      "id":"mdr:0:bc0cb6960acd4515a0e1d638192d77b7:77d5ebee-0310-4d23-b204-83e8613baaac",
      "lastModified":"2017-01-10T21:08:13.1459644+00:00",
      "market":"US",
      "productId":"9NBLGGH52Q8X",
      "skuId":"0024",
      "startTime":"2017-01-10T21:07:49.2552941+00:00",
      "recurrenceState":"Active"
    }
  ]
}
```


### <a name="response-body"></a>响应正文

响应正文包含以下数据。

| 值        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------|
| autoRenew | 布尔值 |  表示是否已将订阅配置为在当前订阅期结束时自动续订。   |
| 受益人 | 字符串 |  与此订阅关联的权利受益人的 ID。   |
| expirationTime | 字符串 | 订阅截止日期和时间（ISO 8601 格式）。 仅当订阅处于特定状态时，此字段才可用。 截止时间通常表示当前状态截止的时间。 例如，对于活动的订阅，截止日期表示下一次自动续订发生的时间。    |
| expirationTimeWithGrace | 字符串 | 过期日期和时间，订阅将包括在宽限期，采用 ISO 8601 格式。 此值指示当该用户将失去对订阅的访问后失败，订阅自动续订。    |
| id | 字符串 |  订阅 ID。 使用此方法表示在你调用[更改用户订阅的计费状态](change-the-billing-state-of-a-subscription-for-a-user.md)方法时想要修改的订阅。    |
| isTrial | 布尔值 |  表示订阅是否为试用版。     |
| lastModified | 字符串 |  上次修改订阅的日期和时间（ISO 8601 格式）。      |
| market | 字符串 | 用户在其中获取订阅的国家/地区代码（两个字母 ISO 3166-1 alpha-2 格式）。      |
| productId | 字符串 | 表示 Microsoft Store 目录中的订阅加载项的[产品](in-app-purchases-and-trials.md#products-skus-and-availabilities)的 [Store ID](in-app-purchases-and-trials.md#store-ids)。 产品的示例应用商店 ID 为 9NBLGGH42CFD。     |
| skuId | 字符串 |  表示 Microsoft Store 目录中的订阅加载项的 [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) 的 [Store ID](in-app-purchases-and-trials.md#store-ids)。 SKU 的示例应用商店 ID 为 0010。    |
| startTime | 字符串 |  订阅的开始日期和时间（ISO 8601 格式）。     |
| recurrenceState | 字符串  |  以下值之一：<ul><li>**None**：&nbsp;&nbsp;这表示永久订阅。</li><li>**Active**：&nbsp;&nbsp;订阅有效且用户有权使用服务。</li><li>**Inactive**：&nbsp;&nbsp;订阅已过期，并且用户已关闭订阅的自动续订选项。</li><li>**Canceled**：&nbsp;&nbsp;订阅已在截止日期之前被故意终止，提供或不提供退款。</li><li>**InDunning**:&nbsp;&nbsp;订阅正处于*催缴*状态（即，订阅即将过期，且 Microsoft 正尝试获取款项以自动续订此订阅）。</li><li>**Failed**：&nbsp;&nbsp;催缴期已结束，并且在多次尝试后仍无法续订此订阅。</li></ul><p>**注意：**</p><ul><li>**Inactive**/**Canceled**/**Failed** 为终端状态。 当订阅处于这些状态之一时，用户必须重新购买订阅才能再次激活订阅。 在这些状态下，用户无权使用服务。</li><li>当订阅处于 **Canceled** 状态时，expirationTime 将会更新为取消的日期和时间。</li><li>在整个生命周期中，订阅 ID 将保持相同。 打开或关闭自动续订选项并不会更改订阅 ID。 如果用户在达到终端状态之后重新购买了订阅，则会创建新的订阅 ID。</li><li>订阅 ID 可用于在单个订阅上执行任何操作。</li><li>当用户在取消或中断订阅之后重新购买了订阅时，如果查询该用户的结果，则你将会获得两个条目：一个处于终端状态的旧订阅 ID 和一个处于活动状态的新订阅 ID。</li><li>最好始终检查 recurrenceState 和 expirationTime，因为更新至 recurrenceState 可能会延迟几分钟（有时候会是几小时）。       |
| cancellationDate | 字符串   |  用户订阅取消的日期和时间（ISO 8601 格式）。     |


## <a name="related-topics"></a>相关主题


* [管理来自服务的产品授权](view-and-grant-products-from-a-service.md)
* [获取用户订阅](get-subscriptions-for-a-user.md)
* [查询产品](query-for-products.md)
* [将可消费产品报告为已完成](report-consumable-products-as-fulfilled.md)
* [续订 Microsoft Store ID 密钥](renew-a-windows-store-id-key.md)
