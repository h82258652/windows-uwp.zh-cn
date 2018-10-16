---
author: Xansky
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: 在 Microsoft Store 提交 API 中使用这些方法，可管理已注册到 Windows 开发人员中心帐户的应用的软件包外部测试版。
title: 管理软件包外部测试版
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 提交 API, 外部测试版
ms.localizationpriority: medium
ms.openlocfilehash: 6a761edf50888fb7f3130886a2c7e6e65b7c0a1d
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2018
ms.locfileid: "4679533"
---
# <a name="manage-package-flights"></a>管理软件包外部测试版

使用 Microsoft Store 提交 API 中的以下方法管理应用的软件包外部测试版。 有关 Microsoft Store 提交 API 的介绍（包括使用 API 的先决条件），请参阅[使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

这些方法只可以用于获取、创建或删除软件包外部测试版。 若要为软件包外部测试版创建提交，请参阅[管理软件包外部测试版提交](manage-flight-submissions.md)中的方法。

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}</td>
<td align="left"><a href="get-a-flight.md">获取软件包外部测试版</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights</td>
<td align="left"><a href="create-a-flight.md">创建软件包外部测试版</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}</td>
<td align="left"><a href="delete-a-flight.md">删除软件包外部测试版</a></td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>先决条件

如果尚未开始操作，请先完成 Microsoft Store 提交 API 的所有[先决条件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)，然后再尝试使用其中任何方法。

## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理软件包外部测试版提交](manage-flight-submissions.md)
