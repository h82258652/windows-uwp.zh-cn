---
title: Xbox One 上的 UWP 的新增功能
description: 突出显示 Xbox One 上的 UWP 应用的新功能。
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
ms.localizationpriority: medium
ms.openlocfilehash: aff65e5f1b4771cbb33bc8b8219224042b7bf7e2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660682"
---
# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Xbox One 上的 UWP 的最新更新中面向开发人员的新增功能

Xbox One 上的通用 Windows 平台 (UWP) 的最新更新包含以下新功能、现有功能的更新和 Bug 修复。

## <a name="x86-apps-and-games-are-no-longer-supported-on-xbox"></a>Xbox 不再支持 x86 应用和游戏  
Xbox 不再支持 x86 应用开发或将 x86 应用商店提交到应用商店。

## <a name="apps-can-now-support-navigating-back-to-the-previous-app"></a>应用现在可支持导航回上一个应用 
Xbox One 上的 UWP 现在可支持导航回上一个应用。 若要实现此目的，请订阅  [**Windows.UI.Core.SystemNavigationManager.BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893595) 事件并将事件处理程序中的 **Handled** 属性设为 **false**。

> [!NOTE]
> 出于兼容性原因，此功能仅可用于 Xbox One 上具有最新 UWP 版本的应用。 

## <a name="dev-home-is-now-the-default-home-experience-on-development-consoles"></a>开发人员主页现在是开发控制台上的默认主页体验
开发控制台现在可将开发人员主页作为默认主页体验启动。 这使你可以马上开始工作，无需通过零售主页屏幕进行单击。 开发人员主页包含用于启动零售主页屏幕的快速操作。 此外，新设置还使你能够将零售主页设为默认体验。 

## <a name="new-dev-home-user-interface"></a>新的开发人员主页用户界面
开发人员主页用户界面现在包含以下生产力增强功能：
 - 重要数据（如 IP 地址和恢复版本）现在显示在屏幕顶部，以便提供可见性。 
 - 开发人员主页现在具有一个将工具分组成逻辑集的选项卡式 UI，以便实现快速导航。
 - 通过开发人员中心第一个选项卡上的快速操作按钮可以快速访问最常用的操作。 

## <a name="wdp-for-xbox-enhancements"></a>Xbox 的 WDP 增强功能
Windows Device Portal (WDP) 现在包含面向控制台设置的其他支持。 

## <a name="you-can-now-switch-the-type-of-your-uwp-title-between-app-and-game"></a>现在，你可以将 UWP 标题类型在“应用”和“游戏”之间切换。
通过在“应用”和“游戏”之间切换 UWP 标题类型，你可以测试游戏方案而无需将其发布到应用商店。 在开发人员主页的**游戏和应用**窗格中选择此应用，按下控制器上的“查看”按钮，选择**应用详细信息**，然后将类型更改为“应用”或“游戏”。

## <a name="see-also"></a>另请参阅
- [已知问题](known-issues.md)
- [在 Xbox One 上 UWP](index.md)
 - 现在，你可以捕获主机的屏幕截图。 有关获取屏幕截图的详细信息，请参阅 [/ext/screenshot](wdp-media-capture-api.md) 参考主题。
 - 该工具可以部署 Loose 文件版本的应用。 有关 Loose 文件版本的详细信息，请参阅 [/api/app/packagemanager/register](wdp-loose-folder-register-api.md) 参考主题。
 - 可以从开发电脑上的“文件资源管理器”访问主机上的开发人员文件。 有关通过“文件资源管理器”访问文件的详细信息，请参阅 [/ext/smb/developerfolder](wdp-smb-api.md) 参考主题。

## <a name="see-also"></a>另请参阅
- [已知问题](known-issues.md)
- [在 Xbox One 上 UWP](index.md)
