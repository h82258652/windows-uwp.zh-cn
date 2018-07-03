---
author: mcleanbyron
description: 在 Microsoft Store 分析 API 中使用此方法获取 Xbox Live 并行使用情况数据。
title: 获取 Xbox Live 并发使用情况数据
ms.author: mcleans
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API, Xbox Live 分析, 并行使用情况
ms.localizationpriority: medium
ms.openlocfilehash: 47e9a23ba046e70a4d755a3ac9dc1bbf4d62f307
ms.sourcegitcommit: 633dd07c3a9a4d1c2421b43c612774c760b4ee58
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2018
ms.locfileid: "1976172"
---
# <a name="get-xbox-live-concurrent-usage-data"></a>获取 Xbox Live 并发使用情况数据


在 Microsoft Store 分析 API 中使用此方法获取近乎实时的使用情况数据（有 5-15 分钟的延迟），这些数据为指定时间范围内每分钟、每小时或每天玩你的[支持 Xbox Live 的游戏](../xbox-live/index.md)的客户的平均数量。 还可以在 Windows 开发人员中心仪表板的 [Xbox 分析报告](../publish/xbox-analytics-report.md)中获取此信息。

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
| applicationId | 字符串 | 你要检索 Xbox Live 并行使用情况数据的游戏的 [Store ID](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| metricType | 字符串 | 指定要检索的 Xbox Live 分析数据的类型的字符串。 对于此方法，指定值 **concurrency**。  |  是  |
| startDate | 日期 | 要检索的并行使用情况数据日期范围中的开始日期。 请参阅 *aggregationLevel* 说明了解默认行为。 |  否  |
| endDate | date | 要检索的并行使用情况数据日期范围中的结束日期。 请参阅 *aggregationLevel* 说明了解默认行为。 |  否  |
| aggregationLevel | 字符串 | 指定用于检索聚合数据的时间范围。 可以是以下字符串之一：**minute**、**hour** 或 **day**。 如果未指定，默认值为 **day**。 <p/><p/>如果你没有指定 *startDate* 或 *endDate*，响应正文将默认为以下值： <ul><li>**minute**：可用数据的前 60 个记录。</li><li>**hour**：可用数据的前 24 个记录。</li><li>**day**：可用数据的前 7 个记录。</li></ul><p/>以下聚合级别对于可返回的记录数有大小限制。 如果请求的时间范围太大，记录将被截断。 <ul><li>**minute**：最多 1440 个记录（24 小时的数据）。</li><li>**hour**：最多 720 个记录（30 天的数据）。</li><li>**day**：最多 60 个记录（60 天的数据）。</li></ul>  |  否  |


### <a name="request-example"></a>请求示例

以下示例演示了一个请求，该请求用于获取你的支持 Xbox Live 的游戏的并行使用情况数据。 此请求检索 2018 年 2 月 1 日与 2018 年 2 月 2 日之间每分钟的数据。 将 *applicationId* 值替换为你的游戏的 Store ID。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=concurrency&aggregationLevel=hour&startDate=2018-02-01&endData=2018-02-02 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

响应正文包含一组对象，其中每个对象都包含一组指定分钟、小时或者天的并行使用情况数据。 每个对象都包含以下值：

| 值      | 类型   | 说明                  |
|------------|--------|-------------------------------------------------------|
| Count      | 数字  | 指定分钟、小时或天玩你的支持 Xbox Live 的游戏的客户平均数量。 <p/><p/>**注意**&nbsp;&nbsp; 值为 0 表示在指定的时间间隔内没有并行用户，或收集指定时间间隔内游戏的并行用户数据时出现故障。 |
| 日期  | 字符串 | 指定产生并行使用情况数据的分钟、小时或天的日期和视觉。  |
| SeriesName | 字符串    | 它的值始终为 **UserConcurrency**。 |


### <a name="response-example"></a>响应示例

以下示例举例说明按分钟请求数据聚合时的 JSON 响应正文。

```json
[   {
        "Count": 418.0,
        "Date": "2018-02-02T04:42:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 418.0,
        "Date": "2018-02-02T04:43:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 415.0,
        "Date": "2018-02-02T04:44:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 412.0,
        "Date": "2018-02-02T04:45:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 414.0,
        "Date": "2018-02-02T04:46:13.65Z",
        "SeriesName": "UserConcurrency"
    }
]
```

## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取 Xbox Live 分析数据](get-xbox-live-analytics.md)
* [获取 Xbox Live 成就数据](get-xbox-live-achievements-data.md)
* [获取 Xbox Live 运行状况数据](get-xbox-live-health-data.md)
* [获取 Xbox Live 游戏中心数据](get-xbox-live-game-hub-data.md)
* [获取 Xbox Live 中心数据](get-xbox-live-club-data.md)
* [获取 Xbox Live 多人游戏数据](get-xbox-live-multiplayer-data.md)
