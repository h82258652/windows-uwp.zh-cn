---
author: Xansky
description: 在 Microsoft Store 分析 API 中使用此方法获取 Xbox Live 运行状况数据。
title: 获取 Xbox Live 运行状况数据
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API, Xbox Live 分析, 运行状况, 客户端错误
ms.localizationpriority: medium
ms.openlocfilehash: 3d08e081491a93a753c2ba0790eec352a9e2ea85
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6856944"
---
# <a name="get-xbox-live-health-data"></a>获取 Xbox Live 运行状况数据


在 Microsoft Store 分析 API 中使用此方法获取你的[支持 Xbox Live 的游戏](../xbox-live/index.md)的运行状况数据。 此信息也是在合作伙伴中心中的[Xbox 分析报告](../publish/xbox-analytics-report.md)中可用。

> [!IMPORTANT]
> 该方法只支持 Xbox 游戏或使用 Xbox Live 服务的游戏。 这些游戏必须经过[概念审批流程](../gaming/concept-approval.md)，其中包括 [Microsoft 合作伙伴](../xbox-live/developer-program-overview.md#microsoft-partners)发布的游戏以及通过 [ID@Xbox 计划](../xbox-live/developer-program-overview.md#id)提交的游戏。 该方法当前不支持通过 [Xbox Live 创意者计划](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)发布的游戏。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数


| 参数        | 类型   |  说明      |  必需  
|---------------|--------|---------------|------|
| applicationId | 字符串 | 你要检索 Xbox Live 运行状况数据的游戏的 [Store ID](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| metricType | 字符串 | 指定要检索的 Xbox Live 分析数据的类型的字符串。 对于此方法，指定值 **callingpattern**。  |  是  |
| startDate | 日期 | 要检索的运行状况数据日期范围中的开始日期。 默认值为当前日期之前 30 天。 |  否  |
| endDate | 日期 | 要检索的运行状况数据日期范围中的结束日期。 默认值为当前日期。 |  否  |
| top | int | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |  否  |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。 |  否  |
| filter | 字符串  | 在响应中筛选行的一条或多条语句。 每条语句包含的响应正文中的字段名称和值使用 **eq** 或 **ne** 运算符进行关联，并且语句可以使用 **and** 或 **or** 进行组合。 *filter* 参数中的字符串值必须使用单引号括起来。 你可以指定响应正文中的以下字段：<p/><ul><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul> | 否   |
| groupby | 字符串 | 仅将数据聚合应用于指定字段的语句。 你可以指定响应正文中的以下字段：<p/><ul><li><strong>日期型</strong></li><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul><p/>如果你指定一个或多个 *groupby* 字段，则在响应正文中，你未指定的任何其他 *groupby* 字段都为值 **All**。 |  否  |


### <a name="request-example"></a>请求示例

以下示例演示了一个请求，该请求用于获取你的支持 Xbox Live 的游戏的运行状况数据。 将 *applicationId* 值替换为你的游戏的 Store ID。


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=callingpattern&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

| 值      | 类型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | 数字  | 包含运行状况数据的对象数组。 有关每个对象中的数据的详细信息，请参阅下表。                                                                                                                      |
| @nextLink  | 字符串 | 如果存在其他数据页，则此字符串包含一个你可用来请求下一页数据的 URI。 例如，当请求的 **top** 参数设置为 10000，但查询的数据超过 10000 行时，就会返回此值。 |
| TotalCount | int    | 查询的数据结果中的行总数。   |


*Value* 数组中的元素包含以下值。

| 值               | 类型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | 字符串 | 要检索其运行状况数据的游戏的 Store ID。     |
| date                | 字符串 | 运行状况数据的日期范围内的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。 |
| deviceType          | 字符串 | 用于指定在其上使用你的游戏的设备类型的以下字符串之一：<p/><ul><li><strong>XboxOne</strong></li><li><strong>WindowsOneCore</strong>（此值指示电脑）</li><li><strong>未知</strong></li></ul>  |
| sandboxId     | 字符串 |   为游戏创建的沙盒的 ID。 这可以为值 RETAIL，或者是私有沙盒的 ID。   |
| packageVersion     | 字符串 |  游戏的四个部分程序包版本。  |
| callingPattern     | object |  [callingPattern](#callingpattern) 对象为指定日期范围内你的游戏所使用的每个 Xbox Live 服务返回的每个状态代码提供服务响应、设备和用户数据。     |


### <a name="callingpattern"></a>callingPattern

| 值      | 类型   | 说明                  |
|------------|--------|-------------------------------------------------------|
| service      | 字符串  |   运行状况数据与之相关的 Xbox Live 服务的名称。       |
| endpoint      | 字符串  |   运行状况数据与之相关的 Xbox Live 服务的端点。        |
| httpStatusCode      | 字符串  |  这一组运行状况数据的 HTTP 状态代码。<p/><p/>**注意**&nbsp;&nbsp; 状态代码**429E** 指示服务呼叫成功的原因只是在呼叫过程中免除了[精细的速率限制](../xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md)。 如果对服务的需求很高，则可以强制实施精细的速率限制，在这种情况下呼叫服务可能会获得 [HTTP 429 状态代码](../xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md#http-429-response-object)。         |
| serviceResponses      | number  | 返回指定状态代码的服务响应的数量。         |
| uniqueDevices      | number  |  呼叫服务并接收状态代码的不同设备的数量。       |
| uniqueUsers      | number  |   接收指定状态代码的不同用户的数量。       |


### <a name="response-example"></a>响应示例

以下示例举例说明此请求的 JSON 响应正文。 在此示例中显示的服务名称和端点并不是真实的，仅在示例中使用。

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "date": "2018-01-30",
      "deviceType": "All",
      "sandboxId": "RETAIL",
      "packageVersion": "Unknown",
      "callingpattern": [
        {
          "service": "userstats",
          "endpoint": "UserStats.BatchReadHandler.POST",
          "httpStatusCode": "200",
          "serviceResponses": 160891,
          "uniqueDevices": 410,
          "uniqueUsers": 410
        },
        {
          "service": "userstats",
          "endpoint": "UserStats.BatchStatReadHandler.GET",
          "httpStatusCode": "200",
          "serviceResponses": 422,
          "uniqueDevices": 0,
          "uniqueUsers": 30
        }
      ],
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取 Xbox Live 分析数据](get-xbox-live-analytics.md)
* [获取 Xbox Live 成就数据](get-xbox-live-achievements-data.md)
* [获取 Xbox Live 游戏中心数据](get-xbox-live-game-hub-data.md)
* [获取 Xbox Live 中心数据](get-xbox-live-club-data.md)
* [获取 Xbox Live 多人游戏数据](get-xbox-live-multiplayer-data.md)
