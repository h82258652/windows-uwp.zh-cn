---
author: mcleanbyron
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: "在 Windows 应用商店提交 API 中使用这些方法，为注册到 Windows 开发人员中心帐户的应用检索数据。"
title: "使用 Windows 应用商店提交 API 获取应用数据"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 99d609decc8c38952961deac5bb8ec6926d91c88

---

# 使用 Windows 应用商店提交 API 获取应用数据

在 Windows 应用商店提交 API 中使用以下方法，为注册到 Windows 开发人员中心帐户的应用获取数据。 有关 Windows 应用商店提交 API 的介绍，请参阅[使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

>**注意**  这些方法只能用于已授予使用 Windows 应用商店提交 API 权限的 Windows 开发人员中心帐户。 并非所有帐户都已启用此权限。 这些方法仅可以用于获取应用的数据。 若要创建或管理应用提交，请参阅[管理应用提交](manage-app-submissions.md)中的方法。

| 方法        | URI    | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications``` | 为注册到 Windows 开发人员中心帐户的所有应用检索数据。 有关详细信息，请参阅[获取所有应用](get-all-apps.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}``` | 检索有关注册到 Windows 开发人员中心帐户的特定应用的数据。 有关详细信息，请参阅[获取应用](get-an-app.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts``` | 为注册到 Windows 开发人员中心帐户的应用列出加载项（也称为应用内产品或 IAP）。 有关详细信息，请参阅[获取应用的加载项](get-add-ons-for-an-app.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights``` | 为注册到 Windows 开发人员中心帐户的应用列出软件包外部测试版。 有关详细信息，请参阅[获取应用的软件包外部测试版](get-flights-for-an-app.md)。 |

<span/>

## 先决条件

如果尚未开始操作，请先完成 Windows 应用商店提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)，然后再尝试使用其中任何方法。

## 资源

这些方法使用以下资源设置数据格式。

<span id="application_object" />
### 应用程序

此资源表示已注册到你的帐户的应用。 以下示例演示了此资源的格式。

```json
{
  "id": "9NBLGGH4R315",
  "primaryName": "ApiTestApp",
  "packageFamilyName": "30481DevCenterAPITester.ApiTestAppForDevbox_ng6try80pwt52",
  "packageIdentityName": "30481DevCenterAPITester.ApiTestAppForDevbox",
  "publisherName": "CN=…",
  "firstPublishedDate": "1601-01-01T00:00:00Z",
  "lastPublishedApplicationSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621086517"
  },
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621243487"
  }
}
```

此资源具有以下值。

| 值           | 类型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | 字符串  | 应用的应用商店 ID。 有关应用商店 ID 的详细信息，请参阅[查看应用标识详细信息](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。   |
| primaryName   | 字符串  | 应用的显示名称。                                                                                                                                                   |
| packageFamilyName | 字符串  | 应用的程序包系列名称。                                                                                                                                                                                                         |
| packageIdentityName          | 字符串  | 应用的程序包标识名称。                                                                                                                                                              |
| publisherName       | 字符串  | 与应用相关联的 Windows 发布者 ID。 这对应于 Windows 开发人员中心仪表板中应用的[应用标识](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)页上显示的 **Package/Identity/Publisher** 值。                                                                                             |
| firstPublishedDate      | 字符串  | 应用的首次发布日期，采用 ISO 8601 格式。                                                                                         |
| lastPublishedApplicationSubmission       | 对象 | 提供有关应用的上次发布提交的信息的对象。 有关详细信息，请参阅下面的[提交](#submission_object)部分。                                                                                                                                                          |
| pendingApplicationSubmission        | 对象  |  提供有关应用的当前挂起提交的信息的对象。 有关详细信息，请参阅下面的[提交](#submission_object)部分。  |   |


<span id="add-on-object" />
### 加载项

该资源提供有关加载项的信息。 以下示例演示了此资源的格式。

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

此资源具有以下值。

| 值           | 类型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inAppProductId            | 字符串  | 加载项的应用商店 ID。 此值由应用商店提供。 应用商店 ID 的一个示例是 9NBLGGH4TNMP。   |


<span id="flight-object" />
### 外部测试版

该资源提供有关应用的软件包外部测试版的信息。 以下示例演示了此资源的格式。

```json
{
    "flightId": "7bfc11d5-f710-47c5-8a98-e04bb5aad310",
    "friendlyName": "myflight",
    "lastPublishedFlightSubmission": {
        "id": "1152921504621086517",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621086517"
    },
    "pendingFlightSubmission": {
        "id": "1152921504621215786",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621215786"
    },
    "groupIds": [
        "1152921504606962205"
    ],
    "rankHigherThan": "Non-flighted submission"
}
```

此资源具有以下值。

| 值           | 类型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | 字符串  | 软件包外部测试版的 ID。 此值由开发人员中心提供。  |
| friendlyName           | 字符串  | 软件包外部测试版的名称，如开发人员所指定。   |
| lastPublishedFlightSubmission       | 对象 | 提供有关软件包外部测试版的上次发布提交的信息的对象。 有关详细信息，请参阅下面的[提交](#submission_object)部分。  |
| pendingFlightSubmission        | 对象  |  提供有关软件包外部测试版的当前挂起提交的信息的对象。 有关详细信息，请参阅下面的[提交](#submission_object)部分。  |    
| groupIds           | 数组  | 包含与软件包外部测试版关联的外部测试版组 ID 的字符串数组。 有关外部测试版组的详细信息，请参阅[软件包外部测试版](https://msdn.microsoft.com/windows/uwp/publish/package-flights)。   |
| rankHigherThan           | 字符串  | 排名紧跟在当前软件包外部测试版之后的软件包外部测试版的友好名称。 有关排名的外部测试版组的详细信息，请参阅[软件包外部测试版](https://msdn.microsoft.com/windows/uwp/publish/package-flights)。  |


<span id="submission_object" />
### 提交

该资源提供有关提交的信息。 以下示例演示了此资源的格式。

```json
{
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9WZDNCRD9MMD/submissions/1152921504621243487"
  }
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
* [使用 Windows 应用商店提交 API 管理应用提交](manage-app-submissions.md)
* [获取所有应用](get-all-apps.md)
* [获取应用](get-an-app.md)
* [获取应用的加载项](get-add-ons-for-an-app.md)
* [获取应用的软件包外部测试版](get-flights-for-an-app.md)



<!--HONumber=Aug16_HO5-->


