---
Description: 你可以通过链接到 Microsoft Store 中的应用列表来帮助客户发现你的应用。
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


你可以通过链接到 Microsoft Store 中的应用列表来帮助客户发现你的应用。

## <a name="getting-the-link-to-your-apps-store-listing"></a>获取指向你的应用的应用商店一览的链接

若要获取有关应用的应用商店一览的 URL，请导航至应用的[应用管理](view-app-identity-details.md)部分的**应用标识**页。 URL 的格式为 **`https://www.microsoft.com/store/apps/<your app's Store ID>`** 。

当客户单击此链接时，它将打开基于 Web 的应用一览页。 在 Windows 设备上，应用商店应用还将启动并显示应用一览。


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>链接到应用商店列表和 Microsoft Store 徽章

你可以使用自定义徽章直接链接到应用的列表，让客户知道你的应用处于 Microsoft Store。

若要创建徽章，请访问[Microsoft Store 徽章](https://developer.microsoft.com/store/badges)页面。 需要具有 12 个字符的应用的**应用商店 ID**才能生成锁屏提醒和链接。 可以在**应用管理**部分的[应用标识](view-app-identity-details.md)页查找应用的**应用商店 ID**。

> [!NOTE]
> 有关使用 Microsoft Store 徽章的信息和要求，请参阅[应用市场营销指导原则](app-marketing-guidelines.md)。


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>直接链接到 Microsoft Store 中的应用

你可以创建一个启动 Microsoft Store 的链接，并直接转到应用的列表页面，而无需使用 " **ms-windows 应用商店：** URI" 方案打开浏览器。

如果知道用户使用的是 Windows 设备，并且希望他们直接到达应用商店中的一览页，这些链接将非常有用。 例如，在检查浏览器中的用户代理字符串以确认用户的操作系统支持应用商店后，或在已通过 UWP 应用进行通信时，可能需要使用此链接。

若要使用此 URI 方案直接链接到应用的存储列表，请将应用的存储 ID 附加到此链接：

`ms-windows-store://pdp/?ProductId=`

有关使用 Microsoft Store 协议的详细信息，请参阅[启动 Microsoft 应用](../launch-resume/launch-store-app.md)。

 

 




