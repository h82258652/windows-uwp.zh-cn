---
author: normesta
Description: "使用 Windows UI 和组件扩展桌面应用程序"
Search.Product: eADQiWindows 10XVcnh
title: "使用 Windows UI 和组件扩展桌面应用程序"
ms.author: normesta
ms.date: 07/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10，uwp"
ms.openlocfilehash: fce6076f07957b50e83cf80e8d12350630b99456
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2017
---
# <a name="extend-your-desktop-application-with-modern-uwp-components"></a>使用新式 UWP 组件扩展桌面应用程序

有些 Windows 10 体验（例如：启用触摸功能的 UI 页面）必须在新式应用容器内运行。 如果要添加这些体验，请用 UWP 组件扩展桌面应用程序。

在许多情况下，你可以直接从桌面应用程序中调用 UWP API，因此在查看本指南前，请参阅[面向 Windows 10 的增强](desktop-to-uwp-enhance.md)。

>[!NOTE]
>本指南假设你已使用桌面桥为桌面应用程序创建了 Windows 应用包。 如果你尚未完成此操作，请参阅[桌面桥](desktop-to-uwp-root.md)。

如果你已准备就绪，那我们开始吧。

## <a name="show-a-modern-xaml-ui"></a>显示新式 XAML UI

作为应用程序流的一部分，你可以将基于 XAML 的新式用户界面并入桌面应用程序。 这些用户界面可自然地适应不同屏幕尺寸和分辨率，并支持新式交互模型，如触摸和墨迹。

例如，借助少量 XAML 标记，即可为用户提供与地图相关的强大可视化功能。

此图像显示 VB6 应用程序，该应用程序可打开包含地图控件的、基于 XAML 的新式 UI。

![自适应设计](images\desktop-to-uwp\extend-xaml-ui.png)

### <a name="have-a-closer-look-at-this-app"></a>详细了解此应用

:heavy_check_mark: [观看视频](https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373/Demo-Add-a-XAML-UI-and-Toast-Notification-to-a-VB6-Application-OsJHC7WhD_8006218965)

