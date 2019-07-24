---
description: 本主题讨论了处理使用 C++/WinRT 编程时出现的错误的策略。
title: 使用 C++/WinRT 的错误处理
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 错误, 处理, 异常
ms.localizationpriority: medium
ms.openlocfilehash: 37819d1626d3adc6f5647f447567a9273e72668d
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270131"
---
# <a name="error-handling-with-cwinrt"></a>使用 C++/WinRT 的错误处理

本主题讨论了处理使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 编程时出现的错误的策略。 更多常规信息和背景，请参阅[错误和异常处理 (Modern C++)](/cpp/cpp/errors-and-exception-handling-modern-cpp)。

## <a name="avoid-catching-and-throwing-exceptions"></a>避免捕获和抛出异常
建议继续编写[异常安全代码](/cpp/cpp/how-to-design-for-exception-safety)，但最好尽量避免捕获和抛出异常。 如果没有异常处理程序，Windows 将自动生成错误报告（包括故障的小型转储），以便跟踪问题所在位置。

不要引发你预计会捕获的异常。 也不要使用预期会失败的异常。 应“仅在发生意外运行时错误时”抛出异常，并处理带有错误/结果代码的任何其他事项 &mdash; 直接并靠近故障原因  。 这样，当异常“被”引发时，你会知道原因是代码中的 bug 还是系统中的异常错误状态  。

考虑访问 Windows 注册表的场景。 如果你的应用无法从注册表读取值，这是预料之中的，你应该正确处理。 不要抛出异常；而应返回 `bool` 或 `enum` 值指示未读取值或原因。 另一方面，无法向注册表写入  值很可能表示你的应用程序中存在的问题更大，是你无法明智处理的。 在这种情况下，你不希望应用程序继续，所以导致生成错误报告的异常是阻止应用程序造成任何损害的最快方式。

