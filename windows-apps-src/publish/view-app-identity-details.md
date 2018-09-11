---
author: jnHs
Description: View details related to the unique identity assigned to your app by the Microsoft Store, and get a link to your app's Store listing.
title: 查看应用标识的详细信息
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
ms.author: wdg-dev-content
ms.date: 12/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cf2c10fd8fa581e29fe20c5bdcb2683c5246af1d
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/11/2018
ms.locfileid: "3846621"
---
# <a name="view-app-identity-details"></a>查看应用标识的详细信息


在 Windows 开发人员中心仪表板中的应用时，你可以查看由 Microsoft 应用商店分配给它的唯一标识符相关的详细信息。 此外，你还可以获取指向应用的应用商店一览的链接。

若要找到此信息，请导航到其中一个应用，然后展开左侧导航菜单中的**应用管理**。 选中**应用标识**查看这些详细信息。


## <a name="values-to-include-in-your-app-package-manifest"></a>要包含在应用程序包清单中的值

以下值必须包含在 .appx 程序包清单中。 如果[使用 Microsoft Visual Studio 生成程序包](../packaging/packaging-uwp-apps.md)，并使用与你的开发者帐户关联的相同 Microsoft 帐户登录，则会自动包含这些详细信息。 如果手动生成程序包，则需要将以下各项添加到程序包中：

-   **程序包/标识/名称**
-   **程序包/标识/发布者**
-   **程序包/属性/发行商显示名称**

有关详细信息，请参阅[程序包清单架构参考](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)中的[**标识**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)。

同时，这些元素声明应用的标识、建立了所有程序包所属于的“程序包系列”。 单个程序包将具有其他详细信息，如体系结构和版本。


## <a name="additional-values-for-package-family"></a>程序包系列的其他值

以下值是指应用的程序包系列的其他值，但不包含在清单内。

-   **程序包系列名称 (PFN)**：此值与某些 Windows API 结合使用。
-   **程序包 SID**：需要该值才能向应用发送 WNS 通知。 有关详细信息，请参阅 [Windows 推送通知服务 (WNS) 概述](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)。


## <a name="link-to-your-apps-listing"></a>链接到应用一览

可以共享应用的页面直接链接来帮助客户在应用商店中查找该应用。 此链接采用格式 **`https://www.microsoft.com/store/apps/<your app's Store ID>`**。 当客户单击此链接时，它将打开基于 Web 的应用一览页。 在 Windows 设备上，应用商店应用还将启动并显示应用一览。

你的应用的**应用商店 ID** 也会在本部分中显示。 此应用商店 ID 可以用来[生成应用商店锁屏提醒](http://go.microsoft.com/fwlink/p/?LinkId=534236)或标识你的应用。

**应用商店协议链接**可直接链接到应用商店中你的应用，无需打开浏览器，例如可从应用内部链接至你的应用。 有关详细信息，请参阅[链接到你的应用](link-to-your-app.md)。



 

 




