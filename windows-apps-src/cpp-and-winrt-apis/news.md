---
description: C++/WinRT 的新增功能和更改。
title: C++/WinRT 中的新增功能
ms.date: 03/16/2020
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 新增功能, 功能, 新增
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3057a3d13ba1e7d368dd6bf8820710030687a04d
ms.sourcegitcommit: 7dcf74b11aa0cb2f3ff4ab10caf26ba769f96dfb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/04/2020
ms.locfileid: "80662400"
---
# <a name="whats-new-in-cwinrt"></a>C++/WinRT 中的新增功能

C++/WinRT 的后续版本发布后，本主题会介绍新增功能和变更的内容。

## <a name="rollup-of-recent-improvementsadditions-as-of-march-2020"></a>2020 年 3 月为止的最新改进/添加汇总

### <a name="up-to-23-shorter-build-times"></a>生成时间缩短了 23%

C++/WinRT 和 C++ 编译器团队共同合作，尽可能缩短生成时间。 我们全力钻研了编译器分析，以便发现如何重构 C++/WinRT 的内部机制来帮助 C++ 编译器消除编译时开销，以及如何改进 C++ 编译器本身来处理 C++/WinRT 库。 C++/WinRT 已针对编译器进行了优化；并且编译器已针对 C++/WinRT 进行了优化。

下面以一个有关生成预编译标头 (PCH) 的最糟糕情况为例，该标头包含每一单个 C++/WinRT 投影命名空间标头。

| 版本 | PCH 大小（字节） | 时间（秒） |
| - | - | - |
| 7 月版的 C++/WinRT，与 Visual C++ 16.3 配合使用 | 3,004,104,632 | 31 |
| 2\.0.200316.3 版的 C++/WinRT，与 Visual C++ 16.5 配合使用 | 2,393,515,336 | 24 |

大小减少 20%，生成时间减少 23%。

### <a name="improved-msbuild-support"></a>改进的 MSBuild 支持

我们投入了大量工作来改进 [MSBuild](/visualstudio/msbuild/msbuild?view=vs-2019) 支持，使其适合广泛选择的不同方案。

### <a name="even-faster-factory-caching"></a>更快的工厂缓存

我们改进了工厂缓存的内联，使其能够更好地内联热路径，从而提高执行速度。

