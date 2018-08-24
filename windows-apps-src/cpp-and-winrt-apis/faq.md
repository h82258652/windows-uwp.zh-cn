---
author: stevewhims
description: 对你可能有的关于通过 C++/WinRT 创作和使用 Windows 运行时 API 的问题的解答。
title: C++/WinRT 常见问题
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 频繁, 问的, 问题, 常见问题
ms.localizationpriority: medium
ms.openlocfilehash: 80c27332c05e285fdad6b8ec8deddd82d24a6e4a
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "2837765"
---
# <a name="frequently-asked-questions-about-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 常见问题
对你可能有的关于通过 C++/WinRT 创作和使用 Windows 运行时 API 的问题的解答。

> [!NOTE]
> 如果问题与你看到的错误消息有关，另请参阅 [C++/WinRT 问题疑难解答](troubleshooting.md)主题。

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsixhttpsakamscppwinrtvsix"></a>[C++/WinRT Visual Studio 扩展 (VSIX)](https://aka.ms/cppwinrt/vsix) 的要求是什么？
[VSIX](https://aka.ms/cppwinrt/vsix) 强制执行最低的 Windows SDK 目标版本 10.0.17134.0（Windows 10，版本 1803）。 还需要 Visual Studio 2017（版本不低于 15.6；建议版本不低于 15.7）。 你可以通过 `.vcxproj` 文件 `<PropertyGroup Label="Globals">` 中 `<CppWinRTEnabled>true</CppWinRTEnabled>` 的存在来识别使用 VSIX 的项目。 有关详细信息，请参阅 [C++/WinRT 的 Visual Studio 支持和 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

## <a name="whats-a-runtime-class"></a>什么是*运行时类*？
运行时类是一个可通过现代 COM 接口进行激活和使用（通常跨可执行文件）的类型。 但是，运行时类也可在实现它的编译单元内使用。 你采用接口定义语言 (IDL) 声明运行时类，而且可以在标准 C++ 中使用 C++/WinRT 实现它。

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>*投影类型*和*实现类型*是什么意思？
如果你仅*使用* Windows 运行时类（运行时类），则将要专门处理*投影类型*。 C++/WinRT 是一种*语言投影*，所以投影类型是通过 C++/WinRT *投影*到 C++ 中的 Windows 运行时的表面的一部分。 有关更多详细信息，请参阅[通过 C++/WinRT 使用 API](consume-apis.md)。

*实现类型*包含运行时类的实现，因此仅在实现该运行时类的项目中可用。 当你在实现运行时类的项目中工作时（Windows 运行时组件项目或使用 XAML UI 的项目），请务必熟悉你对某个运行时类的实现类型与表示该运行时类已投影到 C++/WinRT 中的投影类型之间的区别。 有关更多详细信息，请参阅[通过 C++/WinRT 创作 API](author-apis.md)。

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>我是否需要采用我的运行时类的 IDL 声明构造函数？
仅当运行时类设计为从其实现编译单元外部进行使用时（即适用于 Windows 运行时客户端应用的一般使用的 Windows 运行时组件）。 有关采用 IDL 声明构造函数的目的和结果的完整详细信息，请参阅[运行时类构造函数](author-apis.md#runtime-class-constructors)。

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>链接器为什么给出“LNK2019: 无法解析的外部符号”错误？
如果无法解析的符号是 C++/WinRT 投影的 Windows 命名空间头文件中的 API（位于 **winrt** 命名空间），则是因为该 API 在已包含的头文件中做了前置声明，但其定义位于尚未包含的头文件中。 请包括以 API 的命名空间命名的头文件，并重新生成。 有关详细信息，请参阅 [C++/WinRT 投影头文件](consume-apis.md#cwinrt-projection-headers)。

如果无法解析的符号是 Windows 运行时自由函数，例如 [RoInitialize](https://msdn.microsoft.com/library/br224650)，则需要在项目中显式包含 [WindowsApp.lib](/uwp/win32-and-com/win32-apis) umbrella 库。 C++/WinRT 投影依赖于这些自由（非成员）函数和入口点。 如果为应用程序使用了某个 [C++/WinRT Visual Studio 扩展 (VSIX)](https://aka.ms/cppwinrt/vsix) 项目模板，则会自动链接 `WindowsApp.lib`。 如果没有自动链接，则可以使用项目链接设置包含它，或在源代码中进行。

```cppwinrt
#pragma comment(lib, "windowsapp")
```

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>我是否应实现 [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable)，如果是，该怎么实现？
如果你有在其构造函数中释放资源的运行时类，而且有旨在从其实现编译单元外部所使用的运行时类（即适用于 Windows 运行时客户端应用的一般使用的 Windows 运行时组件），则我们建议你还要实现 **IClosable**，以支持缺乏确定性终止化的语言对运行时类的使用。 确保资源得到释放，无论调用的是析构函数 [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.Close) 还是两者。 可调用 **IClosable::Close** 任意次数。

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosablewindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>我是否需要对所使用的运行时类调用 [**IClosable::Close**](/uwp/api/windows.foundation.iclosable#Windows_Foundation_IClosable_Close_)？
**IClosable** 存在以支持缺乏确定性终止化的语言。 所以，你不应通过 C++/WinRT 调用 **IClosable::Close**，涉及关闭竞争或半死锁的极少数情况除外。 例如，如果使用的是 **Windows.UI.Composition** 类型，则可能会遇到按照设定的顺序释放对象的情况，作为允许 C++/WinRT 包装器的析构为你执行该工作的替代方式。

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>C++/WinRT 可以使用 LLVM/Clang 编译吗？
C++/WinRT 不支持 LLVM 和 Clang 工具链，但我们在内部使用 LLVM 和 Clang 验证 C++/WinRT 的合规性。 例如，如果想模拟内部执行的操作，可以尝试进行如下所述的实验。

转到 [LLVM 下载页面](https://releases.llvm.org/download.html)，查找 **Download LLVM 6.0.0** > **Pre-Built Binaries**，并下载 **Clang for Windows (64-bit)**。 安装期间，选择将 LLVM 添加到 PATH 系统变量，以便能够从命令提示符调用它。 在本实验中，如果遇到任何“Failed to find MSBuild toolsets directory”和/或“MSVC integration install failed”错误，可以忽略。 调用 LLVM/Clang 的方法有很多，下面的示例只演示了其中一种。

```
C:\ExperimentWithLLVMClang>type main.cpp
// main.cpp
#pragma comment(lib, "windowsapp")
#pragma comment(lib, "ole32")

#include "winrt/Windows.Foundation.h"
#include <stdio.h>
#include <iostream>

using namespace winrt;

int main()
{
    winrt::init_apartment();
    Windows::Foundation::Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    std::wcout << rssFeedUri.Domain().c_str() << std::endl;
}

C:\ExperimentWithLLVMClang>clang-cl main.cpp /EHsc /I ..\.. -Xclang -std=c++17 -Xclang -Wno-delete-non-virtual-dtor -o app.exe

C:\ExperimentWithLLVMClang>app
windows.com
```

C++/WinRT 使用 C++ 17 标准版中的功能，因此你需要使用所有必要的编译器标志来获得支持；此类标志因编译器而异。

Visual Studio 是我们支持和推荐用于 C++/WinRT 的开发工具。 请参阅[对 C++/WinRT 的 Visual Studio 支持以及 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>为什么没有只读属性的生成的实现函数`const`限定符？

当声明[MIDL 3.0](/uwp/midl-3/)中的只读属性时，您可能希望`cppwinrt.exe`工具为您生成的实现函数的`const`-限定 （const 函数将视为 const *this*指针）。

我们一定建议使用 const，只要有可能，但`cppwinrt.exe`工具本身不尝试有关哪些实现功能很可能 const，和其不可能原因。 您可以选择使任意实现函数 const，如本例中所示。

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

您可以删除的`const`上**ToString**限定符您应确定您需要更改在其实现一些对象状态。 但是，使您的成员的每个函数 const 或非-const，而不是同时。 换句话说，不在重载的实现函数`const`。

除了您实现的函数，其他另一个放置在 const 进入图片位于 Windows Runtime 函数投影。 请考虑此代码。

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

有关**ToString**上述调用，Visual Studio 中的**转到声明**命令显示的 Windows Runtime **IStringable::ToString**投影到 C + + / WinRT 如下所示。

```
winrt::hstring ToString() const;
```

投影函数是 const 无论您选择若要成为合格的实现。 在后台投影调用的应用程序二进制接口 (ABI) 情况下，呼叫通过 COM 接口指针。 只有与计划的**ToString**交互的状态是该 COM 接口指针;并且一定具有不需要修改该指针，以使的功能是 const。 这将使您保证它不会更改的**IStringable**引用的您通过，呼叫相关的任何内容，并确保，您可以调用**ToString** ，即使使用 const 引用**IStringable**。

了解，这些示例的`const`是实现详细信息的 C + + / WinRT 投影和实现;它们构成为您提供方便的代码安全机制。 没有此类的内容作为`const`上 COM 也 Windows 运行时 ABI （对于成员函数）。

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>您是否有任何建议降低代码大小为 C + + / WinRT 二进制文件？

处理 Windows 运行时对象时，应避免的编码模式，如下所示，因为它可以比生成所需的更多二进制代码，从而在您的应用程序上产生负面影响。

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

在 Windows Runtime 领域，编译器是无法缓存的值`c()`或通过间接寻址调用每种方法的接口 ('。)。 除非您干预的结果在多个虚拟呼叫和引用计数开销。 上面的模式无法轻松地生成两倍，严格所需的代码。 相反，更喜欢在哪里可以如下所示的模式。 它生成很少的代码，并它能也显著提高运行的时的性能。

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

如上所示的建议的模式应用于而不仅仅是 C + + / WinRT 但向所有 Windows Runtime 语言投影。

> [!NOTE]
> 如果本主题没有解决你的问题，你可以[在 Stack Overflow 上使用 `c++-winrt` 标签](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt)查找帮助。
