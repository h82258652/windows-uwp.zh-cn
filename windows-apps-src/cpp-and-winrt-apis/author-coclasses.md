---
description: C++/WinRT 可以帮助你创作经典 COM 组件，就像它可以帮助你创作 Windows 运行时类一样。
title: 通过 C++/WinRT 创作 COM 组件
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10、 uwp、 标准版、 c + +、 cpp、 winrt、 投影、 作者、 COM、 组件
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3badcd59155bc4bb5ef8d9e29271b853c245c24e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360314"
---
# <a name="author-com-components-with-cwinrt"></a>通过 C++/WinRT 创作 COM 组件

[C++/ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)可帮助你创作经典组件对象模型 (COM) 组件 （或组件类），就像它可帮助你创作 Windows 运行时类。 下面是如果您将代码粘贴到可以测试的简单说明`pch.h`并`main.cpp`的新**Windows 控制台应用程序 (C++/WinRT)** 项目。

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

另请参阅[使用 COM 组件与C++/WinRT](consume-com.md)。

## <a name="a-more-realistic-and-interesting-example"></a>一个更现实、 更有趣的示例

此主题的其余部分将指导完成创建的最小的控制台应用程序项目中使用C++/WinRT 来实现基本组件类 （COM 组件或 COM 类） 和类工厂。 示例应用程序演示如何传递包含，一个回调按钮和组件类的 toast 通知 (它实现**INotificationActivationCallback** COM 接口) 允许应用程序启动，并且命名为返回当用户单击该按钮上的 toast 通知。

有关的 toast 通知功能区的更多背景信息，请参阅[发送本地 toast 通知](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)。 在该部分中的文档使用的代码示例都没有C++/WinRT，不过，因此我们建议您希望在本主题中所示的代码。

## <a name="create-a-windows-console-application-project-toastandcallback"></a>创建一个 Windows 控制台应用程序项目 (ToastAndCallback)

首先在 Microsoft Visual Studio 中创建新项目。 创建**Windows 控制台应用程序 (C++/WinRT)** 项目，并将其命名*ToastAndCallback*。

打开`pch.h`，并添加`#include <unknwn.h>`之前包括任何 C + WinRT 标头。 下面是结果;内容替换为你`pch.h`与此列表。

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

打开`main.cpp`，并删除使用指令的项目模板生成。 在其位置插入以下代码 （库、 标头和我们需要的类型名称，请为我们提供了）。 下面是结果;内容替换为你`main.cpp`与此列表 (我们还将删除从代码`main`在列表中，因为我们将会替换该函数更高版本)。

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

该项目不会生成尚未;我们已添加代码后，将提示您生成并运行。

## <a name="implement-the-coclass-and-class-factory"></a>实现的组件类和类工厂

在C++/WinRT，您通过派生自实现组件类和类工厂[ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements)基结构。 立即三个使用-在指令后面如上所示 (以及之前`main`)，粘贴此代码以实现你的 toast 通知 COM 激活器组件。

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

