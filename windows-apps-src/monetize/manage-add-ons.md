---
author: mcleanbyron
ms.assetid: 4F9657E5-1AF8-45E0-9617-45AF64E144FC
description: "在 Windows 应用商店提交 API 中使用这些方法，管理已注册到 Windows 开发人员中心帐户的应用的加载项。"
title: "使用 Windows 应用商店提交 API 管理加载项"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 75548ee4689fd31d734c570f8e3eca5d33a6181f

---

# 使用 Windows 应用商店提交 API 管理加载项




在 Windows 应用商店提交 API 中使用以下方法，管理已注册到 Windows 开发人员中心帐户的应用的加载项（也称为应用内产品或 IAP）。 有关 Windows 应用商店提交 API 的介绍，请参阅[使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

>**注意**&nbsp;&nbsp;这些方法只能用于已授予使用 Windows 应用商店提交 API 权限的 Windows 开发人员中心帐户。 并非所有帐户都已启用此权限。 这些方法仅可用于获取、创建或删除加载项。 若要创建加载项提交，请参阅[管理加载项提交](manage-add-on-submissions.md)中的方法。

| 方法        | URI    | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` | 获取已注册到 Windows 开发人员中心帐户的所有应用的所有加载项数据。 有关详细信息，请参阅[获取所有加载项](get-all-add-ons.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId} ``` | 获取某个特定加载项的数据。 有关详细信息，请参阅[获取加载项](get-an-add-on.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` | 创建新加载项。 有关详细信息，请参阅[创建加载项](create-an-add-on.md)。  |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}``` | 删除加载项。 有关详细信息，请参阅[删除加载项](delete-an-add-on.md)。 |

## 先决条件

如果尚未开始操作，请先完成 Windows 应用商店提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)，然后再尝试使用其中任何方法。

## 资源

这些方法使用以下资源设置数据格式。

<span id="add-on-object" />
### 加载项

此资源表示加载项。 以下示例演示了此资源的格式。

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

| 值      | 类型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applications      | 数组  | 包含表示应用（与此加载项相关联）的对象的数组。 有关此对象中的数据的详细信息，请参阅下面的[应用程序对象](#application-object)部分。 只有一个项在此数组中受支持。  |
| id | 字符串  | 加载项的应用商店 ID。 此值由应用商店提供。 应用商店 ID 的一个示例是 9NBLGGH4TNMP。  |
| productId | 字符串  | 加载项的产品 ID。 这是在创建加载项时由开发人员提供的 ID。 有关详细信息，请参阅[设置你的产品类型和产品 ID](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id)。 |
| productType | 字符串  | 加载项的产品类型。 支持以下值：**Durable** 和 **Consumable**。  |
| lastPublishedInAppProductSubmission       | 对象 | 提供有关加载项的上次发布提交的信息的对象。 有关详细信息，请参阅下面的[提交](#submission-object)部分。                                                                                                                                                          |
| pendingInAppProductSubmission        | 对象  |  提供有关加载项的当前挂起提交的信息的对象。 有关详细信息，请参阅下面的[提交](#submission-object)部分。  |   |

<span id="application-object" />
### 应用程序

此资源表示与加载项相关联的应用。 以下示例演示了此资源的格式。

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

| 值           | 类型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|-----------|
| 值            | 对象  |  包含以下值的对象： <br/><br/> <ul><li>*id*。 应用的应用商店 ID。 有关应用商店 ID 的详细信息，请参阅[查看应用标识详细信息](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。</li><li>*resourceLocation*。 可追加到基本 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 请求 URI 的相对路径，用于检索应用的完整数据。</li></ul>   |
| totalCount   | int  | 响应正文的 *applications* 数组中的应用对象数。                                                                                                                                                 |

<span id="submission-object" />
### 提交

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

| 值           | 类型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | 字符串  | 提交的 ID。    |
| resourceLocation   | 字符串  | 可追加到基本 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 请求 URI 的相对路径，用于检索提交的完整数据。                                                                                                                                               |
 
<span/>

## 相关主题

* [使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [使用 Windows 应用商店提交 API 管理加载项提交](manage-add-on-submissions.md)
* [获取所有加载项](get-all-add-ons.md)
* [获取加载项](get-an-add-on.md)
* [创建加载项](create-an-add-on.md)
* [删除加载项](delete-an-add-on.md)



<!--HONumber=Aug16_HO5-->


