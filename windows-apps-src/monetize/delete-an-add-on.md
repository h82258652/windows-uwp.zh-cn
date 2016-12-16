---
author: mcleanbyron
ms.assetid: 16D4C3B9-FC9B-46ED-9F87-1517E1B549FA
description: "在 Windows 应用商店提交 API 中使用此方法，可删除注册到 Windows 开发人员中心帐户的应用的加载项。"
title: "使用 Windows 应用商店提交 API 删除加载项"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 8c28205ca004bd63b3f6a3fea2971cf927f7e2e4

---

# 使用 Windows 应用商店提交 API 删除加载项




在 Windows 应用商店提交 API 中使用此方法，可删除注册到 Windows 开发人员中心帐户的应用的加载项（也称为应用内产品或 IAP）。

## 先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Windows 应用商店提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

>**注意**  此方法只可以用于授予使用 Windows 应用商店提交 API 权限的 Windows 开发人员中心帐户。 并非所有帐户都已启用此权限。

## 请求

此方法具有以下语法。 请参阅以下部分，获取标头和请求正文的使用示例和描述。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}``` |

<span/>
 

### 请求标头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |

<span/>

### 请求参数

| 名称        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| id | 字符串 | 必需。 要删除的加载项的应用商店 ID。 开发人员中心仪表板上会提供该应用商店 ID。  |

<span/>

### 请求正文

请勿为此方法提供请求正文。

<span/>

### 请求示例

以下示例演示了如何删除加载项。

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP HTTP/1.1
Authorization: Bearer <your access token>
```

## 响应

如果成功，此方法会返回空的响应正文。

## 错误代码

如果无法成功完成请求，该响应中会包含以下 HTTP 错误代码之一。

| 错误代码 |  描述                                                                                                                                                                           |
|--------|------------------|
| 400  | 请求无效。 |
| 404  | 找不到指定的加载项。  |
| 409  | 指定的加载项已找到，但在其当前状态下无法删除；或者加载项使用的开发人员中心仪表板功能[当前不受 Windows 应用商店提交 API 支持](create-and-manage-submissions-using-windows-store-services.md#not_supported)。 |   

<span/>

## 相关主题

* [使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [获取所有加载项](get-all-add-ons.md)
* [获取加载项](get-an-add-on.md)
* [创建加载项](create-an-add-on.md)



<!--HONumber=Aug16_HO5-->


