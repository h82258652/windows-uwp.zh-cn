---
description: 无论你是要削减新代码还是要移植现有应用，本主题中的症状排查和补救措施表都可能对你有帮助。
title: C++/WinRT 问题疑难解答
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 疑难解答, HRESULT, 错误
ms.localizationpriority: medium
ms.openlocfilehash: 3fe67fd0593c9c1f605a5d1cc7418c348b6ca9c3
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360098"
---
# <a name="troubleshooting-cwinrt-issues"></a>C++/WinRT 问题疑难解答

> [!NOTE]
> 有关如何安装和使用信息[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Visual Studio 扩展 (VSIX)，（可提供项目模板支持） 请参阅[适用于 Visual Studio 支持C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

将本主题放在前面是为了让你可以立即意识到它，即使你现在尚不需要它。 无论你是要削减新代码还是要移植现有应用，下面的症状排查和补救措施表都可能对你有帮助。 如果你进行移植，并且迫不及待想要取得进展并到达项目生成和运行的阶段，则你可以通过注释掉或去掉任何导致出现问题的非必要代码并在稍后回来进行修补的方式来暂时向前继续。

有关的常见问题的列表，请参阅[常见问题的解答](faq.md)。

## <a name="tracking-down-xaml-issues"></a>跟踪 XAML 问题
XAML 分析异常可能很难进行诊断，特别是在此类异常中没有含义明确的错误消息时。 请确保已将调试程序配置为捕获第一轮异常（以便试图捕获早期的分析异常）。 你可以检查调试程序中的异常变量，以确定 HRESULT 或消息中是否具有任何有用的信息。 也可以检查 Visual Studio 的输出窗口，以获取由 XAML 分析器输出的错误消息。

如果应用终止，并且你只知道在 XAML 标记分析过程中引发了未经处理的异常，则此错误可能是由于引用（通过键）了缺失的资源导致的。 或者，它可能是在 UserControl、自定义控件或自定义布局面板内部引发的异常。 最后一项措施是进行二进制拆分。 从 XAML 页面中删除大约一半标记并重新运行应用。 然后，你将知道错误是在已删除的那一半中（你现在应该在任何情况下恢复）还是在未删除的那一半中。 通过拆分包含错误的那一半来重复此过程，依此类推，直到完全解决了问题。

## <a name="symptoms-and-remedies"></a>症状和补救方法
| 症状 | 补救方法 |
|---------|--------|
| 在运行时引发异常，并显示 HRESULT 值 REGDB_E_CLASSNOTREGISTERED。 | 此错误的一个原因是 Windows 运行时组件无法加载。 请确保该组件的 Windows 运行时元数据文件 (`.winmd`) 与组件二进制文件 (`.dll`) 的名称相同，这也是项目名称和根命名空间的名称。 此外请确保生成过程已将 Windows 运行时元数据和二进制文件正确地复制到使用应用的 `Appx` 文件夹。 同时确认使用应用的 `AppxManifest.xml`（也在 `Appx` 文件夹中）包含一个正确声明了可激活的类和二进制文件名称的 **&lt;InProcessServer&gt;** 元素。 如果你错误地通过投影类型的默认构造函数实例化了一个在本地实现的运行时类，则也会发生此错误。 请参阅 [XAML 控件; 绑定到 C++/WinRT 属性](binding-property.md) 以了解有关在这种情况下如何正确使用投影类型的更多信息。 |
| C++ 编译器产生错误“ *‘implements_type’: 不是‘&lt;投影类型&gt;’* 的任何直接或间接基类的成员”。 | 当你使用实现类型的未限定命名空间的名称（例如 **MyRuntimeClass**）来调用 **make** 并且没有包括该类型的标头时，将会出现此错误。 编译器会将 **MyRuntimeClass** 解释为投影类型。 解决办法是包括实现类型的标头（例如 `MyRuntimeClass.h`）。 |
| C++ 编译器产生错误“*正在尝试引用已删除的函数*”。 | 当你调用 **make** 并且你作为模板参数传递的实现类型具有 `= delete` 默认构造函数时，将会出现此错误。 编辑实现类型的标头文件并将 `= delete` 更改为 `= default`。 你还可以为运行时类添加一个构造函数到 IDL 中。 |
| 你已经实现 [**INotifyPropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged)，但 XAML 绑定没有更新（UI 没有订阅 [**PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)）。 | 请记得在 XAML 标记中的绑定表达式上设置 `Mode=OneWay`（或 TwoWay）。 请参阅 [XAML 控件; 绑定到 C++/WinRT 属性](binding-property.md)。 |
| 你将一个 XAML 项目控件绑定到一个可观测集合，在运行时引发了异常并显示消息“参数不正确”。 | 在 IDL 和实现中，将任何可观测的集合声明为 **Windows.Foundation.Collections.IVector<IInspectable>** 。 但返回一个实现 **Windows.Foundation.Collections.IObservableVector<T>** （其中的 T 是元素类型）的对象。 请参阅 [XAML 项目控件; 绑定到 C++/WinRT 集合](binding-collection.md)。  |
| C++ 编译器产生以下形式的错误“ *‘MyImplementationType_base&lt;MyImplementationType&gt;’: 没有适当的默认构造函数可用*”。|当你从具有特殊构造函数的类型派生时会出现此错误。 派生类型的构造函数需要传递基类型的构造函数所需的参数。 有关工作示例，请参阅[从具有特殊构造函数的类型派生](author-apis.md#deriving-from-a-type-that-has-a-non-default-constructor)。|
| C++ 编译器产生错误“*无法从‘const std::vector&lt;std::wstring,std::allocator&lt;_Ty&gt;&gt;’转换为‘const winrt::param::async_iterable&lt;winrt::hstring&gt; &’* ”。|当你将 std::wstring 的 std::vector 传递给需要一个集合的 Windows 运行时 API 时，将会出现此错误。 有关更多信息，请参阅[标准 C++ 数据类型和 C++/WinRT](std-cpp-data-types.md)。|
| C++ 编译器产生错误“*无法从‘const std::vector&lt;winrt::hstring,std::allocator&lt;_Ty&gt;&gt;’转换为‘const winrt::param::async_iterable&lt;winrt::hstring&gt; &'* ”。|当你将 winrt::hstring 的 std::vector 传递给需要一个集合的异步 Windows 运行时 API 并且你没有将相应的矢量复制或移动到异步被调用方时，将会出现此错误。 有关更多信息，请参阅[标准 C++ 数据类型和 C++/WinRT](std-cpp-data-types.md)。|
| 当打开项目时，Visual Studio 产生错误“*该项目的应用程序未安装*”。|你需要从 Visual Studio 的**新建项目**对话框中安装 **用于 C++ 开发的 Windows 通用工具**（如果你尚未这样做的话）。 如果仍未解决该问题，则该项目可能依赖于C++WinRT Visual Studio 扩展 (VSIX) (请参阅[适用于 Visual Studio 支持C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。|
| Windows 应用认证工具包测试产生运行时类的一个错误"*不是 Windows 的基类派生。可组合的所有类最终必须从 Windows 命名空间中的类型都派生*"。|（即您在您的应用程序中声明） 从基类派生的任何运行时类被称为*可组合*类。 可组合类的最终基类必须是源自 windows.* 命名空间; 的类型例如， [ **Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject)。 请参阅[XAML 控制; 绑定到C++/WinRT 属性](binding-property.md)的更多详细信息。|
| 对于 EventHandler 或 TypedEventHandler 委托专用化，C++ 编译器产生“*必须是 WinRT 类型*”错误。|请考虑改为使用 **winrt::delegate&lt;…T&gt;** 。 请参阅 [在 C++/WinRT 中创作事件](author-events.md)。|
| 对于 Windows 运行时异步操作专用化，C++ 编译器产生“*必须是 WinRT 类型*”错误。|请考虑改为返回并行模式库 (PPL) [**任务**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class)。 请参阅[并发操作和异步操作](concurrency.md)。|
| C++ 编译器产生“*错误 C2220: 视为错误的警告 - 未生成‘object’文件*”。|请更正此警告，或将设置**C /C++**  > **常规** > **将警告视为错误**向**否 (/ WX-)** 。|
| 应用发生崩溃，因为在 C++/WinRT 对象销毁后调用了其中的一个事件处理程序。|请参阅[安全地访问*这*事件处理委托的指针](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)。|
| C++编译器会生成"*错误 C2338:这是仅用于弱引用支持*"。|你请求针对某个类型的弱引用，该类型将 **winrt::no_weak_ref** 标记结构作为模板参数传递给其基类。 请参阅[选择退出弱引用支持](weak-references.md#opting-out-of-weak-reference-support)。|
| C++链接器生成"*错误 LNK2019:无法解析的外部符号*"|请参阅[为什么会链接器为我提供"LNK2019:无法解析的外部符号"错误？](faq.md#why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error).|
| LLVM 和 Clang 工具链生成错误与一起使用时C++/WinRT。|我们不支持的 LLVM 和 Clang 工具链C++/WinRT，但如果你想要模拟我们如何使用它在内部，则你可以尝试实验，如中所述[我可以使用 LLVM/Clang 编译与C++/WinRT？](faq.md#can-i-use-llvmclang-to-compile-with-cwinrt)。|
| C++编译器会生成"*没有适当的默认构造函数可用*"为提取的类型。 | 如果想要延迟初始化运行时类对象，或要使用和实现的运行时类在同一项目中，则将需要调用`nullptr_t`构造函数。 有关详细信息，请参阅[通过 C++/WinRT 使用 API](consume-apis.md)。 |
| C++编译器会生成"*错误 C3861: from_abi： 找不到标识符*"，以及来自其他错误*base.h*。 可能会看到此错误，如果您使用的 Visual Studio 2017 (版本 15.8.0 或更高版本)，以及针对 Windows SDK 版本 10.0.17134.0 (Windows 10，版本 1803年)。 | 是面向更高版本的 （更符合） 版本的 Windows SDK 或设置项目属性**C /C++**  > **语言** > **符合模式：否**(此外，如果**触发-** 出现在项目属性**C /C++**  > **语言** >  **Command Line**下**其他选项**，然后将其删除)。 |
| C++编译器会生成"*错误 C2039:IUnknown： 不是成员的 '\`全局命名空间'* "。 | 请参阅[如何重定目标，在C++/WinRT 项目到更高版本的 Windows SDK](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)。 |
| C++链接器生成"*错误 LNK2019： 无法解析的外部符号_WINRT_CanUnloadNow@0函数中引用_VSDesignerCanUnloadNow@0* " | 请参阅[如何重定目标，在C++/WinRT 项目到更高版本的 Windows SDK](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)。 |
| 生成过程将生成错误消息 *C++WinRT VSIX 不再提供项目的生成支持。请添加对 Microsoft.Windows.CppWinRT Nuget 包的项目引用*。 | 安装**Microsoft.Windows.CppWinRT**到你的项目的 NuGet 包。 有关详细信息，请参阅[早期版本的 VSIX 扩展](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)。 |
| C++编译器会生成错误消息，指出*std:: experimental'* ，和/或错误的窗体*错误 C3861: resume_background： 找不到标识符*。 | 在[ C++WinRT 2.0](news.md#news-and-changes-in-cwinrt-20)，对于任何协同程序支持 (包括协同程序帮助程序，如**winrt::resume_background**， **winrt::resume_foreground**，和**winrt::resume_on_signal**)，你将需要`#include <winrt/coroutine.h>`。 |
| C++链接器生成*错误 LNK2019： 无法解析的外部符号*，与提到*winrt::impl::consume_Windows_Foundation_Collections_IVector*。 | 在[ C++WinRT 2.0](news.md#news-and-changes-in-cwinrt-20)，如果您正在使用基于范围`for`上的 Windows 运行时集合，然后你将现在需要`#include <winrt/Windows.Foundation.Collections.h>`。 |

> [!NOTE]
> 如果本主题没有回答您的问题，则你可能会发现帮助，请访问[Visual StudioC++开发人员社区](https://developercommunity.visualstudio.com/spaces/62/index.html)，或使用[`c++-winrt`标记 Stack Overflow 上](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt)。
