---
description: 无论你是要削减新代码还是要移植现有应用，本主题中的症状排查和补救措施表都可能对你有帮助。
title: C++/WinRT 问题疑难解答
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 疑难解答, HRESULT, 错误
ms.localizationpriority: medium
ms.openlocfilehash: add3875e15ad747422b2e53e5d8f8438b61b3b20
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270102"
---
# <a name="troubleshooting-cwinrt-issues"></a>C++/WinRT 问题疑难解答

> [!NOTE]
> 有关安装和使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Visual Studio 扩展 (VSIX)（提供项目模板支持）的信息，请参阅 [适用于 C++/WinRT 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

将本主题放在前面是为了让你可以立即意识到它，即使你现在尚不需要它。 无论你是要削减新代码还是要移植现有应用，下面的症状排查和补救措施表都可能对你有帮助。 如果你进行移植，并且迫不及待想要取得进展并到达项目生成和运行的阶段，则你可以通过注释掉或去掉任何导致出现问题的非必要代码并在稍后回来进行修补的方式来暂时向前继续。

有关常见问题的列表，请参阅[常见问题解答](faq.md)。

## <a name="tracking-down-xaml-issues"></a>跟踪 XAML 问题
XAML 分析异常可能很难进行诊断&mdash;特别是在此类异常中没有含义明确的错误消息时。 请确保已将调试程序配置为捕获第一轮异常（以便试图捕获早期的分析异常）。 你可以检查调试程序中的异常变量，以确定 HRESULT 或消息中是否具有任何有用的信息。 也可以检查 Visual Studio 的输出窗口，以获取由 XAML 分析器输出的错误消息。

如果应用终止，并且你只知道在 XAML 标记分析过程中引发了未经处理的异常，则此错误可能是由于（通过键）引用了缺失的资源。 或者，它可以是在 UserControl、自定义控件或自定义布局面板内部引发的异常。 最后一项措施是进行二进制拆分。 从 XAML 页面中删除大约一半标记并重新运行应用。 然后，你将知道错误是在已删除的那一半中（你现在应该在任何情况下恢复）还是在未删除的那一半中。 通过拆分包含错误的那一半来重复此过程，依此类推，直到完全解决了问题。

## <a name="symptoms-and-remedies"></a>症状和补救方法
| 症状 | 补救方法 |
|---------|--------|
| 在运行时抛出异常，并显示 HRESULT 值 REGDB_E_CLASSNOTREGISTERED。 | 请参阅[为什么会收到“类未注册”异常？](faq.md#why-am-i-getting-a-class-not-registered-exception)。 |
| C++ 编译器生成以下错误：“‘implements_type’: 不是 &lt;投影类型&gt;  的任何直接或间接基类的成员”。 | 使用实现类型的未限定命名空间的名称（例如“MyRuntimeClass”）来调用“make”时，如果没有包括该类型的标头，就会出现此错误   。 编译器会将“MyRuntimeClass”解释为投影类型  。 解决办法是包括实现类型的标头（例如 `MyRuntimeClass.h`）。 |
| C++ 编译器生成以下错误：“正在尝试引用已删除的函数”  。 | 调用“make”并且你作为模板参数传递的实现类型具有 `= delete` 默认构造函数时，就会出现此错误  。 编辑实现类型的标头文件并将 `= delete` 更改为 `= default`。 你还可以为运行时类添加一个构造函数到 IDL 中。 |
| 你已经实现 [INotifyPropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged)，但 XAML 绑定没有更新（UI 没有订阅 [PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)）   。 | 请记得在 XAML 标记中的绑定表达式上设置 `Mode=OneWay`（或 TwoWay）。 请参阅 [XAML 控件；绑定到 C++/WinRT 属性](binding-property.md)。 |
| 你将一个 XAML 项目控件绑定到一个可观测集合，在运行时引发了异常并显示消息“参数不正确”。 | 在 IDL 和实现中，将任何可观测的集合声明为“Windows.Foundation.Collections.IVector<IInspectable>”  。 但返回一个实现“Windows.Foundation.Collections.IObservableVector<T>”（其中的 T 是元素类型）的对象  。 请参阅 [XAML 项目控件；绑定到 C++/WinRT 集合](binding-collection.md)。  |
| C++ 编译器生成以下形式的错误：“‘MyImplementationType_base&lt;MyImplementationType&gt;’: 没有适当的默认构造函数可用”  。|从具有特殊构造函数的类型派生时会出现此错误。 派生类型的构造函数需要传递基类型的构造函数所需的参数。 有关工作示例，请参阅[从具有特殊构造函数的类型派生](author-apis.md#deriving-from-a-type-that-has-a-non-default-constructor)。|
| C++ 编译器生成以下错误：“无法从‘const std::vector&lt;std::wstring,std::allocator&lt;_Ty&gt;&gt;’转换为‘const winrt::param::async_iterable&lt;winrt::hstring&gt; &’”  。|将 std::wstring 的 std::vector 传递给需要一个集合的 Windows 运行时 API 时，将会出现此错误。 有关更多信息，请参阅[标准 C++ 数据类型和 C++/WinRT](std-cpp-data-types.md)。|
| C++ 编译器生成以下错误：“无法从‘const std::vector&lt;&lt;_Ty&gt;&gt;’转换为‘const winrt::param::async_iterable&lt;winrt::hstring&gt; &’”  。|将 winrt::hstring 的 std::vector 传递给需要一个集合的异步 Windows 运行时 API 时，如果没有将相应的矢量复制或移动到异步被调用方，就会出现此错误。 有关更多信息，请参阅[标准 C++ 数据类型和 C++/WinRT](std-cpp-data-types.md)。|
| 打开项目时，Visual Studio 生成错误“该项目的应用程序未安装”  。|需要从 Visual Studio 的“新建项目”对话框中安装“用于 C++ 开发的 Windows 通用工具”（如果你尚未这样做的话）   。 如果上述方法未能解决问题，则项目可能依赖于 C++/WinRT Visual Studio Extension (VSIX)（请参阅 [Visual Studio 对于 C++/WinRT 的支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)）。|
| Windows 应用认证工具包测试生成以下错误：“某个运行时类不是从 Windows 基类派生的。所有可组合类必须最终从 Windows 命名空间中的类派生”* 。|从基类派生的任何运行时类（在应用程序中声明）都称为可组合类  。 可组合类的最终基类必须是源自 Windows.* 命名空间的类型；例如，[Windows.UI.Xaml.DependencyObject](/uwp/api/windows.ui.xaml.dependencyobject)  。 有关更多详细信息，请参阅 [XAML 控件；绑定到 C++/WinRT 属性](binding-property.md)。|
| 对于 EventHandler 或 TypedEventHandler 委托专用化，C++ 编译器产生“必须是 WinRT 类型”错误  。|请考虑改为使用“winrt::delegate&lt;…T&gt;”  。 请参阅 [在 C++/WinRT 中创作事件](author-events.md)。|
| 对于 Windows 运行时异步操作专用化，C++ 编译器产生“必须是 WinRT 类型”错误  。|请考虑改为返回并行模式库 (PPL) [任务](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class)  。 请参阅[并发操作和异步操作](concurrency.md)。|
| C++ 编译器生成“错误 C2220: 视为错误的警告 - 未生成 object 文件”  。|更正警告，或者将“C/C++” >“常规” >“将警告视为错误”设置为“否 (/WX-)”   ****  ****  。|
| 应用发生崩溃，因为在 C++/WinRT 对象销毁后调用了其中的一个事件处理程序。|请参阅[使用事件处理委托安全访问该指针](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)  。|
| C++编译器生成“错误 C2338：这仅适用于弱引用支持”* 。|你请求针对某个类型的弱引用，该类型将“winrt::no_weak_ref”标记结构作为模板参数传递给其基类  。 请参阅[选择退出弱引用支持](weak-references.md#opting-out-of-weak-reference-support)|
| C++ 链接器生成“错误 LNK2019：未解析的外部符号”*|请参阅[为什么链接器会提供“LNK2019：未解析的外部符号”错误？](faq.md#why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error)。|
| 与 C++/WinRT 一起使用时，LLVM 和 Clang 工具链会生成错误。|我们不支持适用于 C++/WinRT 的 LLVM 和 Clang 工具链，但是如果你想模拟如何在内部使用它，则可尝试进行实验，如[是否可以结合使用 C++/WinRT 和 LLVM/Clang 进行编译？](faq.md#can-i-use-llvmclang-to-compile-with-cwinrt)中所述。|
| C++ 编译器为投影类型生成“没有适当的默认构造函数”  。 | 如果试图延迟运行时类对象的初始化，或者在同一个项目中使用和实现运行时类，则需要调用 **std::nullptr_t** 构造函数。 有关详细信息，请参阅[通过 C++/WinRT 使用 API](consume-apis.md)。 |
| C++ 编译器生成“错误 C3861:'from_abi'：未找到标识符”，以及源自 base.h 的其他错误   。 如果使用 Visual Studio 2017（版本 15.8.0 或更高版本），并且要面向 Windows SDK 版本 10.0.17134.0（Windows 10 版本 1803），则可能会看到此错误。 | 要么定位 Windows SDK 的更新（更符合）版本，要么设置项目属性“C/C++” > “语言” > “一致性模式：   否”（此外，如果“/permissive-”出现在“其他选项”下的项目属性“C/C++” > “语言” > “命令行”中，将其删除）**      。 |
| C++ 编译器会生成“错误 C2039:IUnknown: 不是 \`global namespace 的成员”* 。 | 请参阅[如何将 C++/WinRT 项目重新定位到更高版本的 Windows SDK](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)。 |
| C++ 链接器生成“错误 LNK2019: 函数 _VSDesignerCanUnloadNow@0 中引用了未解析的外部符号 _WINRT_CanUnloadNow@0”  | 请参阅[如何将 C++/WinRT 项目重新定位到更高版本的 Windows SDK](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)。 |
| 生成过程生成错误消息“C++/WinRT VSIX 不再提供项目生成支持。请将项目引用添加到 Microsoft.Windows.CppWinRT Nuget 包”* 。 | 将 “Microsoft.Windows.CppWinRT”NuGet 包安装到项目中  。 有关详细信息，请参阅 [VSIX 扩展的早期版本](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)。 |
| C++ 链接器生成“错误 LNK2019: 未解析的外部符号”，并提及“winrt::impl::consume_Windows_Foundation_Collections_IVector”   。 | 从 [C++/WinRT 2.0](news.md#news-and-changes-in-cwinrt-20) 开始，如果在 Windows 运行时集合上使用基于范围的 `for`，那么现在需要 `#include <winrt/Windows.Foundation.Collections.h>`。 |
| C++ 编译器会生成“错误 C4002:  类函数宏的调用 GetCurrentTime 参数太多”。 | 请参阅[如何使用 GetCurrentTime 和/或 TRY 解析多义性？](faq.md#how-do-i-resolve-ambiguities-with-getcurrenttime-andor-try)。 |
| C++ 编译器生成“错误 C2334: '{' 的前面有意外标记；跳过明显的函数体”  。 | 请参阅[如何使用 GetCurrentTime 和/或 TRY 解析多义性？](faq.md#how-do-i-resolve-ambiguities-with-getcurrenttime-andor-try)。 |
| C++ 编译器生成“winrt::impl::produce&lt;D,I&gt; 无法实例化抽象类，因为缺少 GetBindingConnector  ”。 | 你需要 `#include <winrt/Windows.UI.Xaml.Markup.h>`。 |
| C++ 编译器生成“错误 C2039: 'promise_type': 不是 'std::experimental::coroutine_traits<void>' 的成员  ”。 | 协同例程需要返回异步操作对象或 **winrt::fire_and_forget**。 请参阅[并发操作和异步操作](concurrency.md)。 |
| 项目生成“'PopulatePropertyInfoOverride' 的访问不明确  ”。 | 在 IDL 中声明一个基类，同时在 XAML 标记中声明一个不同的基类时，可能发生此错误。 |
| 首次加载 C++/WinRT 解决方案时生成“项目 'MyProject.vcxproj' 的配置 'Debug\|x86' 的设计时生成失败。  IntelliSense 可能不可用。”。 | 在首次生成后，此 IntelliSense 问题会解决。 |
| 在注册委托时尝试指定 [**winrt::auto_revoke**](/uwp/cpp-ref-for-winrt/auto-revoke-t) 会生成 [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) 异常。 | 请参阅[如果“自动撤销”委托无法注册](handle-events.md#if-your-auto-revoke-delegate-fails-to-register)。 |

> [!NOTE]
> 如果此主题未回答你的问题，则可以通过访问 [Visual Studio C++ 开发人员社区](https://developercommunity.visualstudio.com/spaces/62/index.html)或使用 [Stack Overflow 上的 `c++-winrt` 标记](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt)获得帮助。
