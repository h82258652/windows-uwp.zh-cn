---
author: stevewhims
description: 本主题介绍如何将 C++/CX 代码移植到 C++/WinRT 中的等效项。
title: 从 C++/CX 移动到 C++/WinRT
ms.author: stwhi
ms.date: 10/18/2018
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 端口, 迁移, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 35fe84747624c9a855df5520322546b83772379b
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7294777"
---
# <a name="move-to-cwinrt-from-ccx"></a>从 C++/CX 移动到 C++/WinRT

本主题介绍如何将在代码移植[C + + CX](/cpp/cppcx/visual-c-language-reference-c-cx)项目中的等效[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)。

## <a name="porting-strategies"></a>移植策略

如果你想要逐渐移植 C + + /CX 代码为 C + + WinRT，则可以。 C + + /CX 和 C + + /winrt 代码可以在同一项目中，XAML 编译器支持和 Windows 运行时组件的例外共存。 有关这些两个例外，你将需要针对 C + + /CX 或 C + + WinRT 在同一项目中的。

> [!IMPORTANT]
> 如果你的项目生成一个 XAML 应用程序，然后一个建议的工作流是先创建新项目在 Visual Studio 中使用 C + + /winrt 项目模板 (请参阅[Visual Studio 支持 C + + /winrt 以及 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix))。 然后，开始菜单复制的源代码和标记从 C + + CX 项目。 你可以添加新**项目**的 XAML 页面 \> **添加新项...** \>  **Visual c + +** > **空白页面 (C + + WinRT)**。
>
> 或者，你可以使用 Windows 运行时组件为因素代码之外的 XAML C + + CX 项目为将其移植。 无论是移动尽可能 C + + CX 代码以及你可以为组件，然后将 XAML 项目更改为 C + + WinRT。 或其他人将 XAML 项目 C + + CX，创建新的 C + + WinRT 组件，并开始移植 C + + /CX 代码出 XAML 项目，并在组件。 你还可以有了 C + + CX 组件项目旁边的 C + + 在同一个解决方案中的 WinRT 组件项目引用这两张信用卡从你的应用程序项目，并逐渐移植到另一个。 请参阅[互 C + + /winrt 与 C + + CX](interop-winrt-cx.md)为在同一项目中使用两个语言投影的更多详细信息。

