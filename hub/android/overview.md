---
title: Windows 上的 Android 开发概述
description: 本指南可帮助你开始在 Windows 上开发 Android。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: windows 上的 android，xamarin，响应本机，cordova，ionic，phonegap，c + + android 游戏，windows defender，模拟器
ms.date: 04/28/2020
ms.openlocfilehash: d43420f442fd5dfcb2b885fb0369964a113e9bac
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255241"
---
# <a name="overview-of-android-development-on-windows"></a>Windows 上的 Android 开发概述

使用 Windows 操作系统开发 Android 设备应用有多个路径。 这些路径分为三个主要类型：**[本机 Android 开发](#native-android)**、**[跨平台开发](#cross-platform)** 和**[Android 游戏开发](#game-development)**。 本概述将帮助你确定要遵循哪些开发路径来开发 Android 应用程序，并提供[后续步骤](#next-steps)来帮助你开始使用 Windows 进行开发：

- [本机 Android](native-android.md)
- [Xamarin.Android](xamarin-android.md)
- [Xamarin.Forms](xamarin-forms.md)
- [响应本机](react-native.md)
- [Cordova、Ionic 或 PhoneGap](pwa.md)
- [用于游戏开发的 c/c + +](native-android.md#use-c-or-c-for-android-game-development)

此外，本指南还将提供有关使用 Windows 执行以下操作的提示：

- [在 Android 设备或仿真程序上测试](emulator.md)
- [更新 Windows Defender 设置以提高性能](defender-settings.md)
- [开发适用于 Android 的双屏幕应用并获取 Surface 双核设备 SDK](https://docs.microsoft.com/dual-screen/android/)

## <a name="native-android"></a>本机 Android

[Windows 上的本机 Android 开发](./native-android.md)意味着你的应用仅面向 Android （而非 IOS 或 Windows 设备）。 你可以使用[Android Studio](https://developer.android.com/studio/install#windows)或[Visual Studio](https://visualstudio.microsoft.com/vs/android/)在专门为 Android 操作系统设计的生态系统中进行开发。 性能将针对 Android 设备进行优化，用户界面的外观将与设备上的其他本机应用一致，并且用户设备的任何功能和功能都将直接进行访问和利用。 以本机格式开发应用程序将有助于它直接 "感觉正确"，因为它遵循专门为 Android 设备建立的所有交互模式和用户体验标准。

## <a name="cross-platform"></a>跨平台

跨平台框架提供单个基本代码，可以（主要）在 Android、iOS 和 Windows 设备之间共享。 使用跨平台框架可帮助你的应用跨设备平台保持相同的外观、感受和体验，以及自动推出更新和修补程序。 应用在共享代码库中开发，通常采用一种语言，而不需要了解各种特定于设备的代码语言。

尽管跨平台框架旨在尽可能接近本地应用程序，但它们永远不会与本机开发的应用程序无缝集成，并且可能会降低速度和性能下降。 此外，用于构建跨平台应用程序的工具可能不具备每个不同设备平台提供的所有功能，这可能需要解决方法。

基本**代码通常**由**UI 代码**组成，用于创建用于调用 web 服务、访问数据库、调用硬件功能和管理状态的用户界面，如页面、按钮控件、标签、列表等。 通常，90% 的可重复使用，不过，通常需要为每个设备平台自定义代码。 这种通用化很大程度上取决于您要生成的应用程序类型，但它提供了一个可帮助您做出决策的上下文。  

## <a name="choosing-a-cross-platform-framework"></a>选择跨平台框架

[Xamarin Native （Xamarin）](xamarin-android.md)

- UI 代码： XML Android Designer 和材料主题
- 逻辑代码： c # 或 F#
- 仍能够利用一些本机 Android 元素，但对于其他平台（iOS、Windows），更适合重用基本代码。
- 仅逻辑代码在平台之间共享，而不是 UI 代码。
- 适用于具有特定于设备的用户界面的更复杂的应用。

[Xamarin 窗体（Xamarin）](xamarin-forms.md)

- UI 代码： XAML 和 .NET （带有 Visual Studio）
- 逻辑代码： C#
- 跨 Android、iOS 和 Windows 设备应用共60到90% 的逻辑和 UI 代码。 
- 使用常见用户控件，如按钮、标签、条目、ListView、StackLayout、Calendar、TabbedPage 等。创建一个按钮，Xamarin 窗体将找出如何使用绑定库调用 c # 中的 Java 或 Swift 代码，来调用每个平台的本机按钮。
- 适用于简单应用，如内部或业务线（LOB）应用、原型或 Mvp。 使用简单的用户界面，可以看起来有点标准或一般的任何应用。

[响应本机](react-native.md)

- UI 代码： JavaScript
- 逻辑代码： JavaScript
- 响应本机的目标不是编写代码一次并在任何平台上运行，而是在任何平台上运行，而不是一次（响应方式）和写入任意位置。
- 社区添加了一些工具（例如，展览和 Create 反应本机应用程序），帮助他们在不使用 Xcode 或 Android Studio 的情况下生成应用程序。
- 类似于 Xamarin （c #），响应本机（JavaScript）调用本机 UI 元素（无需编写 Java/Kotlin 或 Swift）。

[渐进式 Web 应用 (PWA)](pwa.md)

- UI 代码： HTML、CSS、JavaScript
- 逻辑代码： JavaScript
- Pwa 是使用标准模式构建的 web 应用，可让他们利用 web 和本机应用功能。 它们可以在不使用框架的情况下生成，但要考虑的几个常见框架是[Ionic](https://ionicframework.com/docs/intro)和[PhoneGap](https://phonegap.com/about/)。
- Pwa 可以安装在设备（Android、iOS 或 Windows）上，并且可通过将服务辅助角色合并而脱机工作。
- Pwa 可以在不使用应用商店的情况下使用 web URL 分发和安装。 Microsoft Store 和 Google Play 商店允许列出 Pwa，但 Apple Store 当前不支持，但它们仍可安装在运行12.2 或更高版本的任何 iOS 设备上。
- 若要了解详细信息，请参阅 Pwa on MDN[简介](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Introduction)。

## <a name="game-development"></a>游戏开发

Android 游戏开发通常是开发标准 Android 应用程序所特有的，因为游戏通常使用自定义呈现逻辑，通常用 OpenGL 或 Vulkan 编写。 出于此原因，和由于提供支持游戏开发的许多 C 库，开发人员通常使用[带有 Visual Studio 的 c/c + +](https://docs.microsoft.com/cpp/cross-platform/?view=vs-2019)和 Android[本机开发工具包（NDK）](https://docs.microsoft.com/cpp/cross-platform/create-an-android-native-activity-app?view=vs-2019)来创建适用于 android 的游戏。 [开始将 c/c + + 用于游戏开发](native-android.md#use-c-or-c-for-android-game-development)。

开发适用于 Android 的游戏的另一个常见途径是使用游戏引擎。 提供了许多可用的开源引擎，例如[Unity With Visual Studio](https://docs.microsoft.com/visualstudio/cross-platform/visual-studio-tools-for-unity?view=vs-2019)、 [Unreal Engine](https://docs.unrealengine.com/en-US/Platforms/Mobile/Android/GettingStarted/index.html)、 [MonoGame With xamarin](https://docs.microsoft.com/xamarin/graphics-games/monogame/introduction/)、 [UrhoSharp With xamarin](https://docs.microsoft.com/xamarin/graphics-games/urhosharp/introduction)、 [SkiaSharp with Xamarin。 Forms](https://docs.microsoft.com/xamarin/xamarin-forms/user-interface/graphics/skiasharp/) CocoonJS、应用游戏工具包、合成、Corona SDK、科科二维等。

## <a name="next-steps"></a>后续步骤

- [Windows 上的本机 Android 开发入门](native-android.md)
- [使用 Xamarin 进行 Android 开发入门](xamarin-android.md)
- [使用 Xamarin 进行 Android 开发入门](xamarin-forms.md)
- [开始使用响应本机开发 Android](react-native.md)
- [开始为 Android 开发 PWA](pwa.md)
- [开发适用于 Android 的双屏幕应用并获取 Surface 双核设备 SDK](https://docs.microsoft.com/dual-screen/android/)
- [添加 Windows Defender 排除项以提高性能](defender-settings.md)
- [启用虚拟化支持以提高模拟器性能](emulator.md#enable-virtualization-support)