此改进不会影响代码大小&mdash;如下面的[优化的 EH 代码生成](#optimized-exception-handling-eh-code-generation)中所述，如果应用程序频繁使用 C++ 异常处理，则可以使用 `/d2FH4` 选项收缩二进制文件，此选项在使用 Visual Studio 2019 16.3 和更高版本创建的新项目中默认启用。

### <a name="more-efficient-boxing"></a>更高效的装箱

在 XAML 应用程序中使用时，[**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) 现在会更高效（请参阅[装箱和取消装箱](/windows/uwp/cpp-and-winrt-apis/boxing)）。 执行大量装箱操作的应用程序的代码大小也会降低。

### <a name="support-for-implementing-com-interfaces-that-implement-iinspectable"></a>支持实现用于实现 IInspectable 的 COM 接口

如果需要实现一个只实现 [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable) 的（非 Windows 运行时）COM 接口，现在可以使用 C++/WinRT 来完成此任务。 请参阅[实现 IInspectable 的 COM 接口](https://github.com/microsoft/xlang/pull/603)。

### <a name="module-locking-improvements"></a>模块锁定改进

通过对模块锁定进行控制，现在可以实现自定义宿主方案以及消除模块级锁定。 请参阅[模块锁定改进](https://github.com/microsoft/xlang/pull/583)。

### <a name="support-for-non-windows-runtime-error-information"></a>对非 Windows 运行时错误消息的支持

某些 API（甚至某些 Windows 运行时 API）在未使用 Windows 运行时错误源 API 的情况下报告错误。 在这种情况下，C++/WinRT 现在将回退到使用 COM 错误信息。 请参阅[对非 WinRT 错误信息的 C++/WinRT 支持](https://github.com/microsoft/xlang/pull/582)。

### <a name="enable-c-module-support"></a>启用 C++ 模块支持 

C++ 模块支持已恢复，但仅处于实验形式。 此功能尚未在 C++ 编译器中完成。

### <a name="more-efficient-coroutine-resumption"></a>更高效的协同例程恢复

C++/WinRT 协同例程的性能良好，但我们继续寻找对其进行改进的方法。 请参阅[提高协同例程恢复的可伸缩性](https://github.com/microsoft/xlang/pull/546)。

### <a name="new-when_all-and-when_any-async-helpers"></a>全新 **when_all** 和 **when_any** 异步帮助程序

**when_all** 帮助程序函数会创建一个 [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) 对象，该对象在所有提供的 Awaitable 都已完成时完成。 当任何提供的 Awaitable 完成时，**when_any** 帮助程序将创建一个 **IAsyncAction**。 

请参阅[添加 when_any async 帮助程序](https://github.com/microsoft/xlang/pull/520)和[添加 when_all 异步帮助程序](https://github.com/microsoft/xlang/pull/516)。

### <a name="other-optimizations-and-additions"></a>其他优化和添加

此外，还引入了许多 bug 修复以及次要优化和添加，其中包括用于简化调试和优化内部机制及默认实现的各种改进。 单击下面的链接可获得详尽的列表：[https://github.com/microsoft/xlang/pulls?q=is%3Apr+is%3Aclosed](https://github.com/microsoft/xlang/pulls?q=is%3Apr+is%3Aclosed)。

## <a name="news-and-changes-in-cwinrt-20"></a>C++/WinRT 2.0 中的新增功能和更改

有关 [C++WinRT Visual Studio 扩展 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)、[Microsoft.Windows.CppWinRT NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) 和 `cppwinrt.exe` 工具的详细信息（包括如何获取和安装它们），请参阅[针对 C++/WinRT、XAML、VSIX 扩展和 NuGet 包的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

### <a name="changes-to-the-cwinrt-visual-studio-extension-vsix-for-version-20"></a>版本 2.0 的 C++WinRT Visual Studio 扩展 (VSIX) 更改

- 调试可视化工具现在支持 Visual Studio 2019；并且继续支持 Visual Studio 2017。
- 进行了大量 bug 修复。

### <a name="changes-to-the-microsoftwindowscppwinrt-nuget-package-for-version-20"></a>版本 2.0 的 Microsoft.Windows.CppWinRT NuGet 包更改

- `cppwinrt.exe` 工具现在包含在 Microsoft.Windows.CppWinRT NuGet 包中，并且该工具会按需为每个项目生成平台投影标头。 因此，`cppwinrt.exe` 工具不再依赖于 Windows SDK（不过由于兼容性原因，该工具仍附带 SDK）。
- `cppwinrt.exe` 现在会在每个特定于平台/配置的中间文件夹 ($IntDir) 下生成投影标头以实现并行生成。
- C++/WinRT 生成支持（属性/目标）现在可完整记录，以防要手动自定义项目文件。 请参阅 Microsoft.Windows.CppWinRT NuGet 包[自述文件](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing)。
- 进行了大量 bug 修复。

### <a name="changes-to-cwinrt-for-version-20"></a>版本 2.0 的 C++/WinRT 更改

#### <a name="open-source"></a>开放源代码

`cppwinrt.exe` 工具采用 Windows 运行时元数据 (`.winmd`) 文件，通过它生成基于头文件的标准 C++ 库，该库可投影  元数据中所述的 API。 这样便可以从 C++/WinRT 代码使用这些 API。

此工具现在是完全开放源代码的项目，可在 GitHub 上获取。 访问 [Microsoft\/cppwinrt](https://github.com/microsoft/cppwinrt)。

#### <a name="xlang-libraries"></a>xlang 库

一个完全可移植的纯标头库（用于分析 Windows 运行时使用的 ECMA-335 元数据格式）组成今后所有 Windows 运行时和 xlang 工具的基础。 值得注意的是，我们还使用 xlang 库彻底重写了 `cppwinrt.exe` 工具。 这可提供准确性显著更高的元数据查询，从而解决了一些在 C++/WinRT 语言投影方面长期存在的问题。

#### <a name="fewer-dependencies"></a>更少的依赖项

由于存在 xlang 元数据读取器，`cppwinrt.exe` 工具本身的依赖项更少。 这使它要灵活得多，以及可在更多方案中使用 &mdash; 尤其是在受约束的生成环境中。 值得注意的是，它不再依赖于 `RoMetadata.dll`。
 
以下这些是 `cppwinrt.exe` 2.0 的依赖项。
 
- ADVAPI32.dll
- KERNEL32.dll
- SHLWAPI.dll
- XmlLite.dll

所有这些 DLL 不仅在 Windows 10 上提供，而且在 Windows 7 甚至 Windows Vista 上提供。 如果你愿意，现在可以让运行 Windows 7 的旧版服务器运行 `cppwinrt.exe`，为项目生成 C++ 头文件。 如果你有兴趣，你甚至可以[在 Windows 7 上运行 C++/WinRT](https://github.com/kennykerr/win7)，只需完成少量工作即可。

将上面的列表与 `cppwinrt.exe` 1.0 具有的以下依赖项进行比较。

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

#### <a name="the-windows-runtime-noexcept-attribute"></a>Windows 运行时 `noexcept` 属性

Windows 运行时有一个新的 `[noexcept]` 属性，可用于在 [MIDL 3.0](/uwp/midl-3/predefined-attributes) 中修饰方法和属性。 存在该属性会向支持工具指示实现不会引发异常（也不会返回失败 HRESULT）。 这使语言投影可以避免支持可能失败的应用程序二进制接口 (ABI) 调用所需的异常处理开销，从而优化代码生成。

C++/WinRT 通过生成同时使用和创作代码的 C++ `noexcept` 实现来利用这一点。 如果具有不会失败的 API 方法或属性，并且关注代码大小，则可以调查此属性。

#### <a name="optimized-code-generation"></a>优化的代码生成

C++/WinRT 现在可生成更高效的 C++ 源代码（在后台），以便 C++ 编译器可以生成可能是最小和最高效的二进制代码。 许多改进是为了通过避免不必要的展开信息来降低异常处理的成本。 使用大量 C++/WinRT 代码的的二进制文件的代码大小会减少大约 4%。 代码也会由于指令数量减少而更加高效（运行速度更快）。

这些改进还依赖于一个可供使用的新互操作功能。 作为资源所有者的所有 C++/WinRT 类型现在都包括一个用于直接获得所有权的构造函数，从而避免以前的两步方法。

```cppwinrt
ABI::Windows::Foundation::IStringable* raw = ...

IStringable projected(raw, take_ownership_from_abi);

printf("%ls\n", projected.ToString().c_str());
```

#### <a name="optimized-exception-handling-eh-code-generation"></a>优化的异常处理 (EH) 代码生成

此更改补充了 Microsoft C++ 优化器团队为减少异常处理成本而进行的工作。 如果在代码中大量使用应用程序二进制接口 (ABI)（如 COM），则会观察到有许多代码遵循此模式。

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

C++/WinRT 本身会为实现的每个 API 都生成此模式。 借助数千个 API 函数，这里的任何优化都可能十分重大。 在过去，优化器不会检测到这些 catch 块全都相同，因此会围绕每个 ABI 复制大量代码（这进而促使人们相信在系统代码中使用异常会生成大型二进制文件）。 但是从 Visual Studio 2019 起，C++ 编译器会折叠所有这些 catch 函数，只存储唯一的函数。 这样便使非常依赖于此模式的二进制文件的代码大小在整体上进一步减少了 18%。 不仅是 EH 代码现在比使用返回代码更加高效，而且对二进制文件较大的担忧现在已成为过去。

#### <a name="incremental-build-improvements"></a>增量生成改进

`cppwinrt.exe` 工具现在生成的头文件/源文件的输出与磁盘上任何现有文件的内容进行比较，仅当文件实际上已更改时才会写出文件。 这可在磁盘 I/O 方面节省大量时间，并确保 C++ 编译器不会将文件视为“脏”文件。 这样便可在许多情况下避免（或减少）重新编译。

#### <a name="generic-interfaces-are-now-all-generated"></a>泛型接口现在全部是生成的

由于存在 xlang 元数据读取器，C++/WinRT 现在从元数据生成所有参数化（或泛型）接口。 诸如 [Windows::Foundation::Collections::IVector\<T\>](/uwp/api/windows.foundation.collections.ivector_t_) 这类接口现在是从元数据生成，而不是在 `winrt/base.h` 中手动编写。 其结果是 `winrt/base.h` 的大小缩减了一半，并且直接在代码中生成优化（这对于使用手动编写方法比较棘手）。

> [!IMPORTANT]
> 诸如提供的示例这类接口现在会出现在其各自的命名空间标头中，而不是在 `winrt/base.h` 中。 因此，如果尚未这样做，则必须包含相应的命名空间标头以便使用接口。

#### <a name="component-optimizations"></a>组件优化

此更新添加了对适用于 C++/WinRT 的几个附加选择加入优化（如以下各节所述）的支持。 由于这些优化是重大更改（你可能需要进行次要更改以便支持它们），因此需要显式启用它们。 在 Visual Studio 中，将项目属性“常见属性”   > “C++/WinRT”   > “已优化”  设置为“是”  。 该操作的效果是将 `<CppWinRTOptimized>true</CppWinRTOptimized>` 添加到项目文件。 并且它与从命令行调用 `cppwinrt.exe` 时添加 `-opt[imize]` 开关具有相同的效果。

新项目（来自项目模板）在默认情况下会使用 `-opt`。

##### <a name="uniform-construction-and-direct-implementation-access"></a>统一构造和直接实现访问

这两个优化使组件可以直接访问自己的实现类型，即使在它只使用投影类型时。 如果只是要使用公共 API 外围应用，则无需使用 [make  ](/uwp/cpp-ref-for-winrt/make)、[make_self  ](/uwp/cpp-ref-for-winrt/make-self) 和 [get_self  ](/uwp/cpp-ref-for-winrt/get-self)。 调用会向下编译为直接调用实现，甚至可能完全内联。

如需详细信息和代码示例，请参阅[选择加入统一构造和直接实现访问](/windows/uwp/cpp-and-winrt-apis/author-apis#opt-in-to-uniform-construction-and-direct-implementation-access)。

##### <a name="type-erased-factories"></a>类型擦除工厂

此优化可避免 `module.g.cpp` 中的 #include 依赖项，从而无需在每次任何单个实现类发生更改时重新编译它。 这样可改进生成性能。

#### <a name="smarter-and-more-efficient-modulegcpp-for-large-projects-with-multiple-libs"></a>适用于具有多个库的大型项目的更智能且更高效的 `module.g.cpp`

`module.g.cpp` 文件现在还包含两个可组合的附加帮助程序，名为 winrt_can_unload_now  和 winrt_get_activation_factory  。 它们旨在用于较大项目，其中一个 DLL 由多个库组成，每个库都有自己的运行时类。 在这种情况下，需要手动将 DLL 的 DllGetActivationFactory  和 DllCanUnloadNow  拼结在一起。 这些帮助程序通过避免虚假的来源错误，大大降低了执行该操作的难度。 `cppwinrt.exe` 工具的 `-lib` 标志还可用于为每个库提供自己的前导码（而不是 `winrt_xxx`），以便每个库的函数都可以单独命名，因而可明确地合并。

#### <a name="coroutine-support"></a>协同例程支持

协同例程支持会自动包含在内。 以前该支持位于多个位置，我们认为这太过局限。 随后对于 v2.0，临时需要 `winrt/coroutine.h` 头文件，但现在不再需要它。 由于 Windows 运行时异步接口现在是生成的（而不是手动编写），因此它们现在位于 `winrt/Windows.Foundation.h` 中。 除了作为更易维护且受支持之外，这意味着协同例程帮助程序（如 [resume_foreground  ](/uwp/cpp-ref-for-winrt/resume-foreground)）不再必须附加到特定命名空间标头的末尾。 而是可以更自然地包括其依赖项。 这进一步使 resume_foreground  不仅对给定 [Windows::UI::Core::CoreDispatcher  ](/uwp/api/windows.ui.core.coredispatcher) 支持恢复，而且现在可以还对给定 [Windows::System::DispatcherQueue  ](/uwp/api/windows.system.dispatcherqueue) 支持恢复。 以前只能支持一个；而不支持两者，因为定义只能位于一个命名空间中。

下面是 DispatcherQueue  支持的示例。

```cppwinrt
...
#include <winrt/Windows.System.h>
using namespace Windows::System;
...
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

协同例程帮助程序现在还使用 `[[nodiscard]]` 进行修饰，从而提高了其可用性。 如果忘记（或没有意识到必须）对它们执行 `co_await` 操作以使它们可正常工作，则由于 `[[nodiscard]]`，这类错误现在会生成编译器警告。

#### <a name="help-with-diagnosing-direct-stack-allocations"></a>有关诊断直接（堆栈）分配的帮助

由于投影和实现类名称（默认情况下）是相同的，只在命名空间方面有所差异，因此可能会将其中一个错认为另一个，以及并在堆栈中意外地创建实现，而不是使用 [make  ](/uwp/cpp-ref-for-winrt/make) 系列的帮助程序。 这在某些情况下可能难以诊断，因为对象可能已销毁，而未完成的引用仍在工作。 对于调试生成，断言现在可解决此问题。 虽然断言不会检测协程中的堆栈分配，不过它仍然有助于捕获大多数这类错误。

有关详细信息，请参阅 [Diagnosing direct allocations](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc)（诊断直接分配）。

#### <a name="improved-capture-helpers-and-variadic-delegates"></a>改进的捕获帮助程序和可变参数委托

此更新通过也支持投影类型，修复了捕获帮助程序方面的限制。 当 Windows 运行时互操作 API 返回投影类型时，这偶尔会与它们一起出现。

此更新还添加了在创建可变参数（非 Windows 运行时）委托时对[get_strong  ](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) 和 [get_weak  ](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) 的支持。

#### <a name="support-for-deferred-destruction-and-safe-qi-during-destruction"></a>在析构过程中对延迟析构和安全 QI 的支持

在运行时类对象的析构函数中调用暂时影响引用计数的方法并非罕见。 当引用计数返回到零时，该对象将第二次进行析构。 在 XAML 应用程序中，你可能需要在析构函数中执行 [**QueryInterface**](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) (QI)，以便在层次结构中向上或向下调用某个清理实现。 但是，对象的引用计数已达到零，这样 QI 也形成引用计数回弹。

此更新添加了对引用计数进行反跳的支持，从而确保一旦它达到零，便绝不可能恢复；同时仍允许在析构过程为所需的任何临时目标执行 QI。 此过程在某些 XAML 应用程序/控件中是不可避免的，C++/WinRT 现在对它具有弹性。

可以通过在实现类型上提供一个静态 **final_release** 函数来延迟析构。 指向对象的最后一个剩余指针（其形式为 **std:: unique_ptr**）会传递给 **final_release**。 然后，你可以选择将该指针的所有权转移到其他某个上下文。 对指针执行 QI 而不触发双析构，是非常安全的。 但在你析构对象时，对引用计数的净更改必须为零。

**final_release** 的返回值可以是 `void`、异步操作对象（例如 [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)）或 **winrt::fire_and_forget**。

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

    static void final_release(std::unique_ptr<Sample> self) noexcept
    {
        // Move 'self' as needed to delay destruction.
    }
};
```

在以下示例中，一旦发布了 MainPage  （最后一次），便会调用 final_release  。 该函数会等待五秒（在线程池中），然后使用页面的调度程序  进行恢复（这需要 QI/AddRef/Release 正在工作）。 然后，它会清理该 UI 线程上的资源。 最后，它会清除 **unique_ptr**，这会导致实际调用 **MainPage** 析构函数。 甚至在该析构函数中也会调用 **DataContext**，这需要为 **IFrameworkElement** 执行 QI。

不必实现 **final_release** 作为协同例程。 但是这确实有效，并且会使得将析构移动到不同的线程非常简单，下面的示例中正是如此。

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

    static IAsyncAction final_release(std::unique_ptr<MainPage> self)
    {
        co_await 5s;

        co_await resume_foreground(self->Dispatcher());
        co_await self->resource.CloseAsync();

        // The object is destructed normally at the end of final_release,
        // when the std::unique_ptr<MyClass> destructs. If you want to destruct
        // the object earlier than that, then you can set *self* to `nullptr`.
        self = nullptr;
    }
};
```

有关详细信息，请参阅[延迟析构](/windows/uwp/cpp-and-winrt-apis/details-about-destructors#deferred-destruction)。

#### <a name="improved-support-for-com-style-single-interface-inheritance"></a>改进了对 COM 样式单接口继承的支持

并且对于 Windows 运行时编程，C++/WinRT 还用于创作和使用纯 COM API。 通过此更新可以实现其中存在接口层次结构的 COM 服务器。 这不是 Windows 运行时所必需的；但某些 COM 实现需要它。

#### <a name="correct-handling-of-out-params"></a>`out` 参数的正确处理

使用 `out` 参数可能十分麻烦；尤其是 Windows 运行时数组。 借助此更新，C++/WinRT 在涉及到 `out` 参数和数组时可显著提高稳健性以及对错误的弹性；无论这些参数是来自语言投影，还是使用原始 ABI 以及犯了未一致地初始化变量错误的 COM 开发人员。 在任一情况下，当涉及到将投影类型移交给 ABI（通过记住释放任何资源），以及涉及到清零或清除来自 ABI 的参数时，C++/WinRT 现在会执行正确的处理。

#### <a name="events-now-handle-invalid-tokens-reliably"></a>事件现在可靠地处理无效标记

[winrt::event  ](/uwp/cpp-ref-for-winrt/event) 实现现在可适当地处理使用无效标记值（数组中不存在的值）调用其 remove  方法的情况。

#### <a name="coroutine-local-variables-are-now-destroyed-before-the-coroutine-returns"></a>现在会在协同例程返回之前销毁协同例程局部变量

实现协同例程类型的传统方法可能会允许在协同例程返回/完成之后  （而不是在最终挂起之前）销毁协同例程中的局部变量。 任何等待程序的恢复现在会延迟到最终挂起，以便避免此问题并形成其他好处。

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Windows SDK 版本 10.0.17763.0（Windows 10 版本 1809）中的新增功能和更改

下表包含 Windows SDK 版本 10.0.17763.0（Windows 10 版本 1809）中适用于 C++/WinRT 的新增功能和更改。

| 新增功能或更改的功能 | 详细信息 |
| - | - |
| 重大更改  。 为了使它可以编译，C++/WinRT 不依赖于 Windows SDK 中的标头。 | 请参阅下面的[与 Windows SDK 头文件隔离](#isolation-from-windows-sdk-header-files)。 |
| Visual Studio 项目系统格式已更改。 | 请参阅下面的[如何将 C++/WinRT 项目重新定位到更高版本的 Windows SDK](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)。 |
| 有新的函数和基类可帮助将集合对象传递到 Windows 运行时函数，或是实现自己的集合属性和集合类型。 | 请参阅[使用 C++/WinRT 的集合](collections.md)。 |
| 可以将 [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) 标记扩展与 C++/WinRT 运行时类一起使用。 | 有关详细信息和代码示例，请参阅[数据绑定概述](/windows/uwp/data-binding/data-binding-quickstart)。 |
| 对取消协同例程的支持使你可注册取消回调。 | 有关更多信息和代码示例，请参阅[取消异步操作和取消回调](concurrency-2.md#canceling-an-asynchronous-operation-and-cancellation-callbacks)。 |
| 创建指向成员函数的委托时，可以在注册处理程序的位置处建立对当前对象的强引用或弱引用（而不是原始 this  指针）。 | 有关详细信息和代码示例，请参阅[使用事件处理委托安全访问 this  指针](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)一节中的“如果将成员函数用作委托”  子节。 |
| 修复了 Visual Studio 为更加符合 C++ 标准而发现的 bug。 更好地利用 LLVM 和 Clang 工具链来验证 C++/WinRT 的标准符合性。 | 不会再遇到[为何我的新项目不能编译？我使用的是 Visual Studio 2017（15.8.0 或更高版本）和 SDK 版本 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) 中所述的问题 |

其他更改。

- 重大更改  。 [winrt::get_abi(winrt::hstring const&)  ](/uwp/cpp-ref-for-winrt/get-abi) 现在返回 `void*` 而不是 `HSTRING`。 可以使用 `static_cast<HSTRING>(get_abi(my_hstring));` 获取 HSTRING。 请参阅[与 ABI 的 HSTRING 进行互操作](interop-winrt-abi.md#interoperating-with-the-abis-hstring)。
- 重大更改  。 [winrt::put_abi(winrt::hstring&)  ](/uwp/cpp-ref-for-winrt/put-abi) 现在返回 `void**` 而不是 `HSTRING*`。 可以使用 `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` 获取 HSTRING*。 请参阅[与 ABI 的 HSTRING 进行互操作](interop-winrt-abi.md#interoperating-with-the-abis-hstring)。
- 重大更改  。 HRESULT 现在投影为 winrt::hresult  。 如果需要 HRESULT（用于执行类型检查，或支持类型特征），则可以对 winrt::hresult  执行 `static_cast` 操作。 否则，winrt::hresult  会转换为 HRESULT，只要在包含任何 C++/WinRT 标头之前包含 `unknwn.h`。
- 重大更改  。 GUID 现在投影为 winrt::guid  。 对于实现的 API，必须将 winrt::guid  用于 GUID 参数。 否则，winrt::guid  会转换为 GUID，只要在包含任何 C++/WinRT 标头之前包含 `unknwn.h`。 请参阅[与 ABI 的 GUID 结构进行互操作](interop-winrt-abi.md#interoperating-with-the-abis-guid-struct)。
- 重大更改  。 [winrt::handle_type 构造函数  ](/uwp/cpp-ref-for-winrt/handle-type#handle_typehandle_type-constructor)已通过使其显式进行了强化（现在更难使用它编写不正确的代码）。 如果需要分配原始句柄值，则改为调用 [handle_type::attach 函数  ](/uwp/cpp-ref-for-winrt/handle-type#handle_typeattach-function)。
- 重大更改  。 WINRT_CanUnloadNow  和 WINRT_GetActivationFactory  的签名已更改。 完全不能声明这些函数。 而是包含 `winrt/base.h`（在包含任何 C++/WinRT Windows 命名空间头文件时自动包含）以包含这些函数的声明。
- 对于 [winrt::clock struct  ](/uwp/cpp-ref-for-winrt/clock)，from_FILETIME/to_FILETIME  已弃用，以支持 from_file_time/to_file_time  。
- 简化了需要 **IBuffer** 参数的 API。 大多数 API 更喜欢集合或数组。 但我们认为，我们应该使调用依赖于 **IBuffer** 的 API 更轻松。 此更新提供对 **IBuffer** 实现后的数据的直接访问。 它使用的数据命名约定与 C++ 标准库容器使用的数据命名约定相同。 该约定还可避免与通常以大写字母开头的元数据名称冲突。
- 改进了代码生成：进行了各种改进，以减小代码大小、提高内联并优化工厂缓存。
- 删除了不必要的递归。 当命令行引用文件夹，而不是特定 `.winmd` 时，`cppwinrt.exe` 工具不再以递归方式搜索 `.winmd` 文件。 `cppwinrt.exe` 工具现在还可更智能地处理重复项，从而对用户错误和格式不正确的 `.winmd` 文件更具弹性。
- 强化了智能指针。 以前在移动分配新值时，事件撤销程序会无法撤销。 这有助于揭示智能指针类未可靠地处理自我赋值的问题；来源于 [winrt::com_ptr 结构模板  ](/uwp/cpp-ref-for-winrt/com-ptr)。 winrt::com_ptr  进行了修复，并且事件撤销程序进行了修复，可正确处理移动语义正确，以便可在分配时撤销。

> [!IMPORTANT]
> 对 [C++/WinRT Visual Studio 扩展 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) 进行了重要更改（在版本 1.0.181002.2 中，随后在版本 1.0.190128.4 中）。 有关这些更改以及它们如何影响现有项目的详细信息，请参阅[适用于 C++/WinRT 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)和[早期版本的 VSIX 扩展](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)。

### <a name="isolation-from-windows-sdk-header-files"></a>与 Windows SDK 头文件隔离

对于你的代码，这可能是一个重大更改。

为了使它可以编译，C++/WinRT 不再依赖于 Windows SDK 中的头文件。 C 运行时库 (CRT) 和 C++ 标准模板库 (STL) 中的头文件也不包含任何 Windows SDK 标头。 这改进了标准符合性，可避免意外依赖项，并且极大地减少了必须防范的宏的数量。

这种独立性意味着 C++/WinRT 现在具有更强的可移植性和标准符合性，使它更有可能成为跨编译器和跨平台库。 这也意味着，C++/WinRT 标头不是产生负面影响的宏。

如果以前让 C++/WinRT 在项目中包含任何 Windows 标头，则现在需要自己包含它们。 在任何情况下，最佳做法始终是显式包含所依赖的标头，而不是让另一个库包含它们。

当前，Windows SDK 头文件隔离的唯一例外情况是对于内部函数和数字。 在这些最后剩余的依赖项方面没有已知问题。

在项目中，可以在需要时重新启用与 Windows SDK 标头的互操作。 例如，可能要实现 COM 接口（来源于 [IUnknown  ](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)）。 对于该示例，在包含任何 C++/WinRT 标头之前包含 `unknwn.h`。 这样做会导致 C++/WinRT 基库启用各种挂钩，以支持经典 COM 接口。 有关代码示例，请参阅[通过 C++/WinRT 创作 COM 组件](author-coclasses.md)。 同样，显式包含任何其他用于声明要调用的类型和/或函数的 Windows SDK 标头。

### <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>如何将 C++/WinRT 项目重新定位到更高版本的 Windows SDK

可能会形成最少编译器和链接器问题的项目重新定位方法也是最费力的。 该方法涉及创建新项目（面向所选的 Windows SDK 版本），然后将文件从旧项目复制到新项目中。 可以只需复制旧 `.vcxproj` 和 `.vcxproj.filters` 文件的各个部分，从而不必在 Visual Studio 中添加文件。

但是，可通过两种其他方式在 Visual Studio 中重新定位项目。

- 转到项目属性“常规”\>“Windows SDK 版本”，然后选择“所有配置”和“所有平台”。     将“Windows SDK 版本”  设置为要面向的版本。
- 在“解决方案资源管理器”  ，右键单击项目节点，单击“重定向项目”  ，选择要面向的版本，然后单击“确定”  。

如果在使用这两种方法之一之后遇到任何编译器或链接器错误，则可以先尝试清理解决方案（“生成”   > “清理解决方案”  ，并且/或者手动删除所有临时文件夹和文件），然后再尝试重新生成。

如果 C++ 编译器生成“错误 C2039:  \`‘IUnknown’: 不是‘全局命名空间’的成员”，则将 `#include <unknwn.h>` 添加到 `pch.h` 文件顶部（在包含任何 C++/WinRT 标头之前）。

在此之后可能还需要添加 `#include <hstring.h>`。

如果 C++ 链接器生成“错误 LNK2019: 函数 _VSDesignerCanUnloadNow@0 中引用了未解析的外部符号 _WINRT_CanUnloadNow@0”  ，则可以通过将 `#define _VSDESIGNER_DONT_LOAD_AS_DLL` 添加到 `pch.h` 文件来解决该问题。
