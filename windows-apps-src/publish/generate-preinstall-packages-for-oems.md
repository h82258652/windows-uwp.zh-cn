---
author: jnHs
Description: If your developer account has been granted the appropriate permissions, you can generate and download preinstall packages so that an OEM can include your app in their OS image.
title: 生成适用于 OEM 的预安装程序包
ms.assetid: AC3A45E8-7BBD-44E9-B2D3-B74B7C9B2BC9
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 598a73b291d5f8b3c004f1e9adeddf0b92b841ab
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2018
ms.locfileid: "2892545"
---
# <a name="generate-preinstall-packages-for-oems"></a>生成适用于 OEM 的预安装程序包

如果开发者帐户已获取相应权限，则可以生成并下载预安装程序包，这样 OEM 可将应用包含在其操作系统映像中。 预安装权限只在由 OEM 赞助的开发者帐户上启用。


## <a name="important-preinstall-policy--limitations"></a>预安装策略及限制重要提示

预安装应用必须通过 Windows 开发人员中心认证才能获取最新的应用商店许可，否则它们无法连接到应用商店并接收应用更新。

任何预安装的应用必须在所有市场中保持免费。


## <a name="generating-preinstall-packages"></a>生成预安装程序包

使用预安装权限启用帐户后，请完成以下步骤：

1.  在仪表板中，导航至待预安装的应用。
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

 

 




