---
author: Xansky
ms.assetid: 1599605B-4243-4081-8D14-40F6F7734E25
description: 在 Microsoft Store 分析 API 中使用此方法，获取给定日期范围和其他可选筛选器内某一加载项的聚合购置数据。
title: 获取加载项购置
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API, 加载项购置
ms.localizationpriority: medium
ms.openlocfilehash: 4adb202df2806caeb0dc88469521b0f373886c43
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2018
ms.locfileid: "6833270"
---
# <a name="get-add-on-acquisitions"></a>获取加载项购置

使用 Microsoft Store 分析 API 中的此方法，可获取给定日期范围和其他可选筛选器内你的应用的聚合加载项购置数据（格式为 JSON）。 此信息也是在合作伙伴中心中的[加载项购置报告](../publish/add-on-acquisitions-report.md)中可用。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 描述          |
|---------------|--------|--------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

*applicationId* 或 *inAppProductId* 参数是必需参数。 若要检索注册到该应用的所有加载项的购置数据，请指定 *applicationId* 参数。 若要检索单个加载项的购置数据，请指定 *inAppProductId* 参数。 如果同时指定这两个参数，会忽略 *inAppProductId* 参数。

| 参数        | 类型   |  说明      |  必需  
|---------------|--------|---------------|------|
| applicationId | 字符串 | 要检索加载项购置数据的应用的 [Store ID](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| inAppProductId | string | 要检索购置数据的加载项的 [Store ID](in-app-purchases-and-trials.md#store-ids)。  | 是  |
| startDate | 日期 | 要检索的加载项购置数据日期范围中的开始日期。 默认值为当前日期。 |  否  |
| endDate | 日期 | 要检索的加载项购置数据日期范围中的结束日期。 默认值为当前日期。 |  否  |
| top | int | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |  否  |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。 |  否  |
| filter |字符串  | 在响应中筛选行的一条或多条语句。 有关详细信息，请参阅下面的[筛选器字段](#filter-fields)部分。 | 否   |
| aggregationLevel | 字符串 | 指定用于检索聚合数据的时间范围。 可以是以下字符串之一：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果未指定，默认值为 <strong>day</strong>。 | 否 |
| orderby | 字符串 | 对每个加载项购置的结果数据值进行排序的语句。 语法为 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 参数可以是以下字符串之一：<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p><em>order</em> 参数是可选的，可以是 <strong>asc</strong> 或 <strong>desc</strong>，用于指定每个字段的升序或降序排列。 默认值为 <strong>asc</strong>。</p><p>下面是一个 <em>orderby</em> 字符串：<em>orderby=date,market</em></p> |  否  |
| groupby | 字符串 | 仅将数据聚合应用于指定字段的语句。 可以指定的字段如下所示：<ul><li><strong>日期型</strong></li><li><strong>applicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>返回的数据行会包含 <em>groupby</em> 参数中指定的字段，以及以下字段：</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>inAppProductId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p><em>groupby</em> 参数可以与 <em>aggregationLevel</em> 参数结合使用。 例如：<em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  否  |


### <a name="filter-fields"></a>筛选器字段

请求中的 *filter* 参数包含一条或多条语句，用于在响应中筛选行。 每条语句包含的字段和值使用 **eq** 或 **ne** 运算符进行关联，并且语句可以使用 **and** 或 **or** 进行组合。 下面是一些示例 *filter* 参数：

-   *filter=market eq 'US' and gender eq 'm'*
-   *filter=(market ne 'US') and (gender ne 'Unknown') and (gender ne 'm') and (market ne 'NO') and (ageGroup ne 'greater than 55' or ageGroup ne ‘less than 13’)*

有关支持的字段列表，请参阅下表。 *filter* 参数中的字符串值必须使用单引号括起来。

| 字段        |  说明        |
|---------------|-----------------|
| acquisitionType | 以下字符串之一：<ul><li><strong>free</strong></li><li><strong>trial</strong></li><li><strong>paid</strong></li><li><strong>promotional code</strong></li><li><strong>iap</strong></li></ul> |
| ageGroup | 以下字符串之一：<ul><li><strong>less than 13</strong></li><li><strong>13-17</strong></li><li><strong>18-24</strong></li><li><strong>25-34</strong></li><li><strong>35-44</strong></li><li><strong>44-55</strong></li><li><strong>greater than 55</strong></li><li><strong>Unknown</strong></li></ul> |
| storeClient | 以下字符串之一：<ul><li><strong>Windows Phone Store (client)</strong></li><li><strong>Microsoft Store (client)</strong></li><li><strong>Microsoft Store (web)</strong></li><li><strong>Volume purchase by organizations</strong></li><li><strong>Other</strong></li></ul> |
| gender | 以下字符串之一：<ul><li><strong>m</strong></li><li><strong>f</strong></li><li><strong>Unknown</strong></li></ul> |
| market | 包含购置行为所在地市场的 ISO 3166 国家/地区代码的字符串。 |
| osVersion | 以下字符串之一：<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows10</strong></li><li><strong>Unknown</strong></li></ul> |
| deviceType | 以下字符串之一：<ul><li><strong>电脑</strong></li><li><strong>电话</strong></li><li><strong>控制台</strong></li><li><strong>IoT</strong></li><li><strong>全息</strong></li><li><strong>未知</strong></li></ul> |
| orderName | 指定促销充值码（用于获取加载项）订单名称的字符串（仅当用户通过兑换促销充值码获取加载项后才可用）。 |


### <a name="request-example"></a>请求示例

以下示例演示了用于获取加载项购置数据的多个请求。 将 *inAppProductId* 和 *applicationId* 值替换为你的加载项或应用的相应应用商店 ID。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and ageGroup ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| 值      | 类型   | 描述         |
|------------|--------|------------------|
| 值      | 数组  | 包含聚合加载项购置数据的对象数组。 有关每个对象中的数据的详细信息，请参阅下面的[加载项购置值](#add-on-acquisition-values)部分。                                                                                                              |
| @nextLink  | 字符串 | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求下一页数据。 例如，当请求的 **top** 参数设置为 10000，但查询的加载项购置数据超过 10000 行时，就会返回此值。 |
| TotalCount | int    | 查询的数据结果中的行总数。    |


<span id="add-on-acquisition-values" />

### <a name="add-on-acquisition-values"></a>加载项购置值

*Value* 数组中的元素包含以下值。

| 值               | 类型    | 说明        |
|---------------------|---------|---------------------|
| date                | 字符串  | 购置数据的日期范围内的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。 |
| inAppProductId      | 字符串  | 要检索购置数据的加载项的应用商店 ID。                                                                                                                                                                 |
| inAppProductName    | 字符串  | 加载项的显示名称。 当 *aggregationLevel* 参数设置为 **day** 时，该值仅显示在响应数据中，除非在 *groupby* 参数中指定 **inAppProductName** 字段。                                                                                                                                                                                                            |
| applicationId       | 字符串  | 要检索加载项购置数据的应用的应用商店 ID。                                                                                                                                                           |
| applicationName     | 字符串  | 应用的显示名称。                                                                                                                                                                                                             |
| deviceType          | 字符串  | 购置已完成的设备的类型。 有关支持的字符串列表，请参阅上述[筛选器字段](#filter-fields)部分。                                                                                                  |
| orderName           | 字符串  | 订单名称。                                                                                                                                                                                                                   |
| storeClient         | 字符串  | 发生购置行为的应用商店版本。 有关支持的字符串列表，请参阅上述[筛选器字段](#filter-fields)部分。                                                                                            |
| osVersion           | 字符串  | 发生购置行为的操作系统版本。 有关支持的字符串列表，请参阅上述[筛选器字段](#filter-fields)部分。                                                                                                   |
| market              | 字符串  | 发生购置行为的市场的 ISO 3166 国家/地区代码。                                                                                                                                                                  |
| gender              | 字符串  | 进行购置的用户的性别。 有关支持的字符串列表，请参阅上述[筛选器字段](#filter-fields)部分。                                                                                                    |
| ageGroup            | 字符串  | 进行购置的用户的年龄组。 有关支持的字符串列表，请参阅上述[筛选器字段](#filter-fields)部分。                                                                                                 |
| acquisitionType     | 字符串  | 购置类型（免费、付费等）。 有关支持的字符串列表，请参阅上述[筛选器字段](#filter-fields)部分。                                                                                                    |
| acquisitionQuantity | 整数 | 发生的购置数。                        |


### <a name="response-example"></a>响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "date": "2015-01-02",
      "inAppProductId": "9NBLGGH3LHKL",
      "inAppProductName": "Contoso add-on 7",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "GB",
      "gender": "m",
      "ageGroup": "50orover",
      "acquisitionType": "iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "inappacquisitions?applicationId=9NBLGGGZ5QDR&inAppProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```

## <a name="related-topics"></a>相关主题

* [加载项购置报告](../publish/add-on-acquisitions-report.md)
* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)
* [通过通道获取加载项转换](get-add-on-conversions-by-channel.md)
* [获取应用购置](get-app-acquisitions.md)
* [获取应用购置漏斗数据](get-acquisition-funnel-data.md)
* [通过通道获取应用转换](get-app-conversions-by-channel.md)

 

 
