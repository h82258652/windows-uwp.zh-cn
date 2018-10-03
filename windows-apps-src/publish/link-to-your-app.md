---
author: jnHs
Description: You can help customers discover your app by linking to your app's listing in the Microsoft Store.
title: 链接到你的应用
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.author: wdg-dev-content
ms.date: 10/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 链接, windows 应用商店协议, 链接到应用
ms.localizationpriority: medium
ms.openlocfilehash: 0025321aa73a66cc0a976bd347e613de3c3c4765
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/02/2018
ms.locfileid: "4262074"
---
# <a name="link-to-your-app"></a>链接到你的应用


你可以通过帮助客户发现你的应用链接到你的应用的 Microsoft 应用商店中的一览。

## <a name="getting-the-link-to-your-apps-store-listing"></a>获取指向你的应用的应用商店一览的链接

若要获取有关应用的应用商店一览的 URL，请导航至应用的**应用管理**部分的[应用标识](view-app-identity-details.md)页。 URL 的格式为 **`https://www.microsoft.com/store/apps/<your app's Store ID>`**。

当客户单击此链接时，它将打开基于 Web 的应用一览页。 在 Windows 设备上，应用商店应用还将启动并显示应用一览。


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>链接到你的应用具有的 Microsoft 应用商店锁屏提醒的应用商店一览

你可以直接链接到具有自定义的锁屏提醒，以让客户知道你的应用在 Microsoft Store 中的应用的一览。

若要创建锁屏提醒，请访问[的 Microsoft 应用商店锁屏提醒](http://go.microsoft.com/fwlink/p/?LinkID=534236)页面。 需要具有 12 个字符的应用的**应用商店 ID**才能生成锁屏提醒和链接。 可以在**应用管理**部分的[应用标识](view-app-identity-details.md)页查找应用的**应用商店 ID**。

> [!NOTE]
> 有关信息和要求相关的 Microsoft 应用商店锁屏提醒使用，请参阅[应用营销指南](app-marketing-guidelines.md)。


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>直接链接到你的 Microsoft 应用商店中的应用

你可以创建启动 Microsoft 应用商店并直接转到你的应用一览页而无需打开浏览器使用的链接**ms windows 应用商店：** URI 方案。

如果知道用户使用的是 Windows 设备，并且希望他们直接到达应用商店中的一览页，这些链接将非常有用。 例如，在检查浏览器中的用户代理字符串以确认用户的操作系统支持应用商店后，或在已通过 UWP 应用进行通信时，可能需要使用此链接。

若要使用此 URI 方案直接链接到你的应用的应用商店一览，将你的应用的应用商店 ID 附加到此链接：

`ms-windows-store://pdp/?ProductId=`

有关使用 Microsoft 应用商店协议的详细信息，请参阅[启动 Microsoft 应用](../launch-resume/launch-store-app.md)。

 

 




