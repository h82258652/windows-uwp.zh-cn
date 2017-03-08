---
author: jnHs
Description: "可以通过将你的应用链接到应用的应用商店一览来帮助客户发现它。"
title: "链接到你的应用"
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 59f19dbf0cd66679805a9fcf3054427a22fb0e8f
ms.lasthandoff: 02/07/2017

---

# <a name="link-to-your-app"></a>链接到你的应用


可以通过将你的应用链接到应用的应用商店一览来帮助客户发现它。

## <a name="getting-the-link-to-your-apps-store-listing"></a>获取指向你的应用的应用商店一览的链接


在仪表板中的每个应用的**应用管理**部分中，你可以在[应用标识](view-app-identity-details.md)页面上找到应用的应用商店一览的链接。

此链接采用格式 **`https://www.microsoft.com/store/apps/<your app's Store ID>`**。

当客户单击此链接时，它将打开基于 Web 的应用一览页。 如果应用适用于客户的设备，则应用商店应用还将启动并显示你的应用的一览。

> **注意** 你可能会在此处看到多个链接，具体取决于你面向的操作系统版本。 所有应用都将显示 Windows 10 的 URL，这适用于任何操作系统。 你可能会看到适用于 Windows 8.1 及更早版本和/或 Windows Phone 8.1 及更早版本的其他链接，这些链接将仅适用于指定的操作系统版本。

 

## <a name="linking-to-your-apps-store-listing-with-the-windows-store-badge"></a>链接到具有 Windows 应用商店锁屏提醒的应用的应用商店一览


你可以直接链接到具有自定义锁屏提醒的应用一览，以让客户知道你的应用在 Windows 应用商店中。

若要创建锁屏提醒，请访问 [Windows 应用商店锁屏提醒](http://go.microsoft.com/fwlink/p/?LinkID=534236)页。 你将需要具有应用的应用商店 ID 才能使用此形式生成锁屏提醒和链接。 此 ID 为 **Windows 10 的 URL** 的最后 12 个字符，该 URL 显示在**应用管理**部分中的[应用标识](view-app-identity-details.md)页上。

> **注意** 有关使用 Windows 应用商店锁屏提醒的详细信息，请参阅[应用营销指南](app-marketing-guidelines.md)。

 

## <a name="linking-directly-to-your-app-in-the-windows-store"></a>直接链接到 Windows 应用商店中的应用


你可以创建一个链接，该链接启动 Windows 应用商店并直接转到应用的一览页，而无需通过使用 **ms-windows-store:** URI 方案来打开浏览器。

如果你知道你的用户使用的是 Windows 设备，并且你希望他们直接到达应用商店中的一览页，这些链接将非常有用。例如，在检查浏览器中的用户代理字符串以确认用户的操作系统后，或在已通过 UWP 应用进行通信时，你可能需要应用此协议。

若要使用 Windows 应用商店协议直接链接到应用的应用商店一览，请将应用的应用商店 ID 附加到此链接：

`ms-windows-store://pdp/?ProductId=`

有关使用 Windows 应用商店协议的详细信息，请参阅[启动 Windows 应用商店应用](../launch-resume/launch-store-app.md)。

 

 





