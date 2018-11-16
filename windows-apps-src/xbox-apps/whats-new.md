---
author: v-angraf
title: Xbox One 上的 UWP 的新增功能
description: 突出显示 Xbox One 上的 UWP 应用的新功能。
ms.author: v-angraf
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: cc2168014e714de0b43b6ffffe84126764f0a4a3
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6969525"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Xbox One 上的 UWP 的最新更新中面向开发人员的新增功能

最新更新的通用 Windows 平台 (UWP) Xbox One 上包含以下新功能，更新现有功能和 bug 修复。

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>x86 应用和游戏不再受支持在 Xbox 上  
Xbox 不再支持 x86 应用开发或将 x86 应用商店提交到应用商店。

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>应用现在支持向后导航到上一个应用 
现在，Xbox One 应用上的 UWP 可以支持向后导航到上一个应用。 若要执行此操作，订阅[**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893595)事件，并将**Handled**属性设置为**false**事件处理程序中。

> [!NOTE]
> 对于兼容性原因，此功能是仅适用于使用最新版本的 Xbox One 上的 UWP 生成的应用。 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>开发人员主页现已在开发控制台上的默认主页体验
开发主机现在作为默认主页体验启动开发人员主页。 这允许你获取正确无需通过单击零售主页屏幕中工作。 开发人员主页现在包含快速操作来启动零售主页屏幕。 此外，新的设置可以使零售主页默认体验。 

## <a name="new-dev-home-user-interface"></a>新的开发人员主页用户界面
开发人员主页用户界面现在包含以下工作效率的增强：
 - 重要的数据 （例如 IP 地址） 和恢复版本现在显示在屏幕的可见性的顶部。 
 - 开发人员主页现在已为逻辑集，组工具允许快速导航选项卡式的 UI。
 - 开发人员主页的第一个选项卡上的快速操作按钮允许与最常用的操作的快速访问。 

## <a name="wdp-for-xbox-enhancements"></a>WDP Xbox 增强功能
Windows Device Portal (WDP) 现在包含其他支持控制台设置。 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>你现在可以切换的 UWP 游戏"应用"和"游戏"之间的类型
切换你的 UWP 主题作品"应用"和"游戏"之间的类型允许你测试游戏方案，而无需发布到应用商店。 开发人员主页中**的游戏和应用**窗格中，选择应用按控制器上的视图按钮、 选择**应用详细信息**，然后类型更改为"应用"或"游戏"。

## <a name="see-also"></a>另请参阅
- [已知问题](known-issues.md)
- [Xbox One 上的 UWP](index.md)
 - 现在，你可以捕获主机的屏幕截图。 有关获取屏幕截图的详细信息，请参阅 [/ext/screenshot](wdp-media-capture-api.md) 参考主题。
 - 该工具可以部署 Loose 文件版本的应用。 有关 Loose 文件版本的详细信息，请参阅 [/api/app/packagemanager/register](wdp-loose-folder-register-api.md) 参考主题。
 - 可以从开发电脑上的“文件资源管理器”访问主机上的开发人员文件。 有关通过“文件资源管理器”访问文件的详细信息，请参阅 [/ext/smb/developerfolder](wdp-smb-api.md) 参考主题。

## <a name="see-also"></a>另请参阅
- [已知问题](known-issues.md)
- [Xbox One 上的 UWP](index.md)