上面的组件类实现遵循相同的模式中所示[作者 Api 与C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class)。 因此，您可以使用相同技术来实现 COM 接口，以及 Windows 运行时接口。 COM 组件和 Windows 运行时类公开接口通过其功能。 每个 COM 接口最终派生[ **IUnknown 接口**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)接口。 Windows 运行时基于 COM&mdash;一个区别正在 Windows 运行时接口最终派生自[ **IInspectable 接口**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (和**IInspectable**派生自**IUnknown**)。

在上面的代码中组件类，我们实现**INotificationActivationCallback::Activate**方法，即当用户单击上一条 toast 通知的回调按钮时调用的函数。 可以调用该函数之前，请组件类的实例需要创建，并为的作业，但**IClassFactory::CreateInstance**函数。

我们只是实现 coclass 称为*COM 激活器*通知，以及它有其类 id (CLSID) 中的窗体`callback_guid`标识符 (类型的**GUID**)，如上所示。 我们将使用该标识符更高版本，在开始菜单快捷方式和 Windows 注册表项的窗体中。 COM 激活器 CLSID，以及指向其关联的 COM 服务器 （这是我们此处要生成的可执行文件的路径） 的路径是一条 toast 通知知道什么类来创建实例单击其回调按钮时所依据的机制 (是否单击通知在操作中心中或不）。

## <a name="best-practices-for-implementing-com-methods"></a>实现的 COM 方法的最佳做法

有关错误处理和资源管理的技术可以转手中手动。 它是更方便和实际使用比错误代码的异常。 如果您采用资源-获取-即-初始化 (RAII) 惯用做法，然后您可以避免显式检查错误代码，然后显式地释放资源。 此类显式检查使代码更复杂比实际所需，并使 bug 很多要隐藏的位数。 相反，使用 RAII，并引发/catch 异常。 这样一来，资源分配是异常安全的并且你的代码很简单。

但是，你不得允许异常转义您 COM 方法的实现。 可以确保使用`noexcept`COM 方法说明符。 是使用你的调用关系图中任意位置引发异常，只要您的方法退出之前，会处理它们。 如果使用`noexcept`，但然后允许异常转义你的方法，则你的应用程序将终止。

## <a name="add-helper-types-and-functions"></a>添加帮助器类型和函数

在此步骤中，我们将添加代码的其余部分使某些帮助器类型以及函数的使用。 这样之前, `main`，将以下内容添加。

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

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>实现剩余函数和 wmain 入口点函数

删除你`main`函数，并在其原位置粘贴此代码列表，其中包括代码以注册你组件类，，然后将返回调用应用程序的支持 toast 通知。

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

生成该应用程序，然后在以管理员身份在注册时和其他设置，若要运行的代码会导致至少一次运行它。 若要执行此操作的一种方法是以管理员身份运行 Visual Studio，然后从 Visual Studio 运行应用程序。 若要显示跳转列表，右键单击跳转列表中，Visual Studio，然后单击任务栏中右键单击 Visual Studio**以管理员身份运行**。 在提示符下，即表示同意，然后打开项目。 在运行该应用程序时，会显示一条消息是，该值指示以管理员身份运行该应用程序。 如果不是，注册和其他设置不会运行。 该注册和其他安装程序必须至少运行一次为应用程序才能正常工作。

指示是否以管理员身份运行该应用程序，按 'T' 导致 toast 通知显示。 然后可以单击**回叫 ToastAndCallback**按钮直接从弹出，字样的 toast 通知或从操作中心和你的应用程序将启动，实例化的组件类和**INotificationActivationCallback::Activate**执行方法。

## <a name="in-process-com-server"></a>进程内 COM 服务器

*ToastAndCallback*上述示例应用程序充当本地 （或进程外） 的 COM 服务器。 这将由[LocalServer32](/windows/desktop/com/localserver32)可用于将注册其组件类的 CLSID 的 Windows 注册表项。 本地 COM 服务器承载可执行二进制文件在其 coclass(es) ( `.exe`)。

或者 （和无疑是更有可能），您可以选择托管你 coclass(es) 内动态链接库 ( `.dll`)。 为进程内 COM 服务器，已知 DLL 的窗体中的 COM 服务器和使用注册的 Clsid 指示[InprocServer32](/windows/desktop/com/inprocserver32) Windows 注册表项。

### <a name="create-a-dynamic-link-library-dll-project"></a>创建一个动态链接库 (DLL) 项目

你可以开始在 Microsoft Visual Studio 中创建新项目创建进程内 COM 服务器的任务。 创建**可视化C++**   >  **Windows 桌面** > **动态链接库 (DLL)** 项目。

若要添加C++/WinRT 支持添加到新项目中，按照中所述的步骤[修改 Windows 桌面应用程序项目，以添加C++/WinRT 支持](/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support)。

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>实现 coclass、 类工厂和进程内服务器导出

打开`dllmain.cpp`，并向其添加如下所示的代码列表。

如果已有一个 DLL，它实现C++WinRT Windows 运行时类，则您必须已经**DllCanUnloadNow**函数如下所示。 如果你想要将组件类添加到该 DLL，则可以添加**DllGetClassObject**函数。

如果没有现有[Windows 运行时C++模板库 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)你想要保持与兼容，则可以删除 WRL 的代码部分中所示的代码。

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

### <a name="support-for-weak-references"></a>弱引用支持

另请参阅[中的弱引用C++/WinRT](weak-references.md#weak-references-in-cwinrt)。

C++/ WinRT (具体而言， [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements)基结构模板) 实现[ **IWeakReferenceSource** ](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) ，如果你类型实现[ **IInspectable** ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (或任何派生的接口**IInspectable**)。

这是因为**IWeakReferenceSource**并[ **IWeakReference** ](/windows/desktop/api/weakreference/nn-weakreference-iweakreference)专为 Windows 运行时类型。 因此，您可以打开弱引用支持用于 coclass 你只需通过添加**winrt::Windows::Foundation::IInspectable** (或接口派生自**IInspectable**) 到您的实现。

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
