---
author: normesta
Description: Extend your desktop application with Windows UIs and components
Search.Product: eADQiWindows 10XVcnh
title: 使用 Windows UI 和组件扩展桌面应用程序
ms.author: normesta
ms.date: 06/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp
ms.localizationpriority: medium
ms.openlocfilehash: f3354dad1702d275fb7b2af53516689d2c5d5014
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "4154838"
---
# <a name="extend-your-desktop-application-with-modern-uwp-components"></a>使用新式 UWP 组件扩展桌面应用程序

有些 Windows 10 体验（例如：启用触摸功能的 UI 页面）必须在新式应用容器内运行。 如果要添加这些体验，请用 UWP 项目和 Windows 运行时组件扩展桌面应用程序。

在许多情况下，你可以直接从桌面应用程序中调用 UWP API，因此在查看本指南前，请参阅[面向 Windows 10 的增强](desktop-to-uwp-enhance.md)。

>[!NOTE]
>本指南假设你已使用桌面桥为桌面应用程序创建了 Windows 应用包。 如果你尚未完成此操作，请参阅[桌面桥](desktop-to-uwp-root.md)。

如果你已准备就绪，那我们开始吧。

<a id="setup" />

## <a name="first-setup-your-solution"></a>首先，设置你的解决方案

向你的解决方案添加一个或多个 UWP 项目和运行时组件。

从包含 **Windows 应用程序打包项目**（引用你的桌面应用程序）的解决方案开始。

下图显示了一个示例解决方案。

![扩展启动项目](images/desktop-to-uwp/extend-start-project.png)

如果你的解决方案不包含打包项目，请参阅[使用 Visual Studio 打包应用](desktop-to-uwp-packaging-dot-net.md)。

### <a name="add-a-uwp-project"></a>添加 UWP 项目

向你的解决方案中添加一个**空白应用（通用 Windows）**。

这是你将构建新式 XAML UI 或使用只在 UWP 进程中运行的 API 的位置。

![UWP 项目](images/desktop-to-uwp/add-uwp-project-to-solution.png)

在打包项目中，右键单击**应用程序**节点，然后单击**添加引用**。

![引用 UWP 项目](images/desktop-to-uwp/add-uwp-project-reference.png)

然后，添加对该 UWP 项目的引用。

![引用 UWP 项目](images/desktop-to-uwp/choose-uwp-project.png)

解决方案将如下所示：

![包含 UWP 项目的解决方案](images/desktop-to-uwp/uwp-project-reference.png)

### <a name="optional-add-a-windows-runtime-component"></a>（可选）添加 Windows 运行时组件

要实现某些方案，必须向 Windows 运行时组件添加代码。

![运行时组件应用服务](images/desktop-to-uwp/add-runtime-component.png)

然后，在 UWP 项目中添加对运行时组件的引用。 解决方案将如下所示：

![运行时组件引用](images/desktop-to-uwp/runtime-component-reference.png)

以下是可以使用 UWP 项目和运行时组件执行的一些操作。

## <a name="show-a-modern-xaml-ui"></a>显示新式 XAML UI

作为应用程序流的一部分，你可以将基于 XAML 的新式用户界面并入桌面应用程序。 这些用户界面可自然地适应不同屏幕尺寸和分辨率，并支持新式交互模型，如触摸和墨迹。

例如，借助少量 XAML 标记，即可为用户提供与地图相关的强大可视化功能。

此图像显示 Windows 窗体应用程序，该应用程序可打开包含地图控件的、基于 XAML 的新式 UI。

![自适应设计](images/desktop-to-uwp/extend-xaml-ui.png)

### <a name="the-design-pattern"></a>设计模式

要显示基于 XAML 的 UI，请执行以下操作：

