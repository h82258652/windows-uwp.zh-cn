---
author: stevewhims
description: C + + WinRT 有助于创作传统的 COM 组件，就像它有助于你创作 Windows 运行时类。
title: 创作 COM 组件通过 C + + WinRT
ms.author: stwhi
ms.date: 09/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，标准、 c + +，cpp，winrt，投影，作者，COM、 组件
ms.localizationpriority: medium
ms.openlocfilehash: 227ffcd72150e37a513649e69bc7a6709581d65c
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2018
ms.locfileid: "4205323"
---
# <a name="author-com-components-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>使用 COM 组件中创作[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

C + + WinRT 可以帮助你创作经典组件对象模型 (COM) 组件 （或组件类），就像它有助于你创作 Windows 运行时类。 下面是非常简单的图示，你可以测试出，如果你将其粘贴到`main.cpp`新**Windows 控制台应用程序 (C + + WinRT)** 项目。

```cppwinrt
// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;

int main()
{
    init_apartment();

    struct MyCoclass : winrt::implements<MyCoclass, IPersist>
    {
        HRESULT STDMETHODCALLTYPE GetClassID(CLSID* id) noexcept override
        {
            *id = IID_IPersist; // Doesn't matter what we return, for this example.
            return S_OK;
        }
    };

    auto mycoclass_instance{ winrt::make<MyCoclass>() };
    CLSID id{};
    winrt::check_hresult(mycoclass_instance->GetClassID(&id));
}
```

另请参阅[使用 COM 组件与 C + + WinRT](consume-com.md)。

## <a name="a-more-realistic-and-interesting-example"></a>更逼真更具有趣的示例

本主题的其余部分演示了如何创建的最小控制台应用程序项目中使用 C + + /winrt 来实现基本 coclass 和类工厂。 示例应用程序显示了如何提供与回调按钮的 toast 通知，并 coclass （可实现**INotificationActivationCallback** COM 接口） 允许应用程序将启动并调用后退时用户单击 toast 上的按钮。

有关 toast 通知功能区域的更多背景位于[发送本地 toast 通知](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)。 没有任何文档的此部分中的代码示例使用 C + + WinRT，但是，因此我们建议你想在本主题中所示的代码。

## <a name="create-a-windows-console-application-project-toastandcallback"></a>创建 Windows 控制台应用程序项目 (ToastAndCallback)

首先在 Microsoft Visual Studio 中创建新项目。 创建**Visual c + +** > **Windows 桌面版** > **Windows 控制台应用程序 (C + + WinRT)** 项目，然后将其命名为*ToastAndCallback*。

打开`main.cpp`，并删除 using 指令的项目模板生成。 在自己的位置，粘贴下面的代码 （其中的库，标头和类型名称，我们需要为我们提供）。

```cppwinrt
#pragma comment(lib, "shell32")

#include <iomanip>
#include <iostream>
#include <notificationactivationcallback.h>
#include <propkey.h>
#include <propvarutil.h>
#include <shlobj.h>
#include <winrt/Windows.UI.Notifications.h>
#include <winrt/Windows.Data.Xml.Dom.h>

using namespace winrt;
using namespace Windows::Data::Xml::Dom;
using namespace Windows::UI::Notifications;
```

## <a name="implement-the-coclass-and-class-factory"></a>实现 coclass 和类工厂

在 C + + /winrt，你通过从[**winrt:: implements**](/uwp/cpp-ref-for-winrt/implements)基结构派生实现 coclass 和类工厂。 三个 using 指令如上所示后立即 (之前`main`)，将此代码来实现你的 toast 通知 COM 激活器组件粘贴。

