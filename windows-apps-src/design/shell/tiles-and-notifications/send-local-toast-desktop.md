---
author: andrewleader
Description: Learn how Win32 C# apps can send local toast notifications and handle the user clicking the toast.
title: 从桌面 C# 应用发送本地 toast 通知
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from desktop C# apps
template: detail.hbs
ms.author: mijacobs
ms.date: 01/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, win32, 桌面, toast 通知, 发送 toast, 发送本地 toast, 桌面桥, C#, c sharp
ms.localizationpriority: medium
ms.openlocfilehash: 3bda3e85fd89ef7a8b819fcd809acea4fd9a276b
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2018
ms.locfileid: "4131164"
---
# <a name="send-a-local-toast-notification-from-desktop-c-apps"></a>从桌面 C# 应用发送本地 toast 通知

桌面应用（包括桌面桥和经典 Win32）可以像通用 Windows 平台 (UWP) 应用一样发送交互式 toast 通知。 但是，由于激活方案不同，桌面应用存在一些特殊步骤，并且若不使用桌面桥，可能会缺少程序包标识符。

> [!IMPORTANT]
> 如果要编写 UWP 应用，请参阅 [UWP 文档](send-local-toast.md)。 有关其他桌面语言，请参阅[桌面 C++ WRL](send-local-toast-desktop-cpp-wrl.md)。


## <a name="step-1-enable-the-windows-10-sdk"></a>步骤 1：启用 Windows 10 SDK

如果尚未对 Win32 应用启用 Windows 10 SDK，必须先执行该操作。

右键单击项目，选择**卸载项目**。

![正在卸载项目](images/win32-unload-project.png)

然后再次右键单击该项目，选择**编辑 [projectname].csproj**

![编辑项目](images/win32-edit-project.png)

在现有 `<TargetFrameworkVersion>` 节点下，添加新的 `<TargetPlatformVersion>` 节点，指定所支持的最低版本的 Windows 10。 实际使用的 SDK 是开发计算机上安装的最新 SDK。 这会指定所允许的最低版本（可引用 Windows SDK）。

```xml
...
<TargetFrameworkVersion>...</TargetFrameworkVersion>
<TargetPlatformVersion>10.0.10240.0</TargetPlatformVersion>
...
```

保存更改，然后重新加载项目。

![重新加载项目](images/win32-reload-project.png)


## <a name="step-2-reference-the-apis"></a>步骤 2：参考 API

打开参考管理器（右键单击项目，选择**添加 -> 参考**），选择 **Windows-> 核心**并包含以下参考：

* Windows.Data
* Windows.UI

![参考管理器](images/win32-add-windows-reference.png)


## <a name="step-3-copy-compat-library-code"></a>步骤 3：复制兼容库代码

将 [GitHub 中的 DesktopNotificationManagerCompat.cs 文件](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CS/DesktopToastsApp/DesktopNotificationManagerCompat.cs)复制到项目中。 兼容库很大程度上简化了桌面通知的复杂程度。 以下指令需用到兼容库。


## <a name="step-4-implement-the-activator"></a>步骤 4：实现激活器

你必须实现 toast 激活的处理程序，以便用户单击 toast 时，你的应用可以执行某些操作。 这就要求将 toast 保留在操作中心内（因为 toast 可能会在应用关闭几天后被单击）。 可将此类置于项目中的任何位置。

扩展 **NotificationActivator** 类，然后添加下面列出的三个属性，并使用联机 GUID 生成器（有多种可选）为应用创建一个唯一的 GUID CLSID。 操作中心通过此 CLSID（类标识符）了解要对哪个类实施 COM 激活。

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

```csharp
// Register AUMID and COM server (for Desktop Bridge apps, this no-ops)
DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
```

如果同时支持桌面桥和经典 Win32，任何情况下均可调用此方法。 如果在桌面桥下运行，此方法会立即返回。 无需对代码实施分支操作。

使用此方法可调用兼容 API 来发送和管理通知，而无需总是提供 AUMID。 并且它会插入 COM 服务器的 LocalServer32 注册表项。


## <a name="step-6-register-com-activator"></a>步骤 6：注册 COM 激活器

对于桌面桥和经典 Win32 应用，均须注册通知激活器类型，以便能够处理 toast 激活。

在应用的启动代码中，调用以下 **RegisterActivator** 方法，传入步骤 4 中创建的 **NotificationActivator** 类的实现。 必须调用此方法才能够接收任何 toast 激活。

```csharp
// Register COM server and activator type
DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();
```


## <a name="step-7-send-a-notification"></a>步骤 7：发送通知

