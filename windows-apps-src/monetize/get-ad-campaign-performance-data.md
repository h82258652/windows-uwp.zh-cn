---
author: mcleanbyron
ms.assetid: A26A287C-B4B0-49E9-BB28-6F02472AE1BA
description: "使用 Windows 应用商店分析 API 中的此方法，可获取给定日期范围和其他可选筛选器内指定应用程序的广告市场活动性能聚合数据。"
title: "获取广告市场活动性能数据"
translationtype: Human Translation
ms.sourcegitcommit: 67845c76448ed13fd458cb3ee9eb2b75430faade
ms.openlocfilehash: 8859658675d31deccc3403e4862c47a54ca25b1a

---

# 获取广告市场活动性能数据


使用 Windows 应用商店分析 API 中的此方法，可获取给定日期范围和其他可选筛选器内你的应用程序的广告市场活动性能数据的聚合摘要。 此方法返回采用 JSON 格式的数据。

此方法返回 Windows 开发人员中心仪表板上的[应用安装广告报告](../publish/app-install-ads-reports.md)提供的相同数据。 有关广告市场活动的详细信息，请参阅[为你的应用创建广告市场活动](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app)。

若要检索已为你的开发人员中心帐户创建的所有广告市场活动的完整详细信息，可以使用[获取所有广告市场活动详细信息的方法](#get-details-for-all-ad-campaigns)，该方法会在本文稍后部分进行介绍。

## 先决条件


若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Windows 应用商店分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## 请求


### 请求语法

| 方法 | 请求 URI                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion``` |

<span />

### 请求头

| 标头        | 类型   | 说明                |
|---------------|--------|---------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |

<span />

### 请求参数

若要检索特定应用的广告市场活动性能数据，请使用 *applicationId* 参数。 若要检索与你的开发者帐户关联的所有应用的广告性能数据，请忽略 *applicationId* 参数。

| 参数     | 类型   | 说明     | 必需 |
|---------------|--------|-----------------|----------|
| applicationId   | 字符串    | 要检索广告市场活动性能数据的应用的应用商店 ID。 存储 ID 在开发人员中心仪表板的[应用标识页](../publish/view-app-identity-details.md)上提供。 应用商店 ID 的一个示例是 9NBLGGH4R315。 |    否      |
|  startDate  |  日期型   |  要检索的广告市场活动性能数据日期范围中的开始日期，格式为 YYYY/MM/DD。 默认值为当前日期减去 30 天。   |   否    |
| endDate   |  日期型   |  要检索的广告市场活动性能数据日期范围中的结束日期，格式为 YYYY/MM/DD。 默认值为当前日期减去 1 天。   |   否    |
| top   |  int   |  要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。   |   否    |
| skip   | int    |  要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。   |   否    |
| filter   |  字符串   |  在响应中筛选行的一条或多条语句。 唯一受支持的筛选器为 **campaignId**。 每条语句可以使用 **eq** 或 **ne** 运算符，语句还可以使用 **and** 或 **or** 进行组合。  下面是一个 *filter* 参数的示例：```filter=campaignId eq '100023'```。   |   否    |
|  aggregationLevel  |  字符串   | 指定用于检索聚合数据的时间范围。 可以是以下字符串之一：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果未指定，默认值为 <strong>day</strong>。    |   否    |
| orderby   |  字符串   |  <p>对广告市场活动性能数据的结果数据值进行排序的语句。 语法是 <em>orderby=field [order],field [order],...</em>。 <em>field</em> 参数可以是以下字符串之一。</p><ul><li><strong>date</strong></li><li><strong>campaignId</strong></li></ul><p><em>order</em> 参数是可选的，可以是 <strong>asc</strong> 或 <strong>desc</strong>，用于指定每个字段的升序或降序排列。 默认值为 <strong>asc</strong>。</p><p>下面是一个 <em>orderby</em> 字符串的示例：<em>orderby=date,campaignId</em></p>   |   否    |
|  groupby  |  字符串   |  <p>仅将数据聚合应用于指定字段的语句。 可以指定的字段如下所示：</p><ul><li><strong>campaignId</strong></li><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>currencyCode</strong></li></ul><p><em>groupby</em> 参数可以与 <em>aggregationLevel</em> 参数结合使用。 例如：<em>&amp;groupby=applicationId&amp;aggregationLevel=week</em></p>   |   否    |


<span />
 

### 请求示例

以下示例演示用于获取广告市场活动性能数据的多个请求。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?aggregationLevel=week&groupby=applicationId,campaignId,date  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?applicationId=9NBLGGH0XK8Z&startDate=2015/1/20&endDate=2016/8/31&skip=0&filter=campaignId eq '31007388' HTTP/1.1
Authorization: Bearer <your access token>
```

## 响应


### 响应正文

| 值      | 类型   | 说明                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 值      | 数组  | 包含广告市场活动性能聚合数据的对象数组。 有关每个对象中的数据的详细信息，请参阅下面的[市场活动性能对象](#campaign-performance-object)部分。                                                                                                                      |
| @nextLink  | 字符串 | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求下一页数据。 例如，当请求的 **top** 参数设置为 5，但查询的数据超过 5 项时，就会返回此值。 |
| TotalCount | int    | 查询的数据结果中的行总数。                                                                                                                                                                                                                             |

<span id="campaign-performance-object" />
### 市场活动性能对象

*Value* 数组中的元素包含以下值。

| 值               | 类型   | 说明            |
|---------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                | 字符串 | 广告市场活动性能数据的日期范围内的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。 |
| applicationId       | 字符串 | 要检索广告市场活动性能数据的应用的应用商店 ID。                     |
| campaignId     | 字符串 | 广告市场活动的 ID。           |
| currencyCode              | 字符串 | 市场活动预算的货币代码。              |
| spend          | 字符串 |  已为广告市场活动花费的预算金额。     |
| impressions           | 长整型 | 市场活动的广告曝光数。        |
| installs              | 长整型 | 与市场活动有关的应用安装数。   |
| clicks            | 长整型 | 市场活动的广告点击数。      |

<span />

### 响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "date": "2015-04-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "4568",
      "currencyCode": "USD",
      "spend": 700.6,
      "impressions": 200,
      "installs": 30,
      "clicks": 8
    },
    {
      "date": "2015-05-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "1234",
      "currencyCode": "USD",
      "spend": 325.3,
      "impressions": 20,
      "installs": 2,
      "clicks": 5
    }
  ],
  "@nextLink": "promotion?applicationId=9NBLGGGZ5QDR&aggregationLevel=day& startDate=2015/1/20&endDate=2016/8/31&top=2&skip=2",
  "TotalCount": 1917
}
```

<span id="get-details-for-all-ad-campaigns" />
## 获取所有广告市场活动的详细信息


使用此方法，可检索为注册到你的 Windows 开发人员中心帐户的应用创建的所有广告市场活动的详细信息。 此方法返回采用 JSON 格式的数据。 有关广告市场活动的详细信息，请参阅[为你的应用创建广告市场活动](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app)。

### 先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Windows 应用商店分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

### 请求


#### 请求语法

| 方法 | 请求 URI                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/adscampaign``` |

