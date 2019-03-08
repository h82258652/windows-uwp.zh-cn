---
description: 使用 Microsoft Store 分析 API 中的此方法，可获取给定日期范围和其他可选筛选器内某一加载项的通道聚合转换数据。
title: 通过通道获取加载项转换
ms.date: 08/04/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API, 加载项转换, 通道
ms.localizationpriority: medium
ms.openlocfilehash: 1b1cbc33b2ce53ea7f851e78433b74b103e5a035
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603892"
---
# <a name="get-add-on-conversions-by-channel"></a>通过通道获取加载项转换

使用 Microsoft Store 分析 API 中的此方法，可获取给定日期范围和其他可选筛选器内某一加载项的通道聚合转换。

* *转换*是指客户（使用 Microsoft 帐户登录）最近获取了你的加载项应用许可证（无论你是否收费）。
* *通道*是客户用来访问你的应用的列表页面的方法（例如，通过应用商店或[自定义应用推广活动](../publish/create-a-custom-app-promotion-campaign.md)）。

此信息也位于[外接程序收购报表](../publish/add-on-acquisitions-report.md#add-on-page-views-and-conversions-by-campaign-id)在合作伙伴中心。

## <a name="prerequisites"></a>必备条件

若要使用此方法，首先需要执行以下操作：

* 完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)（如果尚未这样做）。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappchannelconversions``` |


### <a name="request-header"></a>请求头

| 标头        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** *token*&lt;&gt;。 |


### <a name="request-parameters"></a>请求参数

| 参数        | 在任务栏的搜索框中键入   |  描述      |  必需  
|---------------|--------|---------------|------|
| applicationId | 字符串 | 你要检索加载项转换数据的应用的[应用商店 ID](in-app-purchases-and-trials.md#store-ids)。 存储 ID 的一个示例是 9WZDNCRFJ3Q8。 |  是  |
| inAppProductId | 字符串 | 你要检索转换数据的加载项的[应用商店 ID](in-app-purchases-and-trials.md#store-ids)。  | 是  |
| startDate | 日期 | 要检索的转换数据日期范围中的开始日期。 默认值为 1/1/2016。 |  否  |
| endDate | 日期 | 要检索的转换数据日期范围中的结束日期。 默认值为当前日期。 |  否  |
| top | int | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |  否  |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。 |  否  |
| filter | 字符串  | 筛选响应正文的一条或多条语句。 每条语句可以使用 **eq** 或 **ne** 运算符，语句还可以使用 **and** 或 **or** 进行组合。 你可以在筛选器语句中指定以下字符串。  有关说明，请参阅本文的[转换值](#conversion-values)部分。 <ul><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li></ul><p>下面是一个 *filter* 参数示例：<em>filter=deviceType eq 'PC'</em>。</p> | 否   |
| aggregationLevel | 字符串 | 指定用于检索聚合数据的时间范围。 可以是以下字符串之一：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果未指定，默认值为 <strong>day</strong>。 | 否 |
| orderby | 字符串 | 对每个转换的结果数据值进行排序的语句。 语法是 <em>orderby=field [order],field [order],...</em>。<em>field</em> 参数可以是以下字符串之一。<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li></ul><p><em>order</em> 参数是可选的，可以是 <strong>asc</strong> 或 <strong>desc</strong>，用于指定每个字段的升序或降序排列。 默认值为 <strong>asc</strong>。</p><p>下面是一个 <em>orderby</em> 字符串的示例：<em>orderby=date,market</em></p> |  否  |
| groupby | 字符串 | 仅将数据聚合应用于指定字段的语句。 可以指定的字段如下所示：<p/><ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li></ul><p>返回的数据行会包含 <em>groupby</em> 参数中指定的字段，以及以下字段：</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>inAppProductId</strong></li><li><strong>inAppProductName</strong></li><li><strong>conversionCount</strong></li><li><strong>clickCount</strong></li></ul><p><em>groupby</em> 参数可以与 <em>aggregationLevel</em> 参数结合使用。 例如：<em>groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  否  |


### <a name="request-example"></a>请求示例

以下示例演示用于获取应用转换数据的多个请求。 将 *applicationId* 值替换为你的应用的存储 ID。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=4/31/2017&skip=0&filter=market eq 'US'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| 值      | 在任务栏的搜索框中键入   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | 数组  | 包含聚合加载项转换数据的对象数组。 有关每个对象中的数据的详细信息，请参阅以下[转换值](#conversion-values)部分。                     |
| @nextLink  | 字符串 | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求下一页数据。 例如，当请求的 **top** 参数设置为 10，但查询的转换数据超过 10 行时，就会返回此值。 |
| TotalCount | int    | 查询的数据结果中的行总数。                                                                                                                                                                                                                             |

### <a name="conversion-values"></a>转换值

在 *Value* 数组中的对象包含以下值。

| 值               | 在任务栏的搜索框中键入   | 描述                           |
|---------------------|--------|-------------------------------------------|
| 日期                | 字符串 | 转换数据的日期范围内的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。 |
| inAppProductId      | 字符串  | 要检索转换数据的加载项的[应用商店 ID](in-app-purchases-and-trials.md#store-ids)。     |
| inAppProductName    | 字符串  | 要检索转换数据的加载项的显示名称。   |
| applicationId       | 字符串 | 要检索转换数据的应用的[应用商店 ID](in-app-purchases-and-trials.md#store-ids)。     |
| applicationName     | 字符串 | 要检索转换数据的应用的显示名称。        |
| appType          | 字符串 |  要检索转换数据的产品的类型。 对于此方法，唯一受支持的值是 **Add-On**。            |
| customCampaignId           | 字符串 |  与应用关联的[自定义应用推广活动](../publish/create-a-custom-app-promotion-campaign.md)的 ID 字符串。   |
| referrerUriDomain           | 字符串 |  指定激活应用列表及自定义应用推广市场 ID 时所在的域。   |
| channelType           | 字符串 |  指定用于转换的通道的以下字符串之一：<ul><li><strong>CustomCampaignId</strong></li><li><strong>存储流量</strong></li><li><strong>其他</strong></li></ul>    |
| storeClient         | 字符串 | 发生转换的应用商店的版本。 当前，唯一受支持的值为 **SFC**。    |
| deviceType          | 字符串 | 以下字符串之一：<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>            |
| market              | 字符串 | 发生转换的市场的 ISO 3166 国家/地区代码。    |
| clickCount              | 数字  |     单击你的应用列表链接的客户数量。      |           
| conversionCount            | 数字  |   客户转换数。         |         


### <a name="response-example"></a>响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "date": "2016-01-01",
      "inAppProductId": "9NBLGGH3LHKL",
      "inAppProductName": "Contoso Add-On",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso App",
      "appType": "Add-On",
      "customCampaignId": "",
      "referrerUriDomain": "Universal Client Store",
      "channelType": "Store Traffic",
      "storeClient": "SFC",
      "deviceType": "PC",
      "market": "CN",
      "clickCount": 1,
      "conversionCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>相关主题

* [加载项购置报告](../publish/add-on-acquisitions-report.md)
* [使用 Microsoft Store 服务的访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取外接程序收购](get-in-app-acquisitions.md)
