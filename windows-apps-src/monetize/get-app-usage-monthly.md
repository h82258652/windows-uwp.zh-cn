---
ms.assetid: 4E4CB1E3-D213-4324-91E4-7D4A0EA19C53
description: 在 Microsoft Store analytics API 中使用此方法获取给定日期范围的每月应用使用情况数据和其他可选筛选器。
title: 获取每月的应用使用情况
ms.date: 08/15/2018
ms.topic: article
keywords: windows 10、uwp、应用商店服务、Microsoft Store analytics API、使用情况
ms.localizationpriority: medium
ms.openlocfilehash: a1b82e1f538a0abfda8cb8d4f7ac677464025c8e
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492812"
---
# <a name="get-monthly-app-usage"></a>获取每月的应用使用情况

在给定的日期范围（仅限最近90天）和其他可选筛选器中，使用 Microsoft Store analytics API 中的此方法以 JSON 格式获取应用程序的聚合使用情况数据（不包括 Xbox 多人参与数据）。 合作伙伴中心的[使用情况报告](../publish/usage-report.md)中也提供了此信息。

## <a name="prerequisites"></a>必备条件

若要使用此方法，首先需要执行以下操作：

* 完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)（如果尚未这样做）。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求

### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI                                                                 |
|--------|-----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt; 。 |


### <a name="request-parameters"></a>请求参数

| 参数     | 类型   |  说明                                                                                                    |  必需  |
|---------------|--------|-----------------------------------------------------------------------------------------------------------------|------------|
| applicationId | 字符串 | 要检索评价数据的应用的 [Store ID](in-app-purchases-and-trials.md#store-ids)。 |  是       |
| startDate     | date   | 要检索的评价数据日期范围中的开始日期。 默认值为当前日期。                   |  否        |
| endDate       | 日期   | 要检索的评价数据日期范围中的结束日期。 默认值为当前日期。                     |  否        |
| top           | int    | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。                          |  否        |
| skip          | int    | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。                         |  否        |  
| filter        |string  | 在响应中筛选行的一条或多条语句。 每条语句包含的响应正文中的字段名称和值使用 eq 或 ne 运算符进行关联，并且语句可以使用 and 或 or 进行组合。 filter 参数中的字符串值必须使用单引号引起来。 可以指定响应正文中的以下字段： <ul><li>**market**</li><li>**deviceType**</li><li>**packageVersion**</li></ul>                                                                                                                                              | 否         |  
| orderby       | string | 对结果数据值进行排序的语句。 语法为 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 参数可以是以下字符串之一：<ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**market**</li><li>**packageVersion**</li><li>**deviceType**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p><em>order</em> 参数是可选的，可以是 **asc** 或 **desc**，用于指定每个字段的升序或降序排列。 默认值为**asc**。</p><p>下面是一个 <em>orderby</em> 字符串的示例：<em>orderby=date,market</em></p> |  否        |
| groupby       | string | 仅将数据聚合应用于指定字段的语句。 可以指定响应正文中的以下字段：<ul><li>**applicationName**</li><li>**subscriptionName**</li><li>**deviceType**</li><li>**packageVersion**</li><li>**market**</li><li>**date**</li></ul><p>返回的数据行会包含 <em>groupby</em> 参数中指定的字段，以及以下字段：</p><ul><li>**applicationId**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p><em>groupby</em> 参数可以与 <em>aggregationLevel</em> 参数结合使用。 例如： <em> &amp; Groupby = ageGroup，marketplace &amp; aggregationLevel = week</em></p> |  否        |


### <a name="request-example"></a>请求示例

下面的示例演示获取每月应用使用数据的请求。 将 *applicationId* 值替换为你的应用的 Store ID。

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly?applicationId=XXXXXXXXXXXX&startDate=2018-06-01&endDate=2018-07-01 HTTP/1.1  
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| 值      | 类型   | 说明                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| Value      | array  | 包含聚合使用情况数据的对象的数组。 有关每个对象中的数据的详细信息，请参阅下表。 |
| @nextLink  | string | 如果存在其他数据页，则此字符串包含一个你可用来请求下一页数据的 URI。 例如，当请求的 **top** 参数设置为 10000，但查询的评价数据超过 10000 行时，就会返回此值。                 |
| TotalCount | int    | 查询的数据结果中的行总数。                                                                          |

 
### <a name="usage-values"></a>用法值

*Value* 数组中的元素包含以下值。

| 值                     | 类型    | 描述                                                                                 |
|---------------------------|---------|---------------------------------------------------------------------------------------------|
| date                      | string  | 使用情况数据的日期范围中的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。                          |
| applicationId             | 字符串  | 要为其检索使用情况数据的应用的存储 ID。                            |
| applicationName           | string  | 应用的显示名称。                                                                |
| market                    | string  | 客户使用应用的市场的 ISO 3166 国家/地区代码。                   |
| packageVersion            | string  | 发生使用的包版本。                                            |
| deviceType                | string  | 以下字符串之一，指定发生使用的设备类型：<ul><li>**PC**</li><li>**移动**</li><li>**控制台-Xbox One**</li><li>**控制台-Xbox 系列 X**</li><li>**平板电脑**</li><li>**IoT**</li><li>**Server**</li><li>**Holographic**</li><li>**Unknown**</li></ul>                                                                                                                           |
| subscriptionName          | string  | 指示使用情况是否通过 Xbox 游戏 Pass。                                              |
| monthlySessionCount       | long    | 该月中的用户会话数。                                              |
| engagementDurationMinutes | Double  | 用户主动使用您的应用程序的分钟数，在应用程序启动时（进程开始）和终止（进程结束）或处于不活动状态一段时间后开始。                               |
| monthlyActiveUsers        | long    | 使用当月的应用的客户数。                                           |
| monthlyActiveDevices      | long    | 在不同的时间段内运行你的应用程序的设备的数量，在应用程序启动时（进程开始）和结束（进程结束）或处于不活动状态一段时间后开始。                                                        |
| monthlyNewUsers           | long    | 此月份第一次使用应用的客户数。                    |
| averageDailyActiveUsers   | Double  | 每日使用应用的客户的平均数量。                             |
| averageDailyActiveDevices | Double  | 每日每个用户与应用程序进行交互的平均设备数。 |


### <a name="response-example"></a>响应示例

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