另一个示例中，请考虑从对 [**StorageFile.GetThumbnailAsync**](/uwp/api/windows.storage.storagefile.getthumbnailasync#Windows_Storage_StorageFile_GetThumbnailAsync_Windows_Storage_FileProperties_ThumbnailMode_) 的调用检索缩略图图像，然后将该缩略图传递到 [**BitmapSource.SetSourceAsync**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync#Windows_UI_Xaml_Media_Imaging_BitmapSource_SetSourceAsync_Windows_Storage_Streams_IRandomAccessStream_)。 如果该调用顺序导致你将 `nullptr` 传递到 **SetSourceAsync**（无法读取图像文件；或许是文件扩展名使它看似包含图像数据，而实际并非如此），那么你将会导致引发无效的指针异常。 如果你发现自己代码存在这类情况，则不应将这种情况作为异常捕获和处理，而应检查从 GetThumbnailAsync  返回的 `nullptr`。

抛出异常异常往往会比使用错误代码更慢。 如果你仅在出现严重错误时抛出异常，如果一切都正常运行，那么你永远不需要在性能方面妥协。

但更有可能的是，性能下降需要付出运行时开销来确保在不太可能抛出异常的情况下调用相应的析构函数。 这种保障成本不论实际是否抛出异常都会产生。 因此，你应该确保编译器清楚地了解哪些功能可以有抛出异常的可能性。 如果编译器可以证明某些功能不会引发任何异常（`noexcept` 规范），那么它可以优化所生成的代码。

## <a name="catching-exceptions"></a>捕获异常
在 [Windows 运行时 ABI](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) 层出现的错误状态以 HRESULT 值的形式返回。 不过你无需处理代码中的 HRESULT。 为每个使用方的 API 生成的 C++/WinRT 投影代码将检测 ABI 层的错误 HRESULT 代码，并将代码转换为你可以捕获并处理的 [winrt::hresult_error](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 异常  。 如果你的确希望处理 HRESULTS，那么请使用“winrt::hresult”类型   。

例如，如果用户碰巧在你的应用程序迭代图片库时从该集合中删除了图像，那么投影将抛出异常。 这是你必须捕获和处理该异常的一种情况。 下面的代码示例展示了这种情况。

```cppwinrt
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.UI.Xaml.Media.Imaging.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage;
using namespace Windows::UI::Xaml::Media::Imaging;

IAsyncAction MakeThumbnailsAsync()
{
    auto imageFiles{ co_await KnownFolders::PicturesLibrary().GetFilesAsync() };

    for (StorageFile const& imageFile : imageFiles)
    {
        BitmapImage bitmapImage;
        try
        {
            auto thumbnail{ co_await imageFile.GetThumbnailAsync(FileProperties::ThumbnailMode::PicturesView) };
            if (thumbnail) bitmapImage.SetSource(thumbnail);
        }
        catch (winrt::hresult_error const& ex)
        {
            winrt::hresult hr = ex.to_abi(); // HRESULT_FROM_WIN32(ERROR_FILE_NOT_FOUND).
            winrt::hstring message = ex.message(); // The system cannot find the file specified.
        }
    }
}
```

请在调用 `co_await` 的函数时在协调程序中使用相同模式。 此 HRESULT 到异常转换的另一个示例是，当组件 API 返回 E_OUTOFMEMORY 时，会导致抛出“std::bad_alloc”  。

## <a name="throwing-exceptions"></a>引发异常
将存在你作此决定的情况，如果你对给定函数的调用失败，你的应用程序将无法恢复（无法再期待它能够如期工作）。 下方代码示例使用 [winrt::handle](/uwp/cpp-ref-for-winrt/handle) 值作为从 [CreateEvent](https://docs.microsoft.com/windows/desktop/api/synchapi/nf-synchapi-createeventa) 返回的 HANDLE 的包装   。 然后将该句柄（从其创建 `bool` 值）传递到 [winrt::check_bool](/uwp/cpp-ref-for-winrt/error-handling/check-bool) 函数模板  。 “winrt::check_bool”使用 `bool` 或任何可转换为 `false`（错误条件）或 `true`（成功条件）的值  。

```cppwinrt
winrt::handle h{ ::CreateEvent(nullptr, false, false, nullptr) };
winrt::check_bool(bool{ h });
winrt::check_bool(::SetEvent(h.get()));
```

如果你传递到 [winrt::check_bool](/uwp/cpp-ref-for-winrt/error-handling/check-bool) 的值为 false，那么以下操作序列将生效  。

- “winrt::check_bool”调用 [winrt::throw_last_error](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) 函数   。
- “winrt::throw_last_error”调用 [GetLastError](https://docs.microsoft.com/windows/desktop/api/errhandlingapi/nf-errhandlingapi-getlasterror) 来检索调用线程的最后一个错误代码值，然后调用 [winrt::throw_hresult](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult) 函数    。
- “winrt::throw_hresult”使用表示该错误代码的 [winrt::hresult_error](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 对象（或标准对象）抛出异常   。

由于 Windows API 使用各个返回值类型报告运行时错误，因此除“winrt::check_bool”外，还有其他一些用于检查值和抛出异常的有用的帮助程序函数  。

- [winrt::check_hresult](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)  。 检查 HRESULT 代码是否表示错误，如果是，则调用“winrt::throw_hresult”  。
- [winrt::check_nt](/uwp/cpp-ref-for-winrt/error-handling/check-nt)  。 检查代码是否表示错误，如果是，则调用“winrt::throw_hresult”  。
- [winrt::check_pointer](/uwp/cpp-ref-for-winrt/error-handling/check-pointer)  。 检查指针是否为 null，如果是，则调用“winrt::throw_last_error”  。
- [winrt::check_win32](/uwp/cpp-ref-for-winrt/error-handling/check-win32)  。 检查代码是否表示错误，如果是，则调用“winrt::throw_hresult”  。

你可以对常见的返回代码类型使用这些帮助程序函数，也可以响应任何错误条件并调用 [winrt::throw_last_error](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) 或 [winrt::throw_hresult](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult)   。 

## <a name="throwing-exceptions-when-authoring-an-api"></a>在创作 API 时抛出异常
所有 [Windows 运行时应用程序二进制接口](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types)边界（简称 ABI 边界）必须为 *noexcept*&mdash;即不得有异常。 创作 API 时，应始终使用 C++ `noexcept` 关键字来标记 ABI 边界。 `noexcept` 在 C++ 中有特定的行为。 如果 C++ 异常遇到 `noexcept` 边界，则会调用 **std::terminate**，导致进程很快失败。 该行为通常是理想的做法，因为未经处理的异常几乎总是意味着进程中出现了未知状态。

由于异常不得跨过 ABI 边界，在实现中出现的错误条件以 HRESULT 错误代码的形式跨 ABI 层返回。 在使用 C++/WinRT 创作 API 时，将生成代码以供你将在实现中抛出的任何异常转换为 HRESULT  。 [Winrt::to_hresult](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) 函数以与此类似的模式用于生成的代码  。

```cppwinrt
HRESULT DoWork() noexcept
{
    try
    {
        // Shim through to your C++/WinRT implementation.
        return S_OK;
    }
    catch (...)
    {
        return winrt::to_hresult(); // Convert any exception to an HRESULT.
    }
}
```

[winrt::to_hresult](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) 处理派生自 std::exception 和 [winrt::hresult_error](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) 及其派生类型的异常    。 在你的实现中，最好使用 winrt::hresult_error 或派生类型，以便你的 API 的使用者可以收到丰富的错误信息  。 “std::exception”（映射到 E_FAIL）在你使用标准模板库时引发异常的情况下受支持  。

### <a name="debuggability-with-noexcept"></a>使用 noexcept 时的可调试性
如前所述，如果 C++ 异常遇到 `noexcept` 边界，则会调用 **std::terminate**，导致进程很快失败。 这不适用于调试，因为 **std::terminate** 通常会失去引发的大部分或所有错误或异常上下文，尤其是在涉及协同程序的情况下。

因此，本部分处理的是 ABI 方法（已使用 `noexcept` 进行适当的批注）使用 `co_await` 来调用异步 C++/WinRT 投影代码的情况。 建议将对 C++/WinRT 项目代码的调用包装在 **winrt::fire_and_forget** 中。 这样做就可以在正确的位置将未经处理的异常正确记录为存放异常，大大提高可调试性。

```cppwinrt
HRESULT MyWinRTObject::MyABI_Method() noexcept
{
    winrt::com_ptr<Foo> foo{ get_a_foo() };

    [/*no captures*/](winrt::com_ptr<Foo> foo) -> winrt::fire_and_forget
    {
        co_await winrt::resume_background();

        foo->ABICall();

        AnotherMethodWithLotsOfProjectionCalls();
    }(foo);

    return S_OK;
}
```

**winrt::fire_and_forget** 有内置的 `unhandled_exception` 方法帮助程序，该程序调用 **winrt::terminate**，后者又调用 **RoFailFastWithErrorContext**。 这样就可以保证任何上下文（存放异常、错误代码、错误消息、堆栈回溯等）都会得到保存，不管是进行实时调试，还是进行事后转储。 为了方便起见，可以将“发后不理”部分重构成一个单独的可返回 **winrt::fire_and_forget** 的函数，然后调用它。

### <a name="synchronous-code"></a>同步代码
在某些情况下，ABI 方法（同样已使用 `noexcept` 进行适当的批注）仅调用同步代码。 换而言之，它从不使用 `co_await`，不管是用来调用异步 Windows 运行时方法，还是用来在前台和后台线程之间切换。 在这种情况下，“发后不理”方法仍可使用，但效率不高。 可以改为执行类似下面的代码。

```cppwinrt
HRESULT abi() noexcept try
{
    // ABI code goes here.
} catch (...) { winrt::terminate(); }
```

### <a name="fail-fast"></a>快速失败
上一部分的代码仍会快速失败。 从编写的内容来看，该代码不处理任何异常。 任何未经处理的异常都会导致程序终止。

但该形式是很好的，因为它确保了可调试性。 在罕见情况下，可能需要使用 `try/catch`，并处理某些异常。 但这应该很罕见，因为正如本主题所述，我们反对将异常作为一种流控制机制用于预期的条件。

记住，让未经处理的异常逃脱无包装的 `noexcept` 上下文是很糟糕的做法。 在该条件下，C++ 运行时会 **std::terminate** 进程，因此会失去任何存放的由 C++/WinRT 仔细记录的异常信息。

## <a name="assertions"></a>断言
对应用程序中的内部假设，存在断言。 最好尽可能地为编译时验证使用“static_assert”  。 对于运行时条件，请使用带布尔值表达式的 `WINRT_ASSERT`。 `WINRT_ASSERT` 是宏定义，并且扩展到 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)。

```cppwinrt
WINRT_ASSERT(pos < size());
```

WINRT_ASSERT 在发布版本中编译；在调试版本中，它停止断言所在的代码行上调试程序中的应用程序。

不应在析构函数中使用异常。 因此，至少在调试版本中，你可以断言从带有 WINRT_VERIFY（带有布尔值表达式）和 WINRT_VERIFY_（带有预期结果和布尔值表达式）的析构函数调用函数的结果。

```cppwinrt
WINRT_VERIFY(::CloseHandle(value));
WINRT_VERIFY_(TRUE, ::CloseHandle(value));
```

## <a name="important-apis"></a>重要的 API
* [winrt::check_bool 函数模板](/uwp/cpp-ref-for-winrt/error-handling/check-bool)
* [winrt::check_hresult 函数](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::check_bool 函数模板](/uwp/cpp-ref-for-winrt/error-handling/check-nt)
* [winrt::check_pointer 函数模板](/uwp/cpp-ref-for-winrt/error-handling/check-pointer)
* [winrt::check_win32 函数模板](/uwp/cpp-ref-for-winrt/error-handling/check-win32)
* [winrt::handle 结构](/uwp/cpp-ref-for-winrt/handle)
* [winrt::hresult_error 结构](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::throw_hresult 函数](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult)
* [winrt::throw_last_error 函数](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error)
* [winrt::to_hresult 函数](/uwp/cpp-ref-for-winrt/error-handling/to-hresult)

## <a name="related-topics"></a>相关主题
* [错误和异常处理 (Modern C++)](/cpp/cpp/errors-and-exception-handling-modern-cpp)
* [How to:针对异常安全设计](/cpp/cpp/how-to-design-for-exception-safety)
