---
Description: '了解 Win32 c # 应用程序如何可以发送本地 toast 通知，并处理用户单击 toast 的操作。'
title: 从桌面 C# 应用发送本地 toast 通知
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from desktop C# apps
template: detail.hbs
ms.date: 01/23/2018
ms.topic: article
keywords: 'windows 10，uwp，win32，桌面，toast 通知，发送 toast，发送本地 toast，桌面桥，.msix，稀疏包，c #，c 清晰，toast 通知，wpf'
ms.localizationpriority: medium
ms.openlocfilehash: 1d8332745b44bc688fbf2ca7cf3b42cf7300d579
ms.sourcegitcommit: 179f8098d10e338ad34fa84934f1654ec58161cd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85717641"
---
# <a name="send-a-local-toast-notification-from-desktop-c-apps"></a>从桌面 C# 应用发送本地 toast 通知

桌面应用（包括打包的[.msix](https://docs.microsoft.com/windows/msix/desktop/source-code-overview)应用、使用[稀疏包](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)获取包标识和经典非打包 Win32 应用的应用）可以像 Windows 应用一样发送交互式 toast 通知。 但对于桌面应用程序，有几个特殊步骤，因为不同的激活方案，如果你未使用 .MSIX 或稀疏包，则可能缺少包标识。

> [!IMPORTANT]
> 如果要编写 UWP 应用，请参阅 [UWP 文档](send-local-toast.md)。 有关其他桌面语言，请参阅[桌面 C++ WRL](send-local-toast-desktop-cpp-wrl.md)。


## <a name="step-1-install-the-notifications-library"></a>步骤1：安装通知库

`Microsoft.Toolkit.Uwp.Notifications`在项目中安装[NuGet 包](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)。

此[通知库](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)添加了兼容的库代码，用于从桌面应用程序中使用 toast 通知。 它还引用 UWP Sdk，允许使用 c # 而不是原始 XML 来构造通知。 此快速入门的其余部分取决于通知库。


## <a name="step-2-implement-the-activator"></a>步骤2：实现激活器

必须实现用于 toast 激活的处理程序，以便在用户单击 toast 时，应用程序可以执行一些操作。 这就要求将 toast 保留在操作中心内（因为 toast 可能会在应用关闭几天后被单击）。 可将此类置于项目中的任何位置。

创建新的**MyNotificationActivator**类并扩展**NotificationActivator**类。 添加下面列出的三个属性，并使用多个联机 GUID 生成器之一创建应用的唯一 GUID CLSID。 操作中心通过此 CLSID（类标识符）了解要对哪个类实施 COM 激活。

**MyNotificationActivator.cs** （创建此文件）

```csharp
// The GUID CLSID must be unique to your app. Create a new GUID if copying this code.
[ClassInterface(ClassInterfaceType.None)]
[ComSourceInterfaces(typeof(INotificationActivationCallback))]
[Guid("replaced-with-your-guid-C173E6ADF0C3"), ComVisible(true)]
public class MyNotificationActivator : NotificationActivator
{
    public override void OnActivated(string invokedArgs, NotificationUserInput userInput, string appUserModelId)
    {
        // TODO: Handle activation
    }
}
```


## <a name="step-3-register-with-notification-platform"></a>步骤3：注册到通知平台

然后，必须注册通知平台。 具体步骤取决于你使用的是 .MSIX/稀疏包还是经典 Win32。 如果两者都支持，用于这两者的步骤均需执行（但无需对代码实施分支操作，库会自行执行此操作！）。


### <a name="msixsparse-packages"></a>.MSIX/稀疏包

如果你使用的是[.msix](https://docs.microsoft.com/windows/msix/desktop/source-code-overview)或[稀疏包](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)（或同时支持两者），请在**appxmanifest.xml**中添加：

1. **xmlns:com** 声明
2. **xmlns:desktop** 声明
3. 在 **IgnorableNamespaces** 属性中，添加 **com** 和 **desktop**
4. 使用步骤 4 中 GUID 的 COM 激活器的 **com:Extension**。 务必包括 `Arguments="-ToastActivated"`，以便了解是从 toast 启动
5. **desktop：** **toastNotificationActivation**的扩展，用于声明 toast 激活器 CLSID （步骤 #3 中的 GUID）。

**Appxmanifest.xml**

```xml
<!--Add these namespaces-->
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

如果使用的是经典 Win32 （或同时支持这两者），则必须在开始时在应用的快捷方式中声明应用程序用户模型 ID （AUMID）和 toast 激活器 CLSID （步骤 #3 中的 GUID）。

选取用于识别 Win32 应用的唯一 AUMID。 通常采用 [CompanyName].[AppName] 的形式，但需确保它在所有应用中均为唯一（可根据需要在末尾添加一些数字）。

#### <a name="step-31-wix-installer"></a>步骤3.1： WiX 安装程序

如果对安装程序使用 WiX，则编辑 **Product.wxs** 文件，将两种快捷方式属性添加到“开始”菜单快捷方式中，如下所示。 请确保步骤 #3 的 GUID 包括在中， `{}` 如下所示。

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


#### <a name="step-32-register-aumid-and-com-server"></a>步骤3.2：注册 AUMID 和 COM 服务器

然后，无论你的安装程序在你的应用程序的启动代码中（在调用任何通知 Api 之前），请调用**RegisterAumidAndComServer**方法，并指定你的 notification activator 类（从步骤 #3 和你上面使用的 AUMID）。

```csharp
// Register AUMID and COM server (for MSIX/sparse package apps, this no-ops)
DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
```

如果同时支持 .MSIX/稀疏包和经典 Win32，则无需调用此方法。 如果在 .MSIX/稀疏包中运行，则此方法将立即返回。 无需对代码实施分支操作。

使用此方法可调用兼容 API 来发送和管理通知，而无需总是提供 AUMID。 并且它会插入 COM 服务器的 LocalServer32 注册表项。


## <a name="step-4-register-com-activator"></a>步骤4：注册 COM 激活器

对于 .MSIX/稀疏包和经典 Win32 应用，必须注册 notification activator 类型，以便可以处理 toast 激活。

在应用程序的启动代码中，调用以下**RegisterActivator**方法，并传入在步骤 #3 中创建的**NotificationActivator**类的实现。 必须调用此方法才能够接收任何 toast 激活。

```csharp
// Register COM server and activator type
DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();
```


## <a name="step-5-send-a-notification"></a>步骤5：发送通知

发送通知与在 UWP 应用中的操作几乎相同，不同之处在于需使用 **DesktopNotificationManagerCompat** 类创建 **ToastNotifier**。 兼容库自动处理 .MSIX/稀疏包和经典 Win32 之间的差异，因此你无需分叉你的代码。 对于经典 Win32，兼容库会缓存调用 **RegisterAumidAndComServer** 时提供的 AUMID，因此无需担心何时提供或不提供 AUMID 的问题。

> [!NOTE]
> 安装[通知库](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，以便能够按以下方式使用 C# 而非原始 XML 来构造通知。

请确保使用下面所示的**ToastContent** （或 ToastGeneric 模板（如果你要手工编写 XML），因为旧 Windows 8.1 toast 通知模板不会激活你在步骤 #3 中创建的 COM 通知激活器。

> [!IMPORTANT]
> Http 映像仅在其清单中具有 internet 功能的 .MSIX/稀疏包应用中受支持。 经典 Win32 应用不支持 http 图像；必须将图像下载到本地应用数据中并在本地进行引用。

```csharp
// Construct the visuals of the toast (using Notifications library)
ToastContent toastContent = new ToastContentBuilder()
    .AddToastActivationInfo("action=viewConversation&conversationId=5", ToastActivationType.Foreground)
    .AddText("Hello world!")
    .GetToastContent();

// And create the toast notification
var toast = new ToastNotification(toastContent.GetXml());

// And then show it
DesktopNotificationManagerCompat.CreateToastNotifier().Show(toast);
```

> [!IMPORTANT]
> 经典 Win32 应用无法使用旧版 Toast 模板（例如 ToastText02）。 当指定 COM CLSID 时，激活旧版模板将失败。 必须使用 Windows 10 ToastGeneric 模板，如上所示。


## <a name="step-6-handling-activation"></a>步骤6：处理激活

用户单击 toast 时，将调用 **NotificationActivator** 类的 **OnActivated** 方法。

在 OnActivated 方法内，可以解析在 toast 中指定的参数，并获取用户键入或选择的用户输入，然后相应激活应用。

> [!NOTE]
> 不在 UI 线程上调用 **OnActivated** 方法。 如果想要执行 UI 线程操作，必须调用 `Application.Current.Dispatcher.Invoke(callback)`。

```csharp
// The GUID must be unique to your app. Create a new GUID if copying this code.
[ClassInterface(ClassInterfaceType.None)]
[ComSourceInterfaces(typeof(INotificationActivationCallback))]
[Guid("replaced-with-your-guid-C173E6ADF0C3"), ComVisible(true)]
public class MyNotificationActivator : NotificationActivator
{
    public override void OnActivated(string invokedArgs, NotificationUserInput userInput, string appUserModelId)
    {
        Application.Current.Dispatcher.Invoke(delegate
        {
            // Tapping on the top-level header launches with empty args
            if (arguments.Length == 0)
            {
                // Perform a normal launch
                OpenWindowIfNeeded();
                return;
            }

            // Parse the query string (using NuGet package QueryString.NET)
            QueryString args = QueryString.Parse(invokedArgs);

            // See what action is being requested 
            switch (args["action"])
            {
                // Open the image
                case "viewImage":

                    // The URL retrieved from the toast args
                    string imageUrl = args["imageUrl"];

                    // Make sure we have a window open and in foreground
                    OpenWindowIfNeeded();

                    // And then show the image
                    (App.Current.Windows[0] as MainWindow).ShowImage(imageUrl);

                    break;

                // Background: Quick reply to the conversation
                case "reply":

                    // Get the response the user typed
                    string msg = userInput["tbReply"];

                    // And send this message
                    SendMessage(msg);

                    // If there's no windows open, exit the app
                    if (App.Current.Windows.Count == 0)
                    {
                        Application.Current.Shutdown();
                    }

                    break;
            }
        });
    }

    private void OpenWindowIfNeeded()
    {
        // Make sure we have a window open (in case user clicked toast while app closed)
        if (App.Current.Windows.Count == 0)
        {
            new MainWindow().Show();
        }

        // Activate the window, bringing it to focus
        App.Current.Windows[0].Activate();

        // And make sure to maximize the window too, in case it was currently minimized
        App.Current.Windows[0].WindowState = WindowState.Normal;
    }
}
```

为了在应用关闭时正确支持启动，请在 `App.xaml.cs` 文件中替代 **OnStartup** 方法（针对 WPF 应用），以确定是否是从 toast 启动。 如果从 toast 启动，将存在一个“-ToastActivated”的启动参数。 如果看到此参数，应停止执行任何常规启动激活代码，并允许 **OnActivated** 代码处理启动。

```csharp
protected override async void OnStartup(StartupEventArgs e)
{
    // Register AUMID, COM server, and activator
    DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
    DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();

    // If launched from a toast
    if (e.Args.Contains("-ToastActivated"))
    {
        // Our NotificationActivator code will run after this completes,
        // and will show a window if necessary.
    }

    else
    {
        // Show the window
        // In App.xaml, be sure to remove the StartupUri so that a window doesn't
        // get created by default, since we're creating windows ourselves (and sometimes we
        // don't want to create a window if handling a background activation).
        new MainWindow().Show();
    }

    base.OnStartup(e);
}
```


### <a name="activation-sequence-of-events"></a>事件的激活顺序

对于 WPF，激活顺序如下…

如果应用已在运行：

1. 调用 **NotificationActivator** 中的 **OnActivated**

如果应用未运行：

1. 使用“-ToastActivated”的**参数**调用 `App.xaml.cs` 中的 **OnStartup**
2. 调用 **NotificationActivator** 中的 **OnActivated**


### <a name="foreground-vs-background-activation"></a>前台与后台激活
对于桌面应用，前台和后台激活的处理方式相同 - 即调用 COM 激活器。 是由应用的代码来决定是显示一个窗口，还是只执行一些操作后退出。 因此，在 toast 内容中指定**背景** **ActivationType**不会更改行为。


## <a name="step-7-remove-and-manage-notifications"></a>步骤7：删除和管理通知

删除和管理通知与 UWP 应用中的操作相同。 但是，建议使用兼容库来获取 **DesktopNotificationHistoryCompat**，这样如果使用经典 Win32，便无需担心提供 AUMID 的问题。

```csharp
// Remove the toast with tag "Message2"
DesktopNotificationManagerCompat.History.Remove("Message2");

// Clear all toasts
DesktopNotificationManagerCompat.History.Clear();
```


## <a name="step-8-deploying-and-debugging"></a>步骤8：部署和调试

若要部署和调试 .MSIX 应用，请参阅[运行、调试和测试打包的桌面应用](/windows/uwp/porting/desktop-to-uwp-debug)。

若要部署和调试经典 Win32 应用，必须在正常调试之前通过安装程序安装应用，以便显示包含有 AUMID 和 CLSID 的“开始”快捷方式。 出现“开始”快捷方式后，可以从 Visual Studio 中使用 F5 进行调试。

如果通知根本无法在经典 Win32 应用中显示（且未引发任何异常），则可能意味着“开始”快捷方式不存在（通过安装程序安装应用），或者代码中使用的 AUMID 与“开始”快捷方式中的 AUMID 不匹配。

如果会出现通知但通知未保留在操作中心中（在弹出窗口关闭后消失），这意味着未正确实现 COM 激活器。

如果已安装 .MSIX/稀疏包和经典 Win32 应用，请注意，.MSIX/稀疏包应用在处理 toast 激活时将取代经典 Win32 应用。 这意味着，在单击经典 Win32 应用程序中的 toast 时，它仍将启动 .MSIX/稀疏包应用。 卸载 .MSIX/稀疏包应用会将激活恢复回经典 Win32 应用。


## <a name="known-issues"></a>已知问题

**已修复：单击 toast 后焦点未移动至应用**：在内部版本 15063 和更早版本中，激活 COM 服务器时，前台权限未转移给应用程序。 因此，在尝试将应用移动到前台时，它只会闪现。 此前此问题没有任何解决方法。 我们在内部版本 16299 和更高版本中修复了此问题。


## <a name="resources"></a>资源

* [GitHub 上的完整代码示例](https://github.com/WindowsNotifications/desktop-toasts)
* [来自桌面应用的 Toast 通知](toast-desktop-apps.md)
* [toast 内容文档](adaptive-interactive-toasts.md)

