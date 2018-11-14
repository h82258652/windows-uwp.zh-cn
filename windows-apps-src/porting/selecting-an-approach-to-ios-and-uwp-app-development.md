---
author: stevewhims
description: 在开发跨平台应用时，应该作何选择？
title: 选择一种方法进行 iOS 和 UWP 应用开发
ms.assetid: 5CDAB313-07B7-4A32-A49B-026361DCC853
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c62624e83d1e8334cce711869088817e2f9dc5e0
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6652946"
---
# <a name="selecting-an-approach-to-ios-and-uwp-app-development"></a>选择一种方法进行 iOS 和 UWP 应用开发


在开发跨平台应用时，应该作何选择？

## <a name="whats-the-best-way-to-support-both-ios-and-windows"></a>同时支持 iOS 和 Windows 的最佳方式是什么？

Windows 和 iOS 这二者似乎完全不同，但如果你需要编写支持这两个平台（Android 亦然）的应用，越来越多的工具和技术都可以为你提供很大帮助。 最佳的解决方案取决于你所编写的应用类型以及你是从零开始编写还是移植现有项目。

## <a name="writing-a-new-app"></a>编写新应用

若从零开始，有许多选项可供你随意使用，其中包括：

-   [Xamarin](http://go.microsoft.com/fwlink/p/?LinkID=320484)

    借助 Xamarin，你可以使用 C# 编写应用、使其在 Windows 上运行，也可以创建本机 iOS 应用。 Visual Studio 中内置了对 Xamarin 的支持；只需选择正确的项目类型即可。

-   [Apache Cordova](http://go.microsoft.com/fwlink/p/?LinkID=400439)

    如果 Javascript 和 HTML 更适合你，Apache Cordova (aka PhoneGap) 将会帮助你创建适用于 iOS、Windows 和 Android 的跨平台应用。 Visual Studio 中也内置了此项目类型。

-   游戏引擎

    借助可供你支配的工具（如 [Unity3D](http://go.microsoft.com/fwlink/p/?LinkID=320479) 和 [Unreal Engine](http://go.microsoft.com/fwlink/p/?LinkID=394062)），你可以编写适用于 Windows 和包括 iOS 在内的许多其他平台的 AAA 级质量游戏。 Unity 支持 C# 脚本，而 Unreal 则使用 C++。

-   [MonoGame](http://go.microsoft.com/fwlink/p/?LinkID=320483)

    XNA 的精神继任者。 现在它是开源跨平台框架，这意味着你可以采用 C# 面向各种支持物理引擎以及 2D 与 3D 图形的平台编写应用。

## <a name="adapting-an-existing-app"></a>适应现有应用

使用现有的 iOS 应用，你的选项将受到稍多限制。 但是，一切都肯定不会丢失。

-   [面向 iOS 的 Windows 桥](https://go.microsoft.com/fwlink/p/?LinkId=619014)

    也称为 Project Islandwood，它是一种可以直接将 Xcode 项目导入到 Visual Studio 的工具，现在仍处于开发中。 OBJECTIVE-C 代码可以在 Visual Studio 中生成并调试。 如果你的项目要为图形使用诸如 Cocos 此类的库，你可能会发现这一种快速移植应用的方法。

-   重新调整 C++ 代码的用途。

    如果你的核心业务逻辑采用 C++（而不是 OBJECTIVE-C 或 Swift）编写，通常在你的项目中仅需细微的改动就可以使用此代码。 然后可以使用 XAML 定义 UI（与其他的 Windows 应用一样），并在必要时调用 C++ 代码。

-   [在 Windows 上使用 ANGLE 运行 OpenGL ES](http://go.microsoft.com/fwlink/p/?linkid=618387)

    移植 OpenGL ES 2.0 项目的中间步骤是使用 ANGLE。 ANGLE 通过将 OpenGL ES API 调用平移到 DirectX 11 API 调用，允许你在 Windows 上运行 OpenGL ES 内容。

## <a name="other-cross-platform-authoring-tools"></a>其他跨平台创作工具

-   [GameSalad](http://go.microsoft.com/fwlink/p/?LinkID=320480)

    一个游戏创作环境。

-   [Construct 2]( http://go.microsoft.com/fwlink/p/?LinkID=320481)

    一个游戏创作环境。

-   [Titanium Studio](http://go.microsoft.com/fwlink/p/?LinkID=320482)

    一个跨平台创作环境。

-   [Cocos2D-x](http://go.microsoft.com/fwlink/p/?LinkID=320485)

    一个用于子画面处理和力学建模的跨平台代码库。

-   [Impact.js](http://go.microsoft.com/fwlink/p/?LinkID=320486)

    一个基于 HTML 的游戏库。

-   [Marmalade](http://go.microsoft.com/fwlink/p/?LinkID=320487)

    一个跨平台 SDK。

-   [OpenFL](http://go.microsoft.com/fwlink/p/?LinkID=320488)

    一个跨平台开发工具。

-   [GameMaker](http://go.microsoft.com/fwlink/p/?LinkID=320490)

    一个游戏专用的创作环境。

-   [PlayCanvas](http://go.microsoft.com/fwlink/p/?LinkID=394061)

    一个基于 HTML 的游戏开发工具。

