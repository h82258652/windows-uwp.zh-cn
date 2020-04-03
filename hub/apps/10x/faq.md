---
Description: 了解有关 Windows 10 的一些基本开发人员问题的解答。
title: Windows 10 10 开发人员常见问题解答
ms.topic: article
ms.date: 04/1/2020
ms.localizationpriority: medium
ms.author: quradic
author: QuinnRadich
ms.openlocfilehash: d168faf16bdd8a88e0cd4c52c680781061bfcf0d
ms.sourcegitcommit: dadbc20564e4b0c0ecbab5fd5649dbf8ab08095a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2020
ms.locfileid: "80581951"
---
# <a name="windows-10x-developer-faq"></a>Windows 10 10 开发人员常见问题解答

Windows 10 倍是 Windows 家族中的一系列产品系列，适用于在双屏幕设备上使用。 作为一名开发人员，您可以通过优化您的应用程序进行 Windows 10 倍，利用特定于移动用户和双屏幕观众的新功能，同时还能享受相同的 Windows 10 功能和丰富的桌面支持，从而实现更大的受众。 [我们已于2019晚的时间发布了 Windows 10](https://blogs.windows.com/windowsexperience/2019/10/02/introducing-windows-10x-enabling-dual-screen-pcs-in-2020/#6qxkItE2XMPu24uw.97)，我们期待在晚2020发布。

![运行 Windows 10 的设备](images/windows-10x-devices.png)
 
*[显示的预发布产品、模拟屏幕并随时更改]*

若要深入了解如何构建双屏幕体验和 Windows 10 倍，请从[Microsoft 365 开发人员日](https://developer.microsoft.com/microsoft-365/virtual-events)或[双屏幕开发人员文档](https://docs.microsoft.com/dual-screen/)中查看虚拟会话。为了一目了然地说明，以下是一些你可能遇到的问题的解答。

### <a name="how-is-this-different-from-developing-for-windows-10"></a>这与开发 Windows 10 有何不同？

大多数应用程序的情况并不完全相同。 通过 Windows 10 SDK 支持编写 Windows 10 10 个应用程序。 Windows 10 的表达式支持 Windows 运行时（WinRT） Api，并通过本机容器运行 Win32 应用。 然后，可以从新的或现有的 UWP 或 Win32 应用程序中为双屏幕设备调用专门设计的新 Api，从而使它们能够访问此新平台的功能和优势。

### <a name="does-this-replace-desktop-windows-10"></a>这是否会替代桌面 Windows 10？

No。 Windows 10 会并行发布到 Windows 10 的桌面版本。 Windows 10 桌面版将继续提供新式桌面应用程序的增强功能和改进功能。 Windows 10 倍是经过优化的另一个平台，支持双屏幕平台。

### <a name="when-will-windows-10x-be-released"></a>何时会发布 Windows 10 倍？

在2020年后，将向 Neo 和其他第三方双重屏幕设备交付 Windows 10 倍。

### <a name="when-can-i-start-development-for-windows-10x"></a>何时可以开始 Windows 10 的开发？

你现在可以下载[Microsoft 仿真程序和 Windows 10 倍模拟器映像](https://docs.microsoft.com/dual-screen/windows/get-dev-tools)。 我们将继续改进此模拟器，并为其提供对其他启用了 Windows 10 10 的设备的支持。 这些仿真程序与 Windows SDK 的预发布版本结合在一起后，你将能够在第一个双屏幕设备公开发布之前，针对 Windows 10 进行开发。

### <a name="will-my-universal-windows-platform-uwp-apps-run-on-windows-10x"></a>我的通用 Windows 平台（UWP）应用是否会在 Windows 10 倍运行？

大多数 UWP 应用在 Windows 10 和更高运行 Windows 10 的设备上完全受支持，无需任何更改。 支持所有 WinRT Api，因为 UWP 应用有权访问的大多数其他功能。 随着预发布版本的开发，我们将发布详细介绍其他不支持的功能的文档。

### <a name="will-my-win32-apps-run-on-windows-10x"></a>我的 Win32 应用是否会在 Windows 10 倍运行？

Windows 10 的本地支持提供了在包含环境中运行 Win32 应用程序的本机支持。 大多数 Win32 应用程序都可以在 Windows 10 的设备上运行和调试，而无需发生事件，还可以使用新的 Win32 Api 向应用添加双屏幕支持。

### <a name="are-there-any-features-of-my-app-that-wont-work-on-windows-10x"></a>我的应用程序中是否有不能正常运行的功能？

随着 Windows 10 倍持续开发的发布，我们将发布突出显示其特定限制的文档。 但是，用于运行 Win32 应用的包含环境不包括 Windows Shell，因此不支持 Shell 扩展和类似功能。 同样，Windows 10 10 设备不支持与某些系统设置（如电源使用选项）相关的 Api。

### <a name="if-i-enhance-my-app-with-windows-10x-features-will-it-still-run-on-devices-running-desktop-windows-10"></a>如果我的应用程序具有 Windows 10 倍功能，它是否仍在运行桌面 Windows 10 的设备上运行？

为 Windows 10 倍设计的应用仍适用于运行 Windows 10 桌面版的设备，不过，在下一个主要版本更新之前，不会将这些新的 Windows Api 添加到 Windows 10 的桌面版本。 就像你在多个版本的桌面 Windows 10 上开发的应用程序一样，请遵循[自适应编码最佳做法](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)，以确保你的应用程序在10和10个 windows 10 上都正常工作。 
