---
Description: 了解有关 Windows 10 的一些基本开发人员问题的解答。
title: Windows 10 10 开发人员常见问题解答
ms.topic: article
ms.date: 06/02/2020
ms.localizationpriority: medium
ms.author: quradic
author: QuinnRadich
ms.openlocfilehash: 3ba14e33c098d3515522a9a5907065751fafba87
ms.sourcegitcommit: 13bda6040988461a61b1b5561fde2f7a54835ccd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/03/2020
ms.locfileid: "84318233"
---
# <a name="windows-10x-developer-faq"></a>Windows 10X 开发人员常见问题解答

> [!重要提醒]
> 在最近我们公开宣布了对 Windows 10 和 Windows 10X 的优先级进行了一些调整。
> 这些公告包含了 Windows 10X 在设备外形上的优先级的变更。[您可以点击这里阅读详细信息。](https://blogs.windows.com/windowsexperience/2020/05/04/accelerating-innovation-in-windows-10-to-meet-customers-where-they-are/)

Windows 10X 是 Windows 家族中的一员，主要适用于在双屏幕设备上。作为一名开发人员，您可以为您的应用程序在 Windows 10X 上进行优化，例如针对移动设备或双屏幕设备的特性，同时还能获得与 Windows 10 相同的功能与丰富的桌面支持，从而实现更大的受众面。[我们在 2019 年末的时候公开宣布了 Windows 10X](https://blogs.windows.com/windowsexperience/2019/10/02/introducing-windows-10x-enabling-dual-screen-pcs-in-2020/#6qxkItE2XMPu24uw.97)，并且我们计划在 2020 年底的时候发布它。

![运行 Windows 10X 的设备](images/windows-10x-devices.png)
 
*[这里显示的是预发布的产品和模拟的屏幕，随时可能会进行更改]*

若想要深入了解如何构建拥有双屏幕体验的应用程序或想深入了解 Windows 10X 的话，请从 [Microsoft 365 开发人员日](https://developer.microsoft.com/microsoft-365/virtual-events)查看相关的虚拟会议或者查阅[双屏幕开发人员文档](https://docs.microsoft.com/dual-screen/)。为了更清晰地说明，这里我们准备了一些可能您想到的一些问题的答案。

### <a name="how-is-this-different-from-developing-for-windows-10"></a>这和在 Windows 10 上开发的时候有不一样的地方吗？

对于绝大部分的应用程序来说，基本上是一样的。您可以通过使用 Windows 10 SDK 的方式来编写 Windows 10X 的应用程序。Windows 10X 作为 Windows 10 的一种表现形式，Windows 10X 将会支持 Windows 运行时（WinRT）的 API，而 Win32 的应用则会运行在一个本地的容器当中。然后您也能从您新开发的或现有的 UWP 或 Win32 应用程序中调用新的专门为双屏幕设备设计的 API，这些 API 将能让您的应用程序访问到新平台的功能和优点。

### <a name="does-this-replace-desktop-windows-10"></a>这是否会取代桌面版的 Windows 10？

请您放心，这不会的。Windows 10X 与桌面版本的 Windows 10 是并行发布的。Windows 10 桌面版将会继续改进和增强现代化桌面应用程序的功能。而 Windows 10X 是为了双屏幕设备而专门优化的另一个平台。

### <a name="when-will-windows-10x-be-released"></a>什么时候会发布 Windows 10X？

我们计划在 2020 年底，向 Surface Neo 和其它第三方厂商的双屏幕设备交付 Windows 10X。

### <a name="when-can-i-start-development-for-windows-10x"></a>我现在能开始 Windows 10X 上的开发了吗？

可以的，您现在可以下载[Microsoft 模拟器和 Windows 10X 模拟器镜像](https://docs.microsoft.com/dual-screen/windows/get-dev-tools)来进行您的开发。我们将会持续改进这个模拟器，并且完善对其它 Windows 10X 设备的支持。通过模拟器配合预发布版本的 Windows SDK，您将能在第一台双屏幕设备公开发布之前开展您在 Windows 10X 上的开发工作。

### <a name="will-my-universal-windows-platform-uwp-apps-run-on-windows-10x"></a>我的通用 Windows 平台（UWP）应用程序是否能在 Windows 10X 上运行？

Windows 10X 支持绝大部分的 UWP 应用程序，并且无需作出任何更改即可在设备上运行。所有的 WinRT API 都会被支持，绝大部分 UWP 应用程序所访问到的功能也会被支持。随着预发布版本的开发，我们将会发布详细的说明文档，说明那些功能是不支持的。

### <a name="will-my-win32-apps-run-on-windows-10x"></a>我的 Win32 应用是否能在 Windows 10X 上运行？

Windows 10X 提供了支持本地运行 Win32 应用程序的容器环境。如无意外的话，大多数的 Win32 应用程序都可以在 Windows 10X 的设备上运行和调试，您可以对您的应用程序添加新的 Win32 API 以支持双屏幕。

### <a name="are-there-any-features-of-my-app-that-wont-work-on-windows-10x"></a>有哪些功能是 Windows 10X 上不能运作的？

随着 Windows 10X 的预发布版本的开发，我们将会发布文档并突出有哪些限制。然而，用于运行 Win32 应用的容器环境不包括 Windows Shell，因此不支持 Shell 扩展或类似的功能。同样的，Windows 10X 设备不支持与某些系统设置（如电源使用选项）相关的 API。

### <a name="if-i-enhance-my-app-with-windows-10x-features-will-it-still-run-on-devices-running-desktop-windows-10"></a>如果我的应用程序包含了 Windows 10X 的功能，它是否仍然可以在桌面版 Windows 10 的设备上运行？

为 Windows 10X 开发的应用程序依然可以在运行 Windows 10 桌面版的设备上运行。不过，在下一个 Windows 10X 的主要版本更新之前，这些新的 Windows API 将不会被添加进 Windows 10 的桌面版本当中。像在多个版本的桌面版 Windows 10 上开发的应用程序一样，建议遵循[自适应编码最佳做法](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)，以确保您的应用程序在 Windows 10X 和桌面版的 Windows 10 上都正常工作。 
