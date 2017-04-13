---
author: mcleanbyron
ms.assetid: 4F9657E5-1AF8-45E0-9617-45AF64E144FC
description: "在 Windows 应用商店提交 API 中使用这些方法，管理已注册到 Windows 开发人员中心帐户的应用的加载项。"
title: "管理加载项"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, Windows 应用商店提交 API, 加载项, 应用内产品, IAP"
ms.openlocfilehash: b442f48b7d03f0f972882ec240dbc1f37f018fd9
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="manage-add-ons"></a>管理加载项

在 Windows 应用商店提交 API 中使用以下方法，管理应用的加载项（也称为应用内产品或 IAP）。 有关 Windows 应用商店提交 API 的介绍（包括使用 API 的先决条件），请参阅[使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

>**注意**&nbsp;&nbsp;这些方法只能用于已授予使用 Windows 应用商店提交 API 权限的 Windows 开发人员中心帐户。 会阶段性地向开发人员帐户启用此权限，但此时所有帐户并非都已启用了此权限。 若要请求先前的访问权限，请登录到开发人员中心仪表板、单击仪表板底部的**反馈**、选择反馈区域的**提交 API**，然后提交你的请求。 当为你的帐户启用了此权限时，你会收到一封电子邮件。

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
<th align="left">说明</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts```</td>
<td align="left">[获取应用的所有加载项](get-all-add-ons.md)</td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}```</td>
<td align="left">[获取特定加载项](get-an-add-on.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts```</td>
<td align="left">[创建加载项](create-an-add-on.md)</td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}```</td>
<td align="left">[删除加载项](delete-an-add-on.md)</td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>先决条件

如果尚未开始操作，请先完成 Windows 应用商店提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)，然后再尝试使用其中任何方法。

## <a name="data-resources"></a>数据资源

管理加载项的 Windows 应用商店提交 API 方法使用以下 JSON 数据资源。

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

| 值      | 类型   | 说明        |
|------------|--------|--------------|
| applications      | 数组  | 包含表示应用（与此加载项相关联）的[应用程序资源](#application-object)的数组。 只有一个项在此数组中受支持。  |
| id | 字符串  | 加载项的应用商店 ID。 此值由应用商店提供。 应用商店 ID 的一个示例是 9NBLGGH4TNMP。  |
| productId | 字符串  | 加载项的产品 ID。 这是在创建加载项时由开发人员提供的 ID。 有关详细信息，请参阅[设置你的产品类型和产品 ID](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id)。 |
| productType | 字符串  | 加载项的产品类型。 支持以下值：**Durable** 和 **Consumable**。  |
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

| 值           | 类型    | 描述        |
|-----------------|---------|-----------|
| 值            | 对象  |  包含以下值的对象： <br/><br/> <ul><li>*id*。 应用的应用商店 ID。 有关应用商店 ID 的详细信息，请参阅[查看应用标识详细信息](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。</li><li>*resourceLocation*。 可追加到基本 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 请求 URI 的相对路径，用于检索应用的完整数据。</li></ul>   |
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

| 值           | 类型    | 描述     |
|-----------------|---------|------------------|
| id            | 字符串  | 提交的 ID。    |
| resourceLocation   | 字符串  | 可追加到基本 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 请求 URI 的相对路径，用于检索提交的完整数据。     |
 
<span/>
## <a name="related-topics"></a>相关主题

* [使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [使用 Windows 应用商店提交 API 管理加载项提交](manage-add-on-submissions.md)
* [获取所有加载项](get-all-add-ons.md)
* [获取加载项](get-an-add-on.md)
* [创建加载项](create-an-add-on.md)
* [删除加载项](delete-an-add-on.md)
