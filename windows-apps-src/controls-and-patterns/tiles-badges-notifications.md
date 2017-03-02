---
author: mijacobs
Description: "了解如何使用磁贴、锁屏提醒、Toast 以及通知提供应用入口点并使用户了解最新信息。"
title: "磁贴、锁屏提醒和通知"
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: f8b063f45afadda50fa9ea091bf6cba71a25e8c1
ms.lasthandoff: 02/07/2017

---
# <a name="tiles-badges-and-notifications-for-uwp-apps"></a>适用于 UWP 应用的磁贴、锁屏提醒和通知
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 


了解如何使用磁贴、锁屏提醒、Toast 以及通知提供应用入口点并使用户了解最新信息。

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="images/tile-and-live-tile.png" />
磁贴是应用在“开始”菜单上的表示形式。 每个 UWP 应用都有一个磁贴。 你可以启用不同的磁贴大小（小、中等、宽形和大）。</p>

<p>你可以使用<em>磁贴通知</em>更新磁贴，以向用户传达新信息，如头条新闻或最近未读邮件的主题。</p>

<p>你可以使用<em>锁屏提醒</em>提供采用系统提供的字形或 1-99 的数字的形式的状态或摘要信息。 锁屏提醒还显示在应用的任务栏上。 </p>

<p><em>Toast 通知</em>是应用通过称为 <em>Toast</em>（或<em>横幅</em>）的弹出 UI 元素发送给用户的通知。 无论用户是否在使用应用，均可看到此通知。</p>
<p><em>推送通知</em>或<em>原始通知</em>是从 Windows 推送通知服务 (WNS) 或后台任务发送到应用的通知。 应用可以通过通知用户发生了某些趣事（借助锁屏提醒更新、磁贴更新或 Toast）来响应这些通知，也可以以你选择的任意方式响应。</p>

 
## <a name="tiles"></a>磁贴 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[创建磁贴](tiles-and-notifications-creating-tiles.md)</p></td>
<td align="left"><p>为你的应用自定义默认磁贴，并提供适用于不同屏幕大小的资源。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[创建自适应磁贴](tiles-and-notifications-create-adaptive-tiles.md)</p></td>
<td align="left"><p>自适应磁贴模板是 Windows 10 中的一项新功能，允许你使用可适应不同屏幕密度的简单而灵活的标记语言来设计你自己的磁贴通知内容。 本文介绍了如何为通用 Windows 平台 (UWP) 应用创建自适应动态磁贴。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[自适应磁贴架构](tiles-and-notifications-adaptive-tiles-schema.md)</p></td>
<td align="left"><p>以下是用于创建自适应磁贴的元素和属性。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[特殊磁贴模板](tiles-and-notifications-special-tile-templates-catalog.md)</p></td>
<td align="left"><p>特殊磁贴模板是独特的模板，可以具有动画效果或只允许你执行自适应磁贴不支持的操作。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[应用图标资源](tiles-and-notifications-app-assets.md)</p></td>
<td align="left"><p>应用图标资源（它以各种形式出现在整个 Windows 10 操作系统中）是通用 Windows 平台 (UWP) 应用的调用卡。 这些指南详细介绍应用图标资源在系统中的显示位置，并提供有关如何创建最完美图标的深入设计提示。</p></td>
</tr>
</tbody>
</table>

## <a name="notifications"></a>通知


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[自适应和交互式 Toast 通知](tiles-and-notifications-adaptive-interactive-toasts.md)</p></td>
<td align="left"><p>借助自适应和交互式 Toast 通知，你可以创建带有更多内容的灵活弹出通知、可选的嵌入式图像和可选的用户交互。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[通知可视化工具](tiles-and-notifications-notifications-visualizer.md)</p></td>
<td align="left"><p>通知可视化工具是[应用商店](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)中一款新的通用 Windows 平台 (UWP) 应用，可帮助开发人员设计适用于 Windows 10 的自适应动态磁贴。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[选择通知传递方法](tiles-and-notifications-choosing-a-notification-delivery-method.md)</p></td>
<td align="left"><p>本文介绍了用于传递磁贴和锁屏提醒更新以及 Toast 通知内容的四个通知选项：本地、计划、定期和推送。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[发送本地磁贴通知](tiles-and-notifications-sending-a-local-tile-notification.md)</p></td>
<td align="left"><p>本文介绍了如何使用自适应磁贴模板将本地磁贴通知发送到主要磁贴和辅助磁贴。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[定期通知概述](tiles-and-notifications-periodic-notification-overview.md)</p></td>
<td align="left"><p>定期通知（也称为轮询通知）通过从云服务下载内容，以固定间隔更新磁贴和锁屏提醒。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Windows 推送通知服务 (WNS) 概述](tiles-and-notifications-windows-push-notification-services--wns--overview.md)</p></td>
<td align="left"><p>Windows 推送通知服务 (WNS) 使第三方开发人员可从自己的云服务发送 Toast、磁贴、锁屏提醒和原始更新。 这提供了一种高效而可靠地向用户提供新更新的机制。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[由推送通知向导生成的代码](tiles-and-notifications-the-code-generated-by-the-push-notification-wizard.md)</p></td>
<td align="left"><p>通过在 Visual Studio 中使用向导，你可以从使用 Azure 移动服务创建的某种移动服务生成推送通知。 Visual Studio 向导生成代码，以帮助你开始操作。 本主题说明向导如何修改你的项目、生成的代码有何作用、如何使用此代码，以及为了发挥推送通知的最大作用，你接下来可以如何操作。 请参阅 [Windows 推送通知服务 (WNS) 概述](tiles-and-notifications-windows-push-notification-services--wns--overview.md)。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[原始通知概述](tiles-and-notifications-raw-notification-overview.md)</p></td>
<td align="left"><p>原始通知是简短的一般用途的推送通知。 它们完全是说明性的，并且不包含 UI 组件。 与其他推送通知一样，WNS 功能将原始通知从云服务传递到应用。</p></td>
</tr>
</tbody>
</table>

 

 

 





