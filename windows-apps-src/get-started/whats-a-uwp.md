---
author: GrantMeStrength
ms.assetid: C9787269-B54F-4FFA-A884-D4A3BF28F80D
title: "什么是通用 Windows 平台 (UWP) 应用？"
description: "了解通用 Windows 应用的不同应用类型：Windows 应用商店应用、Windows Phone 应用商店应用和 Windows 运行时应用。"
ms.author: jken
ms.date: 03/22/2017
ms.topic: article
pms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 3bbced2db33210952b6c8a45f98e36582330d7d9
ms.sourcegitcommit: 214a1dcb24e0811811bd7a4a07bfe707ecd93b18
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/15/2017
---
# <a name="whats-a-universal-windows-platform-uwp-app"></a>什么是通用 Windows 平台 (UWP) 应用？

通用 Windows 平台 (UWP) 是一款面向 Windows 10 的应用平台。 你可以开发适用于 UWP 的应用，只需一个 API 集、一个应用包和一个应用商店，即可访问所有 Windows 10 设备 - PC、平板电脑、手机、Xbox、HoloLens、Surface Hub 等。 对许多屏幕大小以及各种交互模型（无论是触控、鼠标和键盘、游戏控制器还是笔）的支持也更加轻松。 用户希望其*体验*在所有设备上均为移动版，并且希望使用现有的最方便或最高效的设备完成任务，此理念是 UWP 应用的核心。

UWP 同样也非常灵活：如果不愿意的话，也可以不使用 C# 和 XAML。 是否要在 Unity 或 MonoGame 中开发？ 更喜欢 JavaScript？ 这不是问题，使用所有所需的项目。 想要使用 UWP 功能扩展 C++ 桌面应用并将其在应用商店内出售？ 这同样可以实现。 

总而言之：完全可以在单个项目中使用你熟悉的编程语言、框架和 API，将几乎相同的代码运行在现存的大量 Windows 硬件上。 在编写 UWP 应用后，你可以将此应用发布到应用商店以供全球客户浏览。

![Windows 支持的设备](images/1894834-hig-device-primer-01-500.png)
 
##<a name="so-what-exactly-is-a-uwp-app"></a>那么，UWP 应用*确切*的定义是什么？

什么使 UWP 应用与众不同？ 以下特性使 Windows 10 上的 UWP 应用与众不同。

-   **所有设备上均有常用 API 图面。**

    所有 Windows 设备系列均使用相同的通用 Windows 平台 (UWP) 核心 API。 如果应用仅使用核心 API，则它可在任何 Windows 10 设备上运行，不管你是面向台式机、Xbox 还是混合现实耳机。

-   **扩展 SDK 让你的应用可以在特定设备类型上创建出色的内容。**

    扩展 SDK 可为每个设备类添加专用 API。 例如，如果你的 UWP 应用针对 HoloLens，则除了常规的 UWP 核心 API 之外，还可以添加 HoloLens 功能。
    如果面向通用 API，则你的应用包可在运行 Windows 10 的所有设备上运行。 但是，如果希望你的 UWP 应用在其运行于特定设备类时能够充分利用设备特定 API，则在调用 API 之前，可以在运行时检查其是否存在。 

-   **应用使用 .AppX 打包格式进行打包并通过应用商店分配。**

    所有 UWP 应用均可作为 AppX 程序包进行分配。 这提供了值得信赖的安装机制，并确保应用可以无缝进行部署和更新。

-   **存在一个适用于所有设备的应用商店。**

    注册为应用开发人员后，你可以向应用商店提交应用，并使其在所有类型的设备或仅在所选设备类型上可用。 你将在一个位置上提交和管理适用于 Windows 设备的所有应用。

