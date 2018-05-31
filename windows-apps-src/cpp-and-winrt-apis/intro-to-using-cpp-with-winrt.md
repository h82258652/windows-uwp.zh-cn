---
author: stevewhims
description: 对 C++/WinRT（一种适用于 Windows 运行时 API 的标准 C++ 语言投影）的介绍。
title: C++/WinRT 简介
ms.author: stwhi
ms.date: 05/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影
ms.localizationpriority: medium
ms.openlocfilehash: 968afd6fdad1e7bf6b3c38d929ab79eefa71819a
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832321"
---
# <a name="introduction-to-cwinrt"></a>C++/WinRT 简介
> [!NOTE]
> **与在商业发行之前可能会进行实质性修改的预发布产品相关的一些信息。 Microsoft 对于此处提供的信息不作任何明示或默示的担保。**

Windows SDK 在版本 10.0.17134.0（Windows 10，版本 1803）中引用，现在包含 C++/WinRT。

> [!IMPORTANT]
> C++/WinRT 的最需要注意的部分在[对 C++/WinRT 的 SDK 支持](#sdk-support-for-cwinrt)和[对 C++/WinRT 的 Visual Studio 支持以及 VSIX](#visual-studio-support-for-cwinrt-and-the-vsix) 章节中进行了说明。

C++/WinRT 是 Windows 运行时 (WinRT) API 的完全标准新式 C++17 语言投影，仅在标头文件中实现，旨在为你提供对新式 Windows API 的一流访问。 利用 C++/WinRT，你可以采用任何符合标准的 C++17 编译器创作和使用 Windows 运行时 API。

## <a name="language-projections"></a>语言投影
Windows 运行时基于组件对象模型 (COM) API，可通过*语言投影* 访问它。 投影将隐藏 COM 详细信息，并为给定语言提供更自然的编程体验。

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Windows UWP API 引用内容中的 C++/WinRT 语言投影
当你浏览 [Windows UWP API](https://docs.microsoft.com/uwp/api/) 时，请单击右上角的**语言**组合框，然后选择 **C++/WinRT** 以查看 API 语法块（当它们在 C++/WinRT 语言投影中出现时）。

## <a name="sdk-support-for-cwinrt"></a>对 C++/WinRT 的 SDK 支持
从版本 10.0.17134.0（Windows 10，版本 1803）开始，Windows SDK 包含 C++/WinRT 投影 Windows 命名空间标头和工具。 `cppwinrt.exe` 是一种重要工具，它从 `.winmd` 生成将元数据投影到 C++/WinRT 中的源代码文件。 Windows 运行时元数据 (`.winmd`) 文件提供了描述 Windows 运行时 API 图面的 Canonical 方法。 从 Windows 运行时元数据，`cppwinrt.exe` 生成了一个全面描述或*投影* API 图面的标准 C++ 库；无论它们是 Windows API 还是第三方 Windows 运行时组件 API。 `Cppwinrt.exe` 在针对使用优先 API 和第三方 API 的开发工作流程中扮演着重要角色，对于在组件中创作你自己的 API 也很重要。

## <a name="visual-studio-support-for-cwinrt-and-the-vsix"></a>对 C++/WinRT 的 Visual Studio 支持以及 VSIX
对于 Visual Studio 中的 C++/WinRT 项目模板以及 C++/WinRT MSBuild 属性和目标，请从 [Visual Studio Marketplace](https://marketplace.visualstudio.com/) 中下载并安装 [C++/WinRT Visual Studio 扩展 (VSIX)](https://aka.ms/cppwinrt/vsix)。

你需要 Visual Studio 2017 版本 15.6 或更高版本以及 Windows SDK 版本 10.0.17134.0（Windows 10，版本 1803）。 然后，你可以在 Visual Studio 中创建新项目，也可以通过将 `<CppWinRTProject>true</CppWinRTProject>` 属性添加到其 `.vcxproj` 文件（位于“项目”>“PropertyGroup”内）来转换现有项目。 添加该属性后，你将获得对该项目的 C++/WinRT MSBuild 支持，包括调用 `cppwinrt.exe`工具。

由于 C++/WinRT 使用 C++17 标准版中的功能，它需要项目属性 **C/C++** > **语言** > **ISO C++17 标准版(/std:c++17)**。 你可能还希望设置**合规模式: 是(/permissive-)**，它将进一步约束代码以符合标准。

要注意的另一个项目属性是 **C/C++** > **常规** > **将警告视为错误**。 请根据喜好将此项设置为**是(/WX)** 或**否(/WX-)**。 有时候，由 `cppwinrt.exe` 工具生成的源文件会生成警告，除非向其添加实现。

这些是由 VSIX 提供的 Visual Studio 项目模板。

### <a name="windows-console-application-cwinrt"></a>Windows 控制台应用程序 (C++/WinRT)
适用于 Windows 桌面版的 C++/WinRT 客户端应用程序的项目模板，具有控制台用户界面。

### <a name="blank-app-cwinrt"></a>空白应用 (C++/WinRT)
通用 Windows 平台 (UWP) 应用的项目模板，具有 XAML 用户界面。

Visual Studio 提供了 XAML 编译器支持，用于从位于每个 XAML 标记文件后面的接口定义语言 (IDL) (`.idl`) 文件生成实现和标头存根。 在 IDL 文件中，定义你要在应用的 XAML 页面中引用的任何本地运行时类，然后构建项目一次以在 `Generated Files` 中生成实现模板并在 `Generated Files\sources` 中生成存根类型定义。 然后使用这些存根类型定义作为参考以实现本地运行时类。 我们建议在每个运行时类自己的 IDL 文件中声明这些类。

### <a name="core-app-cwinrt"></a>核心应用 (C++/WinRT)
通用 Windows 平台 (UWP) 应用的项目模板，不使用 XAML。

相反，它使用 Windows.ApplicationModel.Core 命名空间的 C++/WinRT 投影 Windows 命名空间标头。 构建并运行后，单击空白空间以添加彩色方块；然后单击彩色方块以拖动它。

### <a name="windows-runtime-component-cwinrt"></a>Windows 运行时组件 (C++/WinRT)
组件的项目模板；通常针对从通用 Windows 平台 (UWP) 的使用。

此模板演示了 `midl.exe` > `cppwinrt.exe` 工具链，其中 Windows 运行时元数据 (`.winmd`) 从 IDL 生成，然后实现和标头存根从 Windows 运行时元数据生成。

在 IDL 文件中，在组件、组件的默认接口和组件实现的任何其他接口中定义运行时类。 构建项目一次以生成 `module.g.cpp`、`module.h.cpp`、`Generated Files` 中的实现模板和 `Generated Files\sources` 中的存根类型定义。 然后使用这些存根类型定义作为参考以在组件中实现运行时类。 我们建议在每个运行时类自己的 IDL 文件中声明这些类。

将生成的 Windows 运行时组件二进制文件及其 `.winmd` 与使用它们的 UWP 应用绑定。

## <a name="a-cwinrt-quick-start"></a>C++/WinRT 快速入门
创建一个新的 **Windows 控制台应用程序(C++/WinRT)** 项目。 按下面所示编辑 `main.cpp`。

```cppwinrt
// main.cpp

#include "pch.h"
#include <iostream>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

int main()
{
    winrt::init_apartment();

    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
    for (const SyndicationItem syndicationItem : syndicationFeed.Items())
    {
        hstring titleAsHstring = syndicationItem.Title().Text();
        std::wcout << titleAsHstring.c_str() << std::endl;
    }
}
```

包含的标头 `winrt/Windows.Foundation.h` 和 `winrt/Windows.Web.Syndication.h` 位于 SDK 中的 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\` 文件夹内。 Visual Studio 将该路径包含在其 *IncludePath* 宏中。 这些标头包含投影到 C++/WinRT 中的 Windows API。 每当你要来自 Windows 命名空间的类型时，请包含对应的 C++/WinRT 投影 Windows 命名空间标头，如下所示。 `using namespace`指令是可选的，不过这种指令很方便。

所有投影的类型都位于 C++/WinRT 根命名空间 **winrt** 中。 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 和 Windows SDK 都在根命名空间 **Windows** 中声明类型。 这些不同的命名空间可让你按照自己的节奏从 C++/CX 迁移到 C++/WinRT。

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) 是异步 Windows 运行时函数的示例。 该代码示例将接收来自 **RetrieveFeedAsync** 的异步操作对象，然后对该对象调用 **get** 以阻止调用线程并等待结果。 要获得有关并发的详细信息和了解非阻止性技术，请参阅[通过 C++/WinRT 的并发和异步操作](concurrency.md)。

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) 是一个范围，由从 **begin** 和 **end** 函数（或其常量、反向和常量-反向变体）返回的迭代程序定义。 因此，你可以使用基于范围的 `for` 语句或使用 **std::for_each** 模板函数枚举**项目**。

该代码随后将获取源的标题文本以作为 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 对象（请参阅 [C++/WinRT 中的字符串处理](strings.md)）。 然后，系统将通过 **c_str** 输出 **Hstring**，如果你使用过 C++ 标准库中的字符串，则会觉得它很眼熟。

如你所见，C++/WinRT 鼓励使用与类相似的新式 C++ 表达式，例如 `syndicationItem.Title().Text()`。 这是与传统的 COM 编程不同的更简洁的编程风格。 你无需显式初始化 COM（**winrt::init_apartment** 将为你执行）、使用 COM 指针和处理 HRESULT 返回代码。 C++/WinRT 会将错误 HRESULT 转换为异常以实现自然、现代化的编程风格。

## <a name="custom-types-in-the-cwinrt-projection"></a>C++/WinRT 投影中的自定义类型
你可以在 C++/WinRT 编程中使用标准 C++ 语言功能以及[标准 C++ 数据类型和 C++/WinRT](std-cpp-data-types.md)（包括一些 C++ 标准库数据类型）。 但你还会在投影中发现一些自定义数据类型，并且可以选择使用它们。 例如，我们在上述快速入门中使用了 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)。

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array) 是你可能在某个时间使用的另一个类型。 但你不太可能直接使用 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) 之类的类型。 或者，你可能选择不使用它，以便在等效类型出现在 C++ 标准库中时不用更改任何代码。

如果你仔细研究 C++/WinRT 投影 Windows 命名空间标头，你可能也会发现一些类型。 示例：**winrt::param::hstring**。 这些类型的存在只是为了提高效率，你不应在代码中使用它们。

## <a name="important-apis"></a>重要的 API
* [SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed.Items](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [winrt::hstring 结构](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 命名空间](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相关主题
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT 中的字符串处理](strings.md)
* [Visual Studio Marketplace](https://marketplace.visualstudio.com/)
* [Windows UWP API](https://docs.microsoft.com/uwp/api/)
