---
author: Xansky
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: 了解如何使用 C# 启动间隙广告。
title: C# 间隙广告示例代码
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 广告, 间隙, c#, 示例代码
ms.localizationpriority: medium
ms.openlocfilehash: 195f13d3a51925925d320b87cd0142d14d449226
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "5438720"
---
# <a name="interstitial-ad-sample-code-in-c"></a>C\# 间隙广告示例代码 #  

本主题提供了显示间隙视频广告的基本 C# 和 XAML 通用 Windows 平台 (UWP) 应用的完整示例代码。 有关显示如何配置项目以使用此代码的分步说明，请参阅[间隙广告](interstitial-ads.md)。 有关完整示例项目，请参阅 [GitHub 上的广告示例](http://aka.ms/githubads)。

## <a name="code-example"></a>代码示例

本部分显示了显示间隙广告的基本应用中的 MainPage.xaml 和 MainPage.xaml.cs 文件内容。 若要使用这些示例，请将代码复制到 Visual Studio 中的 Visual C# **空白应用（通用 Windows）** 项目中。

此示例应用使用两个按钮请求和启动间隙广告。 将应用提交到应用商店之前，将 ```myAppId``` 和 ```myAdUnitId``` 字段的值替换为 Windows 开发人员中心的实时值。 有关详细信息，请参阅[在应用中设置广告单元](set-up-ad-units-in-your-app.md#live-ad-units)。

> [!NOTE]
> 若要更改此示例以显示间隙横幅广告而不是间隙视频广告，请将值 **AdType.Display** 传递至 [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 方法的第一个参数而不是 **AdType.Video**。 有关详细信息，请参阅[间隙广告](interstitial-ads.md)。

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>相关主题

* [GitHub 上的广告示例](http://aka.ms/githubads)
 
