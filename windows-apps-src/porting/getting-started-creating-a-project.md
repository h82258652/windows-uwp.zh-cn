---
author: mcleblanc
ms.assetid: 08C8F359-E8B6-4A45-8F4B-8A1962F0CE38
description: "Microsoft Visual Studio 与 Windows 的关系就像 Xcode 与 iOS 和 Mac OS 的关系一样。 在本演练中，我们将帮助你熟悉如何使用 Visual Studio。"
title: "在 Visual Studio 中创建项目"
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: c8aba48fba1badcfeebc9868215b904c76ac6b98
ms.lasthandoff: 02/07/2017

---

# <a name="getting-started-creating-a-project"></a>入门：创建项目

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## <a name="creating-a-project"></a>创建项目

Microsoft Visual Studio 与 Windows 的关系就像 Xcode 与 iOS 和 Mac OS 的关系一样。 在本演练中，我们将帮助你熟悉如何使用 Visual Studio。 本演练将向你展示入门所需了解的全部基础知识。 每次创建应用时，你都需要遵循以下类似步骤。

下面的视频针对 Xcode 和 Visual Studio 进行了比较。

<iframe src="https://hubs-video.ssl.catalog.video.msn.com/embed/5b7bd91f-6a2f-40b6-9b19-eb2994931d0a/IA?csid=ux-en-us&MsnPlayerLeadsWith=html&PlaybackMode=Inline&MsnPlayerDisplayShareBar=false&MsnPlayerDisplayInfoButton=false&iframe=true&QualityOverride=HD" width="720" height="405" allowFullScreen="true" frameBorder="0" scrolling="no">一份开发备忘录 - 比较 Xcode 与 Visual Studio</iframe>

你还会发现此[生成适用于 Windows 的应用博客文章](https://blogs.windows.com/buildingapps/2016/01/27/visual-studio-walkthrough-for-ios-developers/)非常有用。

创建 Windows 10 应用（更正式的称呼为通用 Windows 平台 (UWP) 应用）与使用情节提要创建 iOS 应用极为相似。 Windows 10 应用通常在多个页面上进行构建，每个页面包含用户界面的不同部分，类似于网站。 通常每个页面具有两个关联的源文件：一个源文件存储用户界面（以 [XAML 概述](https://msdn.microsoft.com/library/windows/apps/mt185595)格式存储），另一个源文件包含源代码（通常为 C#）。 当用户与应用交互时，它们会在这些页面之间导航。 在本演练中，你将创建带有两个页面的应用。

**注意**  Windows 10 应用的一项重要功能是，无论在什么平台上，均可使用相同的源代码和相同的 API 集。 正如你所知，在编写适用于 iPhone 和 iPad 的通用 iOS 应用时，可在运行时决定应用运行的平台，并采取相应操作。 同样地，Windows 10 应用可在运行时分辨出其在什么设备上运行。 相较于桌面版，借助 UWP 应用，无需在源代码中使用 \#ifdef 即可创建手机版。 为方便起见，Windows 10 应用还可根据设备使用其用户界面控件：例如，应用可能引用日期选取器控件，它将自动呈现不同的外观和具有不同的功能，具体取决于它是在台式机屏幕还是在手机屏幕上运行。这也是应用智能的一种体现。 但源代码不会改变。

让我们看看如何创建 Windows 10 应用。 首先运行 Visual Studio。 首次运行时，Visual Studio 将要求你获取开发人员许可证。 开发人员许可证使你可以先在本地计算机上安装和测试 Windows 应用商店应用，然后再将其提交到 Windows 应用商店。 要获得许可证，请按照屏幕说明，使用 Microsoft 帐户登录。 如果没有帐户，请单击“开发人员许可证”****对话框中的“注册”****链接，并按照屏幕上的说明操作。

为了进行比较，在启动 Xcode 时，你首先看到“欢迎使用 Xcode”****屏幕，如下图所示。

![Xcode 欢迎屏幕](images/ios-to-uwp/ios-to-uwp-xcode-welcome.png)

Visual Studio 与此非常相似。 你将看到“起始页”****，如下图所示。

![Visual Studio 开始屏幕](images/ios-to-uwp/ios-to-uwp-vs-welcome.png)

若要创建新应用，请首先通过执行以下操作之一来创建项目：

-   在“开始”****区域中，点击“新建项目”****。
-   点击“文件”****菜单，然后点击“新建项目”****。

为了进行比较，在 Xcode 中创建新项目时，你将看到一个与下图中类似的项目模板列表。

![Xcode“新建项目”对话框](images/ios-to-uwp/ios-to-uwp-xcode-choose-template.png)

在 Visual Studio 中，还可以从若干项目模板中选择，如下图中所示。

![Visual Studio 新建项目对话框](images/ios-to-uwp/ios-to-uwp-vs-choose-template.png) 对于该演练，请点击“Visual C#”****，然后依次点击“Windows”****、“Windows 通用”****和“空白应用(Windows 通用)”****。 在“名称”****框中，键入“MyApp”，然后点击“确定”****。 Visual Studio 将创建并显示你的第一个项目。 现在你可以开始设计你的应用并向其添加代码。

## <a name="next-step"></a>下一步

[入门：选择一种编程语言](getting-started-choosing-a-programming-language.md)

