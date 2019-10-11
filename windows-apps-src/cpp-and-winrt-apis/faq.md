---
description: 对你可能存疑的关于通过 C++/WinRT 创作和使用 Windows 运行时 API 的问题的解答。
title: 有关 C++/WinRT 的常见问题解答
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 频繁, 提问, 问题, 常见问题解答
ms.localizationpriority: medium
ms.openlocfilehash: 5bb19e406df98a24a6d65fc774a29e44ef267272
ms.sourcegitcommit: c079388634cbd328d0d43e7a6185e09bb4bca65b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2019
ms.locfileid: "71939588"
---
# <a name="frequently-asked-questions-about-cwinrt"></a>有关 C++/WinRT 的常见问题解答
对你可能存疑的关于通过 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 创作和使用 Windows 运行时 API 的问题的解答。

> [!NOTE]
> 如果问题与你看到的错误消息有关，另请参阅 [C++/WinRT 问题疑难解答](troubleshooting.md)主题。

## <a name="how-do-i-retarget-my-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>如何将 C++/WinRT 项目重新定位到更高版本的 Windows SDK？
请参阅[如何将 C++/WinRT 项目重新定位到更高版本的 Windows SDK](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)。

## <a name="why-wont-my-new-project-compile-now-that-ive-moved-to-cwinrt-20"></a>我已迁移到 C++WinRT 2.0，但我的新项目不能编译，为什么？
有关完整更改（包括重大更改），请参阅 [C++/WinRT 2.0 中的新增功能和更改](news.md#news-and-changes-in-cwinrt-20)。 例如，如果在 Windows 运行时集合上使用基于范围的 `for`，则现在需要 `#include <winrt/Windows.Foundation.Collections.h>`。

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>为何我的新项目不能编译？ 我使用的是 Visual Studio 2017（15.8.0 或更高版本）和 SDK 版本 17134
如果你使用的是 Visual Studio 2017（15.8.0 或更高版本）并且面向 Windows SDK 版本 10.0.17134.0（Windows 10 版本 1803），则新建的 C++/WinRT 项目可能无法编译并出现错误“错误 C3861: 'from_abi': 找不到标识符”，以及源自 *base.h* 的其他错误。  解决方法是要么面向 Windows SDK 的更高（更相符）版本，要么设置项目属性“C/C++” > “语言” > “一致性模式:    否”（此外，如果 **/permissive-** 显示在“其他选项”下的项目属性“C/C++” > “命令行”中，请将其删除）。   

## <a name="how-do-i-resolve-the-build-error-the-cwinrt-vsix-no-longer-provides-project-build-support--please-add-a-project-reference-to-the-microsoftwindowscppwinrt-nuget-package"></a>如何解决生成错误“C++/WinRT VSIX 不再提供项目生成支持。  请将项目引用添加到 Microsoft.Windows.CppWinRT Nuget 包”？
请在项目中安装 **Microsoft.Windows.CppWinRT** NuGet 包。 有关详细信息，请参阅[早期版本的 VSIX 扩展](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)。

## <a name="how-do-i-customize-the-build-support-in-the-nuget-package"></a>如何自定义 NuGet 包中的生成支持？

C++/WinRT 生成支持（属性/目标）记录在 Microsoft.Windows.CppWinRT NuGet 包[自述文件](https://github.com/microsoft/xlang/tree/master/src/package/cppwinrt/nuget/readme.md#customizing)中。

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsix"></a>C++/WinRT Visual Studio 扩展 (VSIX) 的要求是什么？
对于 VSIX 扩展版本 1.0.190128.4 和更高版本，请参阅 [C++/WinRT 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。 对于其他版本，请参阅[早期版本的 VSIX 扩展](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)。

## <a name="whats-a-runtime-class"></a>什么是运行时类？ 
运行时类是一个可通过现代 COM 接口进行激活和使用（通常跨可执行文件）的类型。 但是，运行时类也可在实现它的编译单元内使用。 以接口定义语言 (IDL) 声明运行时类，可以在标准 C++ 中使用 C++/WinRT 实现该类。

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>什么是投影类型和实现类型？  
如果你仅使用 Windows 运行时类（运行时类），则将要专门处理投影类型   。 C++/WinRT 是一种语言投影，所以投影类型是通过 C++/WinRT 投影到 C++ 中的 Windows 运行时的表面的一部分   。 有关更多详细信息，请参阅[通过 C++/WinRT 使用 API](consume-apis.md)。

实现类型包含运行时类的实现，因此仅在实现该运行时类的项目中可用  。 在实现运行时类的项目中工作时（Windows 运行时组件项目或使用 XAML UI 的项目），请务必熟悉对某个运行时类的实现类型与表示该运行时类已投影到 C++/WinRT 中的投影类型之间的区别。 有关更多详细信息，请参阅[通过 C++/WinRT 创作 API](author-apis.md)。

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>是否需要在运行时类的 IDL 中声明构造函数？
仅当运行时类设计为从其实现编译单元外部进行使用时（即适用于 Windows 运行时客户端应用的一般使用的 Windows 运行时组件）。 有关在 IDL 中声明构造函数的目的和结果的完整详细信息，请参阅[运行时类构造函数](author-apis.md#runtime-class-constructors)。

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>链接器为何显示“LNK2019:未解析的外部符号”错误？
如果无法解析的符号是 C++/WinRT 投影的 Windows 命名空间标头文件中的 API（位于 **winrt** 命名空间），则是因为该 API 在已包含的标头文件中做了前置声明，但其定义位于尚未包含的标头文件中。 请包括以 API 的命名空间命名的标头文件，并重新生成。 有关详细信息，请参阅 [C++/WinRT 投影标头文件](consume-apis.md#cwinrt-projection-headers)。

如果无法解析的符号是 Windows 运行时自由函数，例如 [RoInitialize](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roinitialize)，则需要在项目中显式链接 [WindowsApp.lib](/uwp/win32-and-com/win32-apis) umbrella 库。 C++/WinRT 投影依赖于这些自由（非成员）函数和入口点。 如果为应用程序使用了某个 [C++/WinRT Visual Studio 扩展 (VSIX)](https://aka.ms/cppwinrt/vsix) 项目模板，则会自动链接 `WindowsApp.lib`。 否则，你可以使用项目链接设置包含它，或在源代码中包含它。

```cppwinrt
#pragma comment(lib, "windowsapp")
```

请务必通过链接 **WindowsApp.lib** 而不是替代的静态链接库来解决所有链接器错误，否则应用程序通不过 Visual Studio 和 Microsoft Store 用来验证提交内容的 [Windows 应用认证工具包](../debug-test-perf/windows-app-certification-kit.md)测试（这意味着，应用程序最终无法成功引入到 Microsoft Store）。

## <a name="why-am-i-getting-a-class-not-registered-exception"></a>为什么会收到“类未注册”异常？

在这种情况下，症状是&mdash;在构造运行时类或访问静态成员时&mdash;，看到在运行时引发的异常，其中 HRESULT 值为 REGDB_E_CLASSNOTREGISTERED。

一个原因可能是 Windows 运行时组件无法加载。 请确保该组件的 Windows 运行时元数据文件 (`.winmd`) 与组件二进制文件 (`.dll`) 的名称相同，这也是项目名称和根命名空间的名称。 此外，请确保生成过程已将 Windows 运行时元数据和二进制文件正确地复制到使用应用的 `Appx` 文件夹。 同时确认使用应用的 `AppxManifest.xml`（也在 `Appx` 文件夹中）包含 &lt;InProcessServer&gt;  元素，该元素正确声明了可激活的类和二进制文件名称。

### <a name="uniform-construction"></a>统一构造

如果尝试通过任何投影类型的构造函数（不是其 **std:: nullptr_t** 构造函数）实例化本地实现的运行时类，也可能会发生此错误。 为了解决此问题，你将需要通常称为“统一构造”的 C++/WinRT 2.0 功能。 若要选择加入该功能，并且需要详细信息和代码示例，请参阅[选择加入统一构造和直接实现访问](/windows/uwp/cpp-and-winrt-apis/author-apis#opt-in-to-uniform-construction-and-direct-implementation-access)。

有关实例化不  需要统一构造的本地实现运行时类的方法，请参阅 [XAML 控件；绑定到 C++/WinRT 属性](binding-property.md)。

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>我是否应实现 [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable)，如果是，该怎么实现？
如果你有在其构造函数中释放资源的运行时类，而且有旨在从其实现编译单元外部所使用的运行时类（即适用于 Windows 运行时客户端应用的一般使用的 Windows 运行时组件），则我们建议你还要实现 **IClosable**，以支持缺乏确定性终止化的语言对运行时类的使用。 确保资源得到释放，无论调用的是析构函数 [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) 还是两者。 可调用 **IClosable::Close** 任意次数。

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>我是否需要对所使用的运行时类调用 [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close)？
**IClosable** 存在以支持缺乏确定性终止化的语言。 所以，不应通过 C++/WinRT 调用 **IClosable::Close**，涉及关闭竞争或半死锁的极少数情况除外。 例如，如果使用的是 **Windows.UI.Composition** 类型，则可能会遇到按照设定的顺序释放对象的情况，作为允许 C++/WinRT 包装器的析构为你执行该工作的替代方式。

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>C++/WinRT 可以使用 LLVM/Clang 编译吗？
C++/WinRT 不支持 LLVM 和 Clang 工具链，但我们在内部使用 LLVM 和 Clang 验证 C++/WinRT 的合规性。 例如，如果想模拟内部执行的操作，可以尝试进行如下所述的试验。

转到 [LLVM 下载页](https://releases.llvm.org/download.html)，找到“下载 LLVM 6.0.0” > “预生成的二进制文件”，然后下载“Clang for Windows (64 位)”    。 安装期间，选择将 LLVM 添加到 PATH 系统变量，以便能够从命令提示符调用它。 在此试验中，如果遇到任何“找不到 MSBuild 工具集目录”和/或“MSVC 集成安装失败”错误，可将其忽略。 调用 LLVM/Clang 的方法有很多，下面的示例只演示了其中一种。

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

Visual Studio 是我们支持和推荐用于 C++/WinRT 的开发工具。 请参阅 [C++/WinRT 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>为只读属性生成的实现函数为何不包含 `const` 限定符？
在 [MIDL 3.0](/uwp/midl-3/) 中声明只读属性，你可能预期 `cppwinrt.exe` 工具会为你生成一个带有 `const` 限定符的实现函数（const 函数将 *this* 指针视为 const）。

我们确实建议尽量使用 const，但 `cppwinrt.exe` 工具本身不会尝试推理哪些实现函数很可能是 const，而哪些则不是。 你可以选择将任意实现函数设置为 const，如本示例中所示。

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

如果你决定改变 **ToString** 实现中的某种对象状态，可以删除其中的 `const` 限定符。 但是，请将每个成员函数设置为 const 或非 const，而不要同时设置这两种类型。 换而言之，请不要重写 `const` 中的实现函数。

除了实现函数以外，Windows 运行时函数投影中也会出现 const。 请考虑以下代码。

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

对于上面的 **ToString** 调用，Visual Studio 中的“转到声明”命令会显示从 Windows 运行时 **IStringable::ToString** 到 C++/WinRT 的投影，如下所示。 

```cppwinrt
winrt::hstring ToString() const;
```

无论以何种方式限定投影中函数的实现，这些函数都是 const。 在幕后，投影会调用应用程序二进制接口 (ABI)，这相当于通过 COM 接口指针发出调用。 与投影的 **ToString** 交互的唯一状态是该 COM 接口指针；肯定不需要修改该指针，因此函数是 const。 这样就可以保证不会更改调用时所用的 **IStringable** 引用的任何信息，并确保即使是使用对 **IStringable** 的 const 引用，也能调用 **ToString**。

需要知道，这些 `const` 示例是 C++/WinRT 投影和实现的详细信息；它们构成了有利的代码护理机制。 COM 和 Windows 运行时 ABI（适用于成员函数）中都不存在类似于 `const` 的元素。

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>在减小 C++/WinRT 二进制文件的代码大小方面，你们是否有任何建议？
使用 Windows 运行时对象时，应避免如下所示的代码模式，因为这可能会导致二进制代码超过所需的生成量，从而对应用程序造成负面影响。

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

在 Windows 运行时世界中，编译器无法缓存通过间接表示法（“.”）调用的每个方法的 `c()` 值或接口。 除非进行干预，否则会造成虚拟调用数和引用计数开销增大。 以上模式很容易使得生成的代码量是严格需要量的两倍。 应该尽可能是优先使用下面所示的模式。 此模式生成的代码量要少得多，而且还能明显提高运行时性能。

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

上面所示的建议模式不仅适用于 C++/WinRT，而且也适用于所有 Windows 运行时语言投射。

## <a name="how-do-i-turn-a-string-into-a-typemdashfor-navigation-for-example"></a>如何将字符串转换为其他类型 &mdash; 例如，以方便导航？
在[导航视图代码示例](/windows/uwp/design/controls-and-patterns/navigationview#code-example)（主要是 C# 代码）的末尾，有一个 C++/WinRT 代码片段演示了如何进行这种转换。

## <a name="how-do-i-resolve-ambiguities-with-getcurrenttime-andor-try"></a>如何使用 GetCurrentTime 和/或 TRY 解析多义性？

头文件 `winrt/Windows.UI.Xaml.Media.Animation.h` 声明一个名为 **GetCurrentTime** 的方法，而 `windows.h`（通过 `winbase.h`）定义一个名为 **GetCurrentTime** 的宏。 当二者发生冲突时，C++ 编译器会生成“错误 C4002:  类函数宏的调用 GetCurrentTime 参数太多”。

同样，`winrt/Windows.Globalization.h` 声明一个名为 **TRY** 的方法，而 `afx.h` 定义一个名为 **GetCurrentTime** 的宏。 当这些发生冲突时，C++ 编译器会生成“错误 C2334:‘{’的前面有意外标记；跳过明显的函数体”  。

若要解决一个或两个问题，可以执行此操作。

```cppwinrt
#pragma push_macro("GetCurrentTime")
#pragma push_macro("TRY")
#undef GetCurrentTime
#undef TRY
#include <winrt/include_your_cppwinrt_headers_here.h>
#include <winrt/include_your_cppwinrt_headers_here.h>
#pragma pop_macro("TRY")
#pragma pop_macro("GetCurrentTime")
```

> [!NOTE]
> 如果此主题未回答你的问题，则可以通过访问 [Visual Studio C++ 开发人员社区](https://developercommunity.visualstudio.com/spaces/62/index.html)或使用 [Stack Overflow 上的 `c++-winrt` 标记](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt)获得帮助。
