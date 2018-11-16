---
author: Xansky
ms.assetid: 235EBA39-8F64-4499-9833-4CCA9C737477
description: 使用 Microsoft Store 分析 API 中的此方法，可获取给定日期范围和其他可选筛选器内某一应用程序的广告性能聚合数据。
title: 获取广告性能数据
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API, 广告, 性能
ms.localizationpriority: medium
ms.openlocfilehash: 7310eeb04915933adc149165fa6774ed2f413814
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "6993558"
---
# <a name="get-ad-performance-data"></a>获取广告性能数据


使用 Microsoft Store 分析 API 中的此方法，可获取给定日期范围和其他可选筛选器内你的应用程序的广告性能聚合数据。 此方法返回采用 JSON 格式的数据。

此方法返回[的广告性能报告](../publish/advertising-performance-report.md)合作伙伴中心中提供的相同数据。

## <a name="prerequisites"></a>先决条件


若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

有关详细信息，请参阅[使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/adsperformance``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 说明           |
|---------------|--------|--------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

若要检索特定应用的广告性能数据，请使用 *applicationId* 参数。 若要检索与你的开发者帐户关联的所有应用的广告性能数据，请忽略 *applicationId* 参数。

| 参数     | 类型   | 说明     | 必需 |
|---------------|--------|-----------------|----------|
| applicationId   | 字符串    | 要检索广告性能数据的应用的 [Store ID](in-app-purchases-and-trials.md#store-ids)。  |    否      |
| startDate   | 日期型    | 要检索的广告性能数据日期范围中的开始日期，格式为 YYYY/MM/DD。 默认值为当前日期减去 30 天。 |    否      |
| endDate   | 日期型    | 要检索的广告性能数据日期范围中的结束日期，格式为 YYYY/MM/DD。 默认值为当前日期减去 1 天。 |    否      |
| top   | int    | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |    否      |
| skip   | int    | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。 |    否      |
| filter   | 字符串    | 在响应中筛选行的一条或多条语句。 有关详细信息，请参阅下面的[筛选器字段](#filter-fields)部分。 |    否      |
| aggregationLevel   | 字符串    | 指定用于检索聚合数据的时间范围。 可以是以下字符串之一：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果未指定，默认值为 <strong>day</strong>。 |    否      |
| orderby   | 字符串    | 对结果数据值进行排序的语句。 语法为 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 参数可以是以下字符串之一：<ul><li><strong>date</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>adUnitId</strong></li></ul><p><em>order</em> 参数是可选的，可以是 <strong>asc</strong> 或 <strong>desc</strong>，用于指定每个字段的升序或降序排列。 默认值为 <strong>asc</strong>。</p><p>下面是一个 <em>orderby</em> 字符串：<em>orderby=date,market</em></p> |    否      |
| groupby   | 字符串    | 仅将数据聚合应用于指定字段的语句。 可以指定的字段如下所示：</p><ul><li><strong>applicationId</strong></li><li><strong>applicationName</strong></li><li><strong>date</strong></li><li><strong>accountCurrencyCode</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>adUnitName</strong></li><li><strong>adUnitId</strong></li><li><strong>pubCenterAppName</strong></li><li><strong>adProvider</strong></li></ul><p><em>groupby</em> 参数可以与 <em>aggregationLevel</em> 参数结合使用。 例如：<em>&amp;groupby=applicationId&amp;aggregationLevel=week</em></p> |    否      |


### <a name="filter-fields"></a>筛选器字段

请求正文中的 *filter* 参数包含的一条或多条语句用于在响应中筛选行。 每条语句包含的字段和值使用 **eq** 或 **ne** 运算符进行关联，并且语句可以使用 **and** 或 **or** 进行组合。 下面是一个 *filter* 参数的示例：

-   *filter=market eq 'US' and deviceType eq 'phone'*

有关支持的字段列表，请参阅下表。 *filter* 参数中的字符串值必须使用单引号括起来。

| 字段 | 说明                                                              |
|--------|--------------------------------------------------------------------------|
| market    | 包含广告投放所在地市场的 ISO 3166 国家/地区代码的字符串。 |
| deviceType    | 以下字符串之一：<strong>PC/Tablet</strong> 或 <strong>Phone</strong>。 |
| adUnitId    | 指定要应用到筛选器的广告单元 ID 的字符串。 |
| pubCenterAppName    | 指定要应用到筛选器的当前应用的 pubCenter 名称的字符串。 |
| adProvider    | 指定要应用到筛选器的广告提供商名称的字符串。 |
| date    | 指定要应用到筛选器的日期（格式为 YYYY/MM/DD）的字符串。 |


### <a name="request-example"></a>请求示例

以下示例演示用于获取广告性能数据的多个请求。 将 *applicationId* 值替换为你的应用的应用商店 ID。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/adsperformance?applicationId=9NBLGGH4R315&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/adsperformance?applicationId=9NBLGGH4R315&startDate=8/1/2015&endDate=8/31/2015&skip=0&$filter=market eq 'US' and deviceType eq 'phone’ eq 'US'; and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| 值      | 类型   | 说明                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 值      | 数组  | 包含广告性能聚合数据的对象数组。 有关每个对象中的数据的详细信息，请参阅以下[广告性能值](#ad-performance-values)部分。                                                                                                                      |
| @nextLink  | 字符串 | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求下一页数据。 例如，当请求的 **top** 参数设置为 5，但查询的数据超过 5 项时，就会返回此值。 |
| TotalCount | int    | 查询的数据结果中的行总数。                          |


### <a name="ad-performance-values"></a>广告性能值

*Value* 数组中的元素包含以下值。

| 值               | 类型   | 说明                                                                                                                                                                                                                              |
|---------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                | 字符串 | 广告性能数据的日期范围内的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。 |
| applicationId       | 字符串 | 要检索广告性能数据的应用的应用商店 ID。     |
| applicationName     | 字符串 | 应用的显示名称。                         |
| adUnitId           | 字符串 | 广告单元的 ID。        |
| adUnitName           | 字符串 | 由开发人员在合作伙伴中心中指定的广告单元的名称。              |
| adProvider           |  字符串  |  广告提供商的名称   |
| deviceType          | 字符串 | 广告投放所在设备的类型。 有关支持的字符串列表，请参阅上述[筛选器字段](#filter-fields)部分。                              |
| market              | 字符串 | 广告投放所在地市场的 ISO 3166 国家/地区代码。             |
| accountCurrencyCode     | 字符串 | 帐户的货币代码。        |
| pubCenterAppName       |  字符串  |   与合作伙伴中心中的应用关联的 pubCenter 应用名称。   |
| adProviderRequests        | 整型 | 指定广告提供商的广告请求数。                 |
| impressions           | 整型 | 广告曝光数。        |
| clicks            | 整型 | 广告点击数。       |
| revenueInAccountCurrency       | 数字 | 以帐户所在国家/地区的货币为单位的收入。       |
| requests              | 整型 | 广告请求数。                 |


### <a name="response-example"></a>响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "date": "2015-03-09",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso Demo",
      "market": "US",
      "deviceType": "phone",
      "adUnitId":"10765920",
      "adUnitName":"TestAdUnit",
      "revenueInAccountCurrency": 10.0,
      "impressions": 1000,
      "requests": 10000,
      "clicks": 1,
      "accountCurrencyCode":"USD"
    },
    {
      "date": "2015-03-09",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso Demo",
      "market": "US",
      "deviceType": "phone",
      "adUnitId":"10795110",
      "adUnitName":"TestAdUnit2",
      "revenueInAccountCurrency": 20.0,
      "impressions": 2000,
      "requests": 20000,
      "clicks": 3,
      "accountCurrencyCode":"USD"
    },
  ],
  "@nextLink": "adsperformance?applicationId=9NBLGGH4R315&aggregationLevel=week&startDate=2015/03/01&endDate=2016/02/01&top=2&skip=2",
  "TotalCount": 191753
}

```

## <a name="related-topics"></a>相关主题

* [广告性能报告](../publish/advertising-performance-report.md)
* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)
