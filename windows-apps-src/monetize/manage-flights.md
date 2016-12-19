---
author: mcleanbyron
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: "在 Windows 应用商店提交 API 中使用这些方法，可管理已注册到 Windows 开发人员中心帐户的应用的软件包外部测试版。"
title: "使用 Windows 应用商店提交 API 管理软件包外部测试版"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: de1c23d721cee67d813520e3a23eb553cd90b7e9

---

# 使用 Windows 应用商店提交 API 管理软件包外部测试版




在 Windows 应用商店提交 API 中使用以下方法，可管理已注册到 Windows 开发人员中心帐户的应用的软件包外部测试版。 有关 Windows 应用商店提交 API 的介绍（包括使用 API 的先决条件），请参阅[使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

>**注意**  这些方法只能用于已授予使用 Windows 应用商店提交 API 权限的 Windows 开发人员中心帐户。 并非所有帐户都已启用此权限。 这些方法只可以用于获取、创建或删除软件包外部测试版。 若要为软件包外部测试版创建提交，请参阅[管理软件包外部测试版提交](manage-flight-submissions.md)中的方法。

| 方法        | URI    | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` | 获取已注册到 Windows 开发人员中心帐户的应用的软件包外部测试版的数据。 有关详细信息，请参阅[获取软件包外部测试版](get-a-flight.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights``` | 创建新的软件包外部测试版。 有关详细信息，请参阅[创建软件包外部测试版](create-a-flight.md)。|
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` | 删除软件包外部测试版。 有关详细信息，请参阅[删除软件包外部测试版](delete-a-flight.md)。 |


## 先决条件

如果尚未开始操作，请先完成 Windows 应用商店提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)，然后再尝试使用其中任何方法。

## 相关主题

* [使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理软件包外部测试版提交](manage-flight-submissions.md)



<!--HONumber=Aug16_HO5-->