-   **应用支持自适应控件和输入**

    UI 元素使用*有效像素*（请参阅[适用于 UWP 应用的响应式设计 101](https://msdn.microsoft.com/library/windows/apps/Dn958435)），因此它们可通过基于设备上的可用屏幕像素数工作的布局进行响应。 而且它们与多种输入类型（如键盘、鼠标、触摸、笔和 Xbox One 控制器）配合良好。 如果你需要进一步为特定屏幕大小或设备定制 UI，新的布局面板和工具将帮助你使 UI 适应运行应用的设备。



## <a name="use-a-language-you-already-know"></a>使用一种你已知道的语言


UWP 应用可以使用 Windows 运行时，它是一个内置于操作系统的本机 API。 此 API 通过 C++ 实现，并且在 C#、Visual Basic、C++ 和 JavaScript 中均受支持。 适用于 UWP 中的编写应用的部分选项包括：
-   XAML UI 和 C#、VB 或 C++ 后端
-   DirectX UI 和 C++ 后端
-   JavaScript 和 HTML

Microsoft Visual Studio 2017 为每种语言提供 UWP 应用模板，该模板允许你为所有设备创建单个项目。 完成工作后，你可以生成应用包，并将其从 Visual Studio 提交到 Windows 应用商店，以向任何 Windows 10 设备上的客户提供你的应用。

## <a name="uwp-apps-come-to-life-on-windows"></a>在 Windows 上构建 UWP 应用


在 Windows 上，你的应用可以向用户提供相关且实时的信息，并且吸引他们再次访问更多信息。 在现代应用经济中，你的应用必须具有足够的吸引力才能受到用户的青睐。 Windows 为你提供了大量资源，使你的用户会回过头来使用你的应用：

-   动态磁贴和锁屏可以显示上下文相关且实时的信息概览。

-   推送通知可在用户需要时，提供实时的突发提醒来引起用户的注意。

-   操作中心可让你组织和显示用户需要采取操作的通知和内容。

-   后台执行和触发器使你的应用仅在用户需要时才会运行。

-   你的应用可以使用语音和蓝牙 LE 设备来帮助用户与周围的世界进行交互。

-   支持丰富的数字墨迹和创新 Dial。

-   Cortana 可以为你的软件添加个性。

-   XAML 为你提供创建流畅、动态用户界面所需的工具。

最后，你可以使用漫游数据和 Windows 凭据保险箱，在用户运行你的应用的所有 Windows 屏幕上提供一致的漫游体验。 通过漫游数据可以方便地在云中存储用户的首选项和设置，而无需生成你自己的同步基础结构。 另外，你可以将用户凭据存储在凭据保险箱中，该功能最为重视安全性和可靠性。

##  <a name="monetize-your-app"></a>获取应用收益


在 Windows 上，你可以选择以何种方式销售自己的应用—通过手机、平板电脑、PC 以及其他设备。 我们提供了多种方式让你通过自己的应用及其提供的服务来获得收益。 你只需选择最适合自身的方式即可。

-   付费下载是最简单的选项， 你只需指定价格即可。
-   试用允许用户在购买前先试用你的应用，与更传统的“免费模式”选项相比，用户更易于发现你的应用并转而使用该应用。
-   使用应用和加载项的销售价格。
-   此外还提供应用内购买和广告。

## <a name="lets-get-started"></a>让我们开始吧


有关 UWP 详细信息，请参阅[通用 Windows 平台应用指南](universal-application-platform-guide.md)。 接着，请查看[准备工作](get-set-up.md)以下载创建应用所需的工具，然后再编写[你的第一个应用](your-first-app.md)！


## <a name="more-advanced-topics"></a>更多高级主题

* [.NET 本机 - 对于通用 Windows 平台 (UWP) 开发人员的意义](https://blogs.windows.com/buildingapps/2015/08/20/net-native-what-it-means-for-universal-windows-platform-uwp-developers/#TYsD3tJuBJpK3Hc7.97)
* [.NET 中的通用 Windows 应用](https://blogs.msdn.microsoft.com/dotnet/2015/07/30/universal-windows-apps-in-net)
* [适用于 UWP 应用的 .NET](https://msdn.microsoft.com/library/mt185501.aspx)
