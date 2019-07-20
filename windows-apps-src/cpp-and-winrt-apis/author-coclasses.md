---
description: C++/WinRT 可以帮助你创作经典 COM 组件，就像它可以帮助你创作 Windows 运行时类一样。
title: 通过 C++/WinRT 创作 COM 组件
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 创作, COM, 组件
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8da62908d33c053cee4ba3f55645be9dbdcaada9
ms.sourcegitcommit: b9268ca84af56ee1c4f4ac0314e2452193369f01
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/17/2019
ms.locfileid: "68293365"
---
# <a name="author-com-components-with-cwinrt"></a>通过 C++/WinRT 创作 COM 组件

[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 可以帮助你创作经典组件对象模型 (COM) 组件（或组件类），就像它可以帮助你创作 Windows 运行时类一样。 本主题演示如何执行该操作。

## <a name="how-cwinrt-behaves-by-default-with-respect-to-com-interfaces"></a>默认情况下，C++/WinRT 在 COM 接口方面的表现如何

C++/WinRT 的 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 模板是直接或间接派生运行时类和激活工厂的基础。

默认情况下，**winrt::implements** 仅支持基于 [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable) 的接口，并且会以无提示方式忽略经典的 COM 接口。 因此，通过 **QueryInterface** (QI) 调用经典 COM 接口会失败，并出现 **E_NOINTERFACE** 错误。

我们稍后会讲述如何克服这种情况，现在让我们先来看看一个代码示例，了解默认情况下会发生什么。

```idl
// Sample.idl
runtimeclass Sample
{
    Sample();
    void DoWork();
}

// Sample.h
#include "pch.h"
#include <shobjidl.h> // Needed only for this file.

namespace winrt::MyProject
{
    struct Sample : implements<Sample, IInitializeWithWindow>
    {
        IFACEMETHOD(Initialize)(HWND hwnd);
        void DoWork();
    }
}
```

下面是使用 **Sample** 类的客户端代码。

```cppwinrt
// Client.cpp
Sample sample; // Construct a Sample object via its projection.

// This next line crashes, because the QI for IInitializeWithWindow fails.
sample.as<IInitializeWithWindow>()->Initialize(hwnd); 
```

好在要让 **winrt::implements** 支持经典 COM 接口，只需在包括任何 C++/WinRT 头文件之前包括 `unknwn.h` 即可。

可以显式这样做，也可以间接这样做，只需包括一些其他的头文件（例如 `ole2.h`）即可。 一个建议的方法是包括 `wil\cppwinrt.h` 头文件，该文件是 [Windows 实现库 (WIL)](https://github.com/Microsoft/wil) 的一部分。 `wil\cppwinrt.h` 头文件不仅可确保在 `winrt/base.h` 之前包括 `unknwn.h`，而且可以让 C++/WinRT 和 WIL 了解彼此的异常和错误代码。

## <a name="a-simple-example-of-a-com-component"></a>COM 组件的简单示例

下面是使用 C++/WinRT 编写的 COM 组件的简单示例。 这是一个迷你应用程序的完整列表，因此如果将代码粘贴到新 **Windows 控制台应用程序 (C++/WinRT)** 项目的 `pch.h` 和 `main.cpp` 中，则可对其进行测试。

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

struct __declspec(uuid("ddc36e02-18ac-47c4-ae17-d420eece2281")) IMyComInterface : ::IUnknown
{
    virtual HRESULT __stdcall Call() = 0;
};

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();

    struct MyCoclass : winrt::implements<MyCoclass, IPersist, IStringable, IMyComInterface>
    {
        HRESULT __stdcall Call() noexcept override
        {
            return S_OK;
        }

        HRESULT __stdcall GetClassID(CLSID* id) noexcept override
        {
            *id = IID_IPersist; // Doesn't matter what we return, for this example.
            return S_OK;
        }

        winrt::hstring ToString()
        {
            return L"MyCoclass as a string";
        }
    };

    auto mycoclass_instance{ winrt::make<MyCoclass>() };
    CLSID id{};
    winrt::check_hresult(mycoclass_instance->GetClassID(&id));
    winrt::check_hresult(mycoclass_instance.as<IMyComInterface>()->Call());
}
```

另请参阅[通过 C++/WinRT 使用 COM 组件](consume-com.md)。

## <a name="a-more-realistic-and-interesting-example"></a>一个更现实且有趣的示例

此主题的其余部分演练如何创建使用 C++/WinRT 实现基本组件类（COM 组件或 COM 类）和类工厂的最小控制台应用程序项目。 示例应用程序演示如何提供具有一个回调按钮的 toast 通知，组件类（实现 INotificationActivationCallback  COM 接口）使应用程序可以启动并在用户单击 toast 上的该按钮时进行回调。

有关 toast 通知功能区域的更多背景信息可以在[发送本地 toast 通知](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)中找到。 不过文档该部分中没有代码示例使用 C++/WinRT，因此建议首选本主题中演示的代码。

## <a name="create-a-windows-console-application-project-toastandcallback"></a>创建 Windows 控制台应用程序项目 (ToastAndCallback)

首先在 Microsoft Visual Studio 中创建新项目。 创建“Windows 控制台应用程序(C++/WinRT)”  项目，然后将它命名为 ToastAndCallback  。

打开 `pch.h`，在用于任何 C++/WinRT 标头的 include 语句之前添加 `#include <unknwn.h>`。 下面是结果；可以将你的 `pch.h` 的内容替换为此清单。

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

打开 `main.cpp`，删除项目模板生成 using 指令。 在其位置插入以下代码（该代码提供我们所需的库、标头和类型名称）。 下面是结果；可以将你的 `main.cpp` 的内容替换为此清单（我们还会在下面的清单中删除 `main` 中的代码，因为我们会在以后替换该函数）。

```cppwinrt
// main.cpp : Defines the entry point for the console application.

#include "pch.h"

#pragma comment(lib, "advapi32")
#pragma comment(lib, "ole32")
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

int main() { }
```

项目尚未生成；完成添加代码之后，系统会提示生成并运行。

## <a name="implement-the-coclass-and-class-factory"></a>实现组件类和类工厂

在 C++/WinRT 中，通过从 [winrt::implements  ](/uwp/cpp-ref-for-winrt/implements) 基结构派生来实现组件类和类工厂。 紧接着上面所示的三个 using 指令之后（并且在 `main` 之前），粘贴此代码以实现 toast 通知 COM 激活器组件。

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

上面的组件类实现遵循在[使用 C++/WinRT 创作 API](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class) 中演示的相同模式。 因此，可以使用相同技术实现 COM 接口以及 Windows 运行时接口。 COM 组件和 Windows 运行时类会通过接口公开其功能。 每个 COM 接口最终派生自 [IUnknown 接口  ](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)接口。 Windows 运行时基于 COM &mdash; 一个区别是 Windows 运行时接口最终派生自 [IInspectable 接口  ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)（并且 IInspectable  派生自 IUnknown  ）。

在上面代码中的组件类中，我们实现 INotificationActivationCallback::Activate  方法，这是在用户单击 toast 通知上的回调按钮时调用的函数。 但是在可以调用该函数之前，需要创建组件类的实例，这是 IClassFactory::CreateInstance  函数的工作。

我们刚刚实现的组件类称为通知的 COM 激活器  ，其类 id (CLSID) 的形式为如上所示的 `callback_guid` 标识符（类型为 GUID  ）。 我们会在以后采用“开始”菜单快捷方式和 Windows 注册表项的形式来使用该标识符。 COM 激活器 CLSID 以及指向其关联 COM 服务器的路径（这是在此处生成的可执行文件的路径）是一种机制，toast 通知该机制可知道在单击其回调按钮时要创建哪个类的实例（无论是否在操作中心内单击通知）。

## <a name="best-practices-for-implementing-com-methods"></a>实现 COM 方法的最佳做法

用于错误处理和资源管理的方法可以一起进行。 使用异常比错误代码更加方便且实用。 如果采用资源获取即初始化 (RAII) 惯用做法，则可以避免显式检查错误代码，然后显式释放资源。 这类显式检查使代码比实际所需更复杂，并使 bug 有大量位置可以隐藏。 相反，使用 RAII，并引发/捕获异常。 这样，资源分配是异常安全的，并且代码十分简单。

但是，不得允许异常转义 COM 方法实现。 可以通过对 COM 方法使用 `noexcept` 说明符来确保这一点。 可以在方法调用关系图中的任意位置引发异常，只要在方法退出之前处理它们。 如果使用 `noexcept`，但随后允许异常转义方法，则应用程序会终止。

## <a name="add-helper-types-and-functions"></a>添加帮助程序类型和函数

在此步骤中，我们会添加一些帮助程序类型和函数，代码的其余部分会使用它们。 就在 `main` 之前添加以下内容。

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

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>实现其余函数和 wmain 入口点函数

删除 `main` 函数，在其原位置粘贴此代码清单，其中包括用于注册组件类，然后提供能够回调应用程序的 toast 的代码。

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
    ::Sleep(50); // Give the callback chance to display.
}

