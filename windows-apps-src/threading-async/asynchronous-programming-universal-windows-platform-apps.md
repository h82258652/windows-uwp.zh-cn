---
author: TylerMSFT
ms.assetid: 23FE28F1-89C5-4A17-A732-A722648F9C5E
title: "异步编程"
description: "本主题介绍通用 Windows 平台 (UWP) 中的异步编程及其在 C#、Microsoft Visual Basic .NET、Visual C\\+\\+ 组件扩展 (C\\+\\+/CX) 和 JavaScript 中的表示形式。"
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: c033c1d985b9373d9cbadf38463610aa1922163e

---
# 异步编程

\[ 已针对 Windows 10 上的 UWP 应用更新 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本主题介绍通用 Windows 平台 (UWP) 中的异步编程及其在 C#、Microsoft Visual Basic .NET、Visual C++ 组件扩展 (C++/CX) 和 JavaScript 中的表示形式。

使用异步编程可帮助应用在完成可能花费较长时间的工作时保持响应。 例如，从 Internet 下载内容的应用等待内容到达可能要花费数秒钟。 如果你已在 UI 线程中使用同步方法来检索内容，则应用会在方法返回之前被阻止。 应用将不会响应用户交互，而且因为无响应的原因，用户可能会感到沮丧。 使用异步编程效果更佳。采用此方式时，应用在等待操作完成时继续运行并响应 UI。

对于可能花费较长时间才能完成的方法，异步编程在 UWP 中是规范，而不是特例。 JavaScript、C#、Visual Basic 和 C++/CX 各自都为异步方法提供了语言支持。

## UWP 中的异步编程

许多 UWP 功能（如 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/BR241124) API 和 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) API）都公开为异步 API。 按照惯例，异步 API 的名称以“Async”结尾，表示在调用该 API 后可能会发生其部分执行。

当在通用 Windows 平台 (UWP) 应用中使用异步 API 时，代码将以一致的方式进行非阻止调用。 当在自己的 API 定义中实现这些异步模式后，调用方可理解并按照可预知的方式使用你的代码。

下面是一些需要调用异步 UWP API 的常见任务。

-   显示消息对话框

-   使用文件系统，显示文件选取器

-   向 Internet 发送数据和从 Internet 接收数据

-   使用套接字、流、连接性

-   使用约会、联系人、日历

-   使用文件类型，例如打开可移植文档格式 (PDF) 文件，或者解码图像或媒体格式

-   与设备或服务交互

借助 UWP 异步模式，你可以从根本上避免显式管理线程。 每种编程语言都按照其自己的方式支持用于 UWP 的异步模式：

| 编程语言 | 异步表示形式           |
|----------------------|---------------------------------------|
| C#                  | **async** 关键字、**await** 运算符 |
| Visual Basic         | **Async** 关键字、**Await** 运算符 |
| C++/CX               | **task** 类，**.then** 方法      |
| JavaScript           | 承诺对象，**then** 函数     |

 

## 使用 C# 和 Visual Basic 的 UWP 中的异步模式


用 C# 或 Visual Basic 编写的典型代码段将同步执行，这意味着当执行一行时，它会在下一行执行之前完成。 尽管之前已有用于异步执行的 Microsoft .NET 编程模型，但生成的代码趋向于强调执行异步代码的技术，而不是专注于代码要尝试完成的任务。 UWP、.NET Framework 以及 C# 和 Visual Basic 编译器已添加了可从代码中提取异步机制的功能。 对于 .NET 和 UWP，可编写异步代码，该代码关注于你的代码要做什么，而不是如何做以及何时做。 你的异步代码看起来将非常类似于同步代码。 有关详细信息，请参阅[使用 C# 或 Visual Basic 调用异步 API](call-asynchronous-apis-in-csharp-or-visual-basic.md)。

## 使用 C++ 的 UWP 中的异步模式


在 C++/CX 中， 异步编程基于 [**task class**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750113.aspx) 及其 [**then method**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750044.aspx)。 语法与 JavaScript 承诺的语法类似。 **task 类**及其相关类型还提供取消和管理线程上下文的功能。 有关详细信息，请参阅[使用 C++ 进行异步编程](asynchronous-programming-in-cpp-universal-windows-platform-apps.md)。

[**create\_async 函数**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750102.aspx)提供了相应支持，以便生成可以通过 JavaScript 或任何其他支持 UWP 的语言使用的异步 API。 有关详细信息，请参阅[使用 C++ 创建异步操作](https://msdn.microsoft.com/library/windows/apps/xaml/hh750082.aspx)。

## 使用 JavaScript 的 UWP 中的异步模式

在 JavaScript 中，异步编程通过让异步方法返回承诺对象以遵循 [Common JS Promises/A](http://wiki.commonjs.org/wiki/Promises/A) 建议标准。 承诺可在 UWP 和 Windows JavaScript 库中使用。

承诺对象表示将在未来实现的值。 在 UWP 中，可从工厂函数获取承诺对象，该函数名称按照惯例以“Async”结尾。

在许多情况下，调用异步函数几乎与调用传统函数一样简单。 区别在于使用 [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) 或 [**done**](https://msdn.microsoft.com/library/windows/apps/Hh701079) 方法来指定结果或错误的处理程序以及启动操作。

## 相关主题

* [使用 C# 或 Visual Basic 调用异步 API](call-asynchronous-apis-in-csharp-or-visual-basic.md)
* [使用 Async 和 Await（C# 和 Visual Basic）进行异步编程](http://msdn.microsoft.com/library/hh191443(vs.110).aspx)
* [Reversi 示例功能方案：异步代码](https://msdn.microsoft.com/library/windows/apps/xaml/jj712233.aspx#async)




<!--HONumber=Aug16_HO3-->


