---
author: Xansky
description: 使用 Microsoft Store 分析 API 中的此方法，可获取给定日期范围和其他可选筛选器内某一应用程序的通道聚合转换数据。
title: 通过通道获取应用转换
ms.author: mhopkins
ms.date: 08/04/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API, 应用转换, 通道
ms.localizationpriority: medium
ms.openlocfilehash: 60e166b70c6a2aacf20673b30461002f3aa61305
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5569678"
---
# <a name="get-app-conversions-by-channel"></a>通过通道获取应用转换

使用 Microsoft Store 分析 API 中的此方法，可获取给定日期范围和其他可选筛选器内某一应用程序的通道聚合转换。

* *转换*是指客户（使用 Microsoft 帐户登录）最近获取了你的应用许可证（无论你是否收费）。
* *通道*是客户用来访问你的应用的列表页面的方法（例如，通过应用商店或[自定义应用推广活动](../publish/create-a-custom-app-promotion-campaign.md)）。

还可以在 Windows 开发人员中心仪表板的[购置报告](../publish/acquisitions-report.md#app-page-views-and-conversions-by-channel)中获取此信息。

## <a name="prerequisites"></a>先决条件


若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

| 参数        | 类型   |  说明      |  必需  
|---------------|--------|---------------|------|
| applicationId | string | 你要检索转换数据的应用的[应用商店 ID](in-app-purchases-and-trials.md#store-ids)。 应用商店 ID 的一个示例是 9WZDNCRFJ3Q8。 |  是  |
| startDate | 日期 | 要检索的转换数据日期范围中的开始日期。 默认值为 1/1/2016。 |  否  |
| endDate | 日期 | 要检索的转换数据日期范围中的结束日期。 默认值为当前日期。 |  否  |
| top | int | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |  否  |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。 |  否  |
| filter | string  | 筛选响应正文的一条或多条语句。 每条语句都可以使用 **eq** 或 **ne** 运算符，多条语句还可以使用 **and** 或 **or** 进行组合。 你可以在筛选器语句中指定以下字符串。 有关说明，请参阅本文的[转换值](#conversion-values)部分。 <ul><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li></ul><p>下面是一个 *filter* 参数示例：<em>filter=deviceType eq 'PC'</em>。</p> | 否   |
| aggregationLevel | 字符串 | 指定用于检索聚合数据的时间范围。 可以是以下字符串之一：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果未指定，默认值为 <strong>day</strong>。 | 否 |
| orderby | string | 对每个转换的结果数据值进行排序的语句。 语法为 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 参数可以是以下字符串之一：<ul><li><strong>日期</strong></li><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li></ul><p><em>order</em> 参数为可选，它可以是 <strong>asc</strong> 或 <strong>desc</strong>，以指定对每个字段进行升序还是降序排列。 默认值为 <strong>asc</strong>。</p><p>下面是一个 <em>orderby</em> 字符串：<em>orderby=date,market</em></p> |  否  |
| groupby | 字符串 | 仅将数据聚合应用于指定字段的语句。 可以指定的字段如下所示：<ul><li><strong>日期型</strong></li><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li></ul><p>返回的数据行会包含 <em>groupby</em> 参数中指定的字段，以及以下字段：</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>conversionCount</strong></li><li><strong>clickCount</strong></li></ul><p><em>groupby</em> 参数可以与 <em>aggregationLevel</em> 参数结合使用。 例如：<em>groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  否  |


### <a name="request-example"></a>请求示例

以下示例演示用于获取应用转换数据的多个请求。 将 *applicationId* 值替换为应用的应用商店 ID。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=4/31/2017&skip=0&filter=market eq 'US'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| 值      | 类型   | 说明                  |
|------------|--------|-------------------------------------------------------|
| 值      | array  | 包含聚合应用转换数据的对象数组。 有关每个对象中的数据的详细信息，请参阅以下[转换值](#conversion-values)部分。                                                                                                                      |
| @nextLink  | string | 如果存在其他数据页，此字符串中包含的 URI 可用于请求下一页数据。 例如，当请求的 **top** 参数设置为 10，但查询的转换数据超过 10 行时，就会返回此值。 |
| TotalCount | int    | 查询的数据结果中的行总数。      |


### <a name="conversion-values"></a>转换值

在 *Value* 数组中的对象包含以下值。

| 值               | 类型   | 说明                           |
|---------------------|--------|-------------------------------------------|
| date                | string | 转换数据的日期范围内的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。 |
| applicationId       | string | 要检索转换数据的应用的[应用商店 ID](in-app-purchases-and-trials.md#store-ids)。     |
| applicationName     | string | 要检索转换数据的应用的显示名称。        |
| appType          | string |  要检索转换数据的产品的类型。 对于此方法，唯一受支持的值是 **App**。            |
| customCampaignId           | string |  与应用关联的[自定义应用推广活动](../publish/create-a-custom-app-promotion-campaign.md)的 ID 字符串。   |
| referrerUriDomain           | string |  指定激活应用列表及自定义应用推广市场 ID 时所在的域。   |
| channelType           | string |  指定用于转换的通道的以下字符串之一：<ul><li><strong>CustomCampaignId</strong></li><li><strong>应用商店流量</strong></li><li><strong>其他</strong></li></ul>    |
| storeClient         | string | 发生转换的应用商店的版本。 当前，唯一受支持的值为 **SFC**。    |
| deviceType          | 字符串 | 以下字符串之一：<ul><li><strong>电脑</strong></li><li><strong>电话</strong></li><li><strong>控制台</strong></li><li><strong>IoT</strong></li><li><strong>全息</strong></li><li><strong>Unknown</strong></li></ul>            |
| market              | string | 发生转换的市场的 ISO 3166 国家/地区代码。    |
| clickCount              | 数字  |     单击你的应用列表链接的客户数量。      |           
| conversionCount            | 数字  |   客户转换数。         |          


### <a name="response-example"></a>响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "date": "2016-01-01",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso App",
      "appType": "App",
      "customCampaignId": "",
      "referrerUriDomain": "Universal Client Store",
      "channelType": "Store Traffic",
      "storeClient": "SFC",
      "deviceType": "PC",
      "market": "US",
      "clickCount": 7,
      "conversionCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>相关主题

* [购置报告](../publish/acquisitions-report.md)
* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取应用购置](get-app-acquisitions.md)
