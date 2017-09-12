---
author: jnHs
Description: "可以通过将你的应用链接到应用的应用商店一览来帮助客户发现它。"
title: "链接到你的应用"
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 链接, windows 应用商店协议, 链接到应用"
ms.openlocfilehash: 2d0750493926937a6326c5f72f568d4294b137c5
ms.sourcegitcommit: fadde8afee46238443ec1cb71846d36c91db9fb9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2017
---
# <a name="link-to-your-app"></a>链接到你的应用


可以通过将你的应用链接到应用的应用商店一览来帮助客户发现它。

## <a name="getting-the-link-to-your-apps-store-listing"></a>获取指向你的应用的应用商店一览的链接

若要获取有关应用的应用商店一览的 URL，请导航至应用的**应用管理**部分的[应用标识](view-app-identity-details.md)页。 URL 的格式为 **`https://www.microsoft.com/store/apps/<your app's Store ID>`**。

当客户单击此链接时，它将打开基于 Web 的应用一览页。 在 Windows 设备上，应用商店应用还将启动并显示应用一览。


## <a name="linking-to-your-apps-store-listing-with-the-windows-store-badge"></a>链接到具有 Windows 应用商店锁屏提醒的应用的应用商店一览

你可以直接链接到具有自定义锁屏提醒的应用一览，以让客户知道你的应用在 Windows 应用商店中。

若要创建锁屏提醒，请访问 [Windows 应用商店锁屏提醒](http://go.microsoft.com/fwlink/p/?LinkID=534236)页。 需要具有 12 个字符的应用的**应用商店 ID**才能生成锁屏提醒和链接。 可以在**应用管理**部分的[应用标识](view-app-identity-details.md)页查找应用的**应用商店 ID**。

> [!NOTE]
> 有关使用 Windows 应用商店锁屏提醒的信息和要求，请参阅[应用营销指南](app-marketing-guidelines.md)。


## <a name="linking-directly-to-your-app-in-the-windows-store"></a>直接链接到 Windows 应用商店中的应用

你可以创建一个链接，该链接启动 Windows 应用商店并直接转到应用的一览页，而无需通过使用 **ms-windows-store:** URI 方案来打开浏览器。

如果知道用户使用的是 Windows 设备，并且希望他们直接到达应用商店中的一览页，这些链接将非常有用。 例如，在检查浏览器中的用户代理字符串以确认用户的操作系统支持应用商店后，或在已通过 UWP 应用进行通信时，可能需要使用此链接。

若要使用 Windows 应用商店协议直接链接到应用的应用商店一览，请将应用的应用商店 ID 附加到此链接：

`ms-windows-store://pdp/?ProductId=`

有关使用 Windows 应用商店协议的详细信息，请参阅[启动 Windows 应用商店应用](../launch-resume/launch-store-app.md)。

 

 




