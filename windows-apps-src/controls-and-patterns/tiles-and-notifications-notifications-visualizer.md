---
author: mijacobs
Description: "通知可视化工具是应用商店中一款新的通用 Windows 平台 (UWP) 应用，可帮助开发人员设计适用于 Windows 10 的自适应动态磁贴。"
title: "通知可视化工具"
ms.assetid: FCBB7BB1-2C79-484B-8FFC-26FE1934EC1C
label: TBD
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 04e1a2e4c2bff8c7d7f75604e5665c3525f72174
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2017
---
# <a name="notifications-visualizer"></a>通知可视化工具

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 


通知可视化工具是[应用商店](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)中一款新的通用 Windows 平台 (UWP) 应用，可帮助开发人员设计适用于 Windows 10 的自适应动态磁贴。

## <a name="overview"></a>概述


通知可视化工具应用会在你进行编辑时提供磁贴的即时可视预览，类似于 Visual Studio 的 XAML 编辑器/设计视图。 该应用还会检查错误，从而确保你可以创建有效的磁贴负载。

来自该应用的此屏幕截图显示了 XML 负载以及磁贴大小在所选设备上的显示方式：

![通知可视化工具应用编辑器（带有代码和磁贴）的屏幕截图](images/notif-visualizer-001.png)

 

借助通知可视化工具，你可以创建和测试自适应磁贴负载，而无需编辑和部署应用本身。 创建了视觉效果很理想的负载后，你可以将其集成到你的应用中。 若要了解详细信息，请参阅[发送本地磁贴通知](tiles-and-notifications-sending-a-local-tile-notification.md)。

**注意** 通知可视化工具对 Windows“开始”菜单的模拟并不总是完全准确，并且它不支持某些负载属性，例如 [baseUri](https://msdn.microsoft.com/library/windows/apps/br208712)。 当你拥有所需的磁贴设计时，可通过将磁贴固定到实际的“开始”菜单对其进行测试来验证它是否会按预期显示。

 

## <a name="features"></a>功能


通知可视化工具附带许多示例负载，可展示自适应动态磁贴的功能，并且帮助你开始操作。 你可以对所有不同的文本选项、组/子组、背景图像进行试验，而且可以看到磁贴如何适应不同的设备和屏幕。 在进行更改之后，你可以将已更新的负载保存到文件以供将来使用。

编辑器可提供实时错误和警告。 例如，如果你的应用负载限制为小于 5 KB（平台限制），通知可视化工具会在负载超出该限制时向你发出警告。 如果存在不正确的属性名或值，它将向你发出警告，这有助于你调试视觉问题。

你可以控制磁贴属性，如显示名称、颜色、徽标、ShowName、锁屏提醒值。 这些选项可帮助你立即了解磁贴属性和磁贴通知负载的交互方式以及它们产生的结果。

来自该应用的此屏幕截图显示了磁贴编辑器：

![通知可视化工具编辑器（带有磁贴）的屏幕截图](images/notif-visualizer-004.png)

 

## <a name="related-topics"></a>相关主题


* [在应用商店中获取通知可视化工具](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)
* [创建自适应磁贴](tiles-and-notifications-create-adaptive-tiles.md)
* [自适应磁贴模板：架构和文档](tiles-and-notifications-adaptive-tiles-schema.md)
* [磁贴和 Toast（MSDN 博客）](http://blogs.msdn.com/b/tiles_and_toasts/)