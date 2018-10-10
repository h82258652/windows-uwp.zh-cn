---
author: andrewleader
Description: Learn how Win32 C++ WRL apps can send local toast notifications and handle the user clicking the toast.
title: 从桌面 C++ WRL 应用发送本地 toast 通知
label: Send a local toast notification from desktop C++ WRL apps
template: detail.hbs
ms.author: mijacobs
ms.date: 03/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, win32, 桌面, toast 通知, 发送 toast, 发送本地 toast, 桌面桥, C++, cpp, cplusplus, WRL
ms.localizationpriority: medium
ms.openlocfilehash: a6134e65a27563f96c03880dea026bed11f46641
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2018
ms.locfileid: "4498539"
---
# <a name="send-a-local-toast-notification-from-desktop-c-wrl-apps"></a>从桌面 C++ WRL 应用发送本地 toast 通知

桌面应用（包括桌面桥和经典 Win32）可以像通用 Windows 平台 (UWP) 应用一样发送交互式 toast 通知。 但是，由于激活方案不同，桌面应用存在一些特殊步骤，并且若不使用桌面桥，可能会缺少程序包标识符。

> [!IMPORTANT]
> 如果要编写 UWP 应用，请参阅 [UWP 文档](send-local-toast.md)。 有关其他桌面语言，请参阅[桌面 C#](send-local-toast-desktop.md)。


## <a name="step-1-enable-the-windows-10-sdk"></a>步骤 1：启用 Windows 10 SDK

如果尚未对 Win32 应用启用 Windows 10 SDK，必须先执行该操作。 下面是几个关键步骤...

1. 将 `runtimeobject.lib` 添加到**其他依赖项**
2. 以 Windows 10 SDK 为目标

右键单击项目，选择**属性**。

在顶部**配置**菜单中，选择**全部配置**，以便将以下更改同时应用到“调试”和“发布”。

在**链接器 -> 输入**下，将 `runtimeobject.lib` 添加到**其他依赖项**。

然后在**常规**下，确保将 **Windows SDK 版本**设置为 10.0 或更高版本（而非 Windows 8.1）。


## <a name="step-2-copy-compat-library-code"></a>步骤 2：复制兼容库代码

将 [DesktopNotificationManagerCompat.h](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.h) 和 [DesktopNotificationManagerCompat.cpp](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.cpp) 文件从 GitHub 复制到项目中。 兼容库很大程度上简化了桌面通知的复杂程度。 以下指令需用到兼容库。

如果使用预编译标头，确保 `#include "stdafx.h"`，并将其用作 DesktopNotificationManagerCompat.cpp 文件的第一行。


## <a name="step-3-include-the-header-files-and-namespaces"></a>步骤 3：包括头文件和命名空间

包括兼容库头文件，以及与使用 UWP toast API 相关的头文件和命名空间。

```cpp
#include "DesktopNotificationManagerCompat.h"
#include "NotificationActivationCallback.h"
#include <windows.ui.notifications.h>

using namespace ABI::Windows::Data::Xml::Dom;
using namespace ABI::Windows::UI::Notifications;
using namespace Microsoft::WRL;
```


## <a name="step-4-implement-the-activator"></a>步骤 4：实现激活器

必须实现一个处理程序，用于 toast 激活，以便在用户单击 toast 时，应用可以执行某些操作。 这就要求将 toast 保留在操作中心内（因为 toast 可能会在应用关闭几天后被单击）。 可将此类置于项目中的任何位置。

如下所示，实现 **INotificationActivationCallback** 接口（包括 UUID），并调用 **CoCreatableClass** ，用于将类标记为“可创建 COM”。 对于 UUID，可从众多联机 GUID 生成器中选择一个来创建唯一的 GUID。 操作中心通过此 GUID CLSID（类标识符）了解要对哪个类实施 COM 激活。

```cpp
// The UUID CLSID must be unique to your app. Create a new GUID if copying this code.
class DECLSPEC_UUID("replaced-with-your-guid-C173E6ADF0C3") NotificationActivator WrlSealed WrlFinal
    : public RuntimeClass<RuntimeClassFlags<ClassicCom>, INotificationActivationCallback>
{
public:
    virtual HRESULT STDMETHODCALLTYPE Activate(
        _In_ LPCWSTR appUserModelId,
        _In_ LPCWSTR invokedArgs,
        _In_reads_(dataCount) const NOTIFICATION_USER_INPUT_DATA* data,
        ULONG dataCount) override
    {
        // TODO: Handle activation
    }
};

// Flag class as COM creatable
CoCreatableClass(NotificationActivator);
```


## <a name="step-5-register-with-notification-platform"></a>步骤 5：注册通知平台

然后，必须注册通知平台。 根据所使用的是桌面桥还是经典 Win32，步骤会有所不同。 如果两者都支持，用于这两者的步骤均需执行（但无需对代码实施分支操作，库会自行执行此操作！）。


### <a name="desktop-bridge"></a>桌面桥

如果使用桌面桥（或如果同时支持两者），请在 **Package.appxmanifest** 中添加：

1. **xmlns:com** 声明
2. **xmlns:desktop** 声明
3. 在 **IgnorableNamespaces** 属性中，添加 **com** 和 **desktop**
4. 使用步骤 4 中 GUID 的 COM 激活器的 **com:Extension**。 务必包括 `Arguments="-ToastActivated"`，以便了解是从 toast 启动
5. **windows.toastNotificationActivation** 的 **desktop:Extension**，用于声明 toast 激活器 CLSID（步骤 #4 中的 GUID）。

**Package.appxmanifest**

```xml
<Package
  ...
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="... com desktop">
  ...
  <Applications>
    <Application>
      ...
      <Extensions>

        <!--Register COM CLSID LocalServer32 registry key-->
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="YourProject\YourProject.exe" Arguments="-ToastActivated" DisplayName="Toast activator">
              <com:Class Id="replaced-with-your-guid-C173E6ADF0C3" DisplayName="Toast activator"/>
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>

        <!--Specify which CLSID to activate when toast clicked-->
        <desktop:Extension Category="windows.toastNotificationActivation">
          <desktop:ToastNotificationActivation ToastActivatorCLSID="replaced-with-your-guid-C173E6ADF0C3" /> 
        </desktop:Extension>

      </Extensions>
    </Application>
  </Applications>
 </Package>
```


### <a name="classic-win32"></a>经典 Win32

如果使用经典 Win32（或如果同时支持两者），必须在“开始“中的应用的快捷方式中声明应用程序用户模型 ID (AUMID) 和 toast 激活器 CLSID（步骤 #4 中的 GUID）。

选取用于识别 Win32 应用的唯一 AUMID。 通常采用 [CompanyName].[AppName] 的形式，但需确保它在所有应用中均为唯一（可根据需要在末尾添加一些数字）。

#### <a name="step-51-wix-installer"></a>步骤 5.1：WiX 安装程序

如果对安装程序使用 WiX，则编辑 **Product.wxs** 文件，将两种快捷方式属性添加到“开始”菜单快捷方式中，如下所示。 确保步骤 #4 中的 GUID 包含在 `{}` 中，如下所示。

**Product.wxs**

```xml
<Shortcut Id="ApplicationStartMenuShortcut" Name="Wix Sample" Description="Wix Sample" Target="[INSTALLFOLDER]WixSample.exe" WorkingDirectory="INSTALLFOLDER">
                    
    <!--AUMID-->
    <ShortcutProperty Key="System.AppUserModel.ID" Value="YourCompany.YourApp"/>
    
    <!--COM CLSID-->
    <ShortcutProperty Key="System.AppUserModel.ToastActivatorCLSID" Value="{replaced-with-your-guid-C173E6ADF0C3}"/>
    
</Shortcut>
```

> [!IMPORTANT]
> 为使用通知，必须在正常调试之前通过安装程序安装应用，以便显示包含有 AUMID 和 CLSID 的“开始”快捷方式。 出现“开始”快捷方式后，可以从 Visual Studio 中使用 F5 进行调试。


#### <a name="step-52-register-aumid-and-com-server"></a>步骤 5.2：注册 AUMID 和 COM 服务器

然后，在不考虑安装程序的情况下，在应用的启动代码中（在调用任何通知 API 之前），调用 **RegisterAumidAndComServer** 方法，指定步骤 #4 中的通知激活器类和上面使用的 AUMID。

```cpp
// Register AUMID and COM server (for Desktop Bridge apps, this no-ops)
hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"YourCompany.YourApp", __uuidof(NotificationActivator));
```

如果同时支持桌面桥和经典 Win32，任何情况下均可调用此方法。 如果在桌面桥下运行，此方法会立即返回。 无需对代码实施分支操作。

使用此方法可调用兼容 API 来发送和管理通知，而无需总是提供 AUMID。 并且它会插入 COM 服务器的 LocalServer32 注册表项。


## <a name="step-6-register-com-activator"></a>步骤 6：注册 COM 激活器

对于桌面桥和经典 Win32 应用，均须注册通知激活器类型，以便能够处理 toast 激活。

在应用的启动代码中，调用以下 **RegisterActivator** 方法。 必须调用此方法才能够接收任何 toast 激活。

```cpp
// Register activator type
hr = DesktopNotificationManagerCompat::RegisterActivator();
```


## <a name="step-7-send-a-notification"></a>步骤 7：发送通知

发送通知与在 UWP 应用中的操作几乎相同，不同之处在于需使用 **DesktopNotificationManagerCompat** 创建 **ToastNotifier**。 兼容库会自动根据桌面桥与经典 Win32 之间的区别进行相应处理，因此无需对代码实施分支操作。 对于经典 Win32，兼容库会缓存调用 **RegisterAumidAndComServer** 时提供的 AUMID，因此无需担心何时提供或不提供 AUMID 的问题。

确保按以下所示使用 **ToastGeneric** 绑定，因为旧版 Windows 8.1 toast 通知模板不会激活在步骤 4 中创建的 COM 通知激活器。

> [!IMPORTANT]
> 仅清单中具有 Internet 功能的桌面桥应用才支持 http 图像。 经典 Win32 应用不支持 http 图像；必须将图像下载到本地应用数据中并在本地进行引用。

```cpp
// Construct XML
ComPtr<IXmlDocument> doc;
hr = DesktopNotificationManagerCompat::CreateXmlDocumentFromString(
    L"<toast><visual><binding template='ToastGeneric'><text>Hello world</text></binding></visual></toast>",
    &doc);
if (SUCCEEDED(hr))
{
    // See full code sample to learn how to inject dynamic text, buttons, and more

    // Create the notifier
    // Classic Win32 apps MUST use the compat method to create the notifier
    ComPtr<IToastNotifier> notifier;
    hr = DesktopNotificationManagerCompat::CreateToastNotifier(&notifier);
    if (SUCCEEDED(hr))
    {
        // Create the notification itself (using helper method from compat library)
        ComPtr<IToastNotification> toast;
        hr = DesktopNotificationManagerCompat::CreateToastNotification(doc, &toast);
        if (SUCCEEDED(hr))
        {
            // And show it!
            hr = notifier->Show(toast.Get());
        }
    }
}
```

> [!IMPORTANT]
> 经典 Win32 应用无法使用旧版 Toast 模板（例如 ToastText02）。 当指定 COM CLSID 时，激活旧版模板将失败。 必须使用 Windows 10 ToastGeneric 模板，如上所示。


## <a name="step-8-handling-activation"></a>步骤 8：处理激活

用户单击 toast 或 toast 中的按钮时，将调用 **NotificationActivator** 类的 **Activate** 方法。

在 Activate 方法内，可以解析在 toast 中指定的参数，并获取用户键入或选择的用户输入，然后相应地激活应用。

> [!NOTE]
> 在独立于主线程的线程上调用 **Activate** 方法。

```cpp
// The GUID must be unique to your app. Create a new GUID if copying this code.
class DECLSPEC_UUID("replaced-with-your-guid-C173E6ADF0C3") NotificationActivator WrlSealed WrlFinal
    : public RuntimeClass<RuntimeClassFlags<ClassicCom>, INotificationActivationCallback>
{
public: 
    virtual HRESULT STDMETHODCALLTYPE Activate(
        _In_ LPCWSTR appUserModelId,
        _In_ LPCWSTR invokedArgs,
        _In_reads_(dataCount) const NOTIFICATION_USER_INPUT_DATA* data,
        ULONG dataCount) override
    {
        std::wstring arguments(invokedArgs);
        HRESULT hr = S_OK;

        // Background: Quick reply to the conversation
        if (arguments.find(L"action=reply") == 0)
        {
            // Get the response user typed.
            // We know this is first and only user input since our toasts only have one input
            LPCWSTR response = data[0].Value;

            hr = DesktopToastsApp::SendResponse(response);
        }

        else
        {
            // The remaining scenarios are foreground activations,
            // so we first make sure we have a window open and in foreground
            hr = DesktopToastsApp::GetInstance()->OpenWindowIfNeeded();
            if (SUCCEEDED(hr))
            {
                // Open the image
                if (arguments.find(L"action=viewImage") == 0)
                {
                    hr = DesktopToastsApp::GetInstance()->OpenImage();
                }

                // Open the app itself
                // User might have clicked on app title in Action Center which launches with empty args
                else
                {
                    // Nothing to do, already launched
                }
            }
        }

        if (FAILED(hr))
        {
            // Log failed HRESULT
        }

        return S_OK;
    }

    ~NotificationActivator()
    {
        // If we don't have window open
        if (!DesktopToastsApp::GetInstance()->HasWindow())
        {
            // Exit (this is for background activation scenarios)
            exit(0);
        }
    }
};

// Flag class as COM creatable
CoCreatableClass(NotificationActivator);
```

为了在应用关闭时正确支持启动，请在 WinMain 函数中确定是否从 toast 启动。 如果从 toast 启动，将存在一个“-ToastActivated”的启动参数。 如果出现此参数，应停止执行任何常规启动激活代码，并根据需要允许 **NotificationActivator** 处理窗口启动。

```cpp
// Main function
int WINAPI wWinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE, _In_ LPWSTR cmdLineArgs, _In_ int)
{
    RoInitializeWrapper winRtInitializer(RO_INIT_MULTITHREADED);

    HRESULT hr = winRtInitializer;
    if (SUCCEEDED(hr))
    {
        // Register AUMID and COM server (for Desktop Bridge apps, this no-ops)
        hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"WindowsNotifications.DesktopToastsCpp", __uuidof(NotificationActivator));
        if (SUCCEEDED(hr))
        {
            // Register activator type
            hr = DesktopNotificationManagerCompat::RegisterActivator();
            if (SUCCEEDED(hr))
            {
                DesktopToastsApp app;
                app.SetHInstance(hInstance);

                std::wstring cmdLineArgsStr(cmdLineArgs);

                // If launched from toast
                if (cmdLineArgsStr.find(TOAST_ACTIVATED_LAUNCH_ARG) != std::string::npos)
                {
                    // Let our NotificationActivator handle activation
                }

                else
                {
                    // Otherwise launch like normal
                    app.Initialize(hInstance);
                }

                app.RunMessageLoop();
            }
        }
    }

    return SUCCEEDED(hr);
}
```


### <a name="activation-sequence-of-events"></a>事件的激活顺序

激活顺序如下…

如果应用已在运行：

1. 调用 **NotificationActivator** 中的 **Activate** 

如果应用未运行：

1. 应用已实施 EXE 启动，会得到一个“-ToastActivated”的命令行参数
2. 调用 **NotificationActivator** 中的 **Activate** 


### <a name="foreground-vs-background-activation"></a>前台与后台激活
对于桌面应用，前台和后台激活的处理方式相同 - 即调用 COM 激活器。 是由应用的代码来决定是显示一个窗口，还是只执行一些操作后退出。 因此，在 toast 内容中指定**后台**的 **activationType** 不会改变相应行为。


## <a name="step-9-remove-and-manage-notifications"></a>步骤 9：删除和管理通知

删除和管理通知与 UWP 应用中的操作相同。 但是，建议使用兼容库来获取 **DesktopNotificationHistoryCompat**，这样如果使用经典 Win32，便无需担心提供 AUMID 的问题。

```cpp
std::unique_ptr<DesktopNotificationHistoryCompat> history;
auto hr = DesktopNotificationManagerCompat::get_History(&history);
if (SUCCEEDED(hr))
{
    // Remove a specific toast
    hr = history->Remove(L"Message2");

    // Clear all toasts
    hr = history->Clear();
}
```


## <a name="step-10-deploying-and-debugging"></a>步骤 10：部署和调试

若要部署和调试桌面桥应用，请参阅[运行、调试和测试打包的桌面应用](/porting/desktop-to-uwp-debug.md)。

若要部署和调试经典 Win32 应用，必须在正常调试之前通过安装程序安装应用，以便显示包含有 AUMID 和 CLSID 的“开始”快捷方式。 出现“开始”快捷方式后，可以从 Visual Studio 中使用 F5 进行调试。

如果通知根本无法在经典 Win32 应用中显示（且未引发任何异常），则可能意味着“开始”快捷方式不存在（通过安装程序安装应用），或者代码中使用的 AUMID 与“开始”快捷方式中的 AUMID 不匹配。

如果会出现通知但通知未保留在操作中心中（在弹出窗口关闭后消失），这意味着未正确实现 COM 激活器。

如果同时安装了桌面桥和经典 Win32 应用，应注意，桌面桥应用会取代经典 Win32 应用来处理 toast 激活。 这意味着，单击经典 Win32 应用中的 toast 后，仍会启动桌面桥应用。 卸载桌面桥应用后会还原为由经典 Win32 应用来处理激活。

如果收到 `HRESULT 0x800401f0 CoInitialize has not been called.`，请务必先在应用中调用 `CoInitialize(nullptr)`，然后再调用 API。

如果在调用兼容 API 时收到 `HRESULT 0x8000000e A method was called at an unexpected time.`，这很可能意味着调用所需的 Register 方法失败（或如果使用的是一个桌面桥应用，则意味着当前未在桌面桥上下文中运行应用）。

如果收到大量 `unresolved external symbol` 编译错误，则很可能是因为忘记在步骤 1 中将 `runtimeobject.lib` 添加到**其他依赖项**中（或者只将其添加到“调试”配置而未添加到“发布”配置）。


## <a name="handling-older-versions-of-windows"></a>处理旧版 Windows

如果支持 Windows 8.1 或更低版本，则在调用任何 **DesktopNotificationManagerCompat** API 或发送任何 ToastGeneric toast 前，需要在运行时检查是否是在 Windows 10 中运行。

Windows 8 引入了 toast 通知，但使用的是[旧版 toast 模板](https://docs.microsoft.com/en-us/previous-versions/windows/apps/hh761494(v=win.10))，如 ToastText01。 激活由 **ToastNotification** 类上的内存中 **Activated** 事件处理，因为 toast 只会短暂弹出而不会持久存在。 Windows 10 引入了[交互式 ToastGeneric toast](adaptive-interactive-toasts.md)，还引入了操作中心，通知可在操作中心内保留数天。 要引入操作中心，则需要引入 COM 激活器，这样 toast 才能在创建数天后被激活。

| 操作系统 | ToastGeneric | COM 激活器 | 旧版 toast 模板 |
| -- | ------------ | ------------- | ---------------------- |
| Windows10 | 支持 | 支持 | 支持（但不会激活 COM 服务器） |
| Windows 8.1/8 | 不适用 | 不适用 | 支持 |
| Windows 7 及更低版本 | 不适用 | 不适用 | 不适用 |

若要检查是否是在 Windows 10 上运行，请包含 `<VersionHelpers.h>` 标头并检查 **IsWindows10OrGreater** 方法。 如果返回 true，则继续调用本文档中所述的所有方法！ 

```cpp
#include <VersionHelpers.h>

if (IsWindows10OrGreater())
{
    // Running Windows 10, continue with sending Windows 10 toasts!
}
```


## <a name="known-issues"></a>已知问题

**已修复：单击 toast 后焦点未移动至应用**：在内部版本 15063 和更早版本中，激活 COM 服务器时，前台权限未转移给应用程序。 因此，在尝试将应用移动到前台时，它只会闪现。 此前此问题没有任何解决方法。 我们在内部版本 16299 和更高版本中修复了此问题。


## <a name="resources"></a>资源

* [GitHub 上的完整代码示例](https://github.com/WindowsNotifications/desktop-toasts)
* [来自桌面应用的 Toast 通知](toast-desktop-apps.md)
* [Toast 内容文档](adaptive-interactive-toasts.md)