---
ms.assetid: DD4F6BC4-67CD-4AEF-9444-F184353B0072
description: 使用 Microsoft Store 分析 API 中的此方法，可获取给定日期范围和其他可选筛选器的聚合评分数据。
title: 获取应用评分
ms.date: 11/29/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API, 评分
ms.localizationpriority: medium
ms.openlocfilehash: 228f567ecb0d89f2e5af0b53c7105aa9a9fca213
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492922"
---
# <a name="get-app-ratings"></a>获取应用评分

使用 Microsoft Store 分析 API 中的此方法，可获取给定日期范围和其他可选筛选器的聚合评分数据（格式为 JSON）。 此信息也可在合作伙伴中心的 "[查看" 报表](../publish/reviews-report.md)中获得。

## <a name="prerequisites"></a>必备条件


若要使用此方法，首先需要执行以下操作：

* 完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)（如果尚未这样做）。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。


## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt; 。 |


### <a name="request-parameters"></a>请求参数

| 参数        | 类型   |  说明      |  必需  
|---------------|--------|---------------|------|
| applicationId | 字符串 | 要检索评分数据的应用的 [Store ID](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| startDate | date | 要检索的评分数据日期范围中的开始日期。 默认值为当前日期。 |  否  |
| endDate | 日期 | 要检索的评分数据日期范围中的结束日期。 默认值为当前日期。 |  否  |
| top | int | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |  否  |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。 |  否  |
| filter | string  | 在响应中筛选行的一条或多条语句。 有关详细信息，请参阅下面的[筛选器字段](#filter-fields)部分。 | 否   |
| aggregationLevel | string | 指定用于检索聚合数据的时间范围。 可以是以下字符串之一：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果未指定，默认值为 <strong>day</strong>。 | 否 |
| orderby | string | 对每个评分的结果数据值进行排序的语句。 语法为 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 参数可以是以下字符串之一：<ul><li><strong>date</strong></li><li><strong>osVersion</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>isRevised</strong></li></ul><p><em>order</em> 参数是可选的，可以是 <strong>asc</strong> 或 <strong>desc</strong>，用于指定每个字段的升序或降序排列。 默认值为<strong>asc</strong>。</p><p>下面是一个 <em>orderby</em> 字符串的示例：<em>orderby=date,market</em></p> |  否  |
| groupby | string | 仅将数据聚合应用于指定字段的语句。 可以指定的字段如下所示：<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>isRevised</strong></li></ul><p>返回的数据行会包含 <em>groupby</em> 参数中指定的字段，以及以下字段：</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>fiveStars</strong></li><li><strong>fourStars</strong></li><li><strong>threeStars</strong></li><li><strong>twoStars</strong></li><li><strong>oneStar</strong></li></ul><p><em>groupby</em> 参数可以与 <em>aggregationLevel</em> 参数结合使用。 例如： <em> &amp; Groupby = osVersion，marketplace &amp; aggregationLevel = week</em></p> |  否  |

 
### <a name="filter-fields"></a>筛选器字段

请求中的 *filter* 参数包含一条或多条语句，用于在响应中筛选行。 每条语句包含的字段和值使用 **eq** 或 **ne** 运算符进行关联，并且语句可以使用 **and** 或 **or** 进行组合。

下面是一个 *filter* 字符串示例：*filter=market eq 'US' and deviceType eq 'phone' and isRevised eq true*

有关支持的字段列表，请参阅下表。 *filter* 参数中的字符串值必须使用单引号引起来。

| 字段        |  说明        |
|---------------|-----------------|
| market | 包含对应用评分所在地市场的 ISO 3166 国家/地区代码的字符串。 |
| osVersion | 以下字符串之一：<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul> |
| deviceType | 以下字符串之一：<ul><li><strong>PC</strong></li><li><strong>移动</strong></li><li><strong>控制台-Xbox One</strong></li><li><strong>控制台-Xbox 系列 X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| isRevised | 指定 <strong>true</strong> 可筛选已修改的评分，否则指定 <strong>false</strong>。 |


### <a name="request-example"></a>请求示例

以下示例演示用于获取评分数据的多个请求。 将 *applicationId* 值替换为你的应用的 Store ID。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| 值      | 类型   | 说明                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Value      | array  | 包含聚合评分数据的对象数组。 有关每个对象中的数据的详细信息，请参阅以下[评分值](#rating-values)部分。                                                                                                                           |
| @nextLink  | string | 如果存在其他数据页，则此字符串包含一个你可用来请求下一页数据的 URI。 例如，当请求的 **top** 参数设置为 10000，但查询的评分置数据超过 10000 行时，就会返回此值。 |
| TotalCount | int    | 查询的数据结果中的行总数。                               |


### <a name="rating-values"></a>评分值

*Value* 数组中的元素包含以下值。

| 值           | 类型    | 说明       |
|-----------------|---------|-------------------|
| date            | string  | 评分数据的日期范围内的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。 |
| applicationId   | 字符串  | 要检索评分数据的应用的存储 ID。         |
| applicationName | string  | 应用的显示名称。    |
| market          | string  | 评分已提交的市场的 ISO 3166 国家/地区代码。        |
| osVersion       | string  | 评分已提交的操作系统版本。 有关支持的字符串列表，请参阅上述[筛选器字段](#filter-fields)部分。            |
| deviceType      | string  | 评分已提交的设备的类型。 有关支持的字符串列表，请参阅上述[筛选器字段](#filter-fields)部分。            |
| isRevised       | 布尔 | 值为 **true** 表示评分已修改；否则为 **false**。   |
| oneStar         | 数字  | 一星评分的数值。        |
| twoStars        | 数字  | 二星评分的数值。    |
| threeStars      | 数字  | 三星评分的数值。   |
| fourStars       | 数字  | 四星评分的数值。    |
| fiveStars       | 数字  | 五星评分的数值。    |


### <a name="response-example"></a>响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "date": "2015-02-13",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "CN",
      "osVersion": "8.0.10517.0",
      "deviceType": "Phone",
      "isRevised": false,
      "oneStar": 0,
      "twoStars": 0,
      "threeStars": 0,
      "fourStars": 0,
      "fiveStars": 2
    }
  ],
  "@nextLink": "ratings?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 15242
}

```

## <a name="related-topics"></a>相关主题

* [分级报告](../publish/reviews-report.md)
* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取应用购置](get-app-acquisitions.md)
* [获取加载项购置](get-in-app-acquisitions.md)
* [获取错误报告数据](get-error-reporting-data.md)
* [获取应用评价](get-app-reviews.md)
