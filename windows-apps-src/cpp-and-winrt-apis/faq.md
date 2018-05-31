---
author: stevewhims
description: 对你可能有的关于通过 C++/WinRT 创作和使用 Windows 运行时 API 的问题的解答。
title: C++/WinRT 常见问题
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 频繁, 问的, 问题, 常见问题
ms.localizationpriority: medium
ms.openlocfilehash: aad5c5ed2123af39ebb6aff0c9098586ce958196
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832031"
---
# <a name="frequently-asked-questions-about-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 常见问题
> [!NOTE]
> **与在商业发行之前可能会进行实质性修改的预发布产品相关的一些信息。 Microsoft 对于此处提供的信息不作任何明示或默示的担保。**

对你可能有的关于通过 C++/WinRT 创作和使用 Windows 运行时 API 的问题的解答。

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsixhttpsakamscppwinrtvsix"></a>C++/WinRT [Visual Studio 扩展 (VSIX)](https://aka.ms/cppwinrt/vsix) 的要求是什么？
[VSIX](https://aka.ms/cppwinrt/vsix) 强制执行最低的 Windows SDK 目标版本 10.0.17134.0（Windows 10，版本 1803）。 你还将需要 Visual Studio 2017 版本 15.6 或更高版本。 你可以通过 `.vcxproj` 文件 `<PropertyGroup Label="Globals">` 中 `<CppWinRTEnabled>true</CppWinRTEnabled>` 的存在来识别使用 VSIX 的项目。 有关详细信息，请参阅 [C++/WinRT 的 Visual Studio 支持和 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

## <a name="whats-a-runtime-class"></a>什么是*运行时类*？
运行时类是一个可通过现代 COM 接口进行激活和使用（通常跨可执行文件）的类型。 但是，运行时类也可在实现它的编译单元内使用。 你采用接口定义语言 (IDL) 声明运行时类，而且可以在标准 C++ 中使用 C++/WinRT 实现它。

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>*投影类型*和*实现类型*是什么意思？
如果你仅*使用* Windows 运行时类（运行时类），则将要专门处理*投影类型*。 C++/WinRT 是一种*语言投影*，所以投影类型是通过 C++/WinRT *投影*到 C++ 中的 Windows 运行时的表面的一部分。 有关更多详细信息，请参阅[通过 C++/WinRT 使用 API](consume-apis.md)。

*实现类型*包含运行时类的实现，因此仅在实现该运行时类的项目中可用。 当你在实现运行时类的项目中工作时（Windows 运行时组件项目或使用 XAML UI 的项目），请务必熟悉你对某个运行时类的实现类型与表示该运行时类已投影到 C++/WinRT 中的投影类型之间的区别。 有关更多详细信息，请参阅[通过 C++/WinRT 创作 API](author-apis.md)。

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>我是否应实现 [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable)，如果是，该怎么实现？
如果你有在其构造函数中释放资源的运行时类，而且有旨在从其实现编译单元外部所使用的运行时类（即适用于 Windows 运行时客户端应用的一般使用的 Windows 运行时组件），则我们建议你还要实现 **IClosable**，以支持缺乏确定性终止化的语言对运行时类的使用。 确保资源得到释放，无论调用的是析构函数 [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.Close) 还是两者。 可调用 **IClosable::Close** 任意次数。

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosablewindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>我是否需要对所使用的运行时类调用 [**IClosable::Close**](/uwp/api/windows.foundation.iclosable#Windows_Foundation_IClosable_Close_)？
**IClosable** 存在以支持缺乏确定性终止化的语言。 所以，你不应通过 C++/WinRT 调用 **IClosable::Close**，涉及关闭竞争或半死锁的极少数情况除外。 例如，如果你使用的是 **Windows.UI.Composition** 类型，则你可能会遇到按照设定的顺序释放对象的情况，作为允许 C++/WinRT 包装器的析构为你执行该工作的替代。

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>我是否需要采用我的运行时类的 IDL 声明构造函数？
仅当运行时类设计为从其实现编译单元外部进行使用时（即适用于 Windows 运行时客户端应用的一般使用的 Windows 运行时组件）。 有关采用 IDL 声明构造函数的目的和结果的完整详细信息，请参阅[运行时类构造函数](author-apis.md#runtime-class-constructors)。