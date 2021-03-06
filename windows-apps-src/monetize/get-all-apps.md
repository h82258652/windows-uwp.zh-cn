---
ms.assetid: 2BCFF687-DC12-49CA-97E4-ACEC72BFCD9B
description: 在 Microsoft Store 提交 API 中使用此方法检索到合作伙伴中心帐户注册的所有应用的相关信息。
title: 获取所有应用
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 应用
ms.localizationpriority: medium
ms.openlocfilehash: 267e1d4de3917ae332cdfe15309f3871ef7b6647
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641982"
---
# <a name="get-all-apps"></a>获取所有应用


在 Microsoft Store 提交 API 中使用此方法检索到合作伙伴中心帐户注册的应用的数据。

## <a name="prerequisites"></a>必备条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求正文的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications` |


### <a name="request-header"></a>请求头

| 标头        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** *token*&lt;&gt;。 |


### <a name="request-parameters"></a>请求参数

此方法的所有请求参数均为选填。 如果调用此方法不带参数，则响应包含数据的前 10 个应用程序注册到你的帐户。

|  参数  |  在任务栏的搜索框中键入  |  描述  |  必需  |
|------|------|------|------|
|  top  |  int  |  要在请求中返回的项数（即，要返回的应用数）。 如果帐户具有的应用超过在查询中指定的值，响应正文将包括可追加到方法 URI 的相对 URI 路径，用于请求下一页数据。  |  否  |
|  skip  |  int  |  返回剩余项之前，在查询中绕过的项数。 使用此参数分页浏览数据集。 例如，top=10 和 skip=0 可检索项目 1 到 10，top=10 和 skip=10 可检索项目 11 到 20，依此类推。  |  否  |


### <a name="request-body"></a>请求正文

请勿为此方法提供请求正文。

### <a name="request-examples"></a>请求示例

以下示例演示了如何检索注册到帐户的前 10 个应用。

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications HTTP/1.1
Authorization: Bearer <your access token>
```

以下示例演示了如何检索注册到帐户的所有应用的相关信息。 首先，获取前 10 个应用：

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

然后以递归方式调用`GET https://manage.devcenter.microsoft.com/v1.0/my/{@nextLink}`直到`{@nextlink}`为 null 或响应中不存在。 例如：

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=10&top=10 HTTP/1.1
Authorization: Bearer <your access token>
```
  
```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=20&top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=30&top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

如果您已经知道您的帐户中有的应用的总数，可以只需将传递该数**顶部**参数，以获取有关所有应用的信息。

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?top=23 HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>响应

以下示例演示了注册到开发人员帐户前 10 个应用（应用总数为 21 个）的成功请求返回的 JSON 响应正文。 为简便起见，此示例仅显示该请求返回的前两个应用。 有关响应正文中这些值的详细信息，请参阅以下部分。

```json
{
  "@nextLink": "applications?skip=10&top=10",
  "value": [
    {
      "id": "9NBLGGH4R315",
      "primaryName": "Contoso sample app",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-11T01:32:11.0747851Z",
      "pendingApplicationSubmission": {
        "id": "1152921504621134883",
        "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621134883"
      }
    },
    {
      "id": "9NBLGGH29DM8",
      "primaryName": "Contoso sample app 2",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp2_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp2",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-12T01:49:11.0747851Z",
      "lastPublishedApplicationSubmission": {
        "id": "1152921504621225621",
        "resourceLocation": "applications/9NBLGGH29DM8/submissions/1152921504621225621"
      }
      // Next 8 apps are omitted for brevity ...
    }
  ],
  "totalCount": 21
}
```

### <a name="response-body"></a>响应正文

| 值      | 在任务栏的搜索框中键入   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| value      | 数组  | 包含有关每个注册到帐户的应用的对象组。 有关每个对象中的数据的详细信息，请参阅[应用程序资源](get-app-data.md#application_object)。                                                                                                                           |
| @nextLink  | 字符串 | 如果存在数据的其他页，此字符串中包含可附加到基本 `https://manage.devcenter.microsoft.com/v1.0/my/` 请求 URI 的相对路径，用于请求下一页数据。 例如，如果初始请求正文的 *top* 参数设置为 10，但有 20 个应用注册到你的帐户，响应正文将包含 `applications?skip=10&top=10` 的 @nextLink 值，指示你可以调用 `https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=10&top=10` 请求接下来的 10 个应用。 |
| totalCount | int    | 查询的数据结果中的总行数（即注册到帐户的应用总数）。                                                |


## <a name="error-codes"></a>错误代码

如果无法成功完成请求，该响应中会包含以下 HTTP 错误代码之一。

| 错误代码 |  描述   |
|--------|------------------|
| 404  | 没有找到任何应用。 |
| 409  | 应用使用合作伙伴中心功能[目前不支持通过 Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md#not_supported)。  |


## <a name="related-topics"></a>相关主题

* [创建和管理使用 Microsoft Store 服务的提交](create-and-manage-submissions-using-windows-store-services.md)
* [获取应用](get-an-app.md)
* [获取包航班的应用](get-flights-for-an-app.md)
* [获取外接程序的应用](get-add-ons-for-an-app.md)
