---
description: 使用 Microsoft Store analytics API 中的此方法获取给定日期范围和其他可选筛选器中的外接程序订阅的获取数据。
title: 获取订阅加载项购置
ms.date: 01/25/2018
ms.topic: article
keywords: windows 10，uwp，应用商店服务，Microsoft Store analytics API，订阅
ms.openlocfilehash: 4c307dbf7d17251e3d3c5f8792d8ea3d049924d9
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493542"
---
# <a name="get-subscription-add-on-acquisitions"></a>获取订阅加载项购置

使用 Microsoft Store analytics API 中的此方法获取给定日期范围内的应用程序外接程序订阅的聚合获取数据，以及其他可选筛选器。

## <a name="prerequisites"></a>必备条件

若要使用此方法，首先需要执行以下操作：

* 完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)（如果尚未这样做）。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 描述          |
|---------------|--------|--------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt; 。 |


### <a name="request-parameters"></a>请求参数

| 参数        | 类型   |  说明      |  必需  
|---------------|--------|---------------|------|
| applicationId | 字符串 | 要为其检索订阅外接程序获取数据的应用的[存储 ID](in-app-purchases-and-trials.md#store-ids) 。 |  是  |
| subscriptionProductId  | string | 要检索其获取数据的订阅外接程序的[存储 ID](in-app-purchases-and-trials.md#store-ids) 。 如果未指定此值，则此方法将返回指定应用的所有订阅外接程序的获取数据。  | 否  |
| startDate | date | 要检索的订阅外接程序购买数据的日期范围的开始日期。 默认值为当前日期。 |  否  |
| endDate | 日期 | 要检索的订阅外接程序购买数据的日期范围的结束日期。 默认值为当前日期。 |  否  |
| top | int | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 100。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |  否  |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=100 和 skip=0，将检索前 100 行数据；top=100 和 skip=100，将检索之后的 100 行数据，依此类推。 |  否  |
| filter | string  | 筛选响应正文的一条或多条语句。 每条语句可以使用 **eq** 或 **ne** 运算符，多条语句还可以使用 **and** 或 **or** 进行组合。 您可以在筛选语句中指定以下字符串（这些字符串对应于[响应正文中的值](#subscription-acquisition-values)）： <ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li></ul><p>下面是一个示例*筛选器*参数： <em>filter = date eq ' 2017-07-08 '</em>。</p> | 否   |
| aggregationLevel | string | 指定用于检索聚合数据的时间范围。 可以是以下字符串之一：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果未指定，默认值为 <strong>day</strong>。 | 否 |
| orderby | string | 对每个订阅外接程序购买的结果数据值进行排序的语句。 语法为 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 参数可以是以下字符串之一：<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li></ul><p><em>order</em> 参数是可选的，可以是 <strong>asc</strong> 或 <strong>desc</strong>，用于指定每个字段的升序或降序排列。 默认值为<strong>asc</strong>。</p><p>下面是一个 <em>orderby</em> 字符串的示例：<em>orderby=date,market</em></p> |  否  |
| groupby | string | 仅将数据聚合应用于指定字段的语句。 可以指定的字段如下所示：<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li></ul><p><em>groupby</em> 参数可以与 <em>aggregationLevel</em> 参数结合使用。 例如： <em>groupby = marketplace &amp; aggregationLevel = week</em></p> |  否  |


### <a name="request-example"></a>请求示例

下面的示例演示如何获取订阅外接程序获取数据。 用应用程序的相应存储区 ID 替换*applicationId*值。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions?applicationId=9NBLGGGZ5QDR&startDate=2017-07-07&endDate=2017-07-08 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| 值      | 类型   | 说明         |
|------------|--------|------------------|
| Value      | array  | 对象的数组，其中包含聚合订阅附加获取数据。 有关每个对象中的数据的详细信息，请参阅下面的 "[订阅获取值](#subscription-acquisition-values)" 一节。             |
| @nextLink  | string | 如果存在其他数据页，则此字符串包含一个你可用来请求下一页数据的 URI。 例如，如果请求的**top**参数设置为100，但查询的订阅外接程序获取数据超过了100行，则会返回此值。 |
| TotalCount | int    | 查询的数据结果中的行总数。       |


<span id="subscription-acquisition-values" />

### <a name="subscription-acquisition-values"></a>订阅获取值

*Value* 数组中的元素包含以下值。

| 值               | 类型    | 说明        |
|---------------------|---------|---------------------|
| date                | string  | 购置数据的日期范围内的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。 |
| subscriptionProductId      | string  | 要为其检索获取数据的订阅外接程序的[存储 ID](in-app-purchases-and-trials.md#store-ids) 。    |
| subscriptionProductName    | string  | 订阅外接程序的显示名称。         |
| applicationId       | 字符串  | 要为其检索订阅外接程序获取数据的应用的[存储 ID](in-app-purchases-and-trials.md#store-ids) 。   |
| applicationName     | string  | 应用的显示名称。     |
| skuId     | string  | 要为其检索获取数据的订阅外接程序的[SKU](in-app-purchases-and-trials.md#products-skus) ID。     |
| deviceType          | string  |  用于指定完成购置的设备类型的以下字符串之一：<ul><li><strong>PC</strong></li><li><strong>移动</strong></li><li><strong>控制台-Xbox One</strong></li><li><strong>控制台-Xbox 系列 X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>       |
| market           | string  | 发生购置行为的市场的 ISO 3166 国家/地区代码。     |
| currencyCode         | string  | 税前总销售额的 ISO 4217 格式的货币代码。       |
| grossSalesBeforeTax           | integer  | 按*currencyCode*值指定的本地货币的总销售额。     |
| totalActiveCount             | integer  | 指定时间段内活动订阅总数。 这等效于*goodStandingActiveCount*、 *pendingGraceActiveCount*、 *graceActiveCount*和*lockedActiveCount*值的总和。  |
| totalChurnCount              | integer  | 在指定的时间段内停用的订阅的总计数。 这等效于*billingChurnCount*、 *nonRenewalChurnCount*、 *refundChurnCount*、 *chargebackChurnCount*、 *earlyChurnCount*和*otherChurnCount*值的总和。   |
| newCount            | integer  | 指定时间段内新的订阅收购数量，包括试验。    |
| renewCount     | integer  | 指定时间段内的订阅续订数，包括用户启动的续订和自动续订。        |
| goodStandingActiveCount | integer | 指定时间段内处于活动状态的订阅的数目，以及到期日期 >为查询的*结束*值。    |
| pendingGraceActiveCount | integer | 在指定的时间段内处于活动状态但有计费失败的订阅数，以及订阅到期日期 >= 查询的*终止*值。     |
| graceActiveCount | integer | 在指定的时间段内处于活动状态但有计费失败的订阅数，以及：<ul><li>订阅到期日期为查询 <*终止*日期值。</li><li>宽限期的结束时间为 >=*终止*点值。</li></ul>        |
| lockedActiveCount | integer | 在指定的时间段内，*纠缠*中的订阅数（即，订阅即将过期，Microsoft 尝试获取资金以自动续订订阅）和位置：<ul><li>订阅到期日期为查询 <*终止*日期值。</li><li>宽限期的结束时间为 <=*终止*点值。</li></ul>        |
| billingChurnCount | integer | 在指定的时间段内因处理计费费用失败，以及订阅之前在纠缠中停用的订阅数。        |
| nonRenewalChurnCount | integer | 在指定的时间段内因未续订而停用的订阅数。        |
| refundChurnCount | integer | 在指定的时间段内因被退回而停用的订阅数。        |
| chargebackChurnCount | integer | 由于计费而在指定时间段内停用的订阅数。        |
| earlyChurnCount | integer | 处于良好状态的指定时间段内停用的订阅数。        |
| otherChurnCount | integer | 出于其他原因在指定的时间段内停用的订阅数。        |


### <a name="response-example"></a>响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "date": "2017-07-08",
      "subscriptionProductId": "9KDLGHH6R365",
      "subscriptionProductName": "Contoso App Subscription with One Month Free Trial",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso App",
      "skuId": "0020",
      "market": "Unknown",
      "deviceType": "PC",
      "currencyCode": "USD",
      "grossSalesBeforeTax": 0.0,
      "totalActiveCount": 1,
      "totalChurnCount": 0,
      "newCount": 0,
      "renewCount": 0,
      "goodStandingActiveCount": 1,
      "pendingGraceActiveCount": 0,
      "graceActiveCount": 0,
      "lockedActiveCount": 0,
      "billingChurnCount": 0,
      "nonRenewalChurnCount": 0,
      "refundChurnCount": 0,
      "chargebackChurnCount": 0,
      "earlyChurnCount": 0,
      "otherChurnCount": 0
    },
    {
      "date": "2017-07-08",
      "subscriptionProductId": "9JJFDHG4R478",
      "subscriptionProductName": "Contoso App Monthly Subscription",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso App",
      "skuId": "0020",
      "market": "US",
      "deviceType": "PC",
      "currencyCode": "USD",
      "grossSalesBeforeTax": 0.0,
      "totalActiveCount": 1,
      "totalChurnCount": 0,
      "newCount": 0,
      "renewCount": 0,
      "goodStandingActiveCount": 1,
      "pendingGraceActiveCount": 0,
      "graceActiveCount": 0,
      "lockedActiveCount": 0,
      "billingChurnCount": 0,
      "nonRenewalChurnCount": 0,
      "refundChurnCount": 0,
      "chargebackChurnCount": 0,
      "earlyChurnCount": 0,
      "otherChurnCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>相关主题

* [加载项购置报告](../publish/add-on-acquisitions-report.md)
* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)

 

 
