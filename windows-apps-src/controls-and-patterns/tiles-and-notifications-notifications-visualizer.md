---
通知可视化工具是应用商店中一款新的通用 Windows 平台 (UWP) 应用，可帮助开发人员设计适用于 Windows 10 的自适应动态磁贴。
通知可视化工具
ms.assetid: FCBB7BB1-2C79-484B-8FFC-26FE1934EC1C
待定
template: detail.hbs
---

# 通知可视化工具


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


通知可视化工具是[应用商店](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)中一款新的通用 Windows 平台 (UWP) 应用，可帮助开发人员设计适用于 Windows 10 的自适应动态磁贴。

## <span id="Overview"> </span> <span id="overview"> </span> <span id="OVERVIEW"> </span>概述


通知可视化工具应用会在你进行编辑时提供磁贴的即时可视预览，类似于 Visual Studio 的 XAML 编辑器/设计视图。 该应用还会检查错误，从而确保你可以创建有效的磁贴负载。

来自该应用的此屏幕截图显示了 XML 负载以及磁贴大小在所选设备上的显示方式：

![通知可视化工具应用编辑器（带有代码和磁贴）的屏幕截图](images/notif-visualizer-001.png)

 

借助通知可视化工具，你可以创建和测试自适应磁贴负载，而无需编辑和部署应用本身。 创建了视觉效果很理想的负载后，你可以将其集成到你的应用中。 若要了解详细信息，请参阅[发送本地磁贴通知](tiles-and-notifications-sending-a-local-tile-notification.md)。

**注意** 通知可视化工具对 Windows“开始”菜单的模拟并不总是完全准确，并且它不支持某些负载属性，例如 [baseUri](https://msdn.microsoft.com/library/windows/apps/br208712)。 当你拥有所需的磁贴设计时，可通过将磁贴固定到实际的“开始”菜单对其进行测试来验证它是否会按预期显示。

 

## <span id="Features"> </span> <span id="features"> </span> <span id="FEATURES"> </span>功能


通知可视化工具附带许多示例负载，可展示自适应动态磁贴的功能，并帮助你入门。 你可以对所有不同的文本选项、组/子组、背景图像进行试验，而且可以看到磁贴如何适应不同的设备和屏幕。 在进行更改之后，你可以将已更新的负载保存到文件以供将来使用。

编辑器可提供实时错误和警告。 例如，如果你的应用负载限制为小于 5 KB（平台限制），通知可视化工具会在负载超出该限制时向你发出警告。 如果存在不正确的属性名或值，它将向你发出警告，这有助于你调试视觉问题。

你可以控制磁贴属性，如显示名称、颜色、徽标、ShowName、锁屏提醒值。 这些选项可帮助你立即了解磁贴属性和磁贴通知负载的交互方式以及它们产生的结果。

来自该应用的此屏幕截图显示了磁贴编辑器：

![通知可视化工具编辑器（带有磁贴）的屏幕截图](images/notif-visualizer-004.png)

 

## <span id="related_topics"> </span>相关主题


* [在应用商店中获取通知可视化工具](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)
* [创建自适应磁贴](tiles-and-notifications-create-adaptive-tiles.md)
* [自适应磁贴模板：架构和文档](tiles-and-notifications-adaptive-tiles-schema.md)
* [磁贴和 Toast（MSDN 博客）](http://blogs.msdn.com/b/tiles_and_toasts/)
* [NotificationsExtensions 库（MSDN 博客）](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/08/20/introducing-notificationsextensions-for-windows-10.aspx)
 

 






<!--HONumber=Mar16_HO1-->


