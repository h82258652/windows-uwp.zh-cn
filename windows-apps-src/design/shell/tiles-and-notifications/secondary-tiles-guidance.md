---
author: andrewleader
Description: Learn about when and where you should use secondary tiles in your UWP app.
title: 辅助磁贴
label: Secondary tiles
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp, 辅助磁贴, 指南, 指导, 最佳做法
ms.localizationpriority: medium
ms.openlocfilehash: 1e3d31376b9ac155dab6bffa7739cb880af1cff9
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4358747"
---
# <a name="secondary-tile-guidance"></a>辅助磁贴指南


利用辅助磁贴，用户可以一致并高效地从“开始”菜单直接访问应用中的特定区域。 虽然用户可以选择是否将辅助磁贴“固定”到“开始”菜单，但应用程序中的可固定区域由开发人员决定。 有关更详细的摘要，请参阅[辅助磁贴概述](secondary-tiles.md)。 在应用中启用辅助磁贴和设计相关联的 UI 时，请考虑以下指南。

> [!NOTE]
> 只有用户才能将辅助磁贴固定到“开始”菜单；应用无法以编程方式固定辅助磁贴。 用户还可以控制磁贴删除，并且可以从“开始”菜单或父应用中删除辅助磁贴。


## <a name="recommendations"></a>建议

在应用中启用辅助磁贴时，请考虑以下建议：

* 如果可以固定重点关注的内容，则应用栏应包含一个“固定到‘开始’”按钮，以便为用户创建辅助磁贴。
* 当用户单击“固定到‘开始’”时，你应立即从 UI 线程调用 API 以[固定辅助磁贴](secondary-tiles-pinning.md)。
* 如果已固定重点关注的内容，请将应用栏上的“固定到‘开始’”按钮替换为“从‘开始’取消固定”按钮。 “从‘开始’取消固定”按钮应该会删除现有辅助磁贴。
* 如果无法固定重点关注的内容，则不会显示“固定到‘开始’”按钮（或者会显示一个被禁用的“固定到‘开始’”按钮）。
* 请对“固定到‘开始’”按钮和“从‘开始’取消固定”按钮使用系统提供的字形（请参阅 Windows.UI.Xaml.Controls.Symbol 或 WinJS.UI.AppBarIcon 中的 Pin 和 Unpin 成员)。
* 使用标准按钮文本：“固定到‘开始’”和“从‘开始’取消固定”。 使用系统提供的固定和取消固定字形时，必须替代默认文本。
* 不要将辅助磁贴用作虚拟命令按钮来与父应用交互，如“跳至下一曲目”磁贴。


## <a name="additional-usage-guidance-for-devs"></a>适用于开发人员的其他使用指南

* 当应用启动时，它应当始终枚举它的辅助磁贴，以防它未感知所添加或删除的任何内容。 当通过“开始”屏幕应用栏删除辅助磁贴时，Windows 只是删除该磁贴。 应用本身负责释放由辅助磁贴使用的任何资源。 通过云复制辅助磁贴时，当前磁盘或辅助磁贴上的锁屏提醒通知、计划通知、推送通知通道、与定期通知一起使用的统一资源标识符 (URI) 都不会随辅助磁贴一起复制，而且必须重新设置。
* 应用应该对辅助磁贴使用有意义、可重新创建的唯一 ID。 使用对应用有意义的可预测辅助磁贴 ID 可帮助应用了解当在新计算机上的全新安装中显示这些磁贴时这些磁贴可执行的操作。
  * 在运行时，应用可以查询是否存在特定的磁贴。
  * 可以要求辅助磁贴平台返回属于某个特定应用的所有辅助磁贴的集合。 对这些磁贴使用有意义的唯一 ID 可帮助应用检查辅助磁贴集并执行相应的操作。 例如，对于社交媒体应用，ID 可标识创建磁贴的各个联系人。
* 辅助磁贴与“开始”屏幕中的所有磁贴一样，都是可以使用新内容经常更新的动态出口。 辅助磁贴可以使用任何其他磁贴所使用的相同机制来显示通知和更新。 请参阅[选择通知传递方法](choosing-a-notification-delivery-method.md)了解详细信息。


## <a name="related"></a>相关

* [辅助磁贴概述](secondary-tiles.md)
* [固定辅助磁贴](secondary-tiles-pinning.md)
* [磁贴资源](app-assets.md)
* [磁贴内容文档](create-adaptive-tiles.md)
* [发送本地磁贴通知](sending-a-local-tile-notification.md)
