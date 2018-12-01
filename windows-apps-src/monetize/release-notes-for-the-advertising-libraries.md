---
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: 查看 Microsoft Advertising 库的发行说明。
title: Advertising 库的发行说明
ms.date: 08/23/2017
ms.topic: article
keywords: windows 10, uwp, ads, 广告, 发行说明
ms.localizationpriority: medium
ms.openlocfilehash: 1bab822c81cdd5af1e6b00ca8d33ed7f7ea3838f
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "8345277"
---
# <a name="release-notes-for-the-advertising-libraries"></a>Advertising 库的发行说明




本部分提供当前版本的 Microsoft Advertising 库的发行说明。 这些库支持 windows 10、 Windows8.1、 Windows Phone 8.1 以及 WindowsPhone8 XAML 和 JavaScript/HTML 应用。

## <a name="installation"></a>安装


Microsoft 广告库作为 [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp) 的一部分提供。 有关安装 SDK 的更多信息，请参阅[安装 Microsoft 广告 SDK](install-the-microsoft-advertising-libraries.md)。

## <a name="uninstall-previous-versions"></a>卸载以前版本

安装最新的 Microsoft 广告 SDK 之前，强烈建议你先卸载所有之前的 SDK 实例。 有关详细信息，请参阅[安装 Microsoft 广告 SDK](install-the-microsoft-advertising-libraries.md)。

## <a name="target-architecture-specific-build-outputs"></a>面向特定于体系结构的生成输出

在使用 Microsoft Advertising 库时，你无法在项目中面向**任何 CPU**。 如果你的项目面向**任何 CPU** 平台，在你添加对 Microsoft Advertising 库的引用后，可能会在项目中看到一条警告。 若要删除此警告，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 有关详细信息，请参阅[已知问题](known-issues-for-the-advertising-libraries.md)。

## <a name="c-support"></a>C++ 支持

Microsoft Advertising 库（其中包括 **AdControl** 和 **InterstitialAd** 类）支持在 C++ 和 DirectX 中使用 Windows 运行时互操作性（*互操作*）编写的应用。 有关使用 XAML 和 C++ 编程的详细信息和示例，请参阅[类型系统](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx)。

## <a name="no-toolbox-control"></a>没有工具箱控件

在 [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp) 中的当前版本的 Microsoft 广告库中，没有任何工具箱控件可用于将 **AdControl** 或 **InterstitialAd** 拖动到应用中的设计图面。 有关在标记和代码中添加这些控件的说明，请参阅[开发人员演练](developer-walkthroughs.md)。

## <a name="latitude-and-longitude-properties-no-longer-available"></a>不再可用的纬度和经度属性

**AdControl** 类不会再有适用于 UWP 应用的“纬度”**** 和“经度”**** 属性。 内置于广告控件的代码将以应用的名义检测这些值，并将它们发送到广告服务器。


 

 
