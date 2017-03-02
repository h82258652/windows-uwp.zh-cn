---
author: jnHs
Description: "如果你的开发者帐户已获取相应权限，你可以生成并下载预安装程序包，OEM 可使用它们将你的应用包含在其映像中。"
title: "生成适用于 OEM 的预安装程序包"
ms.assetid: AC3A45E8-7BBD-44E9-B2D3-B74B7C9B2BC9
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 962b6b7361da8b88d13b37219fab538e1510c961
ms.lasthandoff: 02/07/2017

---

# <a name="generate-preinstall-packages-for-oems"></a>生成适用于 OEM 的预安装程序包


如果你的开发者帐户已获取相应权限，你可以生成并下载预安装程序包，OEM 可使用它们将你的应用包含在其映像中。 预安装权限只在由 OEM 赞助的开发者帐户上启用。

## <a name="important-preinstall-policy--limitations"></a>预安装策略及限制重要提示


预安装应用必须通过 Windows 开发人员中心认证才能获取最新的应用商店许可，否则它们无法连接到应用商店并接收应用更新。

任何已预安装的应用必须在所有市场中保持免费。

## <a name="generating-preinstall-packages"></a>生成预安装程序包


使用预安装权限启用帐户后，请完成以下步骤：

1.  在仪表板中，导航至待预安装的应用。
2.  在左侧导航菜单中，展开“应用管理”****，然后单击“当前程序包”****。
3.  在“请求进行操作系统预安装的程序包”****部分中，单击“启用可下载的程序包”****。
4.  此时将显示确认对话框，提示在 Windows 10 之前的操作系统上预安装的应用必须免费。 选择“启用”****。
5.  查找希望下载的程序包，然后单击相应的**“生成程序包”**链接。
    > **注意**  预安装程序包的生成时间长短不一，具体取决于所选程序包的大小。 你可以离开此页面并在稍后返回，或停留在此页面。
6.  在生成程序包后，将显示“下载程序包”****链接。 单击此链接，下载 .zip 文件。

然后，你可以将此 .zip 文件提供给 OEM 以包含在他们的操作系统映像中。

## <a name="support"></a>支持


如果你有关于生成预安装程序包的进一步问题，请发送电子邮件至 <partnerops@microsoft.com>。

 

 





