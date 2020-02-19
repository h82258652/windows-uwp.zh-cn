---
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: 查看 Microsoft Advertising 库的发行说明。
title: Advertising 库的发行说明
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, ads, 广告, 发行说明
ms.localizationpriority: medium
ms.openlocfilehash: 50eea90dcf8ed59f420953eec5da5b7a0fe6b4b2
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463909"
---
# <a name="release-notes-for-the-advertising-libraries"></a>Advertising 库的发行说明

>[!WARNING]
> 从2020年6月1日起，将关闭适用于 Windows UWP 应用的 Microsoft Ad 盈利平台。 [了解详细信息](https://aka.ms/ad-monetization-shutdown)

本部分提供当前版本的 Microsoft Advertising 库的发行说明。 这些库支持适用于 Windows 10、Windows 8.1 Windows Phone 8.1 和 Windows Phone 8 的 XAML 和 JavaScript/HTML 应用。

## <a name="installation"></a>安装


Microsoft 广告库作为 [Microsoft 广告 SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) 的一部分提供。 有关安装 SDK 的更多信息，请参阅[安装 Microsoft 广告 SDK](install-the-microsoft-advertising-libraries.md)。

## <a name="uninstall-previous-versions"></a>卸载以前版本

安装最新的 Microsoft 广告 SDK 之前，强烈建议你先卸载所有之前的 SDK 实例。 有关详细信息，请参阅[安装 Microsoft 广告 SDK](install-the-microsoft-advertising-libraries.md)。

## <a name="target-architecture-specific-build-outputs"></a>面向特定于体系结构的生成输出

在使用 Microsoft Advertising 库时，你无法在项目中面向**任何 CPU**。 如果你的项目面向**任何 CPU** 平台，在你添加对 Microsoft Advertising 库的引用后，可能会在项目中看到一条警告。 若要删除此警告，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 有关详细信息，请参阅[已知问题](known-issues-for-the-advertising-libraries.md)。

## <a name="c-support"></a>C++ 支持

Microsoft Advertising 库（其中包括 **AdControl** 和 **InterstitialAd** 类）支持在 C++ 和 DirectX 中使用 Windows 运行时互操作性（*互操作*）编写的应用。 有关使用 XAML 和 C++ 编程的详细信息和示例，请参阅[类型系统](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx)。

## <a name="no-toolbox-control"></a>没有工具箱控件

在 [Microsoft 广告 SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) 中的当前版本的 Microsoft 广告库中，没有任何工具箱控件可用于将 **AdControl** 或 **InterstitialAd** 拖动到应用中的设计图面。 有关在标记和代码中添加这些控件的说明，请参阅[开发人员演练](developer-walkthroughs.md)。

## <a name="latitude-and-longitude-properties-no-longer-available"></a>不再可用的纬度和经度属性

**AdControl** 类不会再有适用于 UWP 应用的“纬度”和“经度”属性。 内置于广告控件的代码将以应用的名义检测这些值，并将它们发送到广告服务器。


 

 
