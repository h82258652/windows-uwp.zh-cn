---
description: 对你可能有的关于通过 C++/WinRT 创作和使用 Windows 运行时 API 的问题的解答。
title: C++/WinRT 常见问题
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 频繁, 问的, 问题, 常见问题
ms.localizationpriority: medium
ms.openlocfilehash: 914cf884b97d14af523cc61b0fcce719104783ba
ms.sourcegitcommit: 1f39b67f2711b96c6b4e7ed7107a9a47127d4e8f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2019
ms.locfileid: "66721689"
---
# <a name="frequently-asked-questions-about-cwinrt"></a>C++/WinRT 常见问题
您可能具有有关创作和使用与 Windows 运行时 Api 的问题的答案[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)。

> [!NOTE]
> 如果问题与你看到的错误消息有关，另请参阅 [C++/WinRT 问题疑难解答](troubleshooting.md)主题。

## <a name="how-do-i-retarget-my-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>如何执行我重定目标，我C++到更高版本的 Windows SDK /WinRT 项目？
请参阅[如何重定目标，在C++/WinRT 项目到更高版本的 Windows SDK](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)。

## <a name="why-wont-my-new-project-compile-now-that-ive-moved-to-cwinrt-20"></a>为什么不会我新项目进行编译，现在，我已移到C++WinRT 2.0？
有关完整的更改 （包括的重大更改） 集，请参阅[新闻和更改，在C++WinRT 2.0](news.md#news-and-changes-in-cwinrt-20)。 例如，如果您使用的基于范围的`for`上的 Windows 运行时集合，然后您现在需要为`#include <winrt/Windows.Foundation.Collections.h>`。

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>为什么我的新项目将不会编译？ 我使用 Visual Studio 2017 (版本 15.8.0 或更高版本)，和 SDK 版本版本 17134
如果您使用的 Visual Studio 2017 (版本 15.8.0 或更高版本)，以及针对 Windows SDK 版本 10.0.17134.0 (Windows 10，版本 1803年)，然后对新创建C++/WinRT 项目可能无法编译出现错误"*错误 C3861: from_abi:找不到标识符*"，并与来自其他错误同时*base.h*。 解决方法到任一目标是更高版本的 （更符合） 版本的 Windows SDK 或设置项目属性**C /C++**  > **语言** >  **符合模式：否**(此外，如果**触发-** 出现在项目属性**C /C++**  > **命令行**下**其他选项**，然后将其删除)。

## <a name="how-do-i-resolve-the-build-error-the-cwinrt-vsix-no-longer-provides-project-build-support--please-add-a-project-reference-to-the-microsoftwindowscppwinrt-nuget-package"></a>如何解决生成错误" C++WinRT VSIX 不再提供项目的生成支持。  请添加对 Microsoft.Windows.CppWinRT Nuget 包的项目引用"？
安装**Microsoft.Windows.CppWinRT**到你的项目的 NuGet 包。 有关详细信息，请参阅[早期版本的 VSIX 扩展](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)。

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsix"></a>要求是什么C++WinRT Visual Studio 扩展 (VSIX)？
有关版本 1.0.190128.4 VSIX 扩展及更高版本，请参阅[适用于 Visual Studio 支持C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。 对于其他版本，请参阅[早期版本的 VSIX 扩展](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)。

## <a name="whats-a-runtime-class"></a>什么是*运行时类*？
运行时类是一个可通过现代 COM 接口进行激活和使用（通常跨可执行文件）的类型。 但是，运行时类也可在实现它的编译单元内使用。 你采用接口定义语言 (IDL) 声明运行时类，而且可以在标准 C++ 中使用 C++/WinRT 实现它。

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>*投影类型*和*实现类型*是什么意思？
如果你仅*使用* Windows 运行时类（运行时类），则将要专门处理*投影类型*。 C++/WinRT 是一种*语言投影*，所以投影类型是通过 C++/WinRT *投影*到 C++ 中的 Windows 运行时的表面的一部分。 有关更多详细信息，请参阅[与使用 Api C++/WinRT](consume-apis.md)。

*实现类型*包含运行时类的实现，因此仅在实现该运行时类的项目中可用。 当你在实现运行时类的项目中工作时（Windows 运行时组件项目或使用 XAML UI 的项目），请务必熟悉你对某个运行时类的实现类型与表示该运行时类已投影到 C++/WinRT 中的投影类型之间的区别。 有关更多详细信息，请参阅[通过 C++/WinRT 创作 API](author-apis.md)。

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>我是否需要采用我的运行时类的 IDL 声明构造函数？
仅当运行时类设计为从其实现编译单元外部进行使用时（即适用于 Windows 运行时客户端应用的一般使用的 Windows 运行时组件）。 有关采用 IDL 声明构造函数的目的和结果的完整详细信息，请参阅[运行时类构造函数](author-apis.md#runtime-class-constructors)。

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>为什么会链接器为我提供"LNK2019:无法解析的外部符号"错误？
如果无法解析的符号是 C++/WinRT 投影的 Windows 命名空间头文件中的 API（位于 **winrt** 命名空间），则是因为该 API 在已包含的头文件中做了前置声明，但其定义位于尚未包含的头文件中。 请包括以 API 的命名空间命名的头文件，并重新生成。 有关详细信息，请参阅 [C++/WinRT 投影头文件](consume-apis.md#cwinrt-projection-headers)。

如果无法解析的符号是 Windows 运行时的可用函数，例如[RoInitialize](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roinitialize)，则需要显式链接[WindowsApp.lib](/uwp/win32-and-com/win32-apis)涵盖性库项目中的。 C++/WinRT 投影依赖于这些自由（非成员）函数和入口点。 如果为应用程序使用了某个 [C++/WinRT Visual Studio 扩展 (VSIX)](https://aka.ms/cppwinrt/vsix) 项目模板，则会自动链接 `WindowsApp.lib`。 如果没有自动链接，则可以使用项目链接设置包含它，或在源代码中进行。

```cppwinrt
#pragma comment(lib, "windowsapp")
```

务必解决任何链接器错误，你可以通过链接**WindowsApp.lib**而不是可选的静态链接库，否则你的应用程序不会传递[Windows 应用认证工具包](../debug-test-perf/windows-app-certification-kit.md)用于通过 Visual Studio 和 Microsoft Store 验证提交 （因此不会有可能成功引入到 Microsoft Store 应用程序的含义） 的测试。

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>我是否应实现 [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable)，如果是，该怎么实现？
如果你有在其构造函数中释放资源的运行时类，而且有旨在从其实现编译单元外部所使用的运行时类（即适用于 Windows 运行时客户端应用的一般使用的 Windows 运行时组件），则我们建议你还要实现 **IClosable**，以支持缺乏确定性终止化的语言对运行时类的使用。 确保资源得到释放，无论调用的是析构函数 [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) 还是两者。 可调用 **IClosable::Close** 任意次数。

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>我是否需要对所使用的运行时类调用 [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close)？
**IClosable** 存在以支持缺乏确定性终止化的语言。 所以，你不应通过 C++/WinRT 调用 **IClosable::Close**，涉及关闭竞争或半死锁的极少数情况除外。 例如，如果使用的是 **Windows.UI.Composition** 类型，则可能会遇到按照设定的顺序释放对象的情况，作为允许 C++/WinRT 包装器的析构为你执行该工作的替代方式。

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>C++/WinRT 可以使用 LLVM/Clang 编译吗？
C++/WinRT 不支持 LLVM 和 Clang 工具链，但我们在内部使用 LLVM 和 Clang 验证 C++/WinRT 的合规性。 例如，如果想模拟内部执行的操作，可以尝试进行如下所述的实验。

转到 [LLVM 下载页面](https://releases.llvm.org/download.html)，查找 **Download LLVM 6.0.0** > **Pre-Built Binaries**，并下载 **Clang for Windows (64-bit)** 。 安装期间，选择将 LLVM 添加到 PATH 系统变量，以便能够从命令提示符调用它。 在本实验中，如果遇到任何“Failed to find MSBuild toolsets directory”和/或“MSVC integration install failed”错误，可以忽略。 调用 LLVM/Clang 的方法有很多，下面的示例只演示了其中一种。

```cmd
C:\ExperimentWithLLVMClang>type main.cpp
// main.cpp
#pragma comment(lib, "windowsapp")
#pragma comment(lib, "ole32")

#include <winrt/Windows.Foundation.h>
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

Visual Studio 是我们支持和推荐用于 C++/WinRT 的开发工具。 请参阅[适用于 Visual Studio 支持C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>为什么没有只读属性生成的实现函数`const`限定符？
当声明中的只读属性[MIDL 3.0](/uwp/midl-3/)，您所料`cppwinrt.exe`工具来生成实现函数是`const`-限定 (const 函数视为*此*为 const 指针)。

我们确实建议尽量使用常量，但`cppwinrt.exe`工具本身不会尝试将造成哪一种实现有关函数可能无疑是常量，并其中可能不会。 您可以选择要进行任何实现函数的常量，如本例所示。

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

您可以删除该`const`上的限定符**ToString**应确定需要更改在其实现中某些对象状态。 但将每个您的成员不能同时常量或非常量函数。 换而言之，不会出现过载实现函数上`const`。

除了您实现的函数，另一种其他放置其中 const 发挥图片是 Windows 运行时函数投影中。 请考虑此代码。

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

为调用**ToString**上面**转到声明**在 Visual Studio 中的命令显示的 Windows 运行时的投影**IStringable::ToString** C++/WinRT 外观如下所示。

```cppwinrt
winrt::hstring ToString() const;
```

上的投影函数是 const 无论您选择来限定它们的实现。 在后台，投影调用应用程序二进制接口 (ABI) 情况下，通过 COM 接口指针的调用。 唯一状态预计**ToString**交互与为该 COM 接口指针; 并且它当然无需修改该指针，因此该函数是常量。 这使它不会更改任何内容有关的确定性**IStringable**引用，它调用通过，并确保您可以调用**ToString**甚至与的常量引用**IStringable**。

了解这的这些示例`const`的详细信息，实现C++/WinRT 投影和实现; 它们构成代码净化为您提供方便。 没有此类的内容作为`const`上 COM 和 Windows 运行时 ABI （对于成员函数）。

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>您是否有任何建议减小代码大小为C++/WinRT 二进制文件？
当使用 Windows 运行时对象，应避免因为它会在你的应用程序产生负面影响，通过使更多生成所需的二进制代码如下所示的编码模式。

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

在 Windows 运行时世界中，编译器不能缓存的值`c()`或调用通过间接寻址的每个方法的接口 ('。)。 除非您进行干预，该操作会在更多虚拟调用和引用计数开销。 上面的模式可以轻松生成倍严格按所需的代码。 相反，倾向于使用任何位置可以如下所示的模式。 它会生成很少的代码，并它还显著可以提高运行的时性能。

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

如上所示的建议的模式只是个不适用C++/WinRT 而到所有 Windows 运行时语言投射。

## <a name="how-do-i-turn-a-string-into-a-typemdashfor-navigation-for-example"></a>如何为类型启用字符串&mdash;用于导航，例如？
在末尾[导航视图的代码示例](/windows/uwp/design/controls-and-patterns/navigationview#code-example)(这主要是在C#)，没有C++/WinRT 代码段显示如何执行此操作。

> [!NOTE]
> 如果本主题没有回答您的问题，则你可能会发现帮助，请访问[Visual StudioC++开发人员社区](https://developercommunity.visualstudio.com/spaces/62/index.html)，或使用[`c++-winrt`标记 Stack Overflow 上](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt)。
