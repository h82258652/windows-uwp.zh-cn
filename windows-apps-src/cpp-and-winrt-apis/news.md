---
description: C++/WinRT 的新增功能和更改。
title: 新增功能C++/WinRT
ms.date: 04/02/2019
ms.topic: article
keywords: windows 10、 uwp、 标准版、 c + +、 cpp、 winrt、 投影、 新闻、 什么的新
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8ee10450a7a346c1ae032240aaecc65e7f87822d
ms.sourcegitcommit: 940645c705865ba9635ccae2da9d917420faf608
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2019
ms.locfileid: "58812606"
---
# <a name="whats-new-in-cwinrt"></a>新增功能C++/WinRT

## <a name="news-and-changes-in-cwinrt-20"></a>新闻和更改，在C++WinRT 2.0

有关详细信息[ C++WinRT Visual Studio 扩展 (VSIX)](https://aka.ms/cppwinrt/vsix)，则[Microsoft.Windows.CppWinRT NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)，并`cppwinrt.exe`工具&mdash;其中包括如何获取并安装它们&mdash;请参阅[适用于 Visual Studio 支持C++/WinRT、 XAML、 VSIX 扩展中，和 NuGet 包](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

### <a name="changes-to-the-cwinrt-visual-studio-extension-vsix-for-version-20"></a>将更改为C++WinRT Visual Studio 扩展 (VSIX) 版本 2.0

- 调试可视化工具现在支持 Visual Studio 2019;以及继续支持 Visual Studio 2017。
- 进行了大量 bug 修复。

### <a name="changes-to-the-microsoftwindowscppwinrt-nuget-package-for-version-20"></a>对 2.0 版的 Microsoft.Windows.CppWinRT NuGet 包的更改

- `cppwinrt.exe`工具现在包括在 Microsoft.Windows.CppWinRT NuGet 包，并且该工具将生成按需每个项目的平台投影标头。 因此，`cppwinrt.exe`工具不再依赖于 Windows SDK （尽管该工具仍附带有适用于兼容的原因的 SDK）。
- `cppwinrt.exe` 现在会生成每个特定于平台/配置的中间文件夹下 ($IntDir) 以启用并行生成的投影标头。
- C++/WinRT 生成支持 （属性/目标） 现在可以完整存档，以防你想要手动自定义你的项目文件。 请参阅[Microsoft.Windows.CppWinRT NuGet 包](https://github.com/Microsoft/xlang/tree/user/sjones/cppwinrt_nuget/src/package/nuget)。
- 进行了大量 bug 修复。

### <a name="changes-to-cwinrt-for-version-20"></a>将更改为C++版本 2.0 /WinRT

#### <a name="open-source"></a>开放源代码

`cppwinrt.exe`工具使用 Windows 运行时元数据 (`.winmd`) 文件，并从其生成基于标头文件的标准C++库的*项目*元数据中所述的 Api。 这样一来，可以使用这些 Api 从在C++/WinRT 代码。

该工具现在是一个完全开放源代码项目，GitHub 上提供。 请访问[Microsoft\/xlang](https://github.com/Microsoft/xlang)，然后单击到中**src** > **工具** > **cppwinrt**。

#### <a name="xlang-libraries"></a>xlang 库

（有关分析 Windows 运行时使用的 ECMA-335 元数据格式） 的完全可移植的纯标头库窗体的所有 Windows 运行时和工具今后 xlang 的基础。 值得注意的是，我们还重写`cppwinrt.exe`工具从一开始使用 xlang 库。 这提供了更准确的元数据查询，解决一些长期存在的问题C++/WinRT 语言投影。

#### <a name="fewer-dependencies"></a>较少依赖项

由于 xlang 元数据读取器，`cppwinrt.exe`工具本身具有较少依赖项。 这使得它更灵活，以及正在可在更多方案中使用&mdash;尤其是在受约束构建环境。 值得注意的是，它不再依赖于`RoMetadata.dll`。
 
这些是为依赖项`cppwinrt.exe`2.0。
 
- api-ms-win-core-processenvironment-l1-1-0.dll
- api-ms-win-core-libraryloader-l1-2-0.dll
- XmlLite.dll
- api-ms-win-core-memory-l1-1-0.dll
- api-ms-win-core-handle-l1-1-0.dll
- api-ms-win-core-file-l1-1-0.dll
- SHLWAPI.dll
- ADVAPI32.dll
- KERNEL32.dll
- api-ms-win-core-rtlsupport-l1-1-0.dll
- api-ms-win-core-processthreads-l1-1-0.dll
- api-ms-win-core-heap-l1-1-0.dll
- api-ms-win-core-console-l1-1-0.dll
- api-ms-win-core-localization-l1-2-0.dll

对比使用这些依赖关系，其中`cppwinrt.exe`1.0 存在。

- ADVAPI32.dll
- SHELL32.dll
- api-ms-win-core-file-l1-1-0.dll
- XmlLite.dll
- api-ms-win-core-libraryloader-l1-2-0.dll
- api-ms-win-core-processenvironment-l1-1-0.dll
- RoMetadata.dll
- SHLWAPI.dll
- KERNEL32.dll
- api-ms-win-core-rtlsupport-l1-1-0.dll
- api-ms-win-core-heap-l1-1-0.dll
- api-ms-win-core-timezone-l1-1-0.dll
- api-ms-win-core-console-l1-1-0.dll
- api-ms-win-core-localization-l1-2-0.dll
- OLEAUT32.dll
- api-ms-win-core-winrt-error-l1-1-0.dll
- api-ms-win-core-winrt-error-l1-1-1.dll
- api-ms-win-core-winrt-l1-1-0.dll
- api-ms-win-core-winrt-string-l1-1-0.dll
- api-ms-win-core-synch-l1-1-0.dll
- api-ms-win-core-threadpool-l1-2-0.dll
- api-ms-win-core-com-l1-1-0.dll
- api-ms-win-core-com-l1-1-1.dll
- api-ms-win-core-synch-l1-2-0.dll 

#### <a name="the-windows-runtime-noexcept-attribute"></a>Windows 运行时`noexcept`属性

Windows 运行时有一个新的`[noexcept]`属性，可用于修饰方法和属性中的[MIDL 3.0](/uwp/midl-3/predefined-attributes)。 该属性是否存在指示支持您的实现不会引发异常的工具 (也不会返回失败 HRESULT)。 这允许语言投影以优化代码生成通过避免支持都有可能失败的应用程序二进制接口 (ABI) 调用所需的异常处理开销。

C++/ WinRT 通过生成来充分利用此C++`noexcept`同时使用和创作代码的实现。 如果你有 API 方法或属性失败，而且您要关注代码大小，则可以调查此属性。

#### <a name="optimized-code-generation"></a>优化的代码生成

C++/ WinRT 现在会生成更高效C++源的代码 （在后台） 因此，C++编译器可以生成的最小和最高效二进制代码可能。 适用于降低成本的异常处理的许多改进通过避免不必要的展开信息。 使用大量的二进制文件C++/WinRT 代码将看到大约 4%减少代码大小。 代码也是更高效 （其运行速度更快） 由于精简的指令计数。

上一个新的互操作功能，是提供给你，也依赖于这些改进。 所有C++/现在都是资源所有者的 WinRT 类型包括直接，取得所有权的构造函数： 避免上一个两步方法。

```cppwinrt
ABI::Windows::Foundation::IStringable* raw = ...

IStringable projected(raw, take_ownership_from_abi);

printf("%ls\n", projected.ToString().c_str());
```

#### <a name="optimized-exception-handling-eh-code-generation"></a>优化的异常处理 (EH) 代码生成

此更改进行了补充完成了通过 Microsoft 的工作C++优化器团队能够减小异常处理的成本。 如果在代码中的很大程度使用 （如 COM) 的应用程序二进制接口 (Abi)，然后你将注意到大量的代码遵循这种模式。

```cpp
int32_t Function() noexcept
{
    try
    {
        // code here constitutes unique value.
    }
    catch (...)
    {
        // code here is always duplicated.
    }
}
```

C++/ WinRT 本身为实现每个 API 生成此模式。 具有数千个 API 函数，此处的任何优化可能很大。 在过去，则优化器不会检测到的 catch 块是都相同，因此它复制大量的每个 ABI （这又使得到的系统代码中使用异常产生大型二进制文件的理念基础） 周围的代码。 但是，从 Visual Studio 2019 上，C++编译器折叠所有这些捕获 funclets，并只存储都是唯一的。 结果是非常依赖这种模式的二进制文件的代码大小进一步和整体减少 18%。 不仅是 EH 代码现在比使用返回代码，更高效，而且还更大的二进制文件有关的问题现已成为过去。

#### <a name="incremental-build-improvements"></a>增量生成改进

`cppwinrt.exe`工具现在将针对在磁盘上，任何现有文件的内容生成的标头/源代码文件的输出进行比较，它仅将写出文件如果实际上已更改文件。 这将相当长的时间保存与磁盘 I/O，并确保文件不被视为"脏"的C++编译器。 结果是避免使用，或减少，在许多情况下，重新编译。

#### <a name="generic-interfaces-are-now-all-generated"></a>泛型接口现在是所有生成

由于 xlang 元数据读取器， C++/WinRT 现在从元数据中生成所有的参数化，或泛型接口。 接口如[Windows::Foundation::Collections::IVector\<T\> ](/uwp/api/windows.foundation.collections.ivector_t_)是现在从元数据生成而不是用手编写`winrt/base.h`。 结果是的大小`winrt/base.h`已缩短了一半，并生成优化右键到代码 （这是比较棘手，如何处理手动方法）。

> [!IMPORTANT]
> 接口，如给出的示例现在显示在其各自的命名空间标头，而不是在`winrt/base.h`。 因此，如果尚未这样做，您将必须包含相应的命名空间的标头，以便使用接口。

#### <a name="component-optimizations"></a>组件优化

此更新添加了对有关的几个其他参加优化支持C++/WinRT，在以下各节所述。 由于这些优化重大更改 （这可能需要进行次要更改，以便支持），您需要将其使用显式`cppwinrt.exe`工具的`-opt`标志。

（从项目模板） 的新项目将使用`-opt`默认情况下。

##### <a name="uniform-construction-and-direct-implementation-access"></a>统一构造和直接实现访问

这些两个优化允许对其自己的实现类型，在组件直接访问，即使它只使用投影的类型。 若要使用无需[**使**](/uwp/cpp-ref-for-winrt/make)， [ **make_self**](/uwp/cpp-ref-for-winrt/make-self)，也不[ **get_self** ](/uwp/cpp-ref-for-winrt/get-self)如果你只是想要使用公共 API 外围应用。 你的调用会直接调用到实现中，向下编译和那些甚至可能完全内联。

##### <a name="type-erased-factories"></a>类型擦除工厂

此优化可避免 #include 中的依赖关系`module.g.cpp`，以便它需要不会重新编译每次任何单个实现类发生更改。 结果是改进的生成性能。

#### <a name="smarter-and-more-efficient-modulegcpp-for-large-projects-with-multiple-libs"></a>更智能、 更高效`module.g.cpp`对于具有多个库的大型项目

`module.g.cpp`文件现在还包含两个其他可组合的帮助程序，名为**winrt_can_unload_now**，和**winrt_get_activation_factory**。 这些设计为较大的项目，一个 DLL 组成数 libs，每个都有其自己的运行时类。 在这种情况下，您需要手动将拼结在一起的 DLL **DllGetActivationFactory**并**DllCanUnloadNow**。 这些帮助程序使其大大简化了为此，请通过避免虚假资助创始费错误。 `cppwinrt.exe`工具的`-lib`标志还可用于为每个单个 lib 提供其自己的前导码 (而非`winrt_xxx`)，以便每个 lib 函数可能会单独命名，并因此结合使用明确。

#### <a name="new-winrtcoroutineh-header"></a>新`winrt/coroutine.h`标头

`winrt/coroutine.h`标头是的新主服务器的所有C++/WinRT 的协同程序支持。 这种支持以前，驻留在几个地方，我们认为这太有一定的局限性。 由于现在生成 Windows 运行时异步接口，而不是手动编写的它们现在位于`winrt/Windows.Foundation.h`。 除了作为更易维护且受支持，这意味着该协同例程帮助程序，例如[ **resume_foreground** ](/uwp/cpp-ref-for-winrt/resume-foreground)不再需要附加到特定的命名空间标头的末尾。 相反，它们可以更随意地包括其依赖项。 这进一步使**resume_foreground**若要支持不仅恢复上给定[ **Windows::UI::Core::CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher)，但它可以现在还支持在恢复给定[ **Windows::System::DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue)。 以前，无法支持只有一个;但不是同时，由于定义仅可以驻留在一个命名空间中。

下面是举例**DispatcherQueue**支持。

```cppwinrt
fire_and_forget Async(DispatcherQueueController controller)
{
    bool queued = co_await resume_foreground(controller.DispatcherQueue());
    assert(queued);

    // This is just to simulate queue failure...
    co_await controller.ShutdownQueueAsync();

    queued = co_await resume_foreground(controller.DispatcherQueue());
    assert(!queued);
}
```

协同程序帮助程序现在还使用了修饰`[[nodiscard]]`，从而提高其可用性。 如果你忘记 （或没有意识到必须）`co_await`它们，以使其，则由于`[[nodiscard]]`，这类错误现在会生成编译器警告。

#### <a name="help-with-diagnosing-stack-allocations"></a>帮助诊断堆栈分配

由于计划和实现类名称 （默认情况下） 的信息是相同的并且仅由命名空间不同，它是误以为另一个，并为意外地在堆栈中，创建一个实现，而不是使用[ **使**](/uwp/cpp-ref-for-winrt/make)系列的帮助程序。 这可能很难在某些情况下，诊断，因为中仍未完成的引用时，可能会销毁该对象。 断言现在将选取此，对于调试版本。 断言不会检测在协同程序内的堆栈分配，而仍有助于捕获大多数这类错误。

#### <a name="improved-capture-helpers-and-variadic-delegates"></a>改进了的捕获帮助器和可变参数委托

通过支持投影的类型，此更新修复的捕获帮助器的限制。 它是随不时 Windows 运行时互操作 Api，当它们返回投影的类型。

此更新还添加了对支持[ **get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)并[ **get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)创建可变参数 （非 Windows 运行时） 委托时。

#### <a name="support-for-deferred-destruction-and-safe-qi-during-destruction"></a>对延迟的析构和安全 QI 析构期间的支持

XAML 应用程序可能会本身由于需要执行的难度[ **QueryInterface** ](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) (QI) 中的析构函数，以便向上或向层次结构下调用一些清理实现。 但是，调用涉及 QI 之后已有对象的引用计数, 归零。 此更新添加了对 debouncing 引用计数，确保一旦它达到零时它能够永远不会重新; 支持同时仍允许在析构过程所需的任何临时 QI。 此过程是不可避免的中某些 XAML 应用程序/控件，并C++/WinRT 是现在适应它。

可以通过提供一个静态延迟析构**final_release**函数，并移动的所有权**unique_ptr**到某些其他上下文。

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    hstring ToString()
    {
        return L"Sample";
    }

    ~Sample()
    {
        // Called when the unique_ptr below is reset.
    }

    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        // Move 'ptr' as needed to delay destruction.
    }
};
```

在以下示例中，一次**MainPage** （适用于最后一次），发布**final_release**调用。 函数所用 （在线程池），等待五秒，然后恢复使用页面的**调度程序**（需要 QI/AddRef/Release 工作）。 然后它将清除**unique_ptr**，这将导致**MainPage**实际调用的析构函数。 即使此处**DataContext**调用时，这要求为 QI **IFrameworkElement**。 显然，您无需实现你**final_release**作为协同例程。 工作原理，但它可以非常轻松地将析构移到另一个线程。

```cppwinrt
struct MainPage : PageT<MainPage>
{
    MainPage()
    {
    }

