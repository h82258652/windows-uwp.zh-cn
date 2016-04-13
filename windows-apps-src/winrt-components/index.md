---
Windows 运行时组件
Windows 运行时组件是自包含对象，可将其实例化，并可采用任一语言使用它，包括 C#、Visual Basic、JavaScript 和 C++。
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
---

# Windows 运行时组件


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Windows 运行时组件是自包含对象，可将其实例化，并可采用任一语言使用它，包括 C#、Visual Basic、JavaScript 和 C++。

你可以使用 Visual Studio 和 C#、Visual Basic 或 C++ 创建可用于通用 Windows 平台 (UWP) 应用中的 Windows 运行时组件。

| 主题 | 说明 |
|-------|-------------|
| [在 C++ 中创建 Windows 运行时组件](creating-windows-runtime-components-in-cpp.md) | 本文介绍如何使用 C++ 创建 Windows 运行时组件，该组件是从使用 JavaScript、C#、Visual Basic 或 C++ 生成的通用 Windows 应用调用的 DLL。 |
| [演练：使用 C++ 创建一个基本 Windows 运行时组件并从 JavaScript 或 C 中调用此组件#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | 本演练演示了如何创建一个可从 JavaScript、C# 或 Visual Basic 调用的基本 Windows 运行时组件 DLL。 在开始本演练之前，请确保你已了解抽象二进制接口 (ABI)、ref 类以及简化使用 ref 类的 Visual C++ 组件扩展等概念。 有关详细信息，请参阅[使用 C++ 创建 Windows 运行时组件](creating-windows-runtime-components-in-cpp.md)和 [Visual C++ 语言参考 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx)。 |
| [使用 C# 和 Visual Basic 创建 Windows 运行时组件](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | 从 .NET Framework 4.5 开始，你可以使用托管代码创建自己的在 Windows 运行时组件中打包的 Windows 运行时类型。 你可以将通用 Windows 平台 (UWP) 应用与 C++、JavaScript、Visual Basic 或 C# 一起使用。 本文概述了用于创建组件的规则，并讨论了有关 Windows 运行时的 .NET Framework 支持的一些方面。 一般情况下，该支持设计为对 .NET Framework 程序员透明可见。 但是，在你创建要与 JavaScript 或 C++ 一起使用的组件时，需要意识到这些语言支持 Windows 运行时的方法差异。 |
| [演练：创建简单的 Windows 运行时组件并通过 JavaScript 调用它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | 本演练演示了如何将 .NET Framework 与 Visual Basic 或 C# 结合使用来创建自己的 Windows 运行时类型（打包在 Windows 运行时组件中），以及如何调用为使用 JavaScript 的 Windows 生成的通用 Windows 应用中的组件。 |
| [在 Windows 运行时组件中引发事件](raising-events-in-windows-runtime-components.md) | 如果你的 Windows 运行时组件在后台线程（工作线程）中引发了用户定义的委派类型的事件，并且你希望 JavaScript 能够接收该事件，则可以使用以下方法之一实现和/或引发它： |
 

 

 





<!--HONumber=Mar16_HO1-->


