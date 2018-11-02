---
author: stevewhims
ms.assetid: ba2ac5f5-1e0d-4f1d-a6f8-6a65b4cff501
description: 本部分介绍了如何移植现有应用到其中，你可以创建可供客户安装到所有类型的设备上的单个 windows 10 应用包通用 Windows 平台 (UWP)。 你的应用将受益于精彩的新硬件、绝佳的营销机会、现代 API 集、自适应 UI 控件以及包括鼠标/键盘、触摸和语音在内的各种输入形式。
title: 将应用移植到 windows 10
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bb5c6ae373e4e35e640223fe08a5a49f2e7a5dd3
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5939984"
---
# <a name="porting-apps-to-windows10"></a>将应用移植到 windows 10


本部分介绍了如何移植现有应用到其中，你可以创建可供客户安装到所有类型的设备上的单个 windows 10 应用包通用 Windows 平台 (UWP)。 你的应用将受益于精彩的新硬件、绝佳的营销机会、现代 API 集、自适应 UI 控件以及包括鼠标/键盘、触摸和语音在内的各种输入形式。

Windows 运行时 (WinRT) 是可让你生成通用 Windows 平台 (UWP) 应用的技术。 有关 WinRT 和 UWP 应用的更多背景知识，请参考[什么是通用 Windows 平台 (UWP) 应用？](https://msdn.microsoft.com/library/windows/apps/dn726767)。

此移植指南介绍当前应用的技术和通用 Windows 平台 (UWP) 之间的差异。 了解技术之间的差异后，将能够深入了解开发人员中心的其余部分，它是用于开发 UWP 应用的综合性资源。 实现此目的一个好方法是从[如何开发应用商店应用](https://msdn.microsoft.com/library/windows/apps/dn726537)开始。

| 主题 | 说明 |
|-------|-------------|
| [从 Windows Phone Silverlight 移动到 UWP](wpsl-to-uwp-root.md) | 如果你是一名具有 Windows Phone Silverlight 应用的开发人员，你可以在移植到 Windows 10 时充分使用你的技能组合和源代码。 借助 Windows 10，你可以创建 UWP 应用，该应用是可供客户安装到各种设备的单个应用包。 |
| [从 Windows 运行时 8.x 移动到 UWP](w8x-to-uwp-root.md) | 如果你有一个通用 8.1 应用（无论它是面向 Windows 8.1、Windows Phone 8.1 还是同时面向这两者），你会发现你的源代码和技能将顺利地移植到 Windows 10。 借助 Windows 10，你可以创建 UWP 应用，该应用是可供客户安装到各种设备的单个应用包。 |
| [面向 Android 和 iOS 开发人员的 Windows 应用概念映射](android-ios-uwp-map.md) | 如果你是一名拥有 Android 或 iOS 技能或代码的开发人员，并且希望移动到 Windows 10 和通用 Windows 平台，此资源为你提供了在这三种平台之间映射平台功能（及知识）所需的一切。 |
| [从 iOS 移动到 UWP](ios-to-uwp-root.md) | 你是否是一名 iOS 开发人员，正在思考如何转移到 Windows 10 和 UWP？ 这未必像你想象的那样困难。 我们提供创建在 Windows 上与在 iOS 设备上一样运行良好（甚至更好！）的出色应用所需的工具、技术和信息。 |
| [从桌面版移动到 UWP](desktop-to-uwp-root.md) | 将 Win32 和 .NET 4.6.1 桌面应用程序转换为通用 Windows 平台 (UWP) 应用。 |
| [将 Web 应用转换为 PWA](https://docs.microsoft.com/microsoft-edge/progressive-web-apps) | 现在，可以将你的 Web 应用 (PWA) 转换为适用于包括 UWP 在内的所有平台的渐进式 Web 应用！ [PWA Builder 工具](https://www.pwabuilder.com)将为你生成必要的清单。 用于替代托管的 Web 应用 (HWA) 桥。 |

## <a name="related-topics"></a>相关主题

* [从 WPF 和 Silverlight 移动到 WinRT](https://msdn.microsoft.com/library/windows/apps/dn263237)
* [从 Android 移动到 WinRT](https://msdn.microsoft.com/library/windows/apps/jj945421)
* [从 Web 移动到 WinRT](https://msdn.microsoft.com/library/windows/apps/hh465151)
