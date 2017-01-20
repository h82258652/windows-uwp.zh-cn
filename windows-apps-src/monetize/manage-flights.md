---
author: mcleanbyron
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: "在 Windows 应用商店提交 API 中使用这些方法，可管理已注册到 Windows 开发人员中心帐户的应用的软件包外部测试版。"
title: "使用 Windows 应用商店提交 API 管理软件包外部测试版"
translationtype: Human Translation
ms.sourcegitcommit: 020c8b3f4d9785842bbe127dd391d92af0962117
ms.openlocfilehash: 626a59ba848c9ae1d97b85b67ef2c76eed49065a

---

# <a name="manage-package-flights-using-the-windows-store-submission-api"></a>使用 Windows 应用商店提交 API 管理软件包外部测试版

使用 Windows 应用商店提交 API 中的以下方法管理应用的软件包外部测试版。 有关 Windows 应用商店提交 API 的介绍（包括使用 API 的先决条件），请参阅[使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

>**注意**&nbsp;&nbsp;这些方法只能用于已授予使用 Windows 应用商店提交 API 权限的 Windows 开发人员中心帐户。 并非所有帐户都已启用此权限。 这些方法只可以用于获取、创建或删除软件包外部测试版。 若要为软件包外部测试版创建提交，请参阅[管理软件包外部测试版提交](manage-flight-submissions.md)中的方法。

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
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}```</td>
<td align="left">[获取软件包外部测试版](get-a-flight.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights```</td>
<td align="left">[创建软件包外部测试版](create-a-flight.md)</td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}```</td>
<td align="left">[删除软件包外部测试版](delete-a-flight.md)</td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>先决条件

如果尚未开始操作，请先完成 Windows 应用商店提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)，然后再尝试使用其中任何方法。

## <a name="related-topics"></a>相关主题

* [使用 Windows 应用商店服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理软件包外部测试版提交](manage-flight-submissions.md)



<!--HONumber=Dec16_HO3-->


