---
ms.assetid: 646977ed-1705-4ea7-a3db-a6b9aac70703
description: 了解如何使用 JavaScript/HTML 启动间隙广告。
title: JavaScript 中的间隙广告示例代码
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, 广告, 间隙, javascript, 示例代码
ms.localizationpriority: medium
ms.openlocfilehash: 2f08e3425b8364bf8c6beb8a8d136534aea4a9b2
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463509"
---
# <a name="interstitial-ad-sample-code-in-javascript"></a>JavaScript 中的间隙广告示例代码

>[!WARNING]
> 从2020年6月1日起，将关闭适用于 Windows UWP 应用的 Microsoft Ad 盈利平台。 [了解详细信息](https://aka.ms/ad-monetization-shutdown)

本主题提供了显示间隙广告的基本 JavaScript 和 HTML 通用 Windows 平台 (UWP) 应用的完整示例代码。 有关显示如何配置项目以使用此代码的分步说明，请参阅[间隙广告](interstitial-ads.md)。 有关完整示例项目，请参阅 [GitHub 上的广告示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)。

## <a name="code-example"></a>代码示例

本部分显示了显示间隙广告的基本应用中的 HTML 和 JavaScript 文件内容。 若要使用这些示例，请将代码复制到 Visual Studio 中的 JavaScript **WinJS 应用（通用 Windows）** 项目中。

此示例应用使用两个按钮请求和启动间隙广告。 Visual Studio 生成的 main.js 和 index.html 文件已修改，如下所示。 script.js 文件（如下所示）包含示例中的大部分代码，应该将此文件添加到项目的 **js** 文件夹中。

在将应用提交到应用商店之前，请将 ```applicationId``` 和 ```adUnitId``` 变量的值替换为来自合作伙伴中心的实时值。 有关详细信息，请参阅[在应用中设置广告单元](set-up-ad-units-in-your-app.md#live-ad-units)。

> [!NOTE]
> 若要更改此示例以显示间隙横幅广告而不是间隙视频广告，请将值 **InterstitialAdType.display** 传递至 [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 方法的第一个参数而不是 **InterstitialAdType.video**。 有关详细信息，请参阅[间隙广告](interstitial-ads.md)。

### <a name="indexhtml"></a>index.html

> [!div class="tabbedCodeSnippets"]
[!code-html[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/index.html#L1-L21)]

### <a name="scriptjs"></a>script.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#script)]

### <a name="mainjs"></a>main.js

> [!div class="tabbedCodeSnippets"]
[!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/main.js#main)]

## <a name="related-topics"></a>相关主题

* [GitHub 上的广告示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)

 
