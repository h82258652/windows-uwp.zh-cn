---
description: 在 Microsoft Store 分析 API 中使用此方法获取 Xbox Live 多玩家数据。
title: 获取 Xbox Live 多人游戏数据
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API, Xbox Live 分析, 多玩家
ms.localizationpriority: medium
ms.openlocfilehash: 58f470abdf7cbf0770bf01dd123a8fdfd2c2cbea
ms.sourcegitcommit: e63fbd7a63a7e8c03c52f4219f34513f4b2bb411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2019
ms.locfileid: "58162992"
---
# <a name="get-xbox-live-multiplayer-data"></a>获取 Xbox Live 多人游戏数据


在 Microsoft Store 分析 API 中使用此方法每天或每月获取你的[支持 Xbox Live 的游戏](https://docs.microsoft.com/gaming/xbox-live//index.md)的多玩家数据。 此信息也位于[Xbox 的分析报告](../publish/xbox-analytics-report.md)在合作伙伴中心。

> [!IMPORTANT]
> 该方法只支持 Xbox 游戏或使用 Xbox Live 服务的游戏。 这些游戏必须经过[概念审批流程](../gaming/concept-approval.md)，其中包括 [Microsoft 合作伙伴](https://docs.microsoft.com/gaming/xbox-live//developer-program-overview.md#microsoft-partners)发布的游戏以及通过 [ID@Xbox 计划](https://docs.microsoft.com/gaming/xbox-live//developer-program-overview.md#id)提交的游戏。 该方法当前不支持通过 [Xbox Live 创意者计划](https://docs.microsoft.com/gaming/xbox-live//get-started-with-creators/get-started-with-xbox-live-creators.md)发布的游戏。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)（如果尚未这样做）。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>请求头

| Header        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | string | 必需。 Azure AD 访问令牌的格式为 **Bearer** *token*&lt;&gt;。 |


### <a name="request-parameters"></a>请求参数


| 参数        | 在任务栏的搜索框中键入   |  描述      |  必需  
|---------------|--------|---------------|------|
| applicationId | string | 你要检索其 Xbox Live 多玩家数据的游戏的 [Store ID](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| metricType | string | 指定要检索的 Xbox Live 分析数据的类型的字符串。 对于此方法，指定 **multiplayerdaily** 获取每日多玩家数据，或指定 **multiplayermonthly** 获取每月多玩家数据。  |  是  |
| startDate | 日期 | 要检索的多玩家数据日期范围中的开始日期。 对于 **multiplayerdaily**，默认值是当前日期之前 3 个月。 对于 **multiplayermonthly**，默认值是当前日期之前 1 年。 |  否  |
| endDate | 日期 | 要检索的多玩家数据日期范围中的结束日期。 默认值为当前日期。 |  否  |
| top | int | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |  否  |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。 |  否  |
| filter | string  | 在响应中筛选行的一条或多条语句。 每条语句包含的响应正文中的字段名称和值使用 **eq** 或 **ne** 运算符进行关联，并且语句可以使用 **and** 或 **or** 进行组合。 *filter* 参数中的字符串值必须使用单引号括起来。 你可以指定响应正文中的以下字段：<p/><ul><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>market</strong></li><li><strong>subscriptionName</strong></li></ul> | 否   |
| groupby | string | 仅将数据聚合应用于指定字段的语句。 你可以指定响应正文中的以下字段：<p/><ul><li><strong>date</strong></li><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>market</strong></li><li><strong>subscriptionName</strong></li></ul><p/>如果你指定一个或多个 *groupby* 字段，则在响应正文中，你未指定的任何其他 *groupby* 字段都为值 **All**。 |  否  |


### <a name="request-example"></a>请求示例

以下示例演示了一个请求，该请求用于获取你的支持 Xbox Live 的游戏的多玩家数据。 将 *applicationId* 值替换为你的游戏的 Store ID。


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=multiplayerdaily&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

| ReplTest1      | 在任务栏的搜索框中键入   | 描述                  |
|------------|--------|-------------------------------------------------------|
| ReplTest1      | 数组  | 包含多玩家数据的一个对象数组，其中的每个对象表示指定每日或每月时间周期的一组数据，这组数据按指定的 **filter** 和 **groupby** 值进行组织。 有关每个对象中的数据的详细信息，请参阅[每日多玩家分析](#daily-multiplayer-analytics)和[每月多玩家分析](#monthly-multiplayer-analytics)部分。     |
| @nextLink  | string | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求下一页数据。 例如，当请求的 **top** 参数设置为 10000，但查询的数据超过 10000 行时，就会返回此值。 |
| TotalCount | int    | 查询的数据结果中的行总数。 |


### <a name="daily-multiplayer-analytics"></a>每日多玩家分析

当您请求每日多玩家分析数据（即当您为 **metricType** 参数指定 **multiplayerdaily**）时，*Value* 数组中的元素包含以下值。

| ReplTest1               | 在任务栏的搜索框中键入   | 描述                           |
|---------------------|--------|-------------------------------------------|
| 日期                | string | 多玩家数据的日期。 |
| applicationId       | string | 你要检索多玩家数据的游戏的 Store ID。     |
| applicationName       | string |  你要检索多玩家数据的游戏的名称。     |
| market       | string | 多玩家数据的来源市场的两个字母 ISO 3166 国家/地区代码。       |
| packageVersion     | string |  游戏的四个部分程序包版本。  |
| deviceType          | string | 用于指定作为多玩家数据来源的设备类型的以下字符串之一：<p/><ul><li><strong>Console</strong></li><li><strong>PC</strong></li><li>**Unknown**</li></ul>  |
| subscriptionName     | string |  用于多玩家数据订阅的名称。 可能的值包括 **Xbox Game Pass** 和 **""**（适用于任何订阅）。  |
| dailySessionCount     | 数字 |  在指定日期的游戏多玩家会话数量。  |
| engagementDurationMinutes     | 数字 |  客户在指定日期参与游戏的多玩家会话的总分钟数。  |
| dailyActiveUsers     | 数字 |  在指定日期，游戏的活动的多玩家用户总数。  |
| dailyActiveDevices     | 数字 |  在指定日期，参与到游戏中的多玩家会话中的活动设备总数。  |
| dailyNewUsers     | 数字 |  在指定日期，游戏的新的多玩家用户总数。  |
| monthlyActiveUsers     | 数字 |  在指定日期所在的月份中活动的多玩家用户的总数。  |
| monthlyActiveDevices     | 数字 | 在指定日期所在月份参与到游戏中的多玩家会话中的活动设备的总数。   |
| monthlyNewUsers     | 数字 |  在指定日期所在的月份中游戏的新多玩家用户的总数。  |


### <a name="monthly-multiplayer-analytics"></a>每月多人游戏分析

当您请求每月多人游戏分析数据（即当您为 **metricType** 参数指定 **multiplayermonthly**）时，*Value* 数组中的元素包含以下值。

| ReplTest1               | 在任务栏的搜索框中键入   | 描述                           |
|---------------------|--------|-------------------------------------------|
| 日期                | string | 多玩家数据的月份的第一个日期。 |
| applicationId       | string | 你要检索多玩家数据的游戏的 Store ID。     |
| applicationName       | string |  你要检索多玩家数据的游戏的名称。     |
| market       | string | 多玩家数据的来源市场的两个字母 ISO 3166 国家/地区代码。       |
| packageVersion     | string |  游戏的四个部分程序包版本。  |
| deviceType          | string | 用于指定作为多玩家数据来源的设备类型的以下字符串之一：<p/><ul><li><strong>Console</strong></li><li><strong>PC</strong></li><li>**Unknown**</li></ul>  |
| subscriptionName     | string |  用于多玩家数据订阅的名称。 可能的值包括 **Xbox Game Pass** 和 **""**（适用于任何订阅）。  |
| monthlySessionCount     | 数字 |  在指定月份中游戏的多玩家会话的数量。   |
| engagementDurationMinutes     | 数字 |  客户在指定月份参与游戏的多玩家会话的总分钟数。  |
| monthlyActiveUsers     | 数字 | 在指定月份内活动的多玩家用户的总数。   |
| monthlyActiveDevices     | 数字 | 在指定月份内参与到游戏的多玩家会话中的活动设备的总数。   |
| monthlyNewUsers     | 数字 |  在指定月份内游戏的新多玩家用户的总数。  |
| averageDailyActiveUsers     | 数字 |  在指定月份内游戏的每日活动多玩家用户的平均数量。  |
| averageDailyActiveDevices     | 数字 |  在指定月份内参与到游戏的多玩家会话中的活动设备的平均数量。  |


### <a name="response-example"></a>响应示例

下面的示例演示了此请求（即为 **metricType** 参数指定 **multiplayerdaily**）的每日指标变体的 JSON 响应正文的示例。

```json
{
  "Value": [
    {
      "date": "2018-01-07",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Sports",
      "market": "All",
      "packageVersion": "",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 976711,
      "engagementDurationMinutes": 16836064.5,
      "dailyActiveUsers": 180377,
      "dailyActiveDevices": 153359,
      "dailyNewUsers": 8638,
      "monthlyActiveUsers": 779984,
      "monthlyActiveDevices": 606495,
      "monthlyNewUsers": 212093
    },
    {
      "date": "2018-01-05",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Sports",
      "market": "All",
      "packageVersion": "",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 857433,
      "engagementDurationMinutes": 14087724.5,
      "dailyActiveUsers": 166630,
      "dailyActiveDevices": 143065,
      "dailyNewUsers": 9646,
      "monthlyActiveUsers": 764947,
      "monthlyActiveDevices": 595368,
      "monthlyNewUsers": 204248
    },
  ],
  "@nextLink": null,
  "TotalCount":2
}
```

## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务的访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取 Xbox Live 分析数据](get-xbox-live-analytics.md)
* [获取 Xbox Live 成就数据](get-xbox-live-achievements-data.md)
* [获取 Xbox Live 的运行状况数据](get-xbox-live-health-data.md)
* [获取 Xbox Live 游戏中心数据](get-xbox-live-game-hub-data.md)
* [获取 Xbox Live 俱乐部数据](get-xbox-live-club-data.md)