<span />

#### 请求头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |

<span />

#### 请求参数

| 参数     | 类型   | 说明     | 必填 |
|---------------|--------|-----------------|----------|
| top           | int    | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 1000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |    否      |
| skip           | int    | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=100 和 skip=0，将检索前 100 行数据；top=100 和 skip=100，将检索之后的 100 行数据，依此类推。 |    否      |


<span />
 

#### 请求示例

以下示例演示用于获取你的所有广告市场活动详细信息的多个请求。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/adscampaign?top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>
```

### 响应


#### 响应正文

| 值      | 类型   | 说明  |
|------------|--------|--------------|
| 值      | 数组  | 包含你的广告市场活动详细信息的对象数组。 有关每个对象中的数据的详细信息，请参阅下面的[市场活动对象](#campaign-object)部分。                        |
| @nextLink  | 字符串 | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求下一页数据。 例如，当请求的 **top** 参数设置为 5，但查询的数据超过 5 项时，就会返回此值。 |                                   |

<span id="campaign-object" />
#### 市场活动对象

*Value* 数组中的元素包含以下值。

| 值               | 类型   | 描述          |
|---------------------|--------|------------------|
| id     | 字符串 | 广告市场活动的 ID。 |
| name     | 字符串 | 广告市场活动的名称。 |
| createDate     | 字符串 | 广告市场活动的创建日期，采用 UTC 时区。 |
| startDate     | 字符串 | 广告市场活动的开始日期，采用 UTC 时区。 |
| endDate     | 字符串 | 广告市场活动的结束日期，采用 UTC 时区。 |
| applicationId       | 字符串 | 与广告市场活动关联的应用的应用商店 ID。 存储 ID 在开发人员中心仪表板的[应用标识页](../publish/view-app-identity-details.md)上提供。 应用商店 ID 的一个示例是 9NBLGGH4R315。                    |
| budget     | 数字 | 广告市场活动的预算。           |
| budgetType    | 字符串 | 广告市场活动的预算类型。 这可以是以下字符串之一：**Monthly** 或 **Total**。           |
| currencyCode              | 字符串 | 市场活动预算的货币代码。              |
| type              | 字符串 | 广告市场活动类型。 这可以是以下字符串之一：**Paid**、**House** 或 **Community**。             |
| status              | 字符串 | 广告市场活动的状态。 这可以是以下字符串之一：**Active**、**UserPaused**、**Pending**、**Ended** 或 **SystemPaused**。             |
| targetingType              | 字符串 | 广告市场活动的目标类型。 这可以是以下字符串之一：**Auto**、**Manual** 或 **Segment**。             |
| target              | 字典 | 包含市场活动的目标信息的键值对字典。 有关此对象的详细信息，请参阅下面的[目标对象](#target-object)部分。             |

<span />

<span id="target-object" />
#### 目标对象

此资源为以下键值对的字典。

| 键               | 类型   | 值          |
|---------------------|--------|------------------|
| 国家/地区     | 数组 |   包含广告市场活动面向的国家或地区的 ISO 3166 alpha 2 代码的一个或多个字符串。 |
| OsVersion     | 数组 | 指定广告市场活动面向的操作系统版本的以下一个或多个字符串：**10** 或 **8.x**。  |
| Age     |  数组 |  指定目标年龄范围的以下一个或多个字符串：**Age13To17**、**Age18To24**、**Age25To34**、**Age35To49** 或 **Age50AndAbove**。 |
| Gender     |  数组 |  指定目标性别的以下一个或多个字符串：**Female** 或 **Male**。 |
| DeviceType       |  数组  |  指定目标设备类型的以下一个或多个字符串：**PC/Tablet** 或 **Phone**。                  |
| Segment     |  数组 |    包含目标客户群 ID 的一个或多个字符串。       |


<span />

#### 响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
   "Value": [
      {
         "id":31010026,
         "name": "Contoso App 1 Campaign",
         "createDate":"2016-07-14T10:01:21Z",
         "startDate":"2016-07-21T11:23:36Z",
         "applicationId":"9NBLGGH0XK8Z",
         "budget": 100,
         "budgetType":"Monthly",
         "currencyCode": "USD",
         "type": "Paid",
         "status": "Active",
         "targetingType": "Auto",
         "target": null
      },
      {
         "id":31010025,
         "name":"Contoso App 2 Campaign",
         "createDate":"2016-07-14T10:02:21Z",
         "startDate":"2016-07-21T11:18:44Z",
         "endDate":"2016-08-21T11:18:44Z",
         "applicationId":"9WZDNCRDX48S",
         "budget": 50,
         "budgetType":"Total",
         "currencyCode": "USD",
         "type": "Paid",
         "status": "Ended",
         "targetingType": "Manual",
         "target": {
            "Country": [
               "US",
               "BR",
               "FR",
               "IN",
               "IT"
            ],
            "OsVersion": [
               "8.X"
            ],
            "Age": [
               "Age18To24",
               "Age25To34",
               "Age35To49",
               "Age50AndAbove"
            ],
            "Gender": [
               "Male"
            ],
            "DeviceType": [
               "PC/Tablet",
               "Phone"
            ]
         }
      }
   ]
}
```

## 相关主题

* [应用安装广告报告](../publish/app-install-ads-reports.md)
* [为你的应用创建广告市场活动](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app)
* [使用 Windows 应用商店服务访问分析数据](access-analytics-data-using-windows-store-services.md)



<!--HONumber=Nov16_HO1-->


