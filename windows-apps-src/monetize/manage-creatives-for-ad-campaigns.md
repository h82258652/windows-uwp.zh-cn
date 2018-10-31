---
author: Xansky
ms.assetid: c5246681-82c7-44df-87e1-a84a926e6496
description: 在 Microsoft Store 促销 API 中使用此方法管理促销性广告活动的创意。
title: 管理创意
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 促销 API, 广告活动
ms.localizationpriority: medium
ms.openlocfilehash: 97a7ac89585cbcf7a4609aee16978d36be027a24
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5830501"
---
# <a name="manage-creatives"></a>管理创意

在 Microsoft Store 促销 API 中使用这些方法上传你自己的自定义创意以便在促销性广告活动中使用，或获取现有创意。 如果某一创意始终代表同一应用，则其可能与一个投放渠道或各广告活动间的多个投放渠道相关。

有关创意与广告活动、投放渠道和目标市场配置文件之间关系的详细信息，请参阅[使用 Microsoft Store 服务开展广告活动](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api)。

> [!NOTE]
> 使用此 API 上载自己的创意时，允许的最大创意大小为 40 KB。 如果提交的创意文件大于此值，则此 API 将不会返回错误，但不会成功创建市场活动。 

## <a name="prerequisites"></a>必备条件

若要使用这些方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 促销 API 的所有[先决条件](run-ad-campaigns-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在这些方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。


## <a name="request"></a>请求

这些方法具有以下 URI。

| 方法类型 | 请求 URI     |  描述  |
|--------|-----------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative``` |  创建新创意。  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/{creativeId}``` |  获取通过 *creativeId* 指定的创意。  |

> [!NOTE]
> 此 API 当前不支持 PUT 方法。


### <a name="header"></a>Header

| 标头        | 类型   | 说明         |
|---------------|--------|---------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |
| 跟踪 ID   | GUID   | 选填。 跟踪调用流的 ID。                                  |


### <a name="request-body"></a>请求正文

POST 方法需要一个 JSON 请求正文，其中包含[创意](#creative)对象的必填字段。


### <a name="request-examples"></a>请求示例

下面的示例演示如何调用 POST 方法来创建创意。 在此示例中，为简洁起见，*content* 值已缩短。

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative HTTP/1.1
Authorization: Bearer <your access token>

{
  "name": "Contoso App Campaign - Creative 1",
  "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
  "height": 80,
  "width": 480,
  "imageAttributes":
  {
    "imageExtension": "PNG"
  }
}
```

下面的示例演示如何调用 GET 方法来检索创意。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/106851  HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>响应

这些方法返回含有[创意](#creative)对象的 JSON 响应正文，创意对象包含有关已创建或已检索的创意的信息。 下面的示例展示了这些方法的响应正文。 在此示例中，为简洁起见，*content* 值已缩短。

```json
{
    "Data": {
        "id": 106126,
        "name": "Contoso App Campaign - Creative 2",
        "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
        "height": 50,
        "width": 300,
        "format": "Banner",
        "imageAttributes":
        {
          "imageExtension": "PNG"
        },
        "storeProductId": "9nblggh42cfd"
    }
}
```


<span id="creative"/>

## <a name="creative-object"></a>创意对象

这些方法的请求和响应正文包含以下字段。 这张表列出了 POST 方法请求正文中的哪些字段是只读字段（意味着不能在 PUT 方法中更改它们）以及哪些字段是必填字段。

| 字段        | 类型   |  描述      |  只读  | 默认值  |  POST 必填字段 |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  整数   |  创意的 ID。     |   是    |      |    否   |       
|  name   |  字符串   |   创意的名称。    |    否   |      |  是     |       
|  content   |  字符串   |  创意图像的内容（Base64 编码格式）。<br/><br/>**注意**&nbsp;&nbsp;允许的最大创意大小为 40 KB。 如果提交的创意文件大于此值，则此 API 将不会返回错误，但不会成功创建市场活动。      |  否     |      |   是    |       
|  height   |  整数   |   创意的高度。    |    否    |      |   是    |       
|  width   |  整数   |  创意的宽度。     |  否    |     |    是   |       
|  landingUrl   |  字符串   |  如果使用市场活动跟踪服务（如 Kochava、AppsFlyer 或 Tune）测量应用的安装分析，则在调用 POST 方法时，请在此字段指定跟踪 URL（如已指定，则此值必须为有效 URL）。 如果未使用市场活动跟踪服务，则在调用 POST 方法时，请忽略此值（在此情况下，将会成功创建此 URL）。   |  否    |     |   是    |       
|  format   |  字符串   |   广告的格式。 当前，唯一受支持的值为 **Banner**。    |   否    |  Banner   |  否     |       
|  imageAttributes   | [ImageAttributes](#image-attributes)    |   提供创意的属性。     |   否    |      |   是    |       
|  storeProductId   |  字符串   |   与广告活动关联的应用的[应用商店 ID](in-app-purchases-and-trials.md#store-ids)。 产品应用商店 ID 示例：9nblggh42cfd。    |   否    |    |  否     |   |  


<span id="image-attributes"/>

## <a name="imageattributes-object"></a>ImageAttributes 对象

| 字段        | 类型   |  描述      |  只读  | 默认值  | POST 必填字段 |  
|--------------|--------|---------------|------|-------------|------------|
|  imageExtension   |   字符串  |   以下值之一：**PNG** 或 **JPG**。    |    否   |      |   是    |       |


## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务开展广告市场活动](run-ad-campaigns-using-windows-store-services.md)
* [管理广告活动](manage-ad-campaigns.md)
* [管理广告活动的投放渠道](manage-delivery-lines-for-ad-campaigns.md)
* [管理广告活动的目标市场配置文件](manage-targeting-profiles-for-ad-campaigns.md)
* [获取广告活动效果数据](get-ad-campaign-performance-data.md)
