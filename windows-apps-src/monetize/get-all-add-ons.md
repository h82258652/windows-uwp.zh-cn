---
author: Xansky
ms.assetid: 7B6A99C6-AC86-41A1-85D0-3EB39A7211B6
description: 在 Microsoft Store 提交 API 中使用这些方法，为注册到 Windows 开发人员中心帐户的应用检索所有加载项数据。
title: 获取所有加载项
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 提交 API, 加载项, 应用内产品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: c711e2443de4607d2266dcddf513a48ff11522a7
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "5445243"
---
# <a name="get-all-add-ons"></a>获取所有加载项

在 Microsoft Store 提交 API 中使用这些方法，检索注册到 Windows 开发人员中心帐户的所有应用的所有加载项的数据。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求正文的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

此方法的所有请求参数均为选填。 如果不填写参数即调用此方法，则响应将包含注册到帐户的所有应用的所有加载项数据。

|  参数  |  类型  |  说明  |  必填  |
|------|------|------|------|
|  top  |  int  |  要在请求中返回的项数（即，要返回的加载项数）。 如果帐户具有的加载项超过在查询中指定的值，响应正文将包括可追加到方法 URI 的相对 URI 路径，用于请求下一页数据。  |  否  |
|  skip  |  int  |  返回剩余项之前，在查询中绕过的项数。 使用此参数分页浏览数据集。 例如，top=10 和 skip=0 可检索项目 1 到 10，top=10 和 skip=10 可检索项目 11 到 20，依此类推。  |  否  |


### <a name="request-body"></a>请求正文

请勿为此方法提供请求正文。

### <a name="request-examples"></a>请求示例

以下示例演示如何检索注册到帐户的所有应用的所有加载项数据。

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer <your access token>
```

以下示例演示如何仅检索前 10 个加载项。

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

以下示例演示了注册到开发人员帐户前 5 个加载项（加载项总数为 1072 个）的成功请求返回的 JSON 响应正文。 为简便起见，此示例仅显示该请求返回的前两个加载项。 有关响应正文中这些值的详细信息，请参阅以下部分。

```json
{
  "@nextLink": "inappproducts/?skip=5&top=5",
  "value": [
    {
      "applications": {
        "value": [
          {
            "id": "9NBLGGH4R315",
            "resourceLocation": "applications/9NBLGGH4R315"
          }
        ],
        "totalCount": 1
      },
      "id": "9NBLGGH4TNMP",
      "productId": "a8b8310b-fa8d-4da0-aca0-577ef6dce4dd",
      "productType": "Consumable",
      "pendingInAppProductSubmission": {
        "id": "1152921504621243619",
        "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
      },
      "lastPublishedInAppProductSubmission": {
        "id": "1152921504621243705",
        "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
      }
    },
    {
      "applications": {
        "value": [
          {
            "id": "9NBLGGH4R315",
            "resourceLocation": "applications/9NBLGGH4R315"
          }
        ],
        "totalCount": 1
      },
      "id": "9NBLGGH4TNMN",
      "productId": "6a3c9788-a350-448a-bd32-16160a13018a",
      "productType": "Consumable",
      "pendingInAppProductSubmission": {
        "id": "1152921504621243538",
        "resourceLocation": "inappproducts/9NBLGGH4TNMN/submissions/1152921504621243538"
      },
      "lastPublishedInAppProductSubmission": {
        "id": "1152921504621243106",
        "resourceLocation": "inappproducts/9NBLGGH4TNMN/submissions/1152921504621243106"
      }
    },

  // Other add-ons omitted for brevity...
  ],
  "totalCount": 1072
}
```

### <a name="response-body"></a>响应正文

| 值      | 类型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | 字符串 | 如果存在数据的其他页，此字符串中包含可附加到基本 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 请求 URI 的相对路径，用于请求下一页数据。 例如，如果初始请求正文的 *top* 参数设置为 10，但有 100 个加载项注册到你的帐户，响应正文将包含 ```inappproducts?skip=10&top=10``` 的 @nextLink 值，指示你可以调用 ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts?skip=10&top=10``` 请求接下来的 10 个加载项。 |
| 值            | 数组  |  包含提供有关每个加载项信息的对象的数组。 有关详细信息，请参阅[加载项资源](manage-add-ons.md#add-on-object)。   |
| totalCount   | int  | 响应正文的 *value* 数组中的应用对象数。     |


## <a name="error-codes"></a>错误代码

如果无法成功完成请求，该响应中会包含以下 HTTP 错误代码之一。

| 错误代码 |  描述   |
|--------|------------------|
| 404  | 找不到任何加载项。 |
| 409  | 应用或加载项使用的开发人员中心仪表板功能[当前不受 Microsoft Store 提交 API 支持](create-and-manage-submissions-using-windows-store-services.md#not_supported)。  |


## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理加载项提交](manage-add-on-submissions.md)
* [获取加载项](get-an-add-on.md)
* [创建加载项](create-an-add-on.md)
* [删除加载项](delete-an-add-on.md)
