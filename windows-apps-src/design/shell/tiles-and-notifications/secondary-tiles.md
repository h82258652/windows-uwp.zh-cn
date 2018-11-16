---
author: andrewleader
Description: Secondary tiles allow users to pin specific content and deep links from your app onto their Start menu, providing easy future access to the content within your app.
title: 辅助磁贴
label: Secondary tiles
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
keywords: windows 10, uwp, 辅助磁贴
ms.localizationpriority: medium
ms.openlocfilehash: e27786701fa2ae9ac00a7eab57e840ec9a0dc811
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "7100137"
---
# <a name="secondary-tiles"></a>辅助磁贴


利用辅助磁贴，用户可将应用中的特定内容和深层链接固定到“开始”菜单上，方便将来访问应用中的内容。

![辅助磁贴的屏幕截图](images/secondarytiles.png)

例如，用户可以将许多特定位置的天气固定在“开始”菜单上，这样便可以 (1) 通过动态磁贴轻松瞥见有关当前天气的实时信息，以及 (2) 提供他们所关注的特定城市天气的快速入口点。 用户也可以固定特定股票、新闻文章以及对他们很重要的其他项目。

通过将辅助磁贴添加到你的应用中，你可以帮助用户有效地快速重新使用你的应用，并鼓励他们更频繁地返回到应用中，这要归功于辅助磁贴提供的轻松访问功能。

**只有用户才能固定辅助磁贴；未经用户批准，应用无法以编程方式固定辅助磁贴**。 用户必须显式单击应用中的“固定”按钮，此时会使用 API 请求创建辅助磁贴，然后系统会显示一个对话框，请求用户确认是否要固定磁贴。

## <a name="quick-links"></a>快速链接

| 文章 | 描述 |
| --- | --- |
| [有关辅助磁贴的指南](secondary-tiles-guidance.md) | 了解应该在何时以及何处使用辅助磁贴。 |
| [固定辅助磁贴](secondary-tiles-pinning.md) | 了解固定辅助磁贴的方法。 |
| [从桌面应用程序固定](secondary-tiles-desktop-pinning.md) | 借助于桌面桥，Windows 桌面应用程序可以固定辅助磁贴！ |


## <a name="secondary-tiles-in-relation-to-primary-tiles"></a>与主要磁贴相关的辅助磁贴

辅助磁贴与单个父应用相关联。 它们已固定到“开始”菜单，以向用户提供一致且有效的方式来直接启动到父应用的常用区域中。 这可能是包含经常更新的内容的普通父应用子节，或者是到应用中的特定区域的深层链接。

辅助磁贴的方案示例包括：

* 天气应用中特定城市的天气更新
* 日历应用中有关近期活动的摘要
* 社交应用中重要联系人的状态和更新
* RSS 阅读器中的特定源
* 音乐播放列表
* 博客

用户想要监视的任何经常变化的内容都是辅助磁贴的不错候选。 固定辅助磁贴后，用户可通过该磁贴收到概览更新，并可使用它直接启动到父应用。

辅助磁贴在许多方面类似于主要磁贴：

* 它们使用磁贴通知显示丰富的内容。
* 它们必须包括适用于默认磁贴内容的 150 x 150 像素徽标。
* （可选）它们可以包括其他徽标大小以启用较大的磁贴大小。
* 它们可以显示通知和锁屏提醒。
* 它们可以在“开始”菜单上重新排列。
* 卸载应用时，系统将自动删除它们。
* 可以在锁屏界面上显示它们的锁屏提醒和锁屏详细状态文本。

但是，辅助磁贴在一些重要方面不同主要磁贴：

* 用户可以在不删除父应用的情况下随时删除他们的辅助磁贴。
* 辅助磁贴可在运行时创建。 应用磁贴只能在安装过程中创建。
* 在添加辅助磁贴之前，浮出控件会提示用户进行确认。
* 不能请求用户以编程方式为锁屏界面选择辅助磁贴。 在电脑设置，用户必须手动添加辅助磁贴通过个性化页面。

为了发送通知，为与辅助磁贴配合使用的磁贴和锁屏提醒更新程序以及推送通知通道提供了特定方法。 这些特定方法与同主要磁贴配合使用的版本相对应。 例如，CreateBadgeUpdaterForApplication 与 CreateBadgeUpdaterForSecondaryTile 相对应。


## <a name="guidance-on-secondary-tiles"></a>有关辅助磁贴的指南
若要了解有关应在何时及何处使用辅助磁贴以及有关其他用法指南的信息，请参阅[有关辅助磁贴的指南](secondary-tiles-guidance.md)


## <a name="pinning-secondary-tiles"></a>固定辅助磁贴
若要了解如何固定辅助磁贴，请参阅[固定辅助磁贴](secondary-tiles-pinning.md)。


## <a name="desktop-applications-and-secondary-tiles"></a>桌面应用程序和辅助磁贴
若要了解如何通过桌面桥使用桌面应用程序中的辅助磁贴，请参阅[从桌面应用程序固定辅助磁贴](secondary-tiles-desktop-pinning.md)。