```cppwinrt
static constexpr GUID callback_guid // BAF2FA85-E121-4CC9-A942-CE335B6F917F
{
    0xBAF2FA85, 0xE121, 0x4CC9, {0xA9, 0x42, 0xCE, 0x33, 0x5B, 0x6F, 0x91, 0x7F}
};

std::wstring const this_app_name{ L"ToastAndCallback" };

struct callback : winrt::implements<callback, INotificationActivationCallback>
{
    HRESULT __stdcall Activate(
        LPCWSTR app,
        LPCWSTR args,
        [[maybe_unused]] NOTIFICATION_USER_INPUT_DATA const* data,
        [[maybe_unused]] ULONG count) noexcept final
    {
        try
        {
            std::wcout << this_app_name << L" has been called back from a notification." << std::endl;
            std::wcout << L"Value of the 'app' parameter is '" << app << L"'." << std::endl;
            std::wcout << L"Value of the 'args' parameter is '" << args << L"'." << std::endl;
            return S_OK;
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }
};

struct callback_factory : implements<callback_factory, IClassFactory>
{
    HRESULT __stdcall CreateInstance(
        IUnknown* outer,
        GUID const& iid,
        void** result) noexcept final
    {
        *result = nullptr;

        if (outer)
        {
            return CLASS_E_NOAGGREGATION;
        }

        return make<callback>()->QueryInterface(iid, result);
    }

    HRESULT __stdcall LockServer(BOOL) noexcept final
    {
        return S_OK;
    }
};
```

