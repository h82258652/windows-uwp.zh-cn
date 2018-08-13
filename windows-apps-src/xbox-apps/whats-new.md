---
author: v-angraf
title: Xbox One 上的 UWP 的新增功能
description: 突出显示 Xbox One 上的 UWP 应用的新功能。
ms.author: v-angraf
ms.date: 03/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: cbabe9d31b5b9762320df8e4a92d19ae4e33497d
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "301453"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Xbox One 上的 UWP 的最新更新中面向开发人员的新增功能

最新更新的通用 Windows 平台 (UWP) 上 Xbox 一个包含了以下新功能，对现有功能和 bug 修补程序更新。

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>x86 Xbox 上不再支持应用程序和游戏  
Xbox 不再支持 x86 应用开发或将 x86 应用商店提交到应用商店。

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>应用程序现在支持导航回以前的应用程序 
UWP Xbox 一个应用程序现在可支持导航回以前的应用程序。 若要执行此操作， [**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893595)事件订阅并**Handled**属性设置为**false**事件处理程序中。

> [!NOTE]
> 出于兼容性，此功能是仅供使用的最新版本上一个 Xbox UWP 构建的应用程序。 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>开发主页现开发控制台上的默认主页体验
开发控制台立即启动开发主页作为默认主页体验。 这样可以获取右以无需通过从零售主屏幕中单击。 开发主页现在包含一个快速的操作，以便启动零售主屏幕中。 此外，新设置允许您以使零售主页的默认体验。 

## <a name="new-dev-home-user-interface"></a>新的开发主页用户界面
开发主页用户界面现在包括以下提高生产效率：
 - 重要数据如 IP 地址和恢复版本此时将显示在可见性屏幕的顶部。 
 - 开发主页现在具有到逻辑设置组工具允许快速导航选项卡式的 UI。
 - 开发主页的第一个选项卡上的快速操作按钮允许快速访问最常用的操作。 

## <a name="wdp-for-xbox-enhancements"></a>WDP Xbox 增强功能
Windows 设备门户 (WDP) 现在包括其他控制台设置支持。 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>您现在可以在切换的"应用程序"和"游戏"之间您 UWP 标题的类型
切换的"应用程序"和"游戏"之间您 UWP 标题类型允许您测试不发布到商店游戏方案。 在开发家庭、**游戏和应用程序**窗格中选择的应用程序、 按控制器上的视图按钮、 选择**应用程序详细信息**，然后类型更改为"应用程序"或"游戏"。

## <a name="see-also"></a>另请参阅
- [已知问题](known-issues.md)
- [Xbox One 上的 UWP](index.md)
 - 现在，你可以捕获主机的屏幕截图。 有关获取屏幕截图的详细信息，请参阅 [/ext/screenshot](wdp-media-capture-api.md) 参考主题。
 - 该工具可以部署 Loose 文件版本的应用。 有关 Loose 文件版本的详细信息，请参阅 [/api/app/packagemanager/register](wdp-loose-folder-register-api.md) 参考主题。
 - 可以从开发电脑上的“文件资源管理器”访问主机上的开发人员文件。 有关通过“文件资源管理器”访问文件的详细信息，请参阅 [/ext/smb/developerfolder](wdp-smb-api.md) 参考主题。

## <a name="see-also"></a>另请参阅
- [已知问题](known-issues.md)
- [Xbox One 上的 UWP](index.md)
