---
Description: 可以帮助客户发现您的应用程序通过链接到 Microsoft Store 中的应用程序的列表。
title: 链接到你的应用
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 链接, windows 应用商店协议, 链接到应用
ms.localizationpriority: medium
ms.openlocfilehash: 56bc051c3c5a935f3b6b26e478731fcde9c06902
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661532"
---
# <a name="link-to-your-app"></a>链接到你的应用


可以帮助客户发现您的应用程序通过链接到 Microsoft Store 中的应用程序的列表。

## <a name="getting-the-link-to-your-apps-store-listing"></a>获取指向你的应用的应用商店一览的链接

若要获取有关应用的应用商店一览的 URL，请导航至应用的**应用管理**部分的[应用标识](view-app-identity-details.md)页。 URL 的格式为 **`https://www.microsoft.com/store/apps/<your app's Store ID>`**。

当客户单击此链接时，它将打开基于 Web 的应用一览页。 在 Windows 设备上，应用商店应用还将启动并显示应用一览。


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>将链接到你的应用的应用商店列表带有 Microsoft Store 徽标

您可以直接链接到与自定义锁屏提醒，以让客户了解您的应用程序是在 Microsoft Store 中的应用程序的列表。

若要创建您的标记，请访问[Microsoft Store 徽章](https://go.microsoft.com/fwlink/p/?LinkID=534236)页。 需要具有 12 个字符的应用的**应用商店 ID**才能生成锁屏提醒和链接。 可以在**应用管理**部分的[应用标识](view-app-identity-details.md)页查找应用的**应用商店 ID**。

> [!NOTE]
> 请参阅[应用营销准则](app-marketing-guidelines.md)有关信息和与你的 Microsoft Store 徽章使用相关的要求。


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>直接到 Microsoft Store 中的应用链接

可以创建一个链接，启动 Microsoft Store，并将直接转到你的应用列表页而无需打开浏览器通过使用**ms windows 应用商店：** URI 方案。

如果知道用户使用的是 Windows 设备，并且希望他们直接到达应用商店中的一览页，这些链接将非常有用。 例如，在检查浏览器中的用户代理字符串以确认用户的操作系统支持应用商店后，或在已通过 UWP 应用进行通信时，可能需要使用此链接。

若要使用此 URI 方案用于链接直接到应用程序的存储列表，将追加到此链接的应用程序的 Store ID:

`ms-windows-store://pdp/?ProductId=`

有关使用 Microsoft Store 协议的详细信息，请参阅[启动 Microsoft 应用](../launch-resume/launch-store-app.md)。

 

 




