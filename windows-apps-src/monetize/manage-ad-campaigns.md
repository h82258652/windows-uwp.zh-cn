---
author: Xansky
ms.assetid: 7b07a6ca-4be1-497c-a901-0a2da3762555
description: 在 Microsoft Store 促销 API 中使用此方法创建、编辑和获取促销性广告活动。
title: 管理广告活动
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 促销 API, 广告活动
ms.localizationpriority: medium
ms.openlocfilehash: f707c252e404da3aaf6e82317c80a266f4d91d26
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "4962608"
---
# <a name="manage-ad-campaigns"></a>管理广告活动

在 [Microsoft Store 促销 API](run-ad-campaigns-using-windows-store-services.md) 中使用这些方法来创建、编辑和获取适合你的应用的促销性广告活动。 使用此方法创建的每个活动只能与一个应用关联。

>**注意**&nbsp;&nbsp;你还可以使用 Windows 开发人员中心仪表板创建和管理广告活动，并在仪表板中访问以编程方式创建的活动。 有关在仪表板中管理广告活动的详细信息，请参阅[为你的应用创建广告活动](../publish/create-an-ad-campaign-for-your-app.md)。

使用这些方法创建或更新活动时，你通常还需要调用以下一种或多种方法来管理与活动关联的*投放渠道*、*目标市场配置文件*和*创意*。 有关活动与投放渠道、目标市场配置文件和创意之间关系的详细信息，请参阅[使用 Microsoft Store 服务开展广告活动](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api)。

* [管理广告活动的投放渠道](manage-delivery-lines-for-ad-campaigns.md)
* [管理广告活动的目标市场配置文件](manage-targeting-profiles-for-ad-campaigns.md)
* [管理广告活动的创意](manage-creatives-for-ad-campaigns.md)

## <a name="prerequisites"></a>先决条件

