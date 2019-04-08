---
ms.assetid: 4E4CB1E3-D213-4324-91E4-7D4A0EA19C53
description: 在 Microsoft Store 分析 API 中使用此方法以获取给定的日期范围和其他可选的筛选器的每月应用使用情况数据。
title: 获取每月的应用使用情况
ms.date: 08/15/2018
ms.topic: article
keywords: windows 10、 uwp、 存储区服务、 Microsoft Store 分析 API，使用情况
ms.localizationpriority: medium
ms.openlocfilehash: 48ad049b3f310f8b375a28d9695dd9280d686c43
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662922"
---
# <a name="get-monthly-app-usage"></a>获取每月的应用使用情况

中的 Microsoft Store 分析 API 使用此方法以获取 JSON 格式 （不包括多玩家 Xbox） 的聚合使用情况数据在给定的日期范围内 （过去 90 天仅） 和其他可选的筛选器应用程序。 此信息也位于[使用情况报告](../publish/usage-report.md)在合作伙伴中心。

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

| 标头        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** *token*&lt;&gt;。 |


### <a name="request-parameters"></a>请求参数

| 参数     | 在任务栏的搜索框中键入   |  描述                                                                                                    |  必需  |
|---------------|--------|-----------------------------------------------------------------------------------------------------------------|------------|
| applicationId | 字符串 | 要检索评价数据的应用的 [Store ID](in-app-purchases-and-trials.md#store-ids)。 |  是       |
| startDate     | 日期   | 要检索的评价数据日期范围中的开始日期。 默认值为当前日期。                   |  否        |
| endDate       | 日期   | 要检索的评价数据日期范围中的结束日期。 默认值为当前日期。                     |  否        |
| top           | int    | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。                          |  否        |
| skip          | int    | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。                         |  否        |  
| filter        |字符串  | 在响应中筛选行的一条或多条语句。 每个语句包含的响应正文和与 eq 或 ne 运算符相关联的值中的字段名称，可以使用组合语句和或。 字符串值必须用筛选器参数中的单个引号引起来。 你可以指定响应正文中的以下字段： <ul><li>**market**</li><li>**deviceType**</li><li>**packageVersion**</li></ul>                                                                                                                                              | 否         |  
| orderby       | 字符串 | 对结果数据值进行排序的语句。 语法是 <em>orderby=field [order],field [order],...</em>。<em>field</em> 参数可以是以下字符串之一。<ul><li>**date**</li><li>**applicationId**</li><li>**应用程序名称**</li><li>**market**</li><li>**packageVersion**</li><li>**deviceType**</li><li>**SubscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p><em>order</em> 参数是可选的，可以是 **asc** 或 **desc**，用于指定每个字段的升序或降序排列。 默认值为 **asc**。</p><p>下面是一个 <em>orderby</em> 字符串的示例：<em>orderby=date,market</em></p> |  否        |
| groupby       | 字符串 | 仅将数据聚合应用于指定字段的语句。 你可以指定响应正文中的以下字段：<ul><li>**应用程序名称**</li><li>**SubscriptionName**</li><li>**deviceType**</li><li>**packageVersion**</li><li>**market**</li><li>**date**</li></ul><p>返回的数据行会包含 <em>groupby</em> 参数中指定的字段，以及以下字段：</p><ul><li>**applicationId**</li><li>**SubscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p><em>groupby</em> 参数可以与 <em>aggregationLevel</em> 参数结合使用。 例如：<em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  否        |


### <a name="request-example"></a>请求示例

下面的示例演示了用于获取每月应用使用情况数据请求。 将 *applicationId* 值替换为你的应用的存储 ID。

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly?applicationId=XXXXXXXXXXXX&startDate=2018-06-01&endDate=2018-07-01 HTTP/1.1  
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| 值      | 在任务栏的搜索框中键入   | 描述                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| 值      | 数组  | 包含聚合使用情况数据的对象的数组。 有关每个对象中的数据的详细信息，请参阅下表。 |
| @nextLink  | 字符串 | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求下一页数据。 例如，当请求的 **top** 参数设置为 10000，但查询的评价数据超过 10000 行时，就会返回此值。                 |
| TotalCount | int    | 查询的数据结果中的行总数。                                                                          |

 
### <a name="usage-values"></a>使用数值

*Value* 数组中的元素包含以下值。

| 值                     | 在任务栏的搜索框中键入    | 描述                                                                                 |
|---------------------------|---------|---------------------------------------------------------------------------------------------|
| 日期                      | 字符串  | 使用情况数据的第一个日期的日期范围内。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。                          |
| applicationId             | 字符串  | 要为其检索使用情况数据的应用 Store ID。                            |
| applicationName           | 字符串  | 应用的显示名称。                                                                |
| market                    | 字符串  | 客户使用您的应用程序的位置对市场的 ISO 3166 国家/地区代码。                   |
| packageVersion            | 字符串  | 使用情况的发生位置的包的版本。                                            |
| deviceType                | 字符串  | 以下字符串之一，指定使用情况的发生位置的设备的类型：<ul><li>**PC**</li><li>**Phone**</li><li>**Console**</li><li>**平板电脑**</li><li>**IoT**</li><li>**服务器**</li><li>**Holographic**</li><li>**Unknown**</li></ul>                                                                                                                           |
| subscriptionName          | 字符串  | 指示使用情况是否通过 Xbox Game Pass。                                              |
| monthlySessionCount       | 长整型    | 在该月的用户会话数。                                              |
| engagementDurationMinutes | Double  | 在其中用户会主动使用您的应用程序的非重复的一段时间，在应用启动时启动 （进程开始） 和结束时终止 （进程结束） 或处于非活动状态一段时间后的分钟。                               |
| monthlyActiveUsers        | 长整型    | 使用该月的应用程序的客户数。                                           |
| monthlyActiveDevices      | 长整型    | 设备运行您的应用程序不同的一段时间，在应用启动时启动 （进程开始） 和结束时终止 （进程结束） 或处于非活动状态一段时间后的数。                                                        |
| monthlyNewUsers           | 长整型    | 使用您的应用程序第一次该月的客户数。                    |
| averageDailyActiveUsers   | Double  | 平均每天都使用该应用程序的客户数。                             |
| averageDailyActiveDevices | Double  | 使用与您的应用程序进行交互的设备的每日基础上的所有用户的平均数。 |


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

* [使用 Microsoft Store 服务的访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取每日应用 ussage](get-app-usage-daily.md)
* [获取应用收购](get-app-acquisitions.md)
* [获取外接程序收购](get-in-app-acquisitions.md)
* [获取错误报告数据](get-error-reporting-data.md)
