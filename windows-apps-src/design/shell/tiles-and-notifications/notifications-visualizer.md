---
author: andrewleader
Description: Notifications Visualizer is a new Universal Windows Platform (UWP) app in the Store that helps developers design adaptive live tiles for Windows 10.
title: 通知可视化工具
ms.assetid: FCBB7BB1-2C79-484B-8FFC-26FE1934EC1C
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: af8b2489346e1ef81c5cae304802814b79b8b950
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2018
ms.locfileid: "4206445"
---
# <a name="notifications-visualizer"></a>通知可视化工具

 


通知可视化工具是 [Microsoft Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) 中一款新的通用 Windows 平台 (UWP) 应用，可帮助开发人员设计适用于 Windows 10 的自适应动态磁贴和交互式 toast 通知。


## <a name="overview"></a>概述

通知可视化工具会在编辑 XML 有效负载时提供磁贴的即时可视预览和 toast 通知，类似于 Visual Studio 的 XAML 编辑器/设计视图。 该应用还会检查错误，从而确保可以创建有效的磁贴或 Toast 通知有效负载。

来自该应用的此屏幕截图显示了 XML 有效负载以及磁贴大小在所选设备上的显示方式:

![通知可视化工具应用编辑器（带有代码和磁贴）的屏幕截图](images/notif-visualizer-001.png)

 

借助通知可视化工具，可以创建和测试自适应磁贴和 Toast 有效负载，而无需编辑和部署自己的应用。 创建了视觉效果很理想的负载后，可以将其集成到应用中。 若要了解更多详细信息，请参阅[发送本地磁贴通知](sending-a-local-tile-notification.md)和[发送本地 Toast](send-local-toast.md)。

**注意**   通知可视化工具对 Windows“开始”菜单和 Toast 通知的模拟并不总是完全准确，并且它不支持某些高级有效负载属性。 当拥有所需的磁贴和 Toast 时，可通过固定磁贴或弹出 Toast 对其进行测试来验证它是否会按预期显示。

 

## <a name="features"></a>功能

通知可视化工具附带许多示例负载，可展示自适应动态磁贴和交互式 Toast 的功能，并且有助于开始操作。 你可以对所有不同的文本选项、组/子组、背景图像进行试验，而且可以看到磁贴如何适应不同的设备和屏幕。 在进行更改之后，你可以将已更新的负载保存到文件以供将来使用。

编辑器可提供实时错误和警告。 例如，如果有效负载大于 5 KB（平台限制），通知可视化工具会在负载超出该限制时发出警告。 如果存在不正确的属性名或值，它将发出警告，这有助于调试视觉问题。

可以控制磁贴属性，如显示名称、颜色、徽标、ShowName 和锁屏提醒值。 这些选项有助于立即了解磁贴属性和磁贴通知负载的交互方式以及它们产生的结果。

来自该应用的此屏幕截图显示了磁贴编辑器：

![通知可视化工具编辑器（带有磁贴）的屏幕截图](images/notif-visualizer-004.png)

 

## <a name="related-topics"></a>相关主题

* [在应用商店中获取通知可视化工具](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)
* [创建自适应磁贴](create-adaptive-tiles.md)
* [交互式 Toast](adaptive-interactive-toasts.md)