若要使用这些方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 促销 API 的所有[先决条件](run-ad-campaigns-using-windows-store-services.md#prerequisites)。

  >**注意**&nbsp;&nbsp;作为必备条件的一部分，请确保[在开发人员中心仪表板中创建至少一个付费广告活动](../publish/create-an-ad-campaign-for-your-app.md)并在仪表板中为广告活动添加至少一种付款方式。 使用此 API 创建的广告活动的投放渠道将按照在仪表板中的**推广你的应用**页面中选择的默认付款方式自动进行计费。

* [获取 Azure AD 访问令牌](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在这些方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。


## <a name="request"></a>请求

这些方法具有以下 URI。

| 方法类型 | 请求 URI                                                      |  描述  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  创建新广告活动。  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  编辑通过 *campaignId* 指定的广告活动。  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  获取通过 *campaignId* 指定的广告活动。  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  查询广告活动。 请参阅[参数](#parameters)部分了解支持的查询参数。  |


### <a name="header"></a>Header

| 标头        | 类型   | 说明         |
|---------------|--------|---------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |
| 跟踪 ID   | GUID   | 选填。 跟踪调用流的 ID。                                  |


<span id="parameters"/> 

### <a name="parameters"></a>参数

用于查询广告活动的 GET 方法支持以下可选的查询参数。

| 名称        | 类型   |  描述      |    
|-------------|--------|---------------|------|
| skip  |  int   | 要在查询中跳过的行数。 使用此参数分页浏览数据集。 例如，fetch=10 和 skip=0，将检索前 10 行数据；top=10 和 skip=10，将检索之后的 10 行数据，依此类推。    |       
| fetch  |  整数值   | 要在请求中返回的数据行数。    |       
| campaignSetSortColumn  |  字符串   | 将响应正文中的[活动](#campaign)对象按指定字段排序。 语法为 <em>CampaignSetSortColumn=field</em>，其中的 <em>field</em> 参数可以是以下字符串之一：</p><ul><li><strong>id</strong></li><li><strong>createdDateTime</strong></li></ul><p>默认为 **createdDateTime**。     |     
| isDescending  |  布尔值   | 将响应正文中的[活动](#campaign)对象按升序或降序排列。   |         
| storeProductId  |  字符串   | 使用此值只返回与具有指定[应用商店 ID](in-app-purchases-and-trials.md#store-ids) 的应用关联的广告活动。 产品应用商店 ID 示例：9nblggh42cfd。   |         
| label  |  字符串   | 使用此值只返回包含在[活动](#campaign)对象中指定的 *label* 的广告活动。    |       |    


### <a name="request-body"></a>请求正文

POST 和 PUT 方法需要一个 JSON 请求正文，其中包含[活动](#campaign)对象的必填字段以及你要设置或更改的任何其他字段。


### <a name="request-examples"></a>请求示例

下面的示例演示如何调用 POST 方法来创建广告活动。

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign",
    "storeProductId": "9nblggh42cfd",
    "configuredStatus": "Active",
    "objective": "DriveInstalls",
    "type": "Community"
}
```

下面的示例演示如何调用 GET 方法来检索特定的广告活动。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/31043481  HTTP/1.1
Authorization: Bearer <your access token>
```

下面的示例演示如何调用 GET 方法查询一组广告活动（按创建日期排序）。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign?storeProductId=9nblggh42cfd&fetch=100&skip=0&campaignSetSortColumn=createdDateTime HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>响应

这些方法返回一个 JSON 响应正文，其中包含一个或多个[活动](#campaign)对象，具体视你调用的方法而定。 下面的示例演示特定广告活动的 GET 方法的响应正文。

```json
{
    "Data": {
        "id": 31043481,
        "name": "Contoso App Campaign",
        "createdDate": "2017-01-17T10:12:15Z",
        "storeProductId": "9nblggh42cfd",
        "configuredStatus": "Active",
        "effectiveStatus": "Active",
        "effectiveStatusReasons": [
            "{\"ValidationStatusReasons\":null}"
        ],
        "labels": [],
        "objective": "DriveInstalls",
        "type": "Paid",
        "lines": [
            {
                "id": 31043476,
                "name": "Contoso App Campaign - Paid Line"
            }
        ]
    }
}
```


<span id="campaign"/>

## <a name="campaign-object"></a>活动对象

这些方法的请求和响应正文包含以下字段。 这张表列出了 POST 方法请求正文中的哪些字段是只读字段（意味着不能在 PUT 方法中更改它们）以及哪些字段是必填字段。

| 字段        | 类型   |  描述      |  只读  | 默认值  | POST 必填字段 |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  整数   |  广告活动的 ID。     |   是    |      |  否     |       
|  name   |  字符串   |   广告活动的名称。    |    否   |      |  是     |       
|  configuredStatus   |  字符串   |  以下值之一，用于指定开发人员指定的广告活动的状态： <ul><li>**Active**</li><li>**Inactive**</li></ul>     |  否     |  Active    |   是    |       
|  effectiveStatus   |  字符串   |   以下值之一，用于根据系统验证情况指定广告活动的有效状态： <ul><li>**Active**</li><li>**Inactive**</li><li>**Processing**</li></ul>    |    是   |      |   否      |       
|  effectiveStatusReasons   |  数组   |  以下值中的一个或多个，用于指定广告活动处于此有效状态的原因： <ul><li>**AdCreativesInactive**</li><li>**BillingFailed**</li><li>**AdLinesInactive**</li><li>**ValidationFailed**</li><li>**Failed**</li></ul>      |  是     |     |    否     |       
|  storeProductId   |  字符串   |  与广告活动关联的应用的[应用商店 ID](in-app-purchases-and-trials.md#store-ids)。 产品应用商店 ID 示例：9nblggh42cfd。     |   是    |      |  是     |       
|  labels   |  数组   |   一个或多个字符串，代表活动的自定义标签。 这些标签用于搜索和标记活动。    |   否    |  null    |    否     |       
|  type   | 字符串    |  以下值之一，用于指定活动类型： <ul><li>**Paid**</li><li>**House**</li><li>**Community**</li></ul>      |   是    |      |   是    |       
|  objective   |  字符串   |  以下值之一，用于指定活动的目标： <ul><li>**DriveInstall**</li><li>**DriveReengagement**</li><li>**DriveInAppPurchase**</li></ul>     |   否    |  DriveInstall    |   是    |       
|  lines   |  数组   |   标识与广告活动关联的[投放渠道](manage-delivery-lines-for-ad-campaigns.md#line)的一个或多个对象。 此字段中的每个对象都由 *id* 和 *name*（分别指定投放渠道的 ID 和名称）字段组成。     |   否    |      |    否     |       
|  createdDate   |  字符串   |  广告活动的创建日期和时间（ISO 8601 格式）。     |  是     |      |     否    |       |


## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务开展广告市场活动](run-ad-campaigns-using-windows-store-services.md)
* [管理广告活动的投放渠道](manage-delivery-lines-for-ad-campaigns.md)
* [管理广告活动的目标市场配置文件](manage-targeting-profiles-for-ad-campaigns.md)
* [管理广告活动的创意](manage-creatives-for-ad-campaigns.md)
* [获取广告活动效果数据](get-ad-campaign-performance-data.md)
