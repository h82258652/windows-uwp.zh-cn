---
author: Xansky
ms.assetid: 2967C757-9D8A-4B37-8AA4-A325F7A060C5
description: 使用 Microsoft Store 分析 API 中的此方法，可获取给定日期范围和其他可选筛选器的评价数据。
title: 获取应用评价
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API, 评价
ms.localizationpriority: medium
ms.openlocfilehash: 656f00ec8a7711a43c44790b04ea01dace4c4ee2
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6659099"
---
# <a name="get-app-reviews"></a>获取应用评价


使用 Microsoft Store 分析 API 中的此方法，可获取给定日期范围和其他可选筛选器的评价数据（格式为 JSON）。 此信息也是在合作伙伴中心中的[评价报告](../publish/reviews-report.md)中可用。

在检索评价后，可使用 Microsoft Store 评价 API 中的[获取应用评价的回复信息](get-response-info-for-app-reviews.md)和[提交对应用评价的回复](submit-responses-to-app-reviews.md)方法以可编程方式回复评价。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求

### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|---------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

| 参数        | 类型   |  说明      |  必需  
|---------------|--------|---------------|------|
| applicationId | 字符串 | 要检索评价数据的应用的 [Store ID](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| startDate | date | 要检索的评价数据日期范围中的开始日期。 默认值为当前日期。 |  否  |
| endDate | date | 要检索的评价数据日期范围中的结束日期。 默认值为当前日期。 |  否  |
| top | int | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |  否  |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。 |  否  |
| filter |字符串  | 在响应中筛选行的一条或多条语句。 有关详细信息，请参阅下面的[筛选器字段](#filter-fields)部分。 | 否   |
| orderby | 字符串 | 对结果数据值进行排序的语句。 语法为 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 参数可以是以下字符串之一：<ul><li><strong>date</strong></li><li><strong>osVersion</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>isRevised</strong></li><li><strong>packageVersion</strong></li><li><strong>deviceModel</strong></li><li><strong>productFamily</strong></li><li><strong>deviceScreenResolution</strong></li><li><strong>isTouchEnabled</strong></li><li><strong>reviewerName</strong></li><li><strong>reviewTitle</strong></li><li><strong>reviewText</strong></li><li><strong>helpfulCount</strong></li><li><strong>notHelpfulCount</strong></li><li><strong>responseDate</strong></li><li><strong>responseText</strong></li><li><strong>deviceRAM</strong></li><li><strong>deviceStorageCapacity</strong></li><li><strong>rating</strong></li></ul><p><em>order</em> 参数是可选的，可以是 <strong>asc</strong> 或 <strong>desc</strong>，用于指定每个字段的升序或降序排列。 默认值为 <strong>asc</strong>。</p><p>下面是一个 <em>orderby</em> 字符串：<em>orderby=date,market</em></p> |  否  |


### <a name="filter-fields"></a>筛选器字段

请求中的 *filter* 参数包含一条或多条语句，用于在响应中筛选行。 每条语句包含的字段和值使用 **eq** 或 **ne** 运算符进行关联，并且某些字段还支持 **contains**、**gt**、**lt**、**ge** 和 **le** 运算符。 语句可以使用 **and** 或 **or** 进行组合。

下面是一个 *filter* 字符串的示例：*filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US'*

有关支持的字段列表和每个字段支持的运算符，请参阅下表。 *filter* 参数中的字符串值必须使用单引号括起来。

| 字段        | 支持的运算符   |  说明        |
|---------------|--------|-----------------|
| market | eq, ne | 包含设备市场的 ISO 3166 国家/地区代码的字符串。 |
| osVersion  | eq, ne  | 以下字符串之一：<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows10</strong></li><li><strong>Unknown</strong></li></ul>  |
| deviceType  | eq, ne  | 以下字符串之一：<ul><li><strong>电脑</strong></li><li><strong>电话</strong></li><li><strong>控制台</strong></li><li><strong>IoT</strong></li><li><strong>全息</strong></li><li><strong>未知</strong></li></ul>  |
| isRevised  | eq, ne  | 指定 <strong>true</strong> 可筛选已修改的评价，否则指定 <strong>false</strong>。  |
| packageVersion  | eq, ne  | 已评价的应用程序包版本。  |
| deviceModel  | eq, ne  | 应用已评价的设备的类型。  |
| productFamily  | eq, ne  | 以下字符串之一：<ul><li><strong>PC</strong></li><li><strong>Tablet</strong></li><li><strong>Phone</strong></li><li><strong>Wearable</strong></li><li><strong>Server</strong></li><li><strong>Collaborative</strong></li><li><strong>其他</strong></li></ul>  |
| deviceRAM  | eq, ne, gt, lt, ge, le  | 物理 RAM（以 MB 为单位）。  |
| deviceScreenResolution  | eq, ne  | 设备屏幕分辨率采用&quot;<em>宽度</em> x <em>高度</em>&quot;格式。   |
| deviceStorageCapacity  | eq, ne, gt, lt, ge, le   | 主存储器磁盘容量（以 GB 为单位）。  |
| isTouchEnabled  | eq, ne  | 指定 <strong>true</strong> 可筛选支持触摸的设备，否则指定 <strong>false</strong>。   |
| reviewerName  | eq, ne  |  评价者名称。 |
| rating  | eq, ne, gt, lt, ge, le  | 应用评分（以星级为单位）。  |
| reviewTitle  | eq, ne, contains  | 评价的标题。  |
| reviewText  | eq, ne, contains  |  评价的文本内容。 |
| helpfulCount  | eq, ne  |  评价标记为有用的次数。 |
| notHelpfulCount  | eq, ne  | 评价标记为无用的次数。  |
| responseDate  | eq, ne  | 回复的提交时间。  |
| responseText  | eq, ne, contains  | 回复的文本内容。  |
| id  | eq, ne  | 评价的 ID（这是一个 GUID）。        |


### <a name="request-example"></a>请求示例

以下示例演示用于获取评价数据的多个请求。 将 *applicationId* 值替换为你的应用的存储 ID。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| 值      | 类型   | 说明      |
|------------|--------|------------------|
| 值      | array  | 包含评价数据的对象数组。 有关每个对象中的数据的详细信息，请参阅以下[评价值](#review-values)部分。       |
| @nextLink  | 字符串 | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求下一页数据。 例如，当请求的 **top** 参数设置为 10000，但查询的评价数据超过 10000 行时，就会返回此值。 |
| TotalCount | int    | 查询的数据结果中的行总数。  |

 
### <a name="review-values"></a>评价值

*Value* 数组中的元素包含以下值。

| 值           | 类型    | 说明       |
|-----------------|---------|-------------------|
| date            | 字符串  | 评价数据的日期范围内的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。 |
| applicationId   | 字符串  | 要检索评价数据的应用的应用商店 ID。         |
| applicationName | 字符串  | 应用的显示名称。    |
| market          | 字符串  | 评价已提交的市场的 ISO 3166 国家/地区代码。        |
| osVersion       | 字符串  | 评价已提交的操作系统版本。 有关支持的字符串列表，请参阅上述[筛选器字段](#filter-fields)部分。            |
| deviceType      | 字符串  | 评价已提交的设备的类型。 有关支持的字符串列表，请参阅上述[筛选器字段](#filter-fields)部分。            |
| isRevised       | 布尔值 | 值为 **true** 表示评价已修改；否则为 **false**。   |
| packageVersion  | 字符串  | 已评价的应用程序包版本。        |
| deviceModel        | 字符串  |应用已评价的设备的类型。     |
| productFamily      | 字符串  | 设备系列名称。 有关支持的字符串列表，请参阅上述[筛选器字段](#filter-fields)部分。   |
| deviceRAM       | 数字  | 物理 RAM（以 MB 为单位）。    |
| deviceScreenResolution       | 字符串  | 设备屏幕分辨率采用“*宽度* x *高度*”格式。    |
| deviceStorageCapacity | 数字 | 主存储器磁盘容量（以 GB 为单位）。 |
| isTouchEnabled | 布尔值 | 值为 **true** 表示触摸受支持；否则为 **false**。 |
| reviewerName | 字符串 | 评价者名称。 |
| rating | 数字 | 应用评分（以星级为单位）。 |
| reviewTitle | 字符串 | 评价的标题。 |
| reviewText | 字符串 | 评价的文本内容。 |
| helpfulCount | 数字 | 评价标记为有用的次数。 |
| notHelpfulCount | 数字 | 评价标记为无用的次数。 |
| responseDate | 字符串 | 回复的提交时间。 |
| responseText | 字符串 | 回复的文本内容。 |
| id | 字符串 | 评价的 ID（这是一个 GUID）。 你可以在[获取应用评价的回复信息](get-response-info-for-app-reviews.md)和[提交对应用评价的回复](submit-responses-to-app-reviews.md)方法中使用此 ID。 |


### <a name="response-example"></a>回复示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "date": "2015-07-29",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "US",
      "osVersion": "10.0.10240.16410",
      "deviceType": "PC",
      "isRevised": true,
      "packageVersion": "",
      "deviceModel": "Microsoft Corporation-Virtual Machine",
      "productFamily": "PC",
      "deviceRAM": -1,
      "deviceScreenResolution": "1024 x 768",
      "deviceStorageCapacity": 51200,
      "isTouchEnabled": false,
      "reviewerName": "ALeksandra",
      "rating": 5,
      "reviewTitle": "Love it",
      "reviewText": "Great app for demos and great dev response.",
      "helpfulCount": 0,
      "notHelpfulCount": 0,
      "responseDate": "2015-08-07T01:50:22.9874488Z",
      "responseText": "1",
      "id": "6be543ff-1c9c-4534-aced-af8b4fbe0316"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>相关主题

* [评价报告](../publish/reviews-report.md)
* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取应用评价的回复信息](get-response-info-for-app-reviews.md)
* [提交对应用评价的回复](submit-responses-to-app-reviews.md)
* [获取应用购置](get-app-acquisitions.md)
* [获取加载项购置](get-in-app-acquisitions.md)
* [获取错误报告数据](get-error-reporting-data.md)
* [获取应用评分](get-app-ratings.md)
