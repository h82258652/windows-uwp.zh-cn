---
description: 本主题介绍如何将 C++/CX 代码移植到 C++/WinRT 中的等效项。
title: 从 C++/CX 移动到 C++/WinRT
ms.date: 01/17/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 端口, 迁移, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: fe988bffbf024308fb5d43da7ed538e5330b58de
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635072"
---
# <a name="move-to-cwinrt-from-ccx"></a>从 C++/CX 移动到 C++/WinRT

本主题说明如何在代码移植[C + + /cli CX](/cpp/cppcx/visual-c-language-reference-c-cx)项目的对等[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)。

## <a name="porting-strategies"></a>迁移策略

如果您希望将逐渐移植 C + + /CX 代码到 C + + / WinRT，则可以。 C + + /CX 和 C + + WinRT 代码可以共存于同一项目，但 XAML 编译器支持和 Windows 运行时组件的情况除外。 有关这些两个例外情况，你将需要面向 C + + /CX 或 C + + WinRT 同一项目中的。

> [!IMPORTANT]
> 如果项目生成的 XAML 应用程序，则我们建议的一个工作流是首次使用一个 C + 的 Visual Studio 中创建一个新项目 + / WinRT 项目模板 (请参阅[Visual Studio 支持 C + + WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package))。 然后，启动源代码和标记从复制 C + + /cli CX 项目。 可以添加包含新的 XAML 页面**项目** \> **添加新项...**\> **Visual c + +** > **空白页 (C + + WinRT)**。
>
> 或者，可以使用 Windows 运行时组件来分解代码从 XAML C + + /cli CX 项目移植。 将移动尽可能多的 C + + /cli CX 代码按可以一个组件，然后将 XAML 项目更改为 C + + WinRT。 或其他保留 XAML 项目为 C + + /CX 中，创建一个新的 C + + WinRT 组件，并开始移植 C + + /CX 代码移出 XAML 项目和组件。 您还可以让 C + + /cli CX 组件项目和 C + + WinRT 组件项目在同一解决方案中的引用这两个文件从应用程序项目，并逐渐从到另一个端口。 请参阅[互操作之间 C + + WinRT 和 C + + /cli CX](interop-winrt-cx.md)有关同一项目中使用这两种语言投影的详细信息。