实现上述 coclass 遵循中所示的相同模式[创作 Api 通过 C + + WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class)。 请注意，你可以使用此技术不仅对于 Windows 运行时接口 （最终派生自[**IInspectable**](https://msdn.microsoft.com/library/br205821)任何界面），但也要实现 COM 接口 （最终派生自[**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)任何接口）。

在上面的代码中 coclass，我们实现**INotificationActivationCallback::Activate**方法，即当用户单击 toast 通知上的回调按钮时调用的函数。 但可以调用该函数之前，需要创建，组件类的一个实例，即**IClassFactory::CreateInstance**函数的工作。

我们只需实现 coclass 称为通知， *COM 激活器*，它具有其类 id (CLSID) 的形式`callback_guid`标识符 （ **GUID**类型），请参阅上述内容。 我们将使用该标识符更高版本，在开始菜单快捷方式和 Windows 注册表条目的形式。 COM 激活器 CLSID 和其关联的 COM 服务器 （这是我们在此处生成的可执行文件的路径） 的路径是一种的机制的 toast 通知知道什么类创建其回调按钮时的实例 (是否通知，则单击在操作中心或不）。

## <a name="best-practices-for-implementing-com-methods"></a>实现 COM 方法的最佳做法

错误处理和资源管理技术可以转手中手。 它是更加方便和实际使用比错误代码的异常。 并且如果你使用的资源的购置-即-初始化 (RAII) 用法，然后你可以避免显式检查错误代码，然后显式释放资源。 此类显式检查使代码更复杂超出必要，并提供 bug 很多地方来隐藏。 相反，使用 RAII，并引发/catch 异常。 这样一来，资源分配为异常安全的并且你的代码为简单。

但是，不允许异常转义你 COM 方法实现。 你可以确保通过使用`noexcept`COM 方法上的说明符。 只要你的方法退出之前处理它们是方法的确定你的调用图中任意位置引发异常。 如果你使用`noexcept`，但你然后允许异常转义你方法中，则将终止你的应用程序。

## <a name="add-helper-types-and-functions"></a>添加帮助程序类型和函数

在此步骤中，我们将添加一些帮助程序类型和函数可以使其余的代码使用。 因此之前, `main`，添加以下内容。

```cppwinrt
struct prop_variant : PROPVARIANT
{
    prop_variant() noexcept : PROPVARIANT{}
    {
    }

    ~prop_variant() noexcept
    {
        clear();
    }

    void clear() noexcept
    {
        WINRT_VERIFY_(S_OK, ::PropVariantClear(this));
    }
};

struct registry_traits
{
    using type = HKEY;

    static void close(type value) noexcept
    {
        WINRT_VERIFY_(ERROR_SUCCESS, ::RegCloseKey(value));
    }

    static constexpr type invalid() noexcept
    {
        return nullptr;
    }
};

using registry_key = winrt::handle_type<registry_traits>;

std::wstring get_module_path()
{
    std::wstring path(100, L'?');
    uint32_t path_size{};
    DWORD actual_size{};

    do
    {
        path_size = static_cast<uint32_t>(path.size());
        actual_size = ::GetModuleFileName(nullptr, path.data(), path_size);

        if (actual_size + 1 > path_size)
        {
            path.resize(path_size * 2, L'?');
        }
    } while (actual_size + 1 > path_size);

    path.resize(actual_size);
    return path;
}

std::wstring get_shortcut_path()
{
    std::wstring format{ LR"(%ProgramData%\Microsoft\Windows\Start Menu\Programs\)" };
    format += (this_app_name + L".lnk");

    auto required{ ::ExpandEnvironmentStrings(format.c_str(), nullptr, 0) };
    std::wstring path(required - 1, L'?');
    ::ExpandEnvironmentStrings(format.c_str(), path.data(), required);
    return path;
}
```

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>实现其余的函数和 wmain 入口点函数

项目模板生成`main`为你的函数。 删除`main`函数，并在其位置粘贴此代码列表，其中包括注册你 coclass 的代码，然后提供能够调用返回你的应用程序的 toast。

```cppwinrt
void register_callback()
{
    DWORD registration{};

    winrt::check_hresult(::CoRegisterClassObject(
        callback_guid,
        make<callback_factory>().get(),
        CLSCTX_LOCAL_SERVER,
        REGCLS_SINGLEUSE,
        &registration));
}

void create_shortcut()
{
    auto link{ winrt::create_instance<IShellLink>(CLSID_ShellLink) };
    std::wstring module_path{ get_module_path() };
    winrt::check_hresult(link->SetPath(module_path.c_str()));

    auto store = link.as<IPropertyStore>();
    prop_variant value;
    winrt::check_hresult(::InitPropVariantFromString(this_app_name.c_str(), &value));
    winrt::check_hresult(store->SetValue(PKEY_AppUserModel_ID, value));
    value.clear();
    winrt::check_hresult(::InitPropVariantFromCLSID(callback_guid, &value));
    winrt::check_hresult(store->SetValue(PKEY_AppUserModel_ToastActivatorCLSID, value));

    auto file{ store.as<IPersistFile>() };
    std::wstring shortcut_path{ get_shortcut_path() };
    winrt::check_hresult(file->Save(shortcut_path.c_str(), TRUE));

    std::wcout << L"In " << shortcut_path << L", created a shortcut to " << module_path << std::endl;
}

void update_registry()
{
    std::wstring key_path{ LR"(SOFTWARE\Classes\CLSID\{????????-????-????-????-????????????})" };
    ::StringFromGUID2(callback_guid, key_path.data() + 23, 39);
    key_path += LR"(\LocalServer32)";
    registry_key key;

    winrt::check_win32(::RegCreateKeyEx(
        HKEY_CURRENT_USER,
        key_path.c_str(),
        0,
        nullptr,
        0,
        KEY_WRITE,
        nullptr,
        key.put(),
        nullptr));
    ::RegDeleteValue(key.get(), nullptr);

    std::wstring path{ get_module_path() };

    winrt::check_win32(::RegSetValueEx(
        key.get(),
        nullptr,
        0,
        REG_SZ,
        reinterpret_cast<BYTE const*>(path.c_str()),
        static_cast<uint32_t>((path.size() + 1) * sizeof(wchar_t))));

    std::wcout << L"In " << key_path << L", registered local server at " << path << std::endl;
}

void create_toast()
{
    XmlDocument xml;

    std::wstring toastPayload
    {
        LR"(
<toast>
  <visual>
    <binding template='ToastGeneric'>
      <text>)"
    };
    toastPayload += this_app_name;
    toastPayload += LR"(
      </text>
    </binding>
  </visual>
  <actions>
    <action content='Call back )";
    toastPayload += this_app_name;
    toastPayload += LR"(
' arguments='the_args' activationKind='Foreground' />
  </actions>
</toast>)";
    xml.LoadXml(toastPayload);

    ToastNotification toast{ xml };
    ToastNotifier notifier{ ToastNotificationManager::CreateToastNotifier(this_app_name) };
    notifier.Show(toast);
}

void LaunchedNormally(HANDLE, INPUT_RECORD &, DWORD &);
void LaunchedFromNotification(HANDLE, INPUT_RECORD &, DWORD &);

int wmain(int argc, wchar_t * argv[], wchar_t * /* envp */[])
{
    init_apartment();

    register_callback();

    HANDLE consoleHandle{ ::GetStdHandle(STD_INPUT_HANDLE) };
    INPUT_RECORD buffer{};
    DWORD events{};
    ::FlushConsoleInputBuffer(consoleHandle);

    if (argc == 1)
    {
        LaunchedNormally(consoleHandle, buffer, events);
    }
    else if (argc == 2 && wcscmp(argv[1], L"-Embedding") == 0)
    {
        LaunchedFromNotification(consoleHandle, buffer, events);
    }
}

void LaunchedNormally(HANDLE consoleHandle, INPUT_RECORD & buffer, DWORD & events)
{
    try
    {
        bool runningAsAdmin{ ::IsUserAnAdmin() == TRUE };
        std::wcout << this_app_name << L" is running" << (runningAsAdmin ? L" (Administrator)." : L".") << std::endl;

        if (runningAsAdmin)
        {
            create_shortcut();
            update_registry();
        }

        std::wcout << std::endl << L"Press 'T' to display a toast notification (press any other key to exit)." << std::endl;

        ::ReadConsoleInput(consoleHandle, &buffer, 1, &events);
        if (towupper(buffer.Event.KeyEvent.uChar.UnicodeChar) == L'T')
        {
            create_toast();
        }
    }
    catch (winrt::hresult_error const& e)
    {
        std::wcout << L"Error: " << e.message().c_str() << L" (" << std::hex << std::showbase << std::setw(8) << static_cast<uint32_t>(e.code()) << L")" << std::endl;
    }
}

void LaunchedFromNotification(HANDLE consoleHandle, INPUT_RECORD & buffer, DWORD & events)
{
    ::Sleep(50); // Give the callback chance to display its message.
    std::wcout << std::endl << L"Press any key to exit." << std::endl;
    ::ReadConsoleInput(consoleHandle, &buffer, 1, &events);
}
```

## <a name="how-to-test-the-example-application"></a>如何测试示例应用程序

生成应用程序，然后再在以管理员身份会导致在注册时，和其他设置，若要运行的代码至少一次运行它。 指示是否正在运行它作为管理员，然后按 \ 以使一个 toast 以显示。 然后可以单击直接从 toast 通知，将启动 pop，或从操作中心，并且你的应用程序、 实例化，coclass 和 INotificationActivationCallback****调用返回 ToastAndCallback**按钮:: 激活**执行方法。

## <a name="in-process-com-server"></a>进程内 COM 服务器

上述*ToastAndCallback*示例应用作为本地 （或进程外） COM 服务器。 这是你用于注册它，指示的[LocalServer32](/windows/desktop/com/localserver32) Windows 注册表项。 本地 COM 服务器承载其 coclass(es) 内的可执行二进制文件 ( `.exe`)。

或者 （和可以说更有可能），你可以选择托管你 coclass(es) 内动态链接库 ( `.dll`)。 形式的 DLL 的 COM 服务器被称为进程内 COM 服务器，并由正在使用注册的[InprocServer32](/windows/desktop/com/inprocserver32) Windows 注册表项。

### <a name="create-a-dynamic-link-library-dll-project"></a>创建动态链接库 (DLL) 项目

你可以开始通过在 Microsoft Visual Studio 中创建新项目中创建的进程内 COM 服务器的任务。 创建**Visual c + +** > **Windows 桌面版** > **动态链接库 (DLL)** 项目。

### <a name="set-project-properties"></a>设置项目属性

转到项目属性**常规** \> **Windows SDK 版本**，并选择**所有配置**和**所有平台**。 设置**Windows SDK 版本**为*10.0.17134.0 （Windows 10，版本 1803年）*，或更高版本。

若要添加 Visual Studio 支持 C + + WinRT 到你的项目，编辑你`.vcxproj`文件，找到`<PropertyGroup Label="Globals">`，该属性在组内，设置属性`<CppWinRTEnabled>true</CppWinRTEnabled>`。

因为 C + + WinRT 使用中的 C + + 17 标准的功能，请将项目属性**C/c + +** > **语言** > 的**c + + 语言标准** *ISO C + + 17 标准 (/ std:c + + 17)*。

### <a name="the-precompiled-header"></a>在预编译标头

重命名你`stdafx.h`和`stdafx.cpp`到`pch.h`和`pch.cpp`分别。 设置项目属性**C/c + +** > **预编译标头** >  *pch.h***预编译标头文件**。

查找和替换所有`#include "stdafx.h"`与`#include "pch.h"`。

在`pch.h`，包括`winrt/base.h`。

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

确认你正在不受[为什么不会我的新项目编译？](/windows/uwp/cpp-and-winrt-apis/faq)。

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>实现 coclass、 类工厂和进程内服务器导出

打开`dllmain.cpp`，并向其添加代码列表，如下所示。

如果你已经拥有一个 DLL，实现 C + + WinRT Windows 运行时类，则你将已经**DllCanUnloadNow**函数如下所示。 如果你想要添加到此 DLL 组件类，你可以添加**DllGetClassObject**函数。

如果没有现有的[Windows 运行时 c + + 模板库 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)代码你想要保持兼容，则你可以从显示的代码中删除 WRL 部分。

```cppwinrt
// dllmain.cpp

struct MyCoclass : winrt::implements<MyCoclass, IPersist>
{
    HRESULT STDMETHODCALLTYPE GetClassID(CLSID* id) noexcept override
    {
        *id = IID_IPersist; // Doesn't matter what we return, for this example.
        return S_OK;
    }
};

struct __declspec(uuid("85d6672d-0606-4389-a50a-356ce7bded09"))
    MyCoclassFactory : winrt::implements<MyCoclassFactory, IClassFactory>
{
    HRESULT STDMETHODCALLTYPE CreateInstance(IUnknown *pUnkOuter, REFIID riid, void **ppvObject) noexcept override
    {
        try
        {
            *ppvObject = winrt::make<MyCoclass>().get();
            return S_OK;
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }

    HRESULT STDMETHODCALLTYPE LockServer(BOOL fLock) noexcept override
    {
        // ...
        return S_OK;
    }

    // ...
};

HRESULT __stdcall DllCanUnloadNow()
{
#ifdef _WRL_MODULE_H_
    if (!::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().Terminate())
    {
        return S_FALSE;
    }
#endif

    if (winrt::get_module_lock())
    {
        return S_FALSE;
    }

    winrt::clear_factory_cache();
    return S_OK;
}

HRESULT __stdcall DllGetClassObject(GUID const& clsid, GUID const& iid, void** result)
{
    try
    {
        *result = nullptr;

        if (clsid == __uuidof(MyCoclassFactory))
        {
            return winrt::make<MyCoclassFactory>()->QueryInterface(iid, result);
        }

#ifdef _WRL_MODULE_H_
        return ::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().GetClassObject(clsid, iid, result);
#else
        return winrt::hresult_class_not_available().to_abi();
#endif
    }
    catch (...)
    {
        return winrt::to_hresult();
    }
}
```

## <a name="important-apis"></a>重要的 API
* [IInspectable 接口](https://msdn.microsoft.com/library/br205821)
* [IUnknown 接口](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [winrt::implements 结构模板](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>相关主题
* [使用 C++/WinRT 创作 API](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [使用 COM 组件通过 C + + WinRT](consume-com.md)
* [发送本地 toast 通知](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
