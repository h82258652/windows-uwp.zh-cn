---
author: mijacobs
Description: Learn how to use tiles, badges, toasts, and notifications to provide entry points into your app and keep users up-to-date.
title: 磁贴、锁屏提醒和通知
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: eaca23a72ff794a85ffd8ac13c3f522cabf32aa7
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6190283"
---
# <a name="tiles-badges-and-notifications-for-uwp-apps"></a>适用于 UWP 应用的磁贴、锁屏提醒和通知
 

了解如何使用磁贴、锁屏提醒、Toast 和通知提供应用入口点并使用户了解最新信息。

> **重要 API**：[UWP 社区工具包通知 NuGet 程序包](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="images/tile-and-live-tile.png" />
磁贴是应用在“开始”菜单上的表示形式。 每个 UWP 应用都有一个磁贴。 你可以启用不同的磁贴大小（小、中等、宽形和大）。</p>

<p>你可以使用<em>磁贴通知</em>更新磁贴，以向用户传达新信息，如头条新闻或最近未读邮件的主题。</p>

<p>你可以使用<em>锁屏提醒</em>提供采用系统提供的字形或 1-99 的数字的形式的状态或摘要信息。 锁屏提醒还显示在应用的任务栏上。 </p>

<p><em>Toast 通知</em>是应用通过称为 <em>Toast</em>（或<em>横幅</em>）的弹出 UI 元素发送给用户的通知。 无论用户是否在使用应用，均可看到此通知。</p>
<p><em>推送通知</em>或<em>原始通知</em>是从 Windows 推送通知服务 (WNS) 或后台任务发送到应用的通知。 应用可以通过通知用户发生了某些趣事（借助锁屏提醒更新、磁贴更新或 Toast）来响应这些通知，也可以以你选择的任意方式响应。</p>

 
## <a name="tiles"></a>磁贴
| 文章 | 描述 |
| --- | --- |
| [创建磁贴](creating-tiles.md) | 为你的应用自定义默认磁贴，并提供适用于不同屏幕大小的资源。 |
| [应用图标资源](app-assets.md) | 应用图标资源（它以各种形式出现在整个 Windows 10 操作系统中）是通用 Windows 平台 (UWP) 应用的调用卡。 这些指南详细介绍应用图标资源在系统中的显示位置，并提供有关如何创建最完美图标的深入设计提示。 |
| [主要磁贴 API 的](primary-tile-apis.md) | 请求固定你的应用的主磁贴，并检查主磁贴当前是否已固定。 |
| [磁贴内容](create-adaptive-tiles.md) | 使用自适应指定磁贴通知内容，一项新功能在 windows 10，从而允许你设计你自己的磁贴通知内容使用可适应不同屏幕密度的简单而灵活的标记语言。 本文介绍了如何为通用 Windows 平台 (UWP) 应用创建自适应动态磁贴。 |
| [磁贴内容架构](../tiles-and-notifications/tile-schema.md) | 以下是用于创建自适应磁贴的元素和属性。 |
| [特殊磁贴模板](special-tile-templates-catalog.md) | 特殊磁贴模板是独特的模板，可以具有动画效果或只允许你执行自适应磁贴不支持的操作。 |
| [发送本地磁贴通知](sending-a-local-tile-notification.md) | 了解如何发送本地磁贴通知，同时将丰富的动态内容添加到你的动态磁贴。 |


## <a name="notifications"></a>通知

| 文章 | 描述 |
| --- | --- |
| [Toast 通知](adaptive-interactive-toasts.md) | 自适应和交互式 Toast 通知可使你创建带有更多内容的灵活弹出通知、可选的嵌入式图像和可选的用户交互。 |
| [发送本地 toast 通知](send-local-toast.md) | 了解如何发送交互式的 toast 通知。 |
| [通知可视化工具](notifications-visualizer.md) | 通知可视化工具是新的通用 Windows 平台 (UWP) 应用在[应用商店](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)，可帮助开发人员设计自适应动态磁贴的 windows 10。 |
| [选择通知传递方法](choosing-a-notification-delivery-method.md) | 本文介绍了用于传递磁贴和锁屏提醒更新以及 Toast 通知内容的四个通知选项：本地、计划、定期和推送。 |
| [定期通知概述](periodic-notification-overview.md) | 定期通知（也称为轮询通知）通过从云服务下载内容，以固定间隔更新磁贴和锁屏提醒。 |
| [Windows 推送通知服务 (WNS) 概述](windows-push-notification-services--wns--overview.md) | Windows 推送通知服务 (WNS) 使第三方开发人员可从自己的云服务发送 Toast、磁贴、锁屏提醒和原始更新。 这提供了一种高效而可靠地向用户提供新更新的机制。 |
| [由推送通知向导生成的代码](the-code-generated-by-the-push-notification-wizard.md) | 通过在 Visual Studio 中使用向导，你可以从使用 Azure 移动服务创建的某种移动服务生成推送通知。 Visual Studio 向导生成代码，以帮助你开始操作。 本主题说明向导如何修改你的项目、生成的代码有何作用、如何使用此代码，以及为了发挥推送通知的最大作用，你接下来可以如何操作。 请参阅 [Windows 推送通知服务 (WNS) 概述](windows-push-notification-services--wns--overview.md)。 |
| [原始通知概述](raw-notification-overview.md) | 原始通知是简短的一般用途的推送通知。 它们完全是说明性的，并且不包含 UI 组件。 与其他推送通知一样，WNS 功能将原始通知从云服务传递到应用。 |