发送通知与在 UWP 应用中的操作几乎相同，不同之处在于需使用 **DesktopNotificationManagerCompat** 类创建 **ToastNotifier**。 兼容库会自动根据桌面桥与经典 Win32 之间的区别进行相应处理，因此无需对代码实施分支操作。 对于经典 Win32，兼容库会缓存调用 **RegisterAumidAndComServer** 时提供的 AUMID，因此无需担心何时提供或不提供 AUMID 的问题。

> [!NOTE]
> 安装[通知库](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，以便能够按以下方式使用 C# 而非原始 XML 来构造通知。

确保使用如下所示的 **ToastContent**（或如果手动编写 XML，则使用 ToastGeneric 模板），因为旧版 Windows 8.1 toast 通知模板不会激活在步骤 4 中创建的 COM 通知激活器。

> [!IMPORTANT]
> 仅清单中具有 Internet 功能的桌面桥应用才支持 http 图像。 经典 Win32 应用不支持 http 图像；必须将图像下载到本地应用数据中并在本地进行引用。

```csharp
// Construct the visuals of the toast (using Notifications library)
ToastContent toastContent = new ToastContent()
{
    // Arguments when the user taps body of toast
    Launch = "action=viewConversation&conversationId=5",

    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Hello world!"
                }
            }
        }
    }
};

// Create the XML document (BE SURE TO REFERENCE WINDOWS.DATA.XML.DOM)
var doc = new XmlDocument();
doc.LoadXml(toastContent.GetContent());

// And create the toast notification
var toast = new ToastNotification(doc);

// And then show it
DesktopNotificationManagerCompat.CreateToastNotifier().Show(toast);
```

> [!IMPORTANT]
> 经典 Win32 应用无法使用旧版 Toast 模板（例如 ToastText02）。 当指定 COM CLSID 时，激活旧版模板将失败。 必须使用 Windows 10 ToastGeneric 模板，如上所示。


## <a name="step-8-handling-activation"></a>步骤 8：处理激活

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
对于桌面应用，前台和后台激活的处理方式相同 - 即调用 COM 激活器。 是由应用的代码来决定是显示一个窗口，还是只执行一些操作后退出。 因此，在 toast 内容中指定**后台**的 **ActivationType** 不会改变相应行为。


## <a name="step-9-remove-and-manage-notifications"></a>步骤 9：删除和管理通知

删除和管理通知与 UWP 应用中的操作相同。 但是，建议使用兼容库来获取 **DesktopNotificationHistoryCompat**，这样如果使用经典 Win32，便无需担心提供 AUMID 的问题。

```csharp
// Remove the toast with tag "Message2"
DesktopNotificationManagerCompat.History.Remove("Message2");

// Clear all toasts
DesktopNotificationManagerCompat.History.Clear();
```


## <a name="step-10-deploying-and-debugging"></a>步骤 10：部署和调试

若要部署和调试桌面桥应用，请参阅[运行、调试和测试打包的桌面应用](/porting/desktop-to-uwp-debug.md)。

若要部署和调试经典 Win32 应用，必须在正常调试之前通过安装程序安装应用，以便显示包含有 AUMID 和 CLSID 的“开始”快捷方式。 出现“开始”快捷方式后，可以从 Visual Studio 中使用 F5 进行调试。

如果通知根本无法在经典 Win32 应用中显示（且未引发任何异常），则可能意味着“开始”快捷方式不存在（通过安装程序安装应用），或者代码中使用的 AUMID 与“开始”快捷方式中的 AUMID 不匹配。

如果会出现通知但通知未保留在操作中心中（在弹出窗口关闭后消失），这意味着未正确实现 COM 激活器。

如果同时安装了桌面桥和经典 Win32 应用，应注意，桌面桥应用会取代经典 Win32 应用来处理 toast 激活。 这意味着，单击经典 Win32 应用中的 toast 后，仍会启动桌面桥应用。 卸载桌面桥应用后会还原为由经典 Win32 应用来处理激活。


## <a name="known-issues"></a>已知问题

**已修复：单击 toast 后焦点未移动至应用**：在内部版本 15063 和更早版本中，激活 COM 服务器时，前台权限未转移给应用程序。 因此，在尝试将应用移动到前台时，它只会闪现。 此前此问题没有任何解决方法。 我们在内部版本 16299 和更高版本中修复了此问题。


## <a name="resources"></a>资源

* [GitHub 上的完整代码示例](https://github.com/WindowsNotifications/desktop-toasts)
* [来自桌面应用的 Toast 通知](toast-desktop-apps.md)
* [Toast 内容文档](adaptive-interactive-toasts.md)

