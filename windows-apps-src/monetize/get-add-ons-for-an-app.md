---
ms.assetid: E59FB6FE-5318-46DF-B050-73F599C3972A
description: 在 Microsoft Store 提交 API 中使用此方法来检索有关的应用程序注册到合作伙伴中心应用内购买信息。
title: 获取应用的加载项
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 加载项, 应用内产品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 8211d65f04b7487aeca6f683375fe87b80d1b9a9
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334995"
---
# <a name="get-add-ons-for-an-app"></a>获取应用的加载项

在 Microsoft Store 提交 API 中使用此方法，若要列出到合作伙伴中心帐户注册的应用外接程序。

## <a name="prerequisites"></a>系统必备

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求正文的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts` |


### <a name="request-header"></a>请求头

| Header        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | string | 必需。 Azure AD 访问令牌的格式为 **Bearer** *token*&lt;&gt;。 |


### <a name="request-parameters"></a>请求参数


|  名称  |  在任务栏的搜索框中键入  |  描述  |  必需  |
|------|------|------|------|
|  applicationId  |  string  |  要检索加载项的应用的应用商店 ID。 有关应用商店 ID 的详细信息，请参阅[查看应用标识详细信息](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |  是  |
|  top  |  int  |  要在请求中返回的项数（即，要返回的加载项数）。 如果应用具有的加载项超过在查询中指定的值，响应正文将包括可追加到方法 URI 的相对 URI 路径，用于请求下一页数据。  |  否  |
|  skip |  int  | 返回剩余项之前，在查询中绕过的项数。 使用此参数分页浏览数据集。 例如，top=10 和 skip=0 可检索项目 1 到 10，top=10 和 skip=10 可检索项目 11 到 20，依此类推。   |  否  |


### <a name="request-body"></a>请求正文

请勿为此方法提供请求正文。

### <a name="request-examples"></a>请求示例

以下示例演示了如何为应用列出所有加载项。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listinappproducts HTTP/1.1
Authorization: Bearer <your access token>
```

以下示例演示了如何为应用列出前 10 个加载项。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listinappproducts?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

以下示例演示了应用前 10 个加载项（加载项总数为 53 个）的成功请求返回的 JSON 响应正文。 为简便起见，此示例仅显示该请求返回的前三个加载项。 有关响应正文中这些值的详细信息，请参阅以下部分。

```json
{
  "@nextLink": "applications/9NBLGGH4R315/listinappproducts/?skip=10&top=10",
  "value": [
    {
      "inAppProductId": "9NBLGGH4TNMP"
    },
    {
      "inAppProductId": "9NBLGGH4TNMN"
    },
    {
      "inAppProductId": "9NBLGGH4TNNR"
    },
    // Next 7 add-ons  are omitted for brevity ...
  ],
  "totalCount": 53
}
```

### <a name="response-body"></a>响应正文

| ReplTest1      | 在任务栏的搜索框中键入   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | string | 如果存在数据的其他页，此字符串中包含可附加到基本 `https://manage.devcenter.microsoft.com/v1.0/my/` 请求 URI 的相对路径，用于请求下一页数据。 例如，如果初始请求正文的 *top* 参数设置为 10，但应用有 50 个加载项，响应正文将包含 `applications/{applicationid}/listinappproducts/?skip=10&top=10` 的 @nextLink 值，指示你可以调用 `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listinappproducts/?skip=10&top=10` 请求接下来的 10 个加载项。 |
| 值      | 数组  | 一组列出指定应用的每个加载项应用商店 ID 的对象。 有关每个对象中的数据的详细信息，请参阅[加载项资源](get-app-data.md#add-on-object)。                                                                                                                           |
| totalCount | int    | 查询的数据结果中的总行数（即，指定应用的加载项总数）。    |


## <a name="error-codes"></a>错误代码

如果无法成功完成请求，该响应中会包含以下 HTTP 错误代码之一。

| 错误代码 |  描述   |
|--------|------------------|
| 404  | 找不到任何加载项。 |
| 409  | 外接程序使用合作伙伴中心功能[目前不支持通过 Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md#not_supported)。  |


## <a name="related-topics"></a>相关主题

* [创建和管理使用 Microsoft Store 服务的提交](create-and-manage-submissions-using-windows-store-services.md)
* [获取所有应用](get-all-apps.md)
* [获取应用](get-an-app.md)
* [获取包航班的应用](get-flights-for-an-app.md)