    ~MainPage()
    {
        DataContext(nullptr);
    }

    static IAsyncAction final_release(std::unique_ptr<MainPage> ptr)
    {
        co_await 5s;

        co_await resume_foreground(ptr->Dispatcher());

        ptr = nullptr;
    }
};
```

#### <a name="improved-support-for-com-style-single-interface-inheritance"></a>改进了对 COM 样式单个接口继承支持

以及用于 Windows 运行时编程中， C++/WinRT 还用于创作和使用仅限 COM 的 Api。 此更新，使可能实现 COM 服务器，其中存在的接口层次结构。 这不是必需的 Windows 运行时;但某些 COM 实现所必需的。

#### <a name="correct-handling-of-out-params"></a>正确处理的`out`params

它可能难以使用`out`params; 尤其是 Windows 运行时数组。 利用此更新， C++/WinRT 是相当更为可靠和灵活应对错误时`out`params 和数组; 是否通过语言投影，或从 COM 开发人员谁在使用原始的 ABI，和人员到达这些参数不一致地初始化变量的犯错误。 在任一情况下， C++/WinRT 现在采取适当的措施就移交投影的类型给 ABI （通过记住释放任何资源），并谈到清零或清除到达 abi 的参数。

#### <a name="events-now-handle-invalid-tokens-reliably"></a>事件现在可靠地处理无效令牌

[ **Winrt::event** ](/uwp/cpp-ref-for-winrt/event)现在，实现适当地处理这种情况其中其**删除**方法调用具有无效的令牌值 (值中不存在数组）。

#### <a name="coroutine-locals-are-now-destroyed-before-the-coroutine-returns"></a>协同程序返回之前立即销毁协同程序的局部变量

实现协同例程类型的传统方法可能会允许在协同程序内的局部变量要销毁*后*在协同程序返回/完成 （而不是最终挂起之前）。 为了避免此问题和产生其他权益时，任何等待应用程序的恢复现在被延迟到最后一个挂起。

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>新闻和更改，请在 Windows SDK 版本 10.0.17763.0 (Windows 10，版本 1809年)

下表包含新闻，并将更改为[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)在最新公开发布版本的 Windows SDK，这是 10.0.17763.0 (Windows 10，版本 1809年)。 这些更改还可能会更高版本的 SDK Insider Preview 版本中存在。

| 新的或已更改功能 | 详细信息 |
| - | - |
| **重大更改**。 为其进行编译， C++/WinRT 不依赖于从 Windows SDK 标头。 | 请参阅[与 Windows SDK 标头文件隔离](#isolation-from-windows-sdk-header-files)下文。 |
| Visual Studio 项目系统格式已更改。 | 请参阅[如何重定目标，在C++/WinRT 项目到更高版本的 Windows SDK](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)下面。 |
| 有新的函数和基类，这些类将帮助你将集合对象传递给 Windows 运行时函数，或实现自己的集合属性和集合类型。 | 请参阅[集合的C++/WinRT](collections.md)。 |
| 可以使用[{Binding}](/windows/uwp/xaml-platform/binding-markup-extension)与标记扩展在C++/WinRT 运行时类。 | 有关详细信息和代码示例，请参阅[数据绑定概述](/windows/uwp/data-binding/data-binding-quickstart)。 |
| 支持的取消协同程序，可注册取消回调。 | 有关详细信息和代码示例，请参阅[取消异步操作和取消回调](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks)。 |
| 在创建指向成员函数的委托，可以建立具有强名称或对当前对象的弱引用 (而不是原始*这*指针) 在处理程序注册所在的点。 | 有关详细信息和代码示例，请参阅**如果使用成员函数作为委托**部分中的子部分[安全地访问*这*一个事件处理委托的指针](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| Bug 已修复了发现通过 Visual Studio 的改进了符合C++标准。 LLVM 和 Clang 工具链也更好地利用来验证C++/WinRT 的标准符合性。 | 您将不会再遇到中所述的问题[我的新项目将不会为什么编译？我使用 Visual Studio 2017 (版本 15.8.0 或更高版本)，和 SDK 版本版本 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

其他更改。

- **重大更改**。 [**winrt::get_abi(winrt::hstring const&)** ](/uwp/cpp-ref-for-winrt/get-abi)现在将返回`void*`而不是`HSTRING`。 可以使用`static_cast<HSTRING>(get_abi(my_hstring));`获取 HSTRING。
- **重大更改**。 [**winrt::put_abi(winrt::hstring&)** ](/uwp/cpp-ref-for-winrt/put-abi)现在将返回`void**`而不是`HSTRING*`。 可以使用`reinterpret_cast<HSTRING*>(put_abi(my_hstring));`获取 HSTRING *。
- **重大更改**。 HRESULT 现在作为投影**winrt::hresult**。 如果您需要的 HRESULT （若要执行类型检查，或支持类型特征），然后，你可以`static_cast` **winrt::hresult**。 否则为**winrt::hresult**将转换为 HRESULT，只要您包括`unknwn.h`包括任何之前C++/WinRT 标头。
- **重大更改**。 GUID 现在作为投影**winrt::guid**。 对于您实现的 Api，必须使用**winrt::guid**的 GUID 参数。 否则为**winrt::guid**将转换为 GUID，只要您包括`unknwn.h`包括任何之前C++/WinRT 标头。
- **重大更改**。 [ **Winrt::handle_type 构造函数**](/uwp/cpp-ref-for-winrt/handle-type#handle_typehandle_type-constructor)已经强制写入通过使显式 （它是现在更难编写与其不正确的代码）。 如果需要分配原始句柄值，调用[ **handle_type::attach 函数**](/uwp/cpp-ref-for-winrt/handle-type#handle_typeattach-function)相反。
- **重大更改**。 签名**WINRT_CanUnloadNow**并**WINRT_GetActivationFactory**已更改。 您根本不能声明这些函数。 相反，包括`winrt/base.h`(如果包括任何即自动包含C++WinRT Windows 命名空间的标头文件) 来包含这些函数的声明。
- 有关[ **winrt::clock 结构**](/uwp/cpp-ref-for-winrt/clock)， **from_FILETIME/to_FILETIME**支持的弃用**from_file_time/to_file_time**。
- 需要 Api **IBuffer**参数进行了简化。 尽管大多数 Api 首选集合或数组，但足够 Api 依赖**IBuffer**需要更易使用此类 Api 从它C++。 此更新提供了直接访问数据背后**IBuffer**实现中，使用相同数据的命名约定使用的C++标准库容器。 这样还可以避免冲突元数据名称通常以大写字母开头。
- 改进了代码生成： 各种改进来减小代码大小，提高内联，并优化工厂缓存。
- 删除不必要的递归。 当命令行引用到一个文件夹中，而不是特定于`.winmd`，则`cppwinrt.exe`工具不再以递归方式搜索`.winmd`文件。 `cppwinrt.exe`工具现在还处理重复项更加智能化，使其更具弹性用户错误，并且为得不好正确`.winmd`文件。
- 强化的智能指针。 以前，无法撤消时事件 revokers 移动分配新值。 这有助于揭示了智能指针类不可靠地处理自我赋值; 的问题针对根部位于[ **winrt::com_ptr 结构模板**](/uwp/cpp-ref-for-winrt/com-ptr)。 **winrt::com_ptr**已解决和修复，可处理事件 revokers 移动语义正确，以便它们会吊销分配时。

> [!IMPORTANT]
> 对进行了重要更改[ C++WinRT Visual Studio 扩展 (VSIX)](https://aka.ms/cppwinrt/vsix)，在版本 1.0.181002.2，并且以后版本 1.0.190128.4 中。 有关这些更改，以及它们如何影响现有项目的详细信息[适用于 Visual Studio 支持C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)并[早期版本的 VSIX 扩展](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)。

### <a name="isolation-from-windows-sdk-header-files"></a>从 Windows SDK 标头文件隔离

这可能是一项重大更改为你的代码。

为其进行编译， C++/WinRT 不再依赖于从 Windows SDK 标头文件。 C 运行时库 (CRT) 中的标头文件和C++标准模板库 (STL) 也不包含任何 Windows SDK 标头。 并且改进了标准符合性、 可避免意外的依赖项，并极大地减少了的宏，您必须防止出现。

这种独立性意味着， C++/WinRT 是现在更多的可移植且符合，标准，它进一步增强能力就有可能成为跨编译器和跨平台库。 这也意味着， C++/WinRT 标题并不是受产生负面影响的宏。

如果您以前保留到C++/WinRT 包含在项目中，任何 Windows 标头，则现在需要将其包含自己。 它是，在任何情况下，始终最佳做法来显式包括所依赖的标头并不将其留到另一个库，以便将它们包含。

目前，Windows SDK 标头文件隔离到唯一的例外是内部函数，和的数字。 没有这些最后一个剩余依赖关系与任何已知的问题。

在项目中，您可以在需要时重新启用与 Windows SDK 标头进行互操作。 您可能，例如，想要实现 COM 接口 (来源于[ **IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509))。 对于该示例，包括`unknwn.h`包括任何之前C++/WinRT 标头。 执行操作会导致C++/WinRT 基库启用各种挂钩，以支持经典 COM 接口。 有关代码示例，请参阅[作者 COM 组件与C++/WinRT](author-coclasses.md)。 同样，显式包含类型和/或你想要调用的函数声明任何其他 Windows SDK 标头。

### <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>如何重定目标，在C++到更高版本的 Windows SDK /WinRT 项目

重定目标的项目中可能会导致最少的编译器和链接器问题的方法也是最费力的。 该方法包括创建 （面向所选的 Windows SDK 版本） 的新项目，然后通过中将文件复制到新项目中，从你的旧版。 将你的旧版的部分`.vcxproj`和`.vcxproj.filters`文件，则可仅将超过复制以节省你在 Visual Studio 中添加文件。

但是，有两种方法可重定目标，Visual Studio 中的项目。

- 转到项目属性**常规** \> **Windows SDK 版本**，然后选择**所有配置**并**所有平台**。 设置**Windows SDK 版本**到想要面向的版本。
- 在中**解决方案资源管理器**，右键单击项目节点，单击**重定目标项目**，选择你想要为目标，然后依次的版本**确定**。

如果使用这两种方法之一后遇到任何编译器或链接器错误，则可以尝试清理解决方案 (**构建** > **清理解决方案**和/或手动删除所有临时文件夹和文件） 然后再尝试重新生成。

如果C++编译器会生成"*错误 C2039:IUnknown： 不是成员的 '\`全局命名空间'*"，然后添加`#include <unknwn.h>`到顶部你`pch.h`文件 (包括任何之前C++/WinRT 标头)。

您可能还需要添加`#include <hstring.h>`之后。

如果C++链接器生成"*错误 LNK2019： 无法解析的外部符号_WINRT_CanUnloadNow@0函数中引用_VSDesignerCanUnloadNow@0* "，则可以通过添加解析它`#define _VSDESIGNER_DONT_LOAD_AS_DLL`到你`pch.h`文件。
