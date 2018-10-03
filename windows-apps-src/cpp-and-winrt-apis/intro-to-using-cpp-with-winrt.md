---
author: stevewhims
description: 对 C++/WinRT（一种适用于 Windows 运行时 API 的标准 C++ 语言投影）的介绍。
title: C++/WinRT 简介
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 简介
ms.localizationpriority: medium
ms.openlocfilehash: 220c5c7395ed9388b02b74e0cbed5b913971bbba
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/02/2018
ms.locfileid: "4259606"
---
# <a name="introduction-to-cwinrt"></a>C++/WinRT 简介
&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT 是 Windows 运行时 (WinRT) API 的完全标准新式 C++17 语言投影，以基于标头文件的库的形式实现，旨在为你提供对新式 Windows API 的一流访问。 利用 C++/WinRT，你可以采用任何符合标准的 C++17 编译器创作和使用 Windows 运行时 API。 Windows SDK 包含 C++/WinRT；它已在版本 10.0.17134.0（Windows 10，版本 1803）中引用。

C + + /winrt 是 Microsoft 的建议的替换[C + + CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live)语言投影，以及[Windows 运行时 c + + 模板库 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live)。 完整列表[主题有关 C + + WinRT](index.md#topics-about-cwinrt)包括有关，与之和从移植，C + + /CX 和 WRL。

> [!IMPORTANT]
> C++/WinRT 的最需要注意的其中两个部分在[针对 C++/WinRT 的 SDK 支持](#sdk-support-for-cwinrt)和[针对 C++/WinRT 以及 VSIX 的 Visual Studio 支持](#visual-studio-support-for-cwinrt-and-the-vsix)章节中进行了说明。

## <a name="language-projections"></a>语言投影
Windows 运行时基于组件对象模型 (COM) API，可通过*语言投影* 访问它。 投影将隐藏 COM 详细信息，并为给定语言提供更自然的编程体验。

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Windows UWP API 引用内容中的 C++/WinRT 语言投影
当你浏览 [Windows UWP API](https://docs.microsoft.com/uwp/api/) 时，请单击右上角的**语言**组合框，然后选择 **C++/WinRT** 以查看 API 语法块（当它们在 C++/WinRT 语言投影中出现时）。

## <a name="sdk-support-for-cwinrt"></a>对 C++/WinRT 的 SDK 支持
从版本 10.0.17134.0（Windows 10 版本 1803）起，Windows SDK 包含基于标头文件的标准 C++ 库以用于使用第一方 Windows API（Windows 命名空间中的 Windows 运行时 API）。 C++/WinRT 还附带了 `cppwinrt.exe` 工具，你可以在 Windows 运行时元数据 (`.winmd`) 文件指向该工具来生成基于标头文件的*投影* API 的标准 C++ 库，如从 C++/WinRT 代码使用元数据中所述。 Windows 运行时元数据 (`.winmd`) 文件提供了描述 Windows 运行时 API 图面的 Canonical 方法。 通过在元数据指向 `cppwinrt.exe`，在中，你可以生成使用在第二方或第三方 Windows 运行时组件中实现的或在你自己的应用程序中实现的任何运行时类的库。 有关详细信息，请参阅[通过 C++/WinRT 使用 API](consume-apis.md)。

通过 C++/WinRT，你还可以使用标准 C++ 实现自己的运行时类，而不必求助于 COM 样式的编程。 对于运行时类，你只需在 IDL 文件中描述类型，`midl.exe` 和 `cppwinrt.exe` 将为你生成实现样板源代码文件。 你也可以通过从 C++/WinRT 基类派生来只实现接口。 有关详细信息，请参阅[通过 C++/WinRT 创作 API](author-apis.md)。

## <a name="visual-studio-support-for-cwinrt-and-the-vsix"></a>针对 C++/WinRT 以及 VSIX 的 Visual Studio 支持
对于 Visual Studio 中的 C++/WinRT 项目模板以及 C++/WinRT MSBuild 属性和目标，请从 [Visual Studio Marketplace](https://marketplace.visualstudio.com/) 中下载并安装 [C++/WinRT Visual Studio 扩展 (VSIX)](https://aka.ms/cppwinrt/vsix)。

你将需要 Visual Studio 2017（至少是版本 15.6；我们建议至少是 15.7），以及 Windows SDK 版本 10.0.17134.0（Windows 10 版本 1803）。 如果你尚未安装它，你将需要安装 Visual Studio 安装程序内的从**c + + 通用 Windows 平台工具**选项。 并在 Windows**设置** > **更新 \ & 安全** > **适用于开发人员**，选择**开发人员模式**选项，而不是**旁加载应用**选项。

然后将能够创建和生成、 或打开，C + + WinRT 项目在 Visual Studio 中，然后对其进行部署。 或者，也可以通过添加转换现有项目`<CppWinRTEnabled>true</CppWinRTEnabled>`属性及其`.vcxproj`文件。

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

添加该属性后，你将获得对该项目的 C++/WinRT MSBuild 支持，包括调用 `cppwinrt.exe`工具。

因为 C + + /winrt 使用 C + + 17 标准的功能，它必须项目属性**C/c + +** > **语言** > **标准 c + + 语言** > **ISO C + + 17 标准 (/ std:c + + 17)**。 你可能还希望设置**合规模式: 是(/permissive-)**，它将进一步约束代码以符合标准。

要注意的另一个项目属性是 **C/C++** > **常规** > **将警告视为错误**。 请根据喜好将此项设置为**是(/WX)** 或**否(/WX-)**。 有时候，由 `cppwinrt.exe` 工具生成的源文件会生成警告，除非向其添加实现。

VSIX 还为你提供 C++/WinRT 投影类型的 Visual Studio 本机调试可视化效果 (natvis)；提供与 C# 调试类似的体验。 Natvis 对于调试版本是自动的。 你可以通过定义符号 WINRT_NATVIS 选择加入到该发布版本。

下面是由 VSIX 提供的 Visual Studio 项目模板。

### <a name="windows-console-application-cwinrt"></a>Windows 控制台应用程序 (C++/WinRT)
适用于 Windows 桌面版的 C++/WinRT 客户端应用程序的项目模板，具有控制台用户界面。

### <a name="blank-app-cwinrt"></a>空白应用 (C++/WinRT)
通用 Windows 平台 (UWP) 应用的项目模板，具有 XAML 用户界面。

Visual Studio 提供了 XAML 编译器支持，用于从位于每个 XAML 标记文件后面的接口定义语言 (IDL) (`.idl`) 文件生成实现和标头存根。 在 IDL 文件中，定义你要在应用的 XAML 页面中引用的任何本地运行时类，然后构建项目一次以在 `Generated Files` 中生成实现模板并在 `Generated Files\sources` 中生成存根类型定义。 然后使用这些存根类型定义作为参考以实现本地运行时类。 我们建议在每个运行时类自己的 IDL 文件中声明这些类。

### <a name="core-app-cwinrt"></a>核心应用 (C++/WinRT)
通用 Windows 平台 (UWP) 应用的项目模板，不使用 XAML。

相反，它使用 Windows.ApplicationModel.Core 命名空间的 C++/WinRT Windows 命名空间标头。 构建并运行后，单击空白空间以添加彩色方块；然后单击彩色方块以拖动它。

### <a name="windows-runtime-component-cwinrt"></a>Windows 运行时组件 (C++/WinRT)
组件的项目模板；通常针对从通用 Windows 平台 (UWP) 的使用。

此模板演示了 `midl.exe` > `cppwinrt.exe` 工具链，其中 Windows 运行时元数据 (`.winmd`) 从 IDL 生成，然后实现和标头存根从 Windows 运行时元数据生成。

在 IDL 文件中，在组件、组件的默认接口和组件实现的任何其他接口中定义运行时类。 构建项目一次以生成 `module.g.cpp`、`module.h.cpp`、`Generated Files` 中的实现模板和 `Generated Files\sources` 中的存根类型定义。 然后使用这些存根类型定义作为参考以在组件中实现运行时类。 我们建议在每个运行时类自己的 IDL 文件中声明这些类。

将生成的 Windows 运行时组件二进制文件及其 `.winmd` 与使用它们的 UWP 应用绑定。

## <a name="custom-types-in-the-cwinrt-projection"></a>C++/WinRT 投影中的自定义类型
在 C++/WinRT 编程中，你可以在 C++/WinRT 编程中使用标准 C++ 语言功能以及[标准 C++ 数据类型和 C++/WinRT](std-cpp-data-types.md)&mdash;（包括一些 C++ 标准库数据类型）。 但你还会在投影中发现一些自定义数据类型，并且可以选择使用它们。 例如，我们使用 [C++/WinRT 入门](get-started.md)中快速入门代码示例中的 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)。

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array) 是你可能在某个时间使用的另一个类型。 但你不太可能直接使用 [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) 之类的类型。 或者，你可能选择不使用它，以便在等效类型出现在 C++ 标准库中时不用更改任何代码。

> [!WARNING]
> 如果你仔细研究 C++/WinRT Windows 命名空间标头，你可能也会发现一些类型。 一个示例是 **winrt::param::hstring**，另外还有一些示例。 这些单纯用于优化输入参数的绑定，他们实现了显著的性能改进，并使相大多数调用模式“只”为相关的标准 C++ 类型和容器工作。 在增加最大价值的情况下，这些类型只被投影使用过。 它们被高度优化且不用于一般用途；请不要尝试自行使用。 也不应该使用来自 `winrt::impl` 命名空间的任何内容，因为这些属于实现类型，因此可能更改。 你应该继续使用标准类型，或来自 [winrt 命名空间](/uwp/cpp-ref-for-winrt/winrt)的类型。

## <a name="important-apis"></a>重要的 API
* [winrt::hstring 结构](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 命名空间](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相关主题
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT Visual Studio 扩展 (VSIX)](https://aka.ms/cppwinrt/vsix)
* [C++/WinRT 入门](get-started.md)
* [标准 C++ 数据类型和 C++/WinRT](std-cpp-data-types.md)
* [C++/WinRT 中的字符串处理](strings.md)
* [Windows UWP API](https://docs.microsoft.com/uwp/api/)