void LaunchedNormally(HANDLE, INPUT_RECORD &, DWORD &);
void LaunchedFromNotification(HANDLE, INPUT_RECORD &, DWORD &);

int wmain(int argc, wchar_t * argv[], wchar_t * /* envp */[])
{
    winrt::init_apartment();

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
        std::wcout << this_app_name << L" is running" << (runningAsAdmin ? L" (administrator)." : L" (NOT as administrator).") << std::endl;

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

生成应用程序，然后至少以管理员身份运行一次，以便使注册（和其他设置）代码运行。 执行此操作的一种方法是以管理员身份运行 Visual Studio，然后从 Visual Studio 运行应用。 在任务栏中右键单击 Visual Studio 以显示跳转列表，在跳转列表右键单击 Visual Studio，然后单击“以管理员身份运行”  。 同意提示，然后打开项目。 运行应用程序时，会显示一条消息，指出是否在以管理员身份运行应用程序。 如果不是，则注册和其他设置不会运行。 该注册和其他设置必须至少运行一次，才能使应用程序正常工作。

无论是否在以管理员身份运行应用程序，按“T”使 toast 显示。 然后可以直接从弹出的 toast 通知或是从操作中心单击“回调 ToastAndCallback”  按钮，这样会启动应用程序，实例化组件类并执行 INotificationActivationCallback::Activate  方法。

## <a name="in-process-com-server"></a>进程内 COM 服务器

上面的 ToastAndCallback  示例应用可充当本地（或进程外）COM 服务器。 这由 [LocalServer32](/windows/desktop/com/localserver32) Windows 注册表项进行指示，该项用于注册其组件类的 CLSID。 本地 COM 服务器在可执行二进制文件 (`.exe`) 中承载其组件类。

或者（并且可能更有可能），可以选择在动态链接库 (`.dll`) 中承载组件类。 采用 DLL 形式的 COM 服务器称为进程内 COM 服务器，由使用 [InprocServer32](/windows/desktop/com/inprocserver32) Windows 注册表项注册 CLSID 进行指示。

### <a name="create-a-dynamic-link-library-dll-project"></a>创建动态链接库 (DLL) 项目

可以通过在 Microsoft Visual Studio 中创建新项目，来开始创建进程内 COM 服务器的任务。 创建“Visual C++”   > “Windows 桌面”   > “动态链接库(DLL)”  项目。

若要向新项目添加 C++/WinRT 支持，请执行[修改 Windows 桌面应用程序项目以添加 C++/WinRT 支持](/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support)中所述的步骤。

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>实现组件类、类工厂和进程内服务器导出

打开 `dllmain.cpp`，并向其添加如下所示的代码清单。

如果已具有一个实现 C++WinRT Windows 运行时类的 DLL，则表示已具有如下所示的 DllCanUnloadNow  函数。 如果要将组件类添加到该 DLL，则可以添加 DllGetClassObject  函数。

如果没有要保持与之兼容的现有 [Windows 运行时 C++ 模板库 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 代码，则可以从显示的代码中删除 WRL 部分。

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
            return winrt::make<MyCoclass>()->QueryInterface(riid, ppvObject);
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

### <a name="support-for-weak-references"></a>对弱引用的支持

另请参阅 [C++/WinRT 中的弱引用](weak-references.md#weak-references-in-cwinrt)。

如果类型实现 [IInspectable  ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)（或任何派生自 IInspectable  的接口），则 C++/WinRT（具体而言，[winrt::implements  ](/uwp/cpp-ref-for-winrt/implements) 基结构模板）会实现 [IWeakReferenceSource  ](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource)。

这是因为 IWeakReferenceSource  和 [IWeakReference  ](/windows/desktop/api/weakreference/nn-weakreference-iweakreference) 旨在用于 Windows 运行时类型。 因此，只需通过向实现添加 winrt::Windows::Foundation::IInspectable  （或派生自 IInspectable  的接口），即可为组件类启用弱引用支持。

```cppwinrt
struct MyCoclass : winrt::implements<MyCoclass, IMyComInterface, winrt::Windows::Foundation::IInspectable>
{
    //  ...
};
```

## <a name="important-apis"></a>重要的 API
* [IInspectable 接口](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [IUnknown 接口](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)
* [winrt::implements 结构模板](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>相关主题
* [使用 C++/WinRT 创作 API](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [通过 C++/WinRT 使用 COM 组件](consume-com.md)
* [发送本地 toast 通知](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
