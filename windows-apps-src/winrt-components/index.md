---
title: Windows 运行时组件
description: Windows 运行时组件是自包含对象，可将其实例化，并可采用任一语言使用它，包括 C#、Visual Basic、JavaScript 和 C++。
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: db49dfc613b6f7ca2afdb6cc7bdb6b6407751402
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493502"
---
# <a name="windows-runtime-components"></a>Windows 运行时组件

Windows 运行时组件是自包含软件模块，你可以采用任何 Windows 运行时语言（包括 C#、C++/WinRT、Visual Basic、JavaScript 和 C++/CX）创作、引用并使用它。 可以使用 Visual Studio 创建可在通用 Windows 平台 (UWP) 应用中使用的 Windows 运行时组件。

> [!NOTE]
> 对于 C++ 开发人员，我们建议你使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 编写新应用程序。 C++/WinRT 是 Windows 运行时 (WinRT) API 的完全标准新式 C++17 语言投影，以基于标头文件的库的形式实现，旨在为你提供对新式 Windows API 的一流访问。 若要了解如何使用 C++/WinRT 创建 Windows 运行时组件，请参阅[使用 C++/WinRT 创建 Windows 运行时组件](/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt)。

| 主题 | 说明 |
|-------|-------------|
| [使用 C++/WinRT 创建 Windows 运行时组件](/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt) | 本主题展示了如何使用 C++/WinRT 创建和使用 Windows 运行时组件 &mdash; 一种可以从使用任何 Windows 运行时语言构建的通用 Windows 应用调用的组件。 |
| [使用 C++/CX 创建 Windows 运行时组件](creating-windows-runtime-components-in-cpp.md) | 本主题展示了如何使用 C++/CX 创建 Windows 运行时组件 &mdash; 一种可以从使用任何 Windows 运行时语言构建的通用 Windows 应用调用的组件。 |
| [创建 C++/CX Windows 运行时组件并通过 JavaScript 或 C# 调用此组件的演练](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | 本演练演示了如何创建一个可通过 JavaScript、C# 或 Visual Basic 调用的基本 Windows 运行时组件 DLL。 在开始本演练之前，请确保你已了解抽象二进制接口 (ABI)、ref 类以及简化使用 ref 类的 Visual C++ 组件扩展等概念。 有关详细信息，请参阅[使用 C++ 创建 Windows 运行时组件](creating-windows-runtime-components-in-cpp.md)和 [Visual C++ 语言参考 (C++/CX)](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx)。 |
| [使用 C# 和 Visual Basic 创建 Windows 运行时组件](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | 可以使用托管代码创建自己的 Windows 运行时类型（打包在 Windows 运行时组件中）。 你可以将通用 Windows 平台 (UWP) 应用与 C++、JavaScript、Visual Basic 或 C# 一起使用。 本主题概述了用于创建组件的规则，并讨论了有关 Windows 运行时的 .NET 支持的一些方面。 一般情况下，该支持设计为对 .NET 程序员透明可见。 但是，在你创建要与 JavaScript 或 C++ 一起使用的组件时，需要意识到这些语言支持 Windows 运行时的方法差异。 |
| [创建 C# 或 Visual Basic Windows 运行时组件并通过 JavaScript 调用此组件的演练](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | 本演练演示了如何将 .NET 与 Visual Basic 或 C# 结合使用来创建自己的 Windows 运行时类型（打包在 Windows 运行时组件中），以及如何使用 JavaScript 从为 Windows 构建的通用 Windows 应用中调用该组件。 |
| [在 Windows 运行时组件中引发事件](raising-events-in-windows-runtime-components.md) | 如果你的 Windows 运行时组件在后台线程（工作线程）中引发了用户定义的委托类型的事件，并且你希望 JavaScript 能够接收该事件，则可以使用以下方法之一实现和/或引发它： | 
| [适用于旁加载 UWP 应用的中转 Windows 运行时组件](brokered-windows-runtime-components-for-side-loaded-windows-store-apps.md) | 本主题讨论 Windows 10 更新和更高版本支持的面向企业的功能，此功能允许触摸友好的 .NET 应用使用对主要关键业务操作负责的现有代码。 |
