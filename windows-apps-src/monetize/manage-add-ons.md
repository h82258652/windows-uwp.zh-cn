---
ms.assetid: 4F9657E5-1AF8-45E0-9617-45AF64E144FC
description: 在 Microsoft Store 提交 API 中使用这些方法来管理到合作伙伴中心帐户注册的应用的加载项。
title: 管理加载项
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 加载项, 应用内产品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 51c940fffde3c770f397999e566570410528a1e8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617822"
---
# <a name="manage-add-ons"></a>管理加载项

使用 Microsoft Store 提交 API 中的以下方法管理应用的加载项。 有关 Microsoft Store 提交 API 的介绍（包括使用 API 的先决条件），请参阅[使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

这些方法仅可用于获取、创建或删除加载项。 若要创建加载项提交，请参阅[管理加载项提交](manage-add-on-submissions.md)中的方法。

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">方法</th>
<th align="left">URI</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts</td>
<td align="left"><a href="get-all-add-ons.md">获取您的应用程序的所有外接程序</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}</td>
<td align="left"><a href="get-an-add-on.md">获取特定外接程序</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts</td>
<td align="left"><a href="create-an-add-on.md">创建外接程序</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}</td>
<td align="left"><a href="delete-an-add-on.md">删除外接程序</a></td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>必备条件

如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)，然后再尝试使用其中任何方法。

## <a name="data-resources"></a>数据资源

管理加载项的 Microsoft Store 提交 API 方法使用以下 JSON 数据资源。

<span id="add-on-object" />

### <a name="add-on-resource"></a>加载项资源

此资源描述加载项。

```json
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
  "productId": "TestAddOn",
  "productType": "Durable",
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
  "lastPublishedInAppProductSubmission": {
    "id": "1152921504621243705",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
  }
}
```

此资源具有以下值。

| 值      | 在任务栏的搜索框中键入   | 描述        |
|------------|--------|--------------|
| applications      | 数组  | 包含表示应用（与此加载项相关联）的[应用程序资源](#application-object)的数组。 只有一个项在此数组中受支持。  |
| id | 字符串  | 加载项的应用商店 ID。 此值由应用商店提供。 应用商店 ID 的一个示例是 9NBLGGH4TNMP。  |
| productId | 字符串  | 加载项的产品 ID。 这是在创建加载项时由开发人员提供的 ID。 有关详细信息，请参阅[设置你的产品类型和产品 ID](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id)。 |
| productType | 字符串  | 加载项的产品类型。 支持以下值：**持久**并**可使用**。  |
| lastPublishedInAppProductSubmission       | 对象 | 提供有关加载项的上次发布提交的信息的[提交资源](#submission-object)。         |
| pendingInAppProductSubmission        | 对象  |  提供有关加载项的当前挂起提交的信息的[提交资源](#submission-object)。  |   |

<span id="application-object" />

### <a name="application-resource"></a>应用程序资源

此资源描述与加载项相关联的应用。 以下示例演示了此资源的格式。

```json
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
}
```

此资源具有以下值。

| 值           | 在任务栏的搜索框中键入    | 描述        |
|-----------------|---------|-----------|
| value            | 对象  |  包含以下值的对象： <br/><br/> <ul><li>*id*。应用的应用商店 ID。 有关应用商店 ID 的详细信息，请参阅[查看应用标识详细信息](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。</li><li>*resourceLocation*。 可追加到基本 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 请求 URI 的相对路径，用于检索应用的完整数据。</li></ul>   |
| totalCount   | int  | 响应正文的 *applications* 数组中的应用对象数。                                                                                                                                                 |

<span id="submission-object" />

### <a name="submission-resource"></a>提交资源

该资源提供有关加载项提交的信息。 以下示例演示了此资源的格式。

```json
{
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
}
```

此资源具有以下值。

| 值           | 在任务栏的搜索框中键入    | 描述     |
|-----------------|---------|------------------|
| id            | 字符串  | 提交的 ID。    |
| resourceLocation   | 字符串  | 可追加到基本 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 请求 URI 的相对路径，用于检索提交的完整数据。     |
 
<span/>

## <a name="related-topics"></a>相关主题

* [创建和管理使用 Microsoft Store 服务的提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理外接程序使用 API 的 Microsoft Store 提交的提交](manage-add-on-submissions.md)
* [获取所有外接程序](get-all-add-ons.md)
* [获取外接程序](get-an-add-on.md)
* [创建外接程序](create-an-add-on.md)
* [删除外接程序](delete-an-add-on.md)
