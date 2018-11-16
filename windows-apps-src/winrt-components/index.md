---
author: msatranjr
title: Windows 运行时组件
description: Windows 运行时组件是自包含对象，可将其实例化，并可采用任一语言使用它，包括 C#、Visual Basic、JavaScript 和 C++。
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b93b3028c7968c417d4476a6f183805cdec797b0
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6985118"
---
# <a name="windows-runtime-components"></a>Windows 运行时组件
Windows 运行时组件是自包含对象，可将其实例化，并可采用任一语言使用它，包括 C#、Visual Basic、JavaScript 和 C++。

你可以使用 Visual Studio 和 C#、Visual Basic 或 C++ 创建可用于通用 Windows 平台 (UWP) 应用中的 Windows 运行时组件。

| 主题 | 说明 |
|-------|-------------|
| [在 C++ 中创建 Windows 运行时组件](creating-windows-runtime-components-in-cpp.md) | 本主题介绍了如何使用 C + + /CX 创建一个 Windows 运行时组件，它是一个组件，可通过使用 C#、 Visual Basic、 c + + 或 Javascript 生成的通用 Windows 应用调用。 |
| [演练：用 C++ 创建一个基本的 Windows 运行时组件，然后从 JavaScript 或 C# 中调用该组件](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | 本演练演示如何创建一个可从 JavaScript、C# 或 Visual Basic 调用的基本 Windows 运行时组件 DLL。 在开始本演练之前，请确保你已了解抽象二进制接口 (ABI)、ref 类以及简化使用 ref 类的 Visual C++ 组件扩展等概念。 有关详细信息，请参阅[使用 C++ 创建 Windows 运行时组件](creating-windows-runtime-components-in-cpp.md)和 [Visual C++ 语言参考 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx)。 |
| [使用 C# 和 Visual Basic 创建 Windows 运行时组件](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | 从 .NET Framework 4.5 开始，你可以使用托管代码创建自己的在 Windows 运行时组件中打包的 Windows 运行时类型。 你可以将通用 Windows 平台 (UWP) 应用与 C++、JavaScript、Visual Basic 或 C# 一起使用。 本主题概述了用于创建组件，规则，并讨论了.NET Framework 支持的 Windows 运行时的某些方面。 一般情况下，该支持设计为对 .NET Framework 程序员透明可见。 但是，在你创建要与 JavaScript 或 C++ 一起使用的组件时，需要意识到这些语言支持 Windows 运行时的方法差异。 |
| [演练：创建简单的 Windows 运行时组件并通过 JavaScript 调用它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | 本演练演示了如何将 .NET Framework 与 Visual Basic 或 C# 结合使用来创建自己的 Windows 运行时类型（打包在 Windows 运行时组件中），以及如何调用为使用 JavaScript 的 Windows 生成的通用 Windows 应用中的组件。 |
| [在 Windows 运行时组件中引发事件](raising-events-in-windows-runtime-components.md) | 如果你的 Windows 运行时组件在后台线程（工作线程）中引发了用户定义的委托类型的事件，并且你希望 JavaScript 能够接收该事件，则可以使用以下方法之一实现和/或引发它： | 
| [适用于旁加载 UWP 应用的中转 Windows 运行时组件](brokered-windows-runtime-components-for-side-loaded-windows-store-apps.md) | 本主题讨论 Windows 10 更新和更高版本支持的面向企业的功能，此功能允许触摸友好的 .NET 应用使用对主要关键业务操作负责的现有代码。 |
