---
author: stevewhims
description: 无论你是要削减新代码还是要移植现有应用，本主题中的症状排查和补救措施表都可能对你有帮助。
title: C++/WinRT 问题疑难解答
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 疑难解答, HRESULT, 错误
ms.localizationpriority: medium
ms.openlocfilehash: 4129c50a2273c8ac425f6ea972898aa09fe0fcf3
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2018
ms.locfileid: "3983042"
---
# <a name="troubleshooting-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-issues"></a>[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 问题疑难解答
> [!NOTE]
> 有关 C++/WinRT Visual Studio Extension (VSIX)（提供项目模板支持以及 C++/WinRT MSBuild 属性和目标）的安装和使用的信息，请参阅[针对 C++/WinRT 以及 VSIX 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

将本主题放在前面是为了让你可以立即意识到它，即使你现在尚不需要它。 无论你是要削减新代码还是要移植现有应用，下面的症状排查和补救措施表都可能对你有帮助。 如果你进行移植，并且迫不及待想要取得进展并到达项目生成和运行的阶段，则你可以通过注释掉或去掉任何导致出现问题的非必要代码并在稍后回来进行修补的方式来暂时向前继续。

有关常见问题的列表，请参阅[常见问题](faq.md)。

## <a name="tracking-down-xaml-issues"></a>跟踪 XAML 问题
XAML 分析异常可能很难进行诊断，特别是在此类异常中没有含义明确的错误消息时。 请确保已将调试程序配置为捕获第一轮异常（以便试图捕获早期的分析异常）。 你可以检查调试程序中的异常变量，以确定 HRESULT 或消息中是否具有任何有用的信息。 也可以检查 Visual Studio 的输出窗口，以获取由 XAML 分析器输出的错误消息。

如果应用终止，并且你只知道在 XAML 标记分析过程中引发了未经处理的异常，则此错误可能是由于引用（通过键）了缺失的资源导致的。 或者，它可能是在 UserControl、自定义控件或自定义布局面板内部引发的异常。 最后一项措施是进行二进制拆分。 从 XAML 页面中删除大约一半标记并重新运行应用。 然后，你将知道错误是在已删除的那一半中（你现在应该在任何情况下恢复）还是在未删除的那一半中。 通过拆分包含错误的那一半来重复此过程，依此类推，直到完全解决了问题。

## <a name="symptoms-and-remedies"></a>症状和补救方法
| 症状 | 补救方法 |
|---------|--------|
| 在运行时引发异常，并显示 HRESULT 值 REGDB_E_CLASSNOTREGISTERED。 | 此错误的一个原因是 Windows 运行时组件无法加载。 请确保该组件的 Windows 运行时元数据文件 (`.winmd`) 与组件二进制文件 (`.dll`) 的名称相同，这也是项目名称和根命名空间的名称。 此外请确保生成过程已将 Windows 运行时元数据和二进制文件正确地复制到使用应用的 `Appx` 文件夹。 同时确认使用应用的 `AppxManifest.xml`（也在 `Appx` 文件夹中）包含一个正确声明了可激活的类和二进制文件名称的 **&lt;InProcessServer&gt;** 元素。 如果你错误地通过投影类型的默认构造函数实例化了一个在本地实现的运行时类，则也会发生此错误。 请参阅 [XAML 控件; 绑定到 C++/WinRT 属性](binding-property.md) 以了解有关在这种情况下如何正确使用投影类型的更多信息。 |
| C++ 编译器产生错误“*‘implements_type’: 不是‘&lt;投影类型&gt;’* 的任何直接或间接基类的成员”。 | 当你使用实现类型的未限定命名空间的名称（例如 **MyRuntimeClass**）来调用 **make** 并且没有包括该类型的标头时，将会出现此错误。 编译器会将 **MyRuntimeClass** 解释为投影类型。 解决办法是包括实现类型的标头（例如 `MyRuntimeClass.h`）。 |
| C++ 编译器产生错误“*正在尝试引用已删除的函数*”。 | 当你调用 **make** 并且你作为模板参数传递的实现类型具有 `= delete` 默认构造函数时，将会出现此错误。 编辑实现类型的标头文件并将 `= delete` 更改为 `= default`。 你还可以为运行时类添加一个构造函数到 IDL 中。 |
| 你已经实现 [**INotifyPropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged)，但 XAML 绑定没有更新（UI 没有订阅 [**PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)）。 | 请记得在 XAML 标记中的绑定表达式上设置 `Mode=OneWay`（或 TwoWay）。 请参阅 [XAML 控件; 绑定到 C++/WinRT 属性](binding-property.md)。 |
| 你将一个 XAML 项目控件绑定到一个可观测集合，在运行时引发了异常并显示消息“参数不正确”。 | 在 IDL 和实现中，将任何可观测的集合声明为 **Windows.Foundation.Collections.IVector<IInspectable>**。 但返回一个实现 **Windows.Foundation.Collections.IObservableVector<T>**（其中的 T 是元素类型）的对象。 请参阅 [XAML 项目控件; 绑定到 C++/WinRT 集合](binding-collection.md)。  |
| C++ 编译器产生以下形式的错误“*‘MyImplementationType_base&lt;MyImplementationType&gt;’: 没有适当的默认构造函数可用*”。|当你从具有特殊构造函数的类型派生时会出现此错误。 派生类型的构造函数需要传递基类型的构造函数所需的参数。 有关工作示例，请参阅[从具有特殊构造函数的类型派生](author-apis.md#deriving-from-a-type-that-has-a-non-trivial-constructor)。|
| C++ 编译器产生错误“*无法从‘const std::vector&lt;std::wstring,std::allocator&lt;_Ty&gt;&gt;’转换为‘const winrt::param::async_iterable&lt;winrt::hstring&gt; &’*”。|当你将 std::wstring 的 std::vector 传递给需要一个集合的 Windows 运行时 API 时，将会出现此错误。 有关更多信息，请参阅[标准 C++ 数据类型和 C++/WinRT](std-cpp-data-types.md)。|
| C++ 编译器产生错误“*无法从‘const std::vector&lt;winrt::hstring,std::allocator&lt;_Ty&gt;&gt;’转换为‘const winrt::param::async_iterable&lt;winrt::hstring&gt; &'*”。|当你将 winrt::hstring 的 std::vector 传递给需要一个集合的异步 Windows 运行时 API 并且你没有将相应的矢量复制或移动到异步被调用方时，将会出现此错误。 有关更多信息，请参阅[标准 C++ 数据类型和 C++/WinRT](std-cpp-data-types.md)。|
| 当打开项目时，Visual Studio 产生错误“*该项目的应用程序未安装*”。|你需要从 Visual Studio 的**新建项目**对话框中安装 **用于 C++ 开发的 Windows 通用工具**（如果你尚未这样做的话）。 如果上述方法未能解决问题，则项目可能依赖于 C++/WinRT Visual Studio Extension (VSIX)（请参阅 [Visual Studio 对于 C++/WinRT 和 VSIX 的支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)）。|
| Windows 应用认证工具包测试将产生一个错误，表示一个运行时类“*不是派生自 Windows 基类。所有可组合类必须最终派生自 Windows 命名空间中的类型*”。|任何运行时类 （在你的应用程序中声明） 从基类派生被称为*可组合*类。 可组合类的最终基类必须是源自 windows.* 命名空间; 类型例如， [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject)。 请参阅[XAML 控件; 绑定到 C + + /winrt 属性](binding-property.md)更多详细信息。|
| 对于 EventHandler 或 TypedEventHandler 委托专用化，C++ 编译器产生“*必须是 WinRT 类型*”错误。|请考虑改为使用 **winrt::delegate&lt;…T&gt;**。 请参阅 [在 C++/WinRT 中创作事件](author-events.md)。|
| 对于 Windows 运行时异步操作专用化，C++ 编译器产生“*必须是 WinRT 类型*”错误。|请考虑改为返回并行模式库 (PPL) [**任务**](https://msdn.microsoft.com/library/hh750113)。 请参阅[并发操作和异步操作](concurrency.md)。|
| C++ 编译器产生“*错误 C2220: 视为错误的警告 - 未生成‘object’文件*”。|更正警告，或者将 **C/C++** > **常规** > **将警告视为错误**设置为**否 (/WX-)**。|
| 应用发生崩溃，因为在 C++/WinRT 对象销毁后调用了其中的一个事件处理程序。|请参阅[在事件处理程序中使用 *this* 对象](handle-events.md#using-the-this-object-in-an-event-handler)。|
| C++ 编译器产生“*错误 C2338: 此项仅用于弱引用支持*”。|你请求针对某个类型的弱引用，该类型将 **winrt::no_weak_ref** 标记结构作为模板参数传递给其基类。 请参阅[选择退出弱引用支持](weak-references.md#opting-out-of-weak-reference-support)|
| C + + 链接器产生"*错误 LNK2019： 无法解析的外部符号*"|请参阅[链接器为什么遇到 me"LNK2019： 无法解析的外部符号"错误？](faq.md#why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error)|
| LLVM 和 Clang 工具链产生错误时使用 C + + WinRT。|我们不支持 LLVM 和 Clang 工具链 C + + WinRT，但如果你想要模拟我们如何使用它在内部，则可以尝试的实验，如一中所述[可以使用 LLVM/Clang 编译使用 C + + WinRT？](faq.md#can-i-use-llvmclang-to-compile-with-cwinrt)。|
| C + + 编译器产生"*没有适当的默认构造函数可用*"投影类型。 | 如果你尝试延迟初始化的运行时类对象，或使用，并可在同一项目中，实现的运行时类，则你将需要调用`nullptr_t`构造函数。 有关详细信息，请参阅[通过 C++/WinRT 使用 API](consume-apis.md)。 |
| C + + 编译器产生"*错误 C3861: 'from_abi': 找不到标识符*"，和源自*base.h*其他错误。 你可能会看到此错误，如果你使用的 Visual Studio 2017 (版本 15.8.0 或更高版本)，并面向 Windows SDK 版本 10.0.17134.0(windows 10，版本 1803年)。 | 无论是面向更高版本的 （更多一致） 版本的 Windows SDK 或设置项目属性**C/c + +** > **语言** > **一致性模式： 否**(另外，如果 **/ 许可-** 出现在项目属性**C/c + +**  > **语言** > **命令行**下**的其他选项**，然后将其删除)。 |

> [!NOTE]
> 如果本主题没有解决你的问题，你可以[在 Stack Overflow 上使用 `c++-winrt` 标签](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt)查找帮助。
