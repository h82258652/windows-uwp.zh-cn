---
author: mcleanbyron
ms.assetid: 8C1E9E36-13AF-4386-9D0F-F9CB320F02F5
description: "在 Windows 应用商店提交 API 中使用此方法，为注册到 Windows 开发人员中心帐户的应用创建一个软件包外部测试版。"
title: "使用 Windows 应用商店提交 API 创建软件包外部测试版"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 35823bd1fd0c059ebc9b2107c31400a7ad788a1e

---

# 使用 Windows 应用商店提交 API 创建软件包外部测试版




在 Windows 应用商店提交 API 中使用此方法，为注册到 Windows 开发人员中心帐户的应用创建一个软件包外部测试版。

>**注意**  此方法无需任何提交即可创建软件包外部测试版。 若要创建软件包外部测试版的提交，请参阅[管理软件包外部测试版提交](manage-flight-submissions.md)中的方法。

## 先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Windows 应用商店提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

>**注意**  此方法只可以用于授予使用 Windows 应用商店提交 API 权限的 Windows 开发人员中心帐户。 并非所有帐户都已启用此权限。

## 请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求正文的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights``` |

<span/>
 

### 请求标头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |

<span/>

### 请求参数

| 名称        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字符串 | 必需。 要创建软件包外部测试版的应用的应用商店 ID。 有关应用商店 ID 的详细信息，请参阅[查看应用标识详细信息](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |

<span/>

### 请求正文

请求正文具有以下参数。
 
|  参数  |  类型  |  说明  |  必需  |
|------|------|------|------|
|  friendlyName  |  字符串  |  软件包外部测试版的名称，如开发人员所指定。  |  否  |
|  groupIds  |  数组  |  包含与软件包外部测试版关联的外部测试版组 ID 的字符串数组。 有关外部测试版组的详细信息，请参阅[软件包外部测试版](https://msdn.microsoft.com/windows/uwp/publish/package-flights)。  |  否  |
|  rankHigherThan  |  字符串  |  排名紧跟在当前软件包外部测试版之后的软件包外部测试版的友好名称。 如果未设置此参数，新软件包外部测试版在所有软件包外部测试版中排名最高。 有关排名的外部测试版组的详细信息，请参阅[软件包外部测试版](https://msdn.microsoft.com/windows/uwp/publish/package-flights)。    |  否  |

<span/>

### 请求示例

以下示例演示了如何为具有应用商店 ID 9WZDNCRD911W 的应用创建新的软件包外部测试版。

```syntax
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
  "friendlyName": "myflight",
  "groupIds": [
    0
  ],
  "rankHigherThan": null
}

```

## 响应

以下示例演示了成功调用此方法的 JSON 响应正文。 有关响应正文中这些值的更多详细信息，请参阅以下部分。

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
  "groupIds": [
    "0"
  ],
  "rankHigherThan": "671c2857-725e-4faf-9e9e-ea1191ef879c"
}
```

### 响应正文

| 值      | 类型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | 字符串  | 软件包外部测试版的 ID。 此值由开发人员中心提供。  |
| friendlyName           | 字符串  | 软件包外部测试版的名称，如请求中所指定。   |  
| groupIds           | 数组  | 包含与软件包外部测试版关联的外部测试版组 ID 的字符串数组，如请求中所指定。 有关外部测试版组的详细信息，请参阅[软件包外部测试版](https://msdn.microsoft.com/windows/uwp/publish/package-flights)。   |
| rankHigherThan           | 字符串  | 排名紧跟在当前软件包外部测试版之后的软件包外部测试版的友好名称，如请求中所指定。 有关排名的外部测试版组的详细信息，请参阅[软件包外部测试版](https://msdn.microsoft.com/windows/uwp/publish/package-flights)。  |

<span/>

## 错误代码

如果无法成功完成请求，该响应中会包含以下 HTTP 错误代码之一。

| 错误代码 |  描述   |
|--------|------------------|
| 400  | 请求无效。 |
| 409  | 由于应用的当前状态，或者应用使用的开发人员中心仪表板功能[当前不受 Windows 应用商店提交 API 支持](create-and-manage-submissions-using-windows-store-services.md#not_supported)，所以无法创建软件包外部测试版。 |   
<span/>

## 相关主题

* [使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [获取软件包外部测试版](get-a-flight.md)
* [删除软件包外部测试版](delete-a-flight.md)



<!--HONumber=Aug16_HO5-->