> [!NOTE]
> [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 和 Windows SDK 都在根命名空间 **Windows** 中声明类型。 投影到 C++/WinRT 的 Windows 类型具有与 Windows 类型相同的完全限定名称，但放置于 C++ **winrt** 命名空间中。 这些不同的命名空间可让你按照自己的节奏从 C++/CX 移植到 C++/WinRT。

请牢记上面提到的异常的第一步中移植 C + + /cli CX 项目到 C + + WinRT 是手动添加 C + + WinRT 支持添加到它 (，请参阅[Visual Studio 支持 C + + WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package))。 为此，请安装[Microsoft.Windows.CppWinRT NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)到你的项目。 打开 Visual Studio 项目中，单击**项目** \> **管理 NuGet 包...**\> **浏览**，键入或粘贴**Microsoft.Windows.CppWinRT**在搜索框中，在搜索结果中选择的项，然后单击**安装**若要安装该项目的包。 这一更改的一个效果是对 C++/CX 的支持在项目中关闭。 它是一个好办法将处于关闭状态，以便生成消息帮助您查找 （和端口） 的支持在 C + 上所有依赖项 + /CX 中，也可以将重新打开的支持 (在项目属性中， **C/c + +** \> **常规**\> **使用 Windows 运行时扩展** \> **是 (/ZW)**)，并逐渐端口。

请确保该项目属性**常规** \> **目标平台版本**设置为 10.0.17134.0 (Windows 10，版本 1803年) 或更高版本。

在预编译的标头文件（通常为 `pch.h`）中，包括 `winrt/base.h`。

```cppwinrt
#include <winrt/base.h>
```

如果你包括了任何 C++/WinRT 投影 Windows API 标头（例如，`winrt/Windows.Foundation.h`），那么你无需像这样明确包括 `winrt/base.h`，因为它将自动为你包含在内。

如果你的项目还使用 [Windows 运行时 C++ 模板库 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 类型，请参阅[从 WRL 迁移到 C++/WinRT](move-to-winrt-from-wrl.md)。

## <a name="parameter-passing"></a>参数传递
时编写 C + + /cli CX 源代码，您通过 C + + /cli CX 类型作为函数参数为 hat (\^) 的引用。

```cppcx
void LogPresenceRecord(PresenceRecord^ record);
```

在 C++/WinRT 中，对于同步函数，默认情况下应该使用 `const&` 参数。 这将避免复制和互锁开销。 但你的协同程序应使用按值传递来确保它们按值捕获，并避免生命周期问题（更多详细信息，请参阅[利用 C++/WinRT 实现的并发和异步操作](concurrency.md)）。

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

C++/WinRT 对象根本上是一个保留支持 Windows 运行时对象的接口指针的值。 在复制 C++/WinRT 对象时，编译器复制封装的接口指针，从而递增其引用计数。 副本的最终销毁涉及递减引用计数。 因此，仅在必要时产生复制开销。

## <a name="variable-and-field-references"></a>变量和字段引用
当编写 C + + /cli CX 源代码，使用 hat (\^) 变量来引用 Windows 运行时对象，并使用箭头 (-&gt;) 运算符来取消 hat 变量。

```cppcx
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

当移植到等效的 C + + WinRT 代码中，可以通过删除尖角符号，以及更改箭头操作符来获取一大步 (-&gt;) 到点运算符 （.）。 C + + /cli 投影的 WinRT 类型是值和而不是指针。

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

默认构造函数的 C + + /cli CX hat 指针将其初始化为 null。 下面是 C + + /cli CX 我们在其中创建正确的类型，但具有未初始化的局部变量/字段的代码示例。 换而言之，它不最初是指**TextBlock**; 我们想要的引用分配更高版本。

```cppcx
TextBlock^ textBlock;

class MyClass
{
    TextBlock^ textBlock;
};
```

有关等效的 C + + / WinRT，请参阅[延迟初始化](consume-apis.md#delayed-initialization)。

## <a name="properties"></a>属性
C++/CX 语言扩展包括属性概念。 编写 C++/CX 源代码时，你可以像访问字段那样访问属性。 标准 C++ 没有属性概念，因此，在 C++/WinRT 中，你调用获取和设置函数。

在随后的示例中，**XboxUserId**、**UserState**、**PresenceDeviceRecords** 和 **Size** 全部都是属性。

### <a name="retrieving-a-value-from-a-property"></a>从属性检索值
下面介绍如何在 C++/CX 中获取属性值。

```cppcx
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

```cppcx
record->UserState = newValue;
```

若要在 C++/WinRT 执行同等操作，调用与属性同名的函数，并传递参数。

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>创建类的实例
使用 C + + /cli CX 对象的句柄，它通常称为乘幂号的通过 (\^) 引用。 通过 `ref new` 关键字创建新对象，这反过来会调用 [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646) 来激活运行时类的新实例。

```cppcx
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

```cppcx
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

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>从基本运行时类转换为派生的一个
它是通常会有引用到基类，你知道引用派生类型的对象。 在 C + + /CX 中，使用`dynamic_cast`到*强制转换*到引用派生到基类引用。 `dynamic_cast`实际上只是一个对隐藏调用[ **QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521)。 下面是一个典型示例&mdash;处理依赖关系属性更改事件，并且你想要强制转换从**DependencyObject**回实际拥有该依赖属性的类型。

```cppcx
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject^ d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e)
{
    BgLabelControl^ theControl{ dynamic_cast<BgLabelControl^>(d) };

    if (theControl != nullptr)
    {
        // succeeded ...
    }
}
```

等效的 C + + WinRT 代码将替换`dynamic_cast`通过调用[ **IUnknown::try_as** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)函数，它封装**QueryInterface**。 您还可以选择调用[ **IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)，相反，这将引发异常如果未返回查询所需的接口 （所请求的类型的默认接口）。 下面是 C + + WinRT 的代码示例。

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

```cppcx
auto token = myButton->Click += ref new RoutedEventHandler([=](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

它是 C++/WinRT 中的等效项。

```cppwinrt
auto token = myButton().Click([=](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

不使用 lambda 函数，你可以选择作为自由函数或指向成员函数的指针实现代理。 有关详细信息，请参阅[使用 C++/WinRT 中的代理来处理事件](handle-events.md)。

如果你正在从内部使用（不是跨二进制文件）事件和代理的 C++/CX 基本代码移植，[**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) 将帮助你复制 C++/WinRT 中的这个模式。 另请参阅[委托、 简单信号和项目中的回调参数化](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project)。

## <a name="revoking-a-delegate"></a>撤销代理
在 C++/CX 中，使用 `-=` 运算符来撤销之前的事件注册。

```cppcx
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
| **Platform:: agile\^** | [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform:: array\^** | 请参阅[端口**platform:: array\^**](#port-platformarray) |
| **Platform:: exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform:: invalidargumentexception\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform:: object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform:: string\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagileref"></a>端口**platform:: agile\^** 到**winrt::agile_ref**
**Platform:: agile\^** 类型在 C + + /cli CX 表示可以从任意线程访问的 Windows 运行时类。 C + + WinRT 等效项是[ **winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref)。

在 C++/CX 中。

```cppcx
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

在 C++/WinRT 中。

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformarray"></a>端口**platform:: array\^**
您的选择包括使用初始值设定项列表**std:: array**，或**std:: vector**。 有关详细信息和代码示例，请参阅[标准的初始值设定项列表](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists)并[标准数组和矢量](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-arrays-and-vectors)。

### <a name="port-platformexception-to-winrthresulterror"></a>端口**platform:: exception\^** 到**winrt::hresult_error**
**Platform:: exception\^** 类型生成在 C + + /cli CX 时 Windows 运行时 API 将返回非 S\_确定 HRESULT。 C++/WinRT 的等效项是 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)。

移植到 C + + / WinRT，使用的所有代码都更改**platform:: exception\^** 若要使用**winrt::hresult_error**。

在 C++/CX 中。

```cppcx
catch (Platform::Exception^ ex)
```

在 C++/WinRT 中。

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT 提供这些异常类。

| 例外类型 | 基类 | HRESULT |
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

```cppcx
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

以及 C++/WinRT 中的等效项。

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>端口**platform:: object\^** 到**winrt::Windows::Foundation::IInspectable**
与所有 C++/WinRT 类型一样，**winrt::Windows::Foundation::IInspectable** 属于值类型。 下面介绍如何初始化类型为 null 的变量。

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>端口**platform:: string\^** 到**winrt::hstring**
**Platform:: string\^** 等效于 Windows 运行时 HSTRING ABI 类型。 对于 C++/WinRT，等效项是 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)。 但使用 C++/WinRT，你可以使用 C++ 标准库宽字符串类型（如 **std::wstring**）和/或宽字符串文字调用 Windows 运行时 API。 有关更多详细信息和代码示例，请参阅 [C++/WinRT 中的字符串处理](strings.md)。

使用 C + + /CX 中，您可以访问[ **Platform::String::Data** ](https://docs.microsoft.com/en-us/cpp/cppcx/platform-string-class#data)属性来检索为 C 样式字符串**const wchar_t\***  （例如，若要传递的数组它对**std::wcout**)。

```cppcx
auto var{ titleRecord->TitleName->Data() };
```

使用 C++/WinRT 也一样，你可以使用 [**hstring::c_str**](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) 函数获取 null 结尾的 C 样式字符串版本，就像从 **std::wstring** 获取的一样。

```cppwinrt
auto var{ titleRecord.TitleName().c_str() };
```

谈到实现执行或返回字符串的 Api，你通常更改任何 C + + /CX 代码使用**platform:: string\^** 若要使用**winrt::hstring**相反。

下面是获取字符串的 C++/CX API 的示例。

```cppcx
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

#### <a name="tostring"></a>Tostring （)

C + + /cli CX 提供[object:: tostring](/cpp/cppcx/platform-object-class?view=vs-2017#tostring)方法。

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

C + + WinRT 不直接提供此功能，但您可以将替代项。

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

## <a name="important-apis"></a>重要的 API
* [winrt::delegate 结构模板](/uwp/cpp-ref-for-winrt/delegate)
* [winrt::hresult_error 结构](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::hstring 结构](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 命名空间](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相关主题
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [创作事件在 C + + WinRT](author-events.md)
* [并发和异步操作使用 C + + WinRT](concurrency.md)
* [通过 C++/WinRT 使用 API](consume-apis.md)
* [处理事件，通过使用委托中 C + + WinRT](handle-events.md)
* [实现 C++/WinRT 与 C++/CX 之间的互操作](interop-winrt-cx.md)
* [Microsoft 接口定义语言 3.0 引用](/uwp/midl-3)
* [从 WRL 移动到 C++/WinRT](move-to-winrt-from-wrl.md)
* [字符串处理中 C + + WinRT](strings.md)