:one: [设置解决方案](#solution-setup)

:two: [创建 XAML UI](#xaml-UI)

:three: [向 UWP 项目添加协议扩展](#protocol)

:four: [从桌面应用启动 UWP 应用](#start)

:five: [在 UWP 项目中，显示所需页面](#parse)

<a id="solution-setup" />

### <a name="setup-your-solution"></a>设置解决方案

有关如何设置解决方案的一般指导，请参阅本指南开头的[首先，设置解决方案](#setup)部分。

解决方案将如下所示：

![XAML UI 解决方案](images/desktop-to-uwp/xaml-ui-solution.png)

在该示例中，Windows 窗体项目被命名为 **Landmarks**，包含 XAML UI 的 UWP 项目被命名为 **MapUI**。

<a id="xaml-UI" />

### <a name="create-a-xaml-ui"></a>创建 XAML UI

向 UWP 项目添加 XAML UI。 以下是基本地图的 XAML。

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Margin="12,20,12,14">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <maps:MapControl x:Name="myMap" Grid.Column="0" Width="500" Height="500"
                     ZoomLevel="{Binding ElementName=zoomSlider,Path=Value, Mode=TwoWay}"
                     Heading="{Binding ElementName=headingSlider,Path=Value, Mode=TwoWay}"
                     DesiredPitch="{Binding ElementName=desiredPitchSlider,Path=Value, Mode=TwoWay}"    
                     HorizontalAlignment="Left"               
                     MapServiceToken="<Your Key Goes Here" />
    <Grid Grid.Column="1" Margin="12">
        <StackPanel>
            <Slider Minimum="1" Maximum="20" Header="ZoomLevel" Name="zoomSlider" Value="17.5"/>
            <Slider Minimum="0" Maximum="360" Header="Heading" Name="headingSlider" Value="0"/>
            <Slider Minimum="0" Maximum="64" Header=" DesiredPitch" Name="desiredPitchSlider" Value="32"/>
        </StackPanel>
    </Grid>
</Grid>
```

### <a name="add-a-protocol-extension"></a>添加协议扩展

在**解决方案资源管理器**中，在解决方案中，打开打包项目的**package.appxmanifest**文件并添加此扩展。

```xml
<Extensions>
  <uap:Extension Category="windows.protocol" Executable="MapUI.exe" EntryPoint="MapUI.App">
    <uap:Protocol Name="xamluidemo" />
  </uap:Extension>
</Extensions>    
```

为协议给定名称，提供 UWP 项目生成的可执行文件的名称以及入口点类的名称。

还可以在设计器中打开 **package.appxmanifest**，选择**声明**选项卡，然后在此处添加扩展名。

![“声明”选项卡](images/desktop-to-uwp/protocol-properties.png)

> [!NOTE]
> 地图控件可从 Internet 下载数据，因此如果你使用一个地图控件，则必须还要向清单中添加“Internet 客户端”功能。

<a id="start" />

### <a name="start-the-uwp-app"></a>启动 UWP 应用

首先，在桌面应用程序中，创建一个 [Uri](https://msdn.microsoft.com/library/system.uri.aspx)，其中包含协议名称和要传入 UWP 应用的任何参数。 然后，调用 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) 方法。

```csharp

private void Statue_Of_Liberty_Click(object sender, EventArgs e)
{
    ShowMap(40.689247, -74.044502);
}

private async void ShowMap(double lat, double lon)
{
    string str = "xamluidemo://";

    Uri uri = new Uri(str + "location?lat=" +
        lat.ToString() + "&?lon=" + lon.ToString());

    var success = await Windows.System.Launcher.LaunchUriAsync(uri);

}
```

<a id="parse" />

### <a name="parse-parameters-and-show-a-page"></a>分析参数并显示页面

在 UWP 项目的**应用**类中，覆盖 **OnActivated** 事件处理程序。 如果应用已通过协议激活，则分析参数并打开所需页面。

```csharp
protected override void OnActivated(Windows.ApplicationModel.Activation.IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.Protocol)
    {
        ProtocolActivatedEventArgs protocolArgs = (ProtocolActivatedEventArgs)e;
        Uri uri = protocolArgs.Uri;
        if (uri.Scheme == "xamluidemo")
        {
            Frame rootFrame = new Frame();
            Window.Current.Content = rootFrame;
            rootFrame.Navigate(typeof(MainPage), uri.Query);
            Window.Current.Activate();
        }
    }
}
```

重载 ``OnNavigatedTo`` 方法以使用传递给页面的参数。 在本例中，我们将使用传递给该页面的纬度和经度来在地图中显示一个位置。

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
 {
     if (e.Parameter != null)
     {
         WwwFormUrlDecoder decoder = new WwwFormUrlDecoder(e.Parameter.ToString());

         double lat = Convert.ToDouble(decoder[0].Value);
         double lon = Convert.ToDouble(decoder[1].Value);

         BasicGeoposition pos = new BasicGeoposition();

         pos.Latitude = lat;
         pos.Longitude = lon;

         myMap.Center = new Geopoint(pos);

         myMap.Style = MapStyle.Aerial3D;

     }

     base.OnNavigatedTo(e);
 }
```

### <a name="similar-samples"></a>相似示例

[向 VB6 应用程序添加 UWP XAML 用户体验](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/VB6withXaml)

[Northwind 示例：UWA UI & Win32 传统代码的端到端示例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/NorthwindSample)

[Northwind 示例：连接到 SQL Server 的 UWP 应用](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SQLServer)

## <a name="provide-services-to-other-apps"></a>向其他应用提供服务

添加其他应用可以使用的服务。 例如，你可以添加一个服务，以允许其他应用对你的应用之后的数据库进行受控访问。 通过实现后台任务，即使桌面应用未运行时，应用也可以访问该服务。

下面是执行此操作的示例。

![自适应设计](images/desktop-to-uwp/winforms-app-service.png)

### <a name="have-a-closer-look-at-this-app"></a>详细了解该应用

:heavy_check_mark: [获取应用](https://www.microsoft.com/en-us/store/p/winforms-appservice/9p7d9b6nk5tn)

:heavy_check_mark: [浏览代码](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinformsAppService)

### <a name="the-design-pattern"></a>设计模式

要显示提供服务，请执行以下操作：

:one: [实现应用服务](#appservice)

:two: [添加应用服务扩展](#extension)

:three: [测试应用服务](#test)

<a id="appservice" />

### <a name="implement-the-app-service"></a>实现应用服务

下面是可对来自其他应用的请求进行验证和处理的位置。 将下面的代码添加到你的解决方案中的 Windows 运行时组件。

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

<a id="extension" />

### <a name="add-an-app-service-extension-to-the-packaging-project"></a>将应用服务扩展添加到打包项目

打开**package.appxmanifest**文件的打包项目中，并添加到应用服务扩展``<Application>``元素。

```xml
<Extensions>
      <uap:Extension
          Category="windows.appService"
          EntryPoint="AppServiceComponent.AppServiceTask">
        <uap:AppService Name="com.microsoft.samples.winforms" />
      </uap:Extension>
    </Extensions>    
```
为应用服务给定一个名称，并提供入口点类的名称。 这是你实现该服务的类。

<a id="test" />

### <a name="test-the-app-service"></a>测试应用服务

通过从其他应用中调用你的服务来对其进行测试。 该代码可以是桌面应用程序，例如 Windows 窗体应用或其他 UWP 应用。

> [!NOTE]
> 只有在你正确设置了 ``AppServiceConnection`` 类的 ``PackageFamilyName`` 属性时，该代码才能正常工作。 你可以通过在 UWP 项目的上下文中调用 ``Windows.ApplicationModel.Package.Current.Id.FamilyName`` 来获得该名称。 请参阅[创建和使用应用服务](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)。

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

![共享目标](images/desktop-to-uwp/share-target.png)

### <a name="have-a-closer-look-at-this-app"></a>详细了解该应用

:heavy_check_mark: [获取应用](https://www.microsoft.com/en-us/store/p/wpf-app-as-sharetarget/9pjcjljlck37)

:heavy_check_mark: [浏览代码](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WPFasShareTarget)

### <a name="the-design-pattern"></a>设计模式

要使你的应用程序成为共享目标，请执行以下操作：

:one: [添加共享目标扩展](#share-extension)

:two: [覆盖 OnNavigatedTo 事件处理程序](#override)

<a id="share-extension" />

### <a name="add-a-share-target-extension"></a>添加共享目标扩展

在**解决方案资源管理器**中，在你的解决方案中打开打包项目的**package.appxmanifest**文件并添加扩展。

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

<a id="override" />

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

## <a name="create-a-background-task"></a>创建后台任务

你可以添加后台任务，从而在应用挂起后继续运行代码。 后台任务对于不需要用户交互的小任务非常有用。 例如，你的任务可以下载邮件，显示有关传入聊天消息的 Toast 通知，或对系统状况中的更改做出响应。

下面是一个注册后台任务的 WPF 示例应用。

![后台任务](images/desktop-to-uwp/sample-background-task.png)

该任务发出 http 请求并测量从请求到返回响应所用的时间。 你的任务可能会更有意义，但该示例对了解后台任务的基本机制非常有用。

### <a name="have-a-closer-look-at-this-app"></a>详细了解该应用

:heavy_check_mark: [浏览代码](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask)

### <a name="the-design-pattern"></a>设计模式

要创建后台服务，请执行以下操作：

:one: [实现后台任务](#implement-task)

:two: [配置后台任务](#configure-background-task)

:three: [注册后台任务](#register-background-task)

<a id="implement-task" />

### <a name="implement-the-background-task"></a>实现后台任务

通过向 Windows 运行时组件项目添加以下代码来实现该后台任务。

```csharp
public sealed class SiteVerifier : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {

        taskInstance.Canceled += TaskInstance_Canceled;
        BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
        var msg = await MeasureRequestTime();
        ShowToast(msg);
        deferral.Complete();
    }

    private async Task<string> MeasureRequestTime()
    {
        string msg;
        try
        {
            var url = ApplicationData.Current.LocalSettings.Values["UrlToVerify"] as string;
            var http = new HttpClient();
            Stopwatch clock = Stopwatch.StartNew();
            var response = await http.GetAsync(new Uri(url));
            response.EnsureSuccessStatusCode();
            var elapsed = clock.ElapsedMilliseconds;
            clock.Stop();
            msg = $"{url} took {elapsed.ToString()} ms";
        }
        catch (Exception ex)
        {
            msg = ex.Message;
        }
        return msg;
    }
```

<a id="configure-background-task" />

### <a name="configure-the-background-task"></a>配置后台任务

在清单设计器中，打开你的解决方案中的打包项目的**package.appxmanifest**文件。

在**声明**选项卡中，添加一个**后台任务**声明。

![后台任务选项](images/desktop-to-uwp/background-task-option.png)

然后，选择所需属性。 我们的示例使用 **Timer** 属性。

![Timer 属性](images/desktop-to-uwp/timer-property.png)

在 Windows 运行时组件中提供实现后台任务的类的完全限定名称。

![Timer 属性](images/desktop-to-uwp/background-task-entry-point.png)

<a id="register-background-task" />

### <a name="register-the-background-task"></a>注册后台任务

向注册后台任务的桌面应用程序项目添加以下代码。

```csharp
public void RegisterBackgroundTask(String triggerName)
{
    var current = BackgroundTaskRegistration.AllTasks
        .Where(b => b.Value.Name == triggerName).FirstOrDefault().Value;

    if (current is null)
    {
        BackgroundTaskBuilder builder = new BackgroundTaskBuilder();
        builder.Name = triggerName;
        builder.SetTrigger(new MaintenanceTrigger(15, false));
        builder.TaskEntryPoint = "HttpPing.SiteVerifier";
        builder.Register();
        System.Diagnostics.Debug.WriteLine("BGTask registered:" + triggerName);
    }
    else
    {
        System.Diagnostics.Debug.WriteLine("Task already:" + triggerName);
    }
}
```
## <a name="support-and-feedback"></a>支持和反馈

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