> [!NOTE]
> [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 和 Windows SDK 都在根命名空间 **Windows** 中声明类型。 投影到 C++/WinRT 的 Windows 类型具有与 Windows 类型相同的完全限定名称，但放置于 C++ **winrt** 命名空间中。 这些不同的命名空间可让你按照自己的节奏从 C++/CX 移植到 C++/WinRT。

对比记住上面提到的异常的第一步中移植 C + + CX 项目到 C + + WinRT 是手动添加 C + + 向其 WinRT 支持 (，请参阅[Visual Studio 支持 C + + /winrt 以及 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix))。 若要执行该操作，编辑你的 `.vcxproj` 文件，找到 `<PropertyGroup Label="Globals">`，在该属性组内，设置属性 `<CppWinRTEnabled>true</CppWinRTEnabled>`。 这一更改的一个效果是对 C++/CX 的支持在项目中关闭。 它是一个好主意，将留支持处于关闭状态，以便生成消息帮助你查找 （和端口） 所有依赖项的 C + + /CX，或你可以重新打开支持 (在项目属性中， **C/c + +** \> **常规** \> **消耗 Windows 运行时扩展** \> **是 (/ZW)**)，和逐渐移植。

确保**常规**该项目属性 \> **目标平台版本**设置为 10.0.17134.0 (Windows 10 版本 1803年) 或更高版本。

在预编译的标头文件（通常为 `pch.h`）中，包括 `winrt/base.h`。

```cppwinrt
#include <winrt/base.h>
```

如果你包括了任何 C++/WinRT 投影 Windows API 标头（例如，`winrt/Windows.Foundation.h`），那么你无需像这样明确包括 `winrt/base.h`，因为它将自动为你包含在内。

如果你的项目还使用 [Windows 运行时 C++ 模板库 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 类型，请参阅[从 WRL 迁移到 C++/WinRT](move-to-winrt-from-wrl.md)。

## <a name="parameter-passing"></a>参数传递
编写 C++/CX 源代码时，在顶帽 (\^) 引用时，你将 C++/CX 类型作为函数参数传递。

```cpp
void LogPresenceRecord(PresenceRecord^ record);
```

在 C++/WinRT 中，对于同步函数，默认情况下应该使用 `const&` 参数。 这将避免复制和互锁开销。 但你的协同程序应使用按值传递来确保它们按值捕获，并避免生命周期问题（更多详细信息，请参阅[利用 C++/WinRT 实现的并发和异步操作](concurrency.md)）。

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

C++/WinRT 对象根本上是一个保留支持 Windows 运行时对象的接口指针的值。 在复制 C++/WinRT 对象时，编译器复制封装的接口指针，从而递增其引用计数。 副本的最终销毁涉及递减引用计数。 因此，仅在必要时产生复制开销。

## <a name="variable-and-field-references"></a>变量和字段引用
编写 C++/CX 源代码时，你使用顶帽 (\^) 变量引用 Windows 运行时对象，使用箭头 (-&gt;) 运算符来取消引用顶帽变量。

```cpp
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

当移植到等效的 C + + WinRT 代码中，你基本上要删除顶帽并更改箭头运算符 (-&gt;) 为点运算符 （.），因为 C + + /winrt 投影类型是值，而不是指针。

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

## <a name="properties"></a>属性
C++/CX 语言扩展包括属性概念。 编写 C++/CX 源代码时，你可以像访问字段那样访问属性。 标准 C++ 没有属性概念，因此，在 C++/WinRT 中，你调用获取和设置函数。

在随后的示例中，**XboxUserId**、**UserState**、**PresenceDeviceRecords** 和 **Size** 全部都是属性。

### <a name="retrieving-a-value-from-a-property"></a>从属性检索值
下面介绍如何在 C++/CX 中获取属性值。

```cpp
void Sample::LogPresenceRecord(PresenceRecord^ record)
{
    auto id = record->XboxUserId;
    auto state = record->UserState;
    auto size = record->PresenceDeviceRecords->Size;
}
```

等效的 C++/WinRT 源代码调用与属性同名但没有参数的函数。

```cppwinrt
void Sample::LogPresenceRecord(PresenceRecord const& record)
{
    auto id = record.XboxUserId();
    auto state = record.UserState();
    auto size = record.PresenceDeviceRecords().Size();
}
```

请注意，**PresenceDeviceRecords** 函数返回其本身具有 **Size** 函数的 Windows 运行时对象。 由于返回的对象也是 C++/WinRT 投影类型，因此我们使用点运算符调用 **Size** 来取消引用。

### <a name="setting-a-property-to-a-new-value"></a>将属性设置为新值
将属性设置为新值遵循类似模式。 首先，在 C++/CX 中。

```cpp
record->UserState = newValue;
```

若要在 C++/WinRT 执行同等操作，调用与属性同名的函数，并传递参数。

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>创建类的实例
你通过 C++/CX 对象的句柄来处理它，通常称为顶帽 (\^) 引用。 通过 `ref new` 关键字创建新对象，这反过来会调用 [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646) 来激活运行时类的新实例。

```cpp
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

C++/WinRT 对象是一个值；因此你可以在堆栈上进行分配，或作为对象字段分配。 *切勿*使用 `ref new`（或 `new`）来分配 C++/WinRT 对象。 在后台，仍然在调用 **RoActivateInstance**。

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

如果初始化资源的成本很高，通常可以推迟资源的初始化，直到实际需要时再执行。

```cpp
using namespace Windows::Storage::Streams;

class Sample
{
public:
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer^ m_gamerPicBuffer;
};
```

移植到 C++/WinRT 的同一个代码。 请注意 `nullptr` 构造函数的使用。 有关该构造函数的详细信息，请参阅[通过 C++/WinRT 使用 API](consume-apis.md)。

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer m_gamerPicBuffer{ nullptr };
};
```

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>从一个基本的运行时类转换为一个派生
它是通常具有引用到的基础，你知道指的是派生类型的对象。 在 C + + CX，你使用`dynamic_cast`为*强制转换*为基准引用到引用派生。 `dynamic_cast`是实际上只是隐藏的[**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521)调用。 下面是一个典型示例&mdash;你处理依赖属性更改事件，并且你想要从**DependencyObject**转换回拥有依赖属性的实际类型。

```cpp
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject^ d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e)
{
    BgLabelControl^ theControl{ dynamic_cast<BgLabelControl^>(d) };

    if (theControl != nullptr)
    {
        // succeeded ...
    }
}
```

等效的 C + + /winrt 代码替换`dynamic_cast`通过[**Try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)函数调用，它封装**QueryInterface**。 你还可以选择改为调用[**iunknown:: As**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)，将引发异常，如果未返回查询所需的接口 （你请求的类型的默认接口）。 下面是 C + + WinRT 的代码示例。

```cppwinrt
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // succeeded ...
    }

    try
    {
        BgLabelControlApp::BgLabelControl theControl{ d.as<BgLabelControlApp::BgLabelControl>() };
        // succeeded ...
    }
    catch (winrt::hresult_no_interface const&)
    {
        // failed ...
    }
}
```

## <a name="event-handling-with-a-delegate"></a>通过代理进行事件处理
下面介绍了在 C++/CX 中处理事件的典型示例，在本例中将 lambda 函数用作代理。

```cpp
auto token = myButton->Click += ref new RoutedEventHandler([&](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
});
```

它是 C++/WinRT 中的等效项。

```cppwinrt
auto token = myButton().Click([&](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
});
```

不使用 lambda 函数，你可以选择作为自由函数或指向成员函数的指针实现代理。 有关详细信息，请参阅[使用 C++/WinRT 中的代理来处理事件](handle-events.md)。

如果你正在从内部使用（不是跨二进制文件）事件和代理的 C++/CX 基本代码移植，[**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) 将帮助你复制 C++/WinRT 中的这个模式。 另请参阅[参数化委托、 简单信号和在项目内的回调](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project)。

## <a name="revoking-a-delegate"></a>撤销代理
在 C++/CX 中，使用 `-=` 运算符来撤销之前的事件注册。

```cpp
myButton->Click -= token;
```

它是 C++/WinRT 中的等效项。

```cppwinrt
myButton().Click(token);
```

有关详细信息和选项，请参阅[撤销已注册的代理](handle-events.md#revoke-a-registered-delegate)。

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>将 C++/CX **平台**类型映射到 C++/WinRT 类型
C++/CX 在**平台**命名空间中提供了多个数据类型。 这些类型不是标准的 C++，因此只能在启用 Windows 运行时语言扩展（Visual Studio 项目属性 **C/C++** > **常规** > **使用 Windows 运行时扩展** > **是 (/ZW)**）的情况下使用。 下表帮助你从**平台**类型移植到 C++/WinRT 中的等效项。 完成后，由于 C++/WinRT 是标准 C++，因此你可以关闭 `/ZW` 选项。

| C++/CX | C++/WinRT |
| ---- | ---- |
| **平台:: Agile\ ^** | [**winrt:: agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagileref"></a>端口**平台:: Agile\ ^** **winrt:: agile_ref**到
**平台:: Agile\ ^** 类型在 C + + CX 表示一个 Windows 运行时类，可以从任何线程访问。 C + + /winrt 的等效项是[**winrt:: agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref)。

在 C++/CX 中。

```cpp
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

