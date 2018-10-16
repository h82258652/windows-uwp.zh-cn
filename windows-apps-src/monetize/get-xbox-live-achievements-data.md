---
author: Xansky
description: 在 Microsoft Store 分析 API 中使用此方法获取 Xbox Live 成就数据。
title: 获取 Xbox Live 成就数据
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API, Xbox Live 分析, 成就
ms.localizationpriority: medium
ms.openlocfilehash: f4f60cc8ecbdd3d6d05312238166bd666e238e37
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2018
ms.locfileid: "4615995"
---
# <a name="get-xbox-live-achievements-data"></a>获取 Xbox Live 成就数据

在 Microsoft Store 分析 API 中使用此方法获取在成就数据可用的最近一天内、到那天的前 30 天，游戏的总存在时间内到那天为止已经解锁你的[支持 Xbox Live 的游戏](../xbox-live/index.md)的每个成就的客户的数量。 还可以在 Windows 开发人员中心仪表板的 [Xbox 分析报告](../publish/xbox-analytics-report.md)中获取此信息。

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
| applicationId | 字符串 | 你要检索 Xbox Live 成就数据的游戏的 [Store ID](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| metricType | 字符串 | 指定要检索的 Xbox Live 分析数据的类型的字符串。 对于此方法，指定值 **achievements**。  |  是  |
| top | int | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |  否  |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。 |  否  |


### <a name="request-example"></a>请求示例

以下示例演示了一个请求，该请求用于获取已经解锁你的支持 Xbox Live 的游戏前 10 个成就的客户的成就数据。 将 *applicationId* 值替换为你的游戏的 Store ID。


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=achievements&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

| 值      | 类型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | array  | 包含你的游戏中每个成就的数据的对象数组。 有关每个对象中的数据的详细信息，请参阅下表。                                                                                                                      |
| @nextLink  | 字符串 | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求数据的下一页。 例如，当请求的 **top** 参数设置为 100，但查询的数据超过 100 行时，就会返回此值。 |
| TotalCount | int    | 查询的数据结果中的行总数。  |


*Value* 数组中的元素包含以下值。

| 值               | 类型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | 字符串 | 你要检索成就数据的游戏的 Store ID。     |
| reportDateTime     | 字符串 |  成就数据的日期。    |
| achievementId          | 数字 |  成就 ID。 |
| achievementName           | 字符串 | 成就的名称。  |
| gamerscore           | 数字 |  对于成就的玩家分数奖励。  |
| dailyUnlocks           | 数字 |  在 *reportDateTime* 指定的那一天解锁成就的客户的数量。  |
| monthlyUnlocks              | 数字 |  在 *reportDateTime* 指定的那一天之前 30 天内解锁成就的客户的数量。   |
| totalUnlocks | 数字 |  在你的游戏存在时间内，到 *reportDateTime* 指定的那一天解锁成就的客户的数量。   |


### <a name="response-example"></a>响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 6,
      "achievementName": "Yoink!",
      "gamerscore": 10,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 10310,
      "totalUnlocks": 1215360
    },
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 7,
      "achievementName": "Ding!",
      "gamerscore": 10,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 10897,
      "totalUnlocks": 1282524
    },
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 8,
      "achievementName": "End of the Beginning",
      "gamerscore": 30,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 9848,
      "totalUnlocks": 1105074
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}
```

## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取 Xbox Live 分析数据](get-xbox-live-analytics.md)
* [获取 Xbox Live 运行状况数据](get-xbox-live-health-data.md)
* [获取 Xbox Live 游戏中心数据](get-xbox-live-game-hub-data.md)
* [获取 Xbox Live 中心数据](get-xbox-live-club-data.md)
* [获取 Xbox Live 多人游戏数据](get-xbox-live-multiplayer-data.md)
