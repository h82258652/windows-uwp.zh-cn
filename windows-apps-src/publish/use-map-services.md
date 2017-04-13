---
author: jnHs
Description: "若要在适用于 Windows Phone 8.1 及更早版本的应用中使用地图服务，需要在应用代码中包含地图服务应用程序 ID 和令牌。 你可以在“服务”部分中的“地图”页面上的开发人员中心仪表板中获取此令牌。"
title: "使用地图服务"
ms.assetid: E5EE6B56-B86F-4D62-B16A-F023FE98EFAB
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 853816bcb994628a8864a066309c5abd0e31fd2b
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="use-map-services"></a>使用地图服务


若要在适用于 Windows Phone 8.1 及更早版本的应用中使用地图服务，需要在应用代码中包含地图服务应用程序 ID 和令牌。 你可以在**“服务”**部分中的**“地图”**页面上的开发人员中心仪表板中获取此令牌。

> **注意**  若要在面向其他操作系统的应用中使用地图服务，请访问[必应地图开发人员中心](http://go.microsoft.com/fwlink/p/?LinkId=614880)。 有关详细信息，请参阅[请求地图身份验证密钥](https://msdn.microsoft.com/library/windows/apps/mt219694)。

[保留应用名称](create-your-app-by-reserving-a-name.md)后，请在左侧导航菜单中查找**“服务”**部分并将其展开以显示**“地图”**页。 当你单击**“获取令牌”**时，将生成**ApplicationID**和**AuthenticationToken**并显示在此页面上。

> **注意**  你无需在此时完成应用提交。 在请求令牌和 ID 后，此信息将保存在此页面上。 你可以随时返回此页面以访问此信息。

你还需要确保先将**ApplicationID**和**AuthenticationToken**添加到代码中，然后再打包和提交你的应用。 有关详细信息，请参阅[如何将地图控件添加某一页面 (Windows Phone 8.1)](http://go.microsoft.com/fwlink/p/?LinkId=614882)。

 

 