:heavy_check_mark: [获取应用](https://www.microsoft.com/en-us/store/p/vb6-app-with-xaml-sample/9n191ncxf2f6)

:heavy_check_mark: [浏览代码](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/VB6withXaml)

### <a name="the-design-pattern"></a>设计模式

要显示基于 XAML 的 UI，请执行以下操作：

:一: [向你的解决方案中添加一个 UWP 项目](#project)

:二: [向该项目中添加协议扩展](#protocol)

:三: [从桌面应用中启动 UWP 应用](#start)

:四: [在 UWP 项目中，显示所需页面](#parse)

<span id="project" />
### <a name="add-a-uwp-project"></a>添加 UWP 项目

向你的解决方案中添加一个**空白应用（通用 Windows）**项目。

<span id="protocol" />
### <a name="add-a-protocol-extension"></a>添加协议扩展

在**解决方案资源管理器**中，打开项目的 **package.appxmanifest** 文件并添加扩展。

```xml
<Extensions>
      <uap:Extension
          Category="windows.protocol"
          Executable="MapUI.exe"
          EntryPoint=" MapUI.App">
        <uap:Protocol Name="desktopbridgemapsample" />
      </uap:Extension>
    </Extensions>     
```

为协议给定名称，提供 UWP 项目生成的可执行文件的名称以及入口点类的名称。

还可以在设计器中打开 **package.appxmanifest**，选择**声明**选项卡，然后在此处添加扩展名。

![“声明”选项卡](images\desktop-to-uwp\protocol-properties.png)



> [!NOTE]
> 地图控件可从 Internet 下载数据，因此如果你使用一个地图控件，则必须还要向清单中添加“Internet 客户端”功能。

<span id="start" />
### <a name="start-the-uwp-app"></a>启动 UWP 应用

首先，创建一个 [Uri](https://msdn.microsoft.com/library/system.uri.aspx)，其中包含协议名称和要传入 UWP 应用的任何参数。 然后，调用 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) 方法。

下面是 C# 基本示例。

```csharp

private async void showMap(double lat, double lon)
{
    string str = "desktopbridgemapsample://";

    Uri uri = new Uri(str + "location?lat=" +
        lat.ToString() + "&?lon=" + lon.ToString());

    var success = await Windows.System.Launcher.LaunchUriAsync(uri);

    if (success)
    {
        // URI launched
    }
    else
    {
        // URI launch failed
    }
}
```
在我们的示例中，我们将以更加间接的方式执行一些操作。 我们已将该调用包装在一个名为 ``LaunchMap`` 的 VB6 可调用互操作函数中。 该函数是使用 C++ 编写的。

以下是 VB 块：

```VB
Private Declare Function LaunchMap Lib "UWPWrappers.dll" _
  (ByVal lat As Double, ByVal lon As Double) As Boolean
 
Private Sub EiffelTower_Click()
    LaunchMap 48.858222, 2.2945
End Sub
```

以下是 C++ 函数：

```C++

DllExport bool __stdcall LaunchMap(double lat, double lon)
{
  try
  {
    String ^str = ref new String(L"desktopbridgemapsample://");
    Uri ^uri = ref new Uri(
      str + L"location?lat=" + lat.ToString() + L"&?lon=" + lon.ToString());
 
    // now launch the UWP component
    Launcher::LaunchUriAsync(uri);
  }
  catch (Exception^ ex) { return false; }
  return true;
}

```

<span id="parse" />
### <a name="parse-parameters-and-show-a-page"></a>分析参数并显示页面

在 UWP 项目的**应用**类中，覆盖 **OnActivated** 事件处理程序。 如果应用已通过协议激活，则分析参数并打开所需页面。

```C++
void App::OnActivated(Windows::ApplicationModel::Activation::IActivatedEventArgs^ e)
{
  if (e->Kind == ActivationKind::Protocol)
  {
    ProtocolActivatedEventArgs^ protocolArgs = (ProtocolActivatedEventArgs^)e;
    Uri ^uri = protocolArgs->Uri;
    if (uri->SchemeName == "desktopbridgemapsample")
    {
      Frame ^rootFrame = ref new Frame();
      Window::Current->Content = rootFrame;
      rootFrame->Navigate(TypeName(MainPage::typeid), uri->Query);
      Window::Current->Activate();
    }
  }
}
```

### <a name="similar-samples"></a>相似示例

[Northwind 示例：UWA UI & Win32 传统代码的端到端示例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/NorthwindSample)

[Northwind 示例：连接到 SQL Server 的 UWP 应用](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SQLServer)

## <a name="provide-services-to-other-apps"></a>向其他应用提供服务

添加其他应用可以使用的服务。 例如，你可以添加一个服务，以允许其他应用对你的应用之后的数据库进行受控访问。 通过实现后台任务，即使桌面应用未运行时，应用也可以访问该服务。

下面是执行此操作的示例。

![自适应设计](images\desktop-to-uwp\winforms-app-service.png)

### <a name="have-a-closer-look-at-this-app"></a>详细了解此应用

:heavy_check_mark: [观看视频](https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373/Demo-Expose-an-AppService-from-a-Windows-Forms-Data-Application-GiqNS7WhD_706218965)

:heavy_check_mark: [获取应用](https://www.microsoft.com/en-us/store/p/winforms-appservice/9p7d9b6nk5tn)

:heavy_check_mark: [浏览代码](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinformsAppService)

### <a name="the-design-pattern"></a>设计模式

要显示提供服务，请执行以下操作：

:一: [添加 Windows 运行时组件](#component)

:二: [添加应用服务扩展](#extension)

:三: [实现应用服务](#appservice)

:四: [测试应用服务](#test)

<span id="component" />

### <a name="add-a-windows-runtime-component"></a>添加 Windows 运行时组件

向你的解决方案中添加 **Windows 运行时组件（通用 Windows）**项目。

然后，从你的 UWP 打包项目中引用该运行时组件的项目。

<span id="extension" />
### <a name="add-an-app-service-extension"></a>添加应用服务扩展

在**解决方案资源管理器**中，打开打包项目的 **package.appxmanifest** 文件并添加应用服务扩展。

```xml
<Extensions>
      <uap:Extension
          Category="windows.appService"
          EntryPoint="MyAppService.AppServiceTask">
        <uap:AppService Name="com.microsoft.samples.winforms" />
      </uap:Extension>
    </Extensions>    
```

为应用服务给定一个名称，并提供入口点类的名称。 这是你用于实现应用服务的类。

<span id="appservice" />
### <a name="implement-the-app-service"></a>实现应用服务

下面是可对来自其他应用的请求进行验证和处理的位置。

```csharp
public sealed class AppServiceTask : IBackgroundTask
{
    private BackgroundTaskDeferral backgroundTaskDeferral;
 
    public void Run(IBackgroundTaskInstance taskInstance)
    {
        this.backgroundTaskDeferral = taskInstance.GetDeferral();
        taskInstance.Canceled += OnTaskCanceled;
        var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
        details.AppServiceConnection.RequestReceived += OnRequestReceived;
    }
 
    private async void OnRequestReceived(AppServiceConnection sender,
                                         AppServiceRequestReceivedEventArgs args)
    {
        var messageDeferral = args.GetDeferral();
        ValueSet message = args.Request.Message;
        string id = message["ID"] as string;
        ValueSet returnData = DataBase.GetData(id);
        await args.Request.SendResponseAsync(returnData);
        messageDeferral.Complete();
    }
 
 
    private void OnTaskCanceled(IBackgroundTaskInstance sender,
                                BackgroundTaskCancellationReason reason)
    {
        if (this.backgroundTaskDeferral != null)
        {
            this.backgroundTaskDeferral.Complete();
        }
    }
}
```

<span id="test" />
### <a name="test-the-app-service"></a>测试应用服务

通过从其他应用中调用你的服务来对其进行测试。

```csharp
private async void button_Click(object sender, RoutedEventArgs e)
{
    AppServiceConnection dataService = new AppServiceConnection();
    dataService.AppServiceName = "com.microsoft.samples.winforms";
    dataService.PackageFamilyName = "Microsoft.SDKSamples.WinformWithAppService";
 
    var status = await dataService.OpenAsync();
    if (status == AppServiceConnectionStatus.Success)
    {
        string id = int.Parse(textBox.Text);
        var message = new ValueSet();
        message.Add("ID", id);
        AppServiceResponse response = await dataService.SendMessageAsync(message);
        string result = "";
 
        if (response.Status == AppServiceResponseStatus.Success)
        {
            if (response.Message["Status"] as string == "OK")
            {
                DisplayResult(response.Message["Result"]);
            }
        }
    }
}
```

在此处了解有关应用服务的更多信息：[创建和使用应用服务](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)。

### <a name="similar-samples"></a>相似示例

[应用服务桥示例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/AppServiceBridgeSample)

[使用 C++ win32 应用的应用服务桥示例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/AppServiceBridgeSample_C%2B%2B)

[接收推送通知的 MFC 应用程序](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/MFCwithPush)


## <a name="making-your-desktop-application-a-share-target"></a>使桌面应用程序成为共享目标

可使你的桌面应用程序成为共享目标，以使用户能够轻松地共享数据，如来自支持共享的其他应用的图片。

例如，用户可能选择你的应用来共享来自 Microsoft Edge 这一照片应用的图片。 下面是具有该功能的 WPF 示例应用。

![共享目标](images\desktop-to-uwp\share-target.png)

### <a name="have-a-closer-look-at-this-app"></a>详细了解此应用

:heavy_check_mark: [观看视频](https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373/Demo-Make-a-WPF-Application-a-Share-Target-xd6Fu6WhD_8406218965)

:heavy_check_mark: [获取应用](https://www.microsoft.com/en-us/store/p/wpf-app-as-sharetarget/9pjcjljlck37)

:heavy_check_mark: [浏览代码](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WPFasShareTarget)

### <a name="the-design-pattern"></a>设计模式

要使你的应用程序成为共享目标，请执行以下操作：

:一: [向你的解决方案中添加一个 UWP 项目](#project2)

:二: [添加共享目标扩展](#share-extension)

:三: [覆盖 OnNavigatedTo 事件处理程序](#override)

<span id="project2" />
### <a name="add-a-uwp-project-to-your-solution"></a>向你的解决方案中添加一个 UWP 项目

向你的解决方案中添加一个**空白应用（通用 Windows）**项目。

<span id="share-extension" />
### <a name="add-a-share-target-extension"></a>添加共享目标扩展

在**解决方案资源管理器**中，打开项目的 **package.appxmanifest** 文件并添加扩展。

```xml
<Extensions>
      <uap:Extension
          Category="windows.shareTarget"
          Executable="ShareTarget.exe"
          EntryPoint="ShareTarget.App">
        <uap:ShareTarget>
          <uap:SupportedFileTypes>
            <uap:SupportsAnyFileType />
          </uap:SupportedFileTypes>
          <uap:DataFormat>Bitmap</uap:DataFormat>
        </uap:ShareTarget>
      </uap:Extension>
</Extensions>  
```

提供 UWP 项目生成的可执行文件的名称以及入口点类的名称。 你还必须指定可用你的应用共享的文件类型。

<span id="override" />
### <a name="override-the-onnavigatedto-event-handler"></a>覆盖 OnNavigatedTo 事件处理程序

覆盖 UWP 项目的**应用**类中的 **OnNavigatedTo** 事件处理程序。

当用户选择你的应用来共享文件时，将调用此事件处理程序。

```csharp
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
  this.shareOperation = (ShareOperation)e.Parameter;
  if (this.shareOperation.Data.Contains(StandardDataFormats.StorageItems))
  {
      this.sharedStorageItems =
        await this.shareOperation.Data.GetStorageItemsAsync();
       
      foreach (StorageFile item in this.sharedStorageItems)
      {
          ProcessSharedFile(item);
      }
  }
}
```

## <a name="support-and-feedback"></a>支持和反馈

**查找特定问题的答案**

我们的团队会监视这些 [StackOverflow 标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)

**提供关于本文的反馈**

请使用下面的评论区。
