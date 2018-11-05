---
author: Xansky
ms.assetid: 4E4CB1E3-D213-4324-91E4-7D4A0EA19C53
description: 在 Microsoft Store 分析 API 中使用此方法，可获取给定的日期范围和其他可选筛选器的每月的应用使用情况数据。
title: 获取每月的应用使用情况
ms.author: mhopkins
ms.date: 08/15/2018
ms.topic: article
keywords: windows 10，uwp，应用商店服务，Microsoft Store 分析 API，使用情况
ms.localizationpriority: medium
ms.openlocfilehash: 585e44a884bc90c5c7e69458ad5d024d7f26a79f
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6039680"
---
# <a name="get-monthly-app-usage"></a>获取每月的应用使用情况

在 Microsoft Store 分析 API 中使用此方法采用 JSON 格式的聚合的使用情况数据 （不包括 Xbox 多人游戏） 获取给定的日期范围 （过去 90 天内仅） 和其他可选筛选器应用程序。 此信息也是在合作伙伴中心中的[使用情况报告](../publish/usage-report.md)中可用。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求

### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI                                                                 |
|--------|-----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

| 参数     | 类型   |  说明                                                                                                    |  必需  |
|---------------|--------|-----------------------------------------------------------------------------------------------------------------|------------|
| applicationId | 字符串 | 要检索评价数据的应用的 [Store ID](in-app-purchases-and-trials.md#store-ids)。 |  是       |
| startDate     | date   | 要检索的评价数据日期范围中的开始日期。 默认值为当前日期。                   |  否        |
| endDate       | date   | 要检索的评价数据日期范围中的结束日期。 默认值为当前日期。                     |  否        |
| top           | int    | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。                          |  否        |
| skip          | int    | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。                         |  否        |  
| filter        |字符串  | 在响应中筛选行的一条或多条语句。 每条语句包含的响应正文中的字段名称和值使用 eq 或 ne 运算符进行关联，并且语句可以使用 and 或 or 进行组合。 filter 参数中的字符串值必须使用单引号括起来。 你可以指定响应正文中的以下字段： <ul><li>**market**</li><li>**deviceType**</li><li>**packageVersion**</li></ul>                                                                                                                                              | 否         |  
| orderby       | 字符串 | 对结果数据值进行排序的语句。 语法为 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 参数可以是以下字符串之一：<ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**market**</li><li>**packageVersion**</li><li>**deviceType**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p><em>order</em> 参数是可选的，可以是 **asc** 或 **desc**，用于指定每个字段的升序或降序排列。 默认值为 **asc**。</p><p>下面是一个 <em>orderby</em> 字符串：<em>orderby=date,market</em></p> |  否        |
| groupby       | 字符串 | 仅将数据聚合应用于指定字段的语句。 你可以指定响应正文中的以下字段：<ul><li>**applicationName**</li><li>**subscriptionName**</li><li>**deviceType**</li><li>**packageVersion**</li><li>**market**</li><li>**date**</li></ul><p>返回的数据行会包含 <em>groupby</em> 参数中指定的字段，以及以下字段：</p><ul><li>**applicationId**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p><em>groupby</em> 参数可以与 <em>aggregationLevel</em> 参数结合使用。 例如：<em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  否        |


### <a name="request-example"></a>请求示例

以下示例演示了一个请求用于获取每月的应用使用情况数据。 将 *applicationId* 值替换为你的应用的 Store ID。

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly?applicationId=XXXXXXXXXXXX&startDate=2018-06-01&endDate=2018-07-01 HTTP/1.1  
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| 值      | 类型   | 说明                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| 值      | array  | 包含聚合的使用情况数据的对象数组。 有关每个对象中的数据的详细信息，请参阅以下表格。 |
| @nextLink  | 字符串 | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求下一页数据。 例如，当请求的 **top** 参数设置为 10000，但查询的评价数据超过 10000 行时，就会返回此值。                 |
| TotalCount | int    | 查询的数据结果中的行总数。                                                                          |

 
### <a name="usage-values"></a>使用情况值

*Value* 数组中的元素包含以下值。

| 值                     | 类型    | 说明                                                                                 |
|---------------------------|---------|---------------------------------------------------------------------------------------------|
| date                      | 字符串  | 有关使用情况数据日期范围中的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。                          |
| applicationId             | 字符串  | 要为其检索使用情况数据的应用商店 ID。                            |
| applicationName           | 字符串  | 应用的显示名称。                                                                |
| market                    | 字符串  | 客户使用你的应用的市场的 ISO 3166 国家/地区代码。                   |
| packageVersion            | 字符串  | 使用情况发生的位置的程序包版本。                                            |
| deviceType                | 字符串  | 以下字符串之一，指定使用情况发生的位置的设备的类型：<ul><li>**电脑**</li><li>**电话**</li><li>**控制台**</li><li>**Tablet**</li><li>**IoT**</li><li>**服务器**</li><li>**全息**</li><li>**未知**</li></ul>                                                                                                                           |
| subscriptionName          | 字符串  | 指示用法是通过 Xbox Game Pass。                                              |
| monthlySessionCount       | 长型    | 在该月期间的用户会话的数量。                                              |
| engagementDurationMinutes | Double  | 用户在其中主动使用你的应用由不同的时间段，在应用启动时启动测量 （进程开始） 或终止 （进程结束） 或非活动状态一段时间后结束分钟。                               |
| monthlyActiveUsers        | 长型    | 使用该月的应用的客户数量。                                           |
| monthlyActiveDevices      | 长型    | 设备运行你的应用不同的时间段，在应用启动时启动 （进程开始） 和结束时终止 （进程结束） 或非活动状态一段时间后数。                                                        |
| monthlyNewUsers           | 长型    | 使用你的应用第一次该月的客户数。                    |
| averageDailyActiveUsers   | Double  | 在日常使用该应用的客户的平均数量。                             |
| averageDailyActiveDevices | Double  | 用于与应用交互的日常上的所有用户设备的平均数量。 |


### <a name="response-example"></a>回复示例

以下示例举例说明此请求的 JSON 响应正文。

```http
{
  "Value": [
    {
      "date": "2018-06-01",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "monthlySessionCount": 582973,
      "engagementDurationMinutes": 16941418.7,
      "monthlyActiveUsers": 139604,
      "monthlyActiveDevices": 132296,
      "monthlyNewUsers": 88127,
      "averageDailyActiveUsers": 9099.23,
      "averageDailyActiveDevices": 8999.0
    },
    {
      "date": "2018-07-01",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "monthlySessionCount": 681460,
      "engagementDurationMinutes": 21656645.3,
      "monthlyActiveUsers": 130481,
      "monthlyActiveDevices": 123583,
      "monthlyNewUsers": 78465,
      "averageDailyActiveUsers": 8257.55,
      "averageDailyActiveDevices": 8170.58
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取每日应用 ussage](get-app-usage-daily.md)
* [获取应用购置](get-app-acquisitions.md)
* [获取加载项购置](get-in-app-acquisitions.md)
* [获取错误报告数据](get-error-reporting-data.md)