在 C++/WinRT 中。

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformexception-to-winrthresulterror"></a>将 **Platform::Exception\^** 移植到 **winrt::hresult_error**
当 Windows 运行时 API 返回非 S\_OK HRESULT 时，**Platform::Exception\^** 类型在 C++/CX 中生成。 C++/WinRT 的等效项是 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)。

若要移植到 C++/WinRT，将使用 **Platform::Exception\^** 的所有代码更改为使用 **winrt::hresult_error**。

在 C++/CX 中。

```cpp
catch (Platform::Exception^ ex)
```

在 C++/WinRT 中。

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT 提供这些异常类。

| 异常类型 | 基类 | HRESULT |
| ---- | ---- | ---- |
| [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | 调用 [**hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function) |
| [**winrt::hresult_access_denied**](/uwp/cpp-ref-for-winrt/error-handling/hresult-access-denied) | **winrt::hresult_error** | E_ACCESSDENIED |
| [**winrt::hresult_canceled**](/uwp/cpp-ref-for-winrt/error-handling/hresult-canceled) | **winrt::hresult_error** | ERROR_CANCELLED |
| [**winrt::hresult_changed_state**](/uwp/cpp-ref-for-winrt/error-handling/hresult-changed-state) | **winrt::hresult_error** | E_CHANGED_STATE |
| [**winrt::hresult_class_not_available**](/uwp/cpp-ref-for-winrt/error-handling/hresult-class-not-available) | **winrt::hresult_error** | CLASS_E_CLASSNOTAVAILABLE |
| [**winrt::hresult_illegal_delegate_assignment**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-delegate-assignment) | **winrt::hresult_error** | E_ILLEGAL_DELEGATE_ASSIGNMENT |
| [**winrt::hresult_illegal_method_call**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-method-call) | **winrt::hresult_error** | E_ILLEGAL_METHOD_CALL |
| [**winrt::hresult_illegal_state_change**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-state-change) | **winrt::hresult_error** | E_ILLEGAL_STATE_CHANGE |
| [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) | **winrt::hresult_error** | E_INVALIDARG |
| [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) | **winrt::hresult_error** | E_NOINTERFACE |
| [**winrt::hresult_not_implemented**](/uwp/cpp-ref-for-winrt/error-handling/hresult-not-implemented) | **winrt::hresult_error** | E_NOTIMPL |
| [**winrt::hresult_out_of_bounds**](/uwp/cpp-ref-for-winrt/error-handling/hresult-out-of-bounds) | **winrt::hresult_error** | E_BOUNDS |
| [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) | **winrt::hresult_error** | RPC_E_WRONG_THREAD |

请注意，每个类（通过 **hresult_error** 基类）均提供 [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function) 函数，其返回错误 HRESULT，并提供 [**message**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrormessage-function) 函数，其返回该 HRESULT 的字符串表示形式。

下面是在 C++/CX 中抛出异常的示例。

```cpp
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

以及 C++/WinRT 中的等效项。

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>将 **Platform::Object\^** 移植到 **winrt::Windows::Foundation::IInspectable**
与所有 C++/WinRT 类型一样，**winrt::Windows::Foundation::IInspectable** 属于值类型。 下面介绍如何初始化类型为 null 的变量。

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>从 **Platform::String\^** 移植到 **winrt::hstring**
**Platform::String\^** 等同于 Windows 运行时 HSTRING ABI 类型。 对于 C++/WinRT，等效项是 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)。 但使用 C++/WinRT，你可以使用 C++ 标准库宽字符串类型（如 **std::wstring**）和/或宽字符串文字调用 Windows 运行时 API。 有关更多详细信息和代码示例，请参阅 [C++/WinRT 中的字符串处理](strings.md)。

通过 C++/CX，你可以访问 [**Platform::String::Data**](https://docs.microsoft.com/en-us/cpp/cppcx/platform-string-class#data) 属性来作为 C 样式 **const wchar_t\*** 数组检索字符串（例如，将其传递到 **std::wcout**）。

```C++
auto var = titleRecord->TitleName->Data();
```

使用 C++/WinRT 也一样，你可以使用 [**hstring::c_str**](/uwp/api/windows.foundation.uri#hstringcstr-function) 函数获取 null 结尾的 C 样式字符串版本，就像从 **std::wstring** 获取的一样。

```C++
auto var = titleRecord.TitleName().c_str();
```

在实现获取或返回字符串的 API 时，通常要将使用 **Platform::String\^** 的任何 C++/CX 代码更改为使用 **winrt::hstring**。

下面是获取字符串的 C++/CX API 的示例。

```cpp
void LogWrapLine(Platform::String^ str);
```

对于 C++/WinRT，你可以像这样在 [MIDL 3.0](/uwp/midl-3) 中声明该 API。

```idl
// LogType.idl
void LogWrapLine(String str);
```

C++/WinRT 工具链随后将为你生成源代码，如下所示。

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

## <a name="important-apis"></a>重要的 API
* [winrt::delegate 结构模板](/uwp/cpp-ref-for-winrt/delegate)
* [winrt::hresult_error 结构](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::hstring 结构](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 命名空间](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相关主题
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [在 C++/WinRT 中创作事件](author-events.md)
* [利用 C++/WinRT 实现的并发和异步操作](concurrency.md)
* [通过 C++/WinRT 使用 API](consume-apis.md)
* [在 C++/WinRT 中使用代理处理事件](handle-events.md)
* [实现 C++/WinRT 与 C++/CX 之间的互操作](interop-winrt-cx.md)
* [Microsoft 接口定义语言 3.0 参考](/uwp/midl-3)
* [从 WRL 移动到 C++/WinRT](move-to-winrt-from-wrl.md)
* [C++/WinRT 中的字符串处理](strings.md)
