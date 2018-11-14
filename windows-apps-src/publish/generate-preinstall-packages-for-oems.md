---
author: jnHs
Description: If your developer account has been granted the appropriate permissions, you can generate and download preinstall packages so that an OEM can include your app in their OS image.
title: 生成适用于 OEM 的预安装程序包
ms.assetid: AC3A45E8-7BBD-44E9-B2D3-B74B7C9B2BC9
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 02f7c1ad1a396464532a1c63c925bf9e19600f1b
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6266222"
---
# <a name="generate-preinstall-packages-for-oems"></a>生成适用于 OEM 的预安装程序包

如果开发者帐户已获取相应权限，则可以生成并下载预安装程序包，这样 OEM 可将应用包含在其操作系统映像中。 预安装权限只在由 OEM 赞助的开发者帐户上启用。


## <a name="important-preinstall-policy--limitations"></a>预安装策略及限制重要提示

预安装的应用必须通过[合作伙伴中心](https://partner.microsoft.com/dashboard)，以便它们无法连接到应用商店并接收应用更新具有最新的应用商店许可进行认证。

任何预安装的应用必须在所有市场中保持免费。


## <a name="generating-preinstall-packages"></a>生成预安装程序包

使用预安装权限启用帐户后，请完成以下步骤：

1.  在合作伙伴中心中，导航至待预安装的应用。
2.  在左侧导航菜单中，展开**应用管理**，然后选择**当前程序包**。
3.  在**请求用于操作系统预安装的程序包**部分中，选择**启用可下载的程序包**。
4.  在确认对话框中，选择**启用**。
5.  查找希望下载的程序包，然后选择相应的**生成程序包**链接。

    > [!NOTE]
    > 预安装程序包的生成时间长短不一，具体取决于所选程序包的大小。 在生成程序包的过程中，可以离开此页面并在稍后返回，或停留在此页面。

6.  生成程序包后，将显示**下载程序包**链接。 选择此链接，下载 .zip 文件。

然后可以将此 .zip 文件提供给 OEM 以包含在他们的操作系统映像中。


## <a name="support"></a>支持

如果你有关于生成预安装程序包的进一步问题，请发送电子邮件至 <partnerops@microsoft.com>。

 

 




