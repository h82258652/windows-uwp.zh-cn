---
description: 在 Microsoft Store 分析 API 中使用此方法以获取要在给定的日期范围和其他可选的筛选器外接程序订阅获取数据。
title: 获取订阅加载项购置
ms.date: 01/25/18
ms.topic: article
keywords: windows 10、 uwp、 存储区服务、 Microsoft Store 分析 API，订阅
ms.openlocfilehash: e33a3ded219fb4d223137b40ebe871f66589addf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594772"
---
# <a name="get-subscription-add-on-acquisitions"></a>获取订阅加载项购置

在 Microsoft Store 分析 API 中使用此方法以获取您的应用程序的外接程序订阅期间给定的日期范围和其他可选的筛选器聚合采集数据。

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

| 标头        | 在任务栏的搜索框中键入   | 描述          |
|---------------|--------|--------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** *token*&lt;&gt;。 |


### <a name="request-parameters"></a>请求参数

| 参数        | 在任务栏的搜索框中键入   |  描述      |  必需  
|---------------|--------|---------------|------|
| applicationId | 字符串 | [Store ID](in-app-purchases-and-trials.md#store-ids)想检索订阅外接程序获取数据的应用。 |  是  |
| subscriptionProductId  | 字符串 | [Store ID](in-app-purchases-and-trials.md#store-ids)你想要检索采集数据订阅外接程序。 如果不指定此值，此方法将返回为指定的应用程序的所有订阅外接程序的采集数据。  | 否  |
| startDate | 日期 | 开始订阅外接程序获取数据的日期范围内要检索的日期。 默认值为当前日期。 |  否  |
| endDate | 日期 | 结束订阅外接程序获取数据的日期范围内要检索的日期。 默认值为当前日期。 |  否  |
| top | int | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 100。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |  否  |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=100 和 skip=0，将检索前 100 行数据；top=100 和 skip=100，将检索之后的 100 行数据，依此类推。 |  否  |
| filter | 字符串  | 筛选响应正文的一条或多条语句。 每条语句可以使用 **eq** 或 **ne** 运算符，语句还可以使用 **and** 或 **or** 进行组合。 可以在筛选器语句中指定以下字符串 (这些会对应于[响应正文中的值](#subscription-acquisition-values)): <ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li></ul><p>下面是一个示例*筛选器*参数：<em>筛选器 = 日期 eq 2017年-07-08'</em>。</p> | 否   |
| aggregationLevel | 字符串 | 指定用于检索聚合数据的时间范围。 可以是以下字符串之一：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果未指定，默认值为 <strong>day</strong>。 | 否 |
| orderby | 字符串 | 一个语句，每个订阅外接程序获取的数据值对结果进行排序。 语法是 <em>orderby=field [order],field [order],...</em>。<em>field</em> 参数可以是以下字符串之一。<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li></ul><p><em>order</em> 参数是可选的，可以是 <strong>asc</strong> 或 <strong>desc</strong>，用于指定每个字段的升序或降序排列。 默认值为 <strong>asc</strong>。</p><p>下面是一个 <em>orderby</em> 字符串的示例：<em>orderby=date,market</em></p> |  否  |
| groupby | 字符串 | 仅将数据聚合应用于指定字段的语句。 可以指定的字段如下所示：<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li></ul><p><em>groupby</em> 参数可以与 <em>aggregationLevel</em> 参数结合使用。 例如： <em>groupby = 市场&amp;aggregationLevel = 周</em></p> |  否  |


### <a name="request-example"></a>请求示例

下面的示例演示如何获取订阅外接程序获取数据。 替换*applicationId*与您的应用程序适当 Store ID 的值。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions?applicationId=9NBLGGGZ5QDR&startDate=2017-07-07&endDate=2017-07-08 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| 值      | 在任务栏的搜索框中键入   | 描述         |
|------------|--------|------------------|
| 值      | 数组  | 包含聚合订阅外接程序获取数据的对象的数组。 有关每个对象中的数据的详细信息，请参阅[订阅获取值](#subscription-acquisition-values)下面一节。             |
| @nextLink  | 字符串 | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求下一页数据。 例如，如果返回此值**顶部**请求的参数设置为 100，但有 100 多个行的查询的订阅外接程序获取数据。 |
| TotalCount | int    | 查询的数据结果中的行总数。       |


<span id="subscription-acquisition-values" />

### <a name="subscription-acquisition-values"></a>订阅获取值

*Value* 数组中的元素包含以下值。

| 值               | 在任务栏的搜索框中键入    | 描述        |
|---------------------|---------|---------------------|
| 日期                | 字符串  | 购置数据的日期范围内的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。 |
| subscriptionProductId      | 字符串  | [Store ID](in-app-purchases-and-trials.md#store-ids)订阅外接程序为其检索采集数据。    |
| subscriptionProductName    | 字符串  | 订阅外接程序显示名称。         |
| applicationId       | 字符串  | [Store ID](in-app-purchases-and-trials.md#store-ids)检索订阅外接程序获取数据的应用程序。   |
| applicationName     | 字符串  | 应用的显示名称。     |
| skuId     | 字符串  | ID [SKU](in-app-purchases-and-trials.md#products-skus)订阅外接程序为其检索采集数据。     |
| deviceType          | 字符串  |  用于指定完成购置的设备类型的以下字符串之一：<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>       |
| market           | 字符串  | 发生购置行为的市场的 ISO 3166 国家/地区代码。     |
| currencyCode         | 字符串  | 税前的总销售额的 ISO 4217 格式中的货币代码。       |
| grossSalesBeforeTax           | 整数  | 在指定的本地货币的销售总额*currencyCode*值。     |
| totalActiveCount             | 整数  | 在指定的时间段内的总活动订阅数。 这相当于的总和*goodStandingActiveCount*， *pendingGraceActiveCount*， *graceActiveCount*，和*lockedActiveCount*值。  |
| totalChurnCount              | 整数  | 指定的时间段内已停用的订阅的总计数。 这相当于的总和*billingChurnCount*， *nonRenewalChurnCount*， *refundChurnCount*， *chargebackChurnCount*， *earlyChurnCount*，并*otherChurnCount*值。   |
| newCount            | 整数  | 在指定的时间段，包括试用版的新订阅购买的数量。    |
| renewCount     | 整数  | 在指定的时间段，其中包括用户启动的续订和自动续订的订阅更新数。        |
| goodStandingActiveCount | 整数 | 指定的时间段内处于活动状态和到期日期所在的订阅数 > = *endDate*用于查询的值。    |
| pendingGraceActiveCount | 整数 | 指定的时间段内处于活动状态，但计费故障，并订阅到期日期所在的订阅数 > = *endDate*用于查询的值。     |
| graceActiveCount | 整数 | 指定的时间段内处于活动状态，但计费故障，订阅数量和位置：<ul><li>订阅的到期日期 < *endDate*用于查询的值。</li><li>宽限期结束是 > = *endDate*值。</li></ul>        |
| lockedActiveCount | 整数 | 中的订阅数*dunning* （也就是说，此订阅已接近过期和 Microsoft 尝试获取自动续订此订阅的资金） 期间指定的时间段，并在其中：<ul><li>订阅的到期日期 < *endDate*用于查询的值。</li><li>是的宽限期结束 < = *endDate*值。</li></ul>        |
| billingChurnCount | 整数 | 订阅的已停用在指定的时间段内失败，因为无法处理的计费费用并订阅以前在 dunning 数。        |
| nonRenewalChurnCount | 整数 | 已停用在指定的时间段内因为时未对其进行续订的订阅数。        |
| refundChurnCount | 整数 | 已停用在指定的时间段内因为它们已退款的订阅数。        |
| chargebackChurnCount | 整数 | 已停用在指定的时间段期间由于退款的订阅数。        |
| earlyChurnCount | 整数 | 已停用在指定的时间段内而致使中处于正常状态的订阅数。        |
| otherChurnCount | 整数 | 由于其他原因在指定的时间段内已停用的订阅数。        |


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
* [使用 Microsoft Store 服务的访问分析数据](access-analytics-data-using-windows-store-services.md)

 

 
