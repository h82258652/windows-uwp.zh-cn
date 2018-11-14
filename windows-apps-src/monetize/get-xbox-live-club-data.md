---
author: Xansky
description: 在 Microsoft Store 分析 API 中使用此方法获取 Xbox Live 俱乐部数据。
title: 获取 Xbox Live 中心数据
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API, Xbox Live 分析, 俱乐部
ms.localizationpriority: medium
ms.openlocfilehash: fd28e6877f5b2bb3fa7d85f9dbc7c82672b7ea2f
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/08/2018
ms.locfileid: "6145995"
---
# <a name="get-xbox-live-club-data"></a>获取 Xbox Live 中心数据

在 Microsoft Store 分析 API 中使用此方法获取你的[支持 Xbox Live 的游戏](../xbox-live/index.md)的俱乐部数据。 此信息也是在合作伙伴中心中的[Xbox 分析报告](../publish/xbox-analytics-report.md)中可用。

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
| applicationId | 字符串 | 你要检索 Xbox Live 俱乐部数据的游戏的 [Store ID](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| metricType | 字符串 | 指定要检索的 Xbox Live 分析数据的类型的字符串。 对于此方法，指定值 **communitymanagerclub**。  |  是  |
| startDate | 日期 | 要检索的俱乐部数据日期范围中的开始日期。 默认值为当前日期之前 30 天。 |  否  |
| endDate | 日期 | 要检索的俱乐部数据日期范围中的结束日期。 默认值为当前日期。 |  否  |
| top | int | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |  否  |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。 |  否  |


### <a name="request-example"></a>请求示例

以下示例演示了一个请求，该请求用于获取你的支持 Xbox Live 的游戏的俱乐部数据。 将 *applicationId* 值替换为你的游戏的 Store ID。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=communitymanagerclub&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

| 值      | 类型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | array  | 一个数组，其中一个 [ProductData](#productdata) 对象包含与你的游戏相关的俱乐部的数据，一个 [XboxwideData](#xboxwidedata) 对象包含所有 Xbox Live 客户的俱乐部数据。 包含此数据是为了对您的游戏数据进行比较。  |
| @nextLink  | 字符串 | 如果存在其他数据页，则此字符串包含一个你可用来请求下一页数据的 URI。 例如，当请求的 **top** 参数设置为 10000，但查询的数据超过 10000 行时，就会返回此值。 |
| TotalCount | int    | 查询的数据结果中的行总数。 |


### <a name="productdata"></a>ProductData

此资源包含你的游戏的俱乐部数据。

| 值           | 类型    | 说明        |
|-----------------|---------|------|
| date            |  字符串 |   俱乐部数据的日期。   |
|  applicationId               |    字符串     |  你检索了其俱乐部数据的游戏的 [Store ID](in-app-purchases-and-trials.md#store-ids)。   |
|  clubsWithTitleActivity               |    整型     |  使用社交方式参与你的游戏的俱乐部的数量。   |     
|  clubsExclusiveToGame               |   整型      |  使用社交方式仅参与你的游戏的俱乐部的数量。   |     
|  clubFacts               |   数组      |   包含一个或多个有关使用社交方式参与你的游戏的俱乐部的 [ClubFacts](#clubfacts) 对象。   |


### <a name="xboxwidedata"></a>XboxwideData

此资源包含所有 Xbox Live 客户的平均俱乐部数据。

| 值           | 类型    | 说明        |
|-----------------|---------|------|
| date            |  字符串 |   俱乐部数据的日期。   |
|  applicationId  |    字符串     |   在 **XboxwideData** 对象中，此字符串的值始终是 **XBOXWIDE**。  |
|  clubsWithTitleActivity               |   整型     |  具有以社交方式与支持 Xbox Live 的游戏交互的客户的俱乐部平均数量。    |     
|  clubsExclusiveToGame               |   整型      |  具有以社交方式仅与支持 Xbox Live 的游戏交互的客户的俱乐部平均数量。   |     
|  clubFacts               |   object      |  包含一个 [ClubFacts](#clubfacts) 对象。 此对象在 **XboxwideData** 对象上下文中没有意义，并具有默认值。  |


### <a name="clubfacts"></a>clubFacts

在 **ProductData** 对象中，此对象包含具有与你的游戏相关活动的特定俱乐部的数据。 在 **XboxwideData** 对象中，此对象没有意义，并具有默认值。

| 值           | 类型    | 说明        |
|-----------------|---------|--------------------|
|  name            |  字符串  |   在 **ProductData** 对象中，这是俱乐部名称。 在 **XboxwideData** 对象中，它的值始终是 **XBOXWIDE**。           |
|  memberCount               |    整型     | 在 **ProductData** 对象中，这是俱乐部中成员的数量，不包括只访问俱乐部的非成员。 在 **XboxwideData** 对象中，这始终为 0。    |
|  titleSocialActionsCount               |    整型     |  在 **ProductData** 对象中，这是俱乐部中已执行与你的游戏有关的社交操作的数量。 在 **XboxwideData** 对象中，这始终为 0   |
|  isExclusiveToGame               |    Boolean     |  在 **ProductData** 对象中，这表示当前俱乐部是否以社交方式仅与你的游戏交互。 在 **XboxwideData** 对象中，这始终为 true。  |


### <a name="response-example"></a>响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "ProductData": [
        {
          "date": "2018-01-30",
          "applicationId": "9NBLGGGZ5QDR",
          "clubsWithTitleActivity": 3,
          "clubsExclusiveToGame": 1,
          "clubFacts": [
            {
              "name": "Club Contoso Racing",
              "memberCount": 1,
              "titleSocialActionsCount": 1,
              "isExclusiveToGame": false
            },
            {
              "name": "Club Contoso Sports",
              "memberCount": 12,
              "titleSocialActionsCount": 9,
              "isExclusiveToGame": false
            },
            {
              "name": "Club Contoso Maze",
              "memberCount": 4,
              "titleSocialActionsCount": 2,
              "isExclusiveToGame": false
            }
          ]
        }
      ],
      "XboxwideData": [
        {
          "date": "2018-01-30",
          "applicationId": "XBOXWIDE",
          "clubsWithTitleActivity": 25,
          "clubsExclusiveToGame": 3,
          "clubFacts": [
            {
              "name": "XBOXWIDE",
              "memberCount": 0,
              "titleSocialActionsCount": 0,
              "isExclusiveToGame": true
            }
          ]
        }
      ]
    }
  ],
  "@nextLink": "gameanalytics?applicationId=9NBLGGGZ5QDR&metricType=communitymanagerclub&top=1&skip=1&startDate=2018/01/04&endDate=2018/02/02&orderby=date desc",
  "TotalCount": 27
}
```

## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取 Xbox Live 分析数据](get-xbox-live-analytics.md)
* [获取 Xbox Live 成就数据](get-xbox-live-achievements-data.md)
* [获取 Xbox Live 运行状况数据](get-xbox-live-health-data.md)
* [获取 Xbox Live 游戏中心数据](get-xbox-live-game-hub-data.md)
* [获取 Xbox Live 多人游戏数据](get-xbox-live-multiplayer-data.md)
