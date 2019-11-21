---
Description: You can help customers discover your app by linking to your app's listing in the Microsoft Store.
title: 链接到你的应用
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 链接, windows 应用商店协议, 链接到应用
ms.localizationpriority: medium
ms.openlocfilehash: de22505cf42193932a5bbd951c983e02eea37bd7
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259989"
---
# <a name="link-to-your-app"></a>链接到你的应用


You can help customers discover your app by linking to your app's listing in the Microsoft Store.

## <a name="getting-the-link-to-your-apps-store-listing"></a>获取指向你的应用的应用商店一览的链接

若要获取有关应用的应用商店一览的 URL，请导航至应用的**应用管理**部分的[应用标识](view-app-identity-details.md)页。 URL 的格式为 **`https://www.microsoft.com/store/apps/<your app's Store ID>`** 。

当客户单击此链接时，它将打开基于 Web 的应用一览页。 在 Windows 设备上，应用商店应用还将启动并显示应用一览。


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Linking to your app's Store listing with the Microsoft Store badge

You can link directly to your app's listing with a custom badge to let customers know your app is in the Microsoft Store.

To create your badge, visit the [Microsoft Store badges](https://developer.microsoft.com/store/badges) page. 需要具有 12 个字符的应用的**应用商店 ID**才能生成锁屏提醒和链接。 可以在**应用管理**部分的[应用标识](view-app-identity-details.md)页查找应用的**应用商店 ID**。

> [!NOTE]
> See [App marketing guidelines](app-marketing-guidelines.md) for info and requirements related to your use of the Microsoft Store badge.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Linking directly to your app in the Microsoft Store

You can create a link that launches the Microsoft Store and goes directly to your app's listing page without opening a browser by using the **ms-windows-store:** URI scheme.

如果知道用户使用的是 Windows 设备，并且希望他们直接到达应用商店中的一览页，这些链接将非常有用。 例如，在检查浏览器中的用户代理字符串以确认用户的操作系统支持应用商店后，或在已通过 UWP 应用进行通信时，可能需要使用此链接。

To use this URI scheme to link directly to your app's Store listing, append your app's Store ID to this link:

`ms-windows-store://pdp/?ProductId=`

For more about using the Microsoft Store protocol, see [Launch the Microsoft app](../launch-resume/launch-store-app.md).

 

 




