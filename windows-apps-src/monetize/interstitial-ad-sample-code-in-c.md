---
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: 了解如何使用 C# 启动间隙广告。
title: C# 中的间隙广告示例代码
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, 广告, 间隙, c#, 示例代码
ms.localizationpriority: medium
ms.openlocfilehash: 9d908151e30510977669f2bc101575754a946560
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507041"
---
# <a name="interstitial-ad-sample-code-in-c"></a>C\# 中的插播式 ad 示例代码 #  

>[!WARNING]
> 从2020年6月1日起，将关闭适用于 Windows UWP 应用的 Microsoft Ad 盈利平台。 [了解详细信息](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

本主题提供了显示间隙视频广告的基本 C# 和 XAML 通用 Windows 平台 (UWP) 应用的完整示例代码。 有关显示如何配置项目以使用此代码的分步说明，请参阅[间隙广告](interstitial-ads.md)。 有关完整示例项目，请参阅 [GitHub 上的广告示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)。

## <a name="code-example"></a>代码示例

本部分显示了显示间隙广告的基本应用中的 MainPage.xaml 和 MainPage.xaml.cs 文件内容。 若要使用这些示例，请将代码复制到 Visual Studio 中的 Visual C# **空白应用（通用 Windows）** 项目中。

此示例应用使用两个按钮请求和启动间隙广告。 在将应用提交到应用商店之前，请将 "```myAppId```" 和 "```myAdUnitId```" 字段的值替换为 "合作伙伴中心" 中的实时值。 有关详细信息，请参阅[在应用中设置广告单元](set-up-ad-units-in-your-app.md#live-ad-units)。

> [!NOTE]
> 若要更改此示例以显示间隙横幅广告而不是间隙视频广告，请将值 **AdType.Display** 传递至 [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 方法的第一个参数而不是 **AdType.Video**。 有关详细信息，请参阅[间隙广告](interstitial-ads.md)。

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-csharp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>相关主题

* [GitHub 上的广告示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
 
