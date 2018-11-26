---
Description: Extend your desktop application with Windows UIs and components
Search.Product: eADQiWindows 10XVcnh
title: 使用 Windows UI 和组件扩展桌面应用程序
ms.date: 06/08/2018
ms.topic: article
keywords: windows 10，uwp
ms.localizationpriority: medium
ms.openlocfilehash: 76e4b60e1cd25a205d6a304f12a0b04f5db693b5
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7707076"
---
# <a name="extend-your-desktop-application-with-modern-uwp-components"></a>使用新式 UWP 组件扩展桌面应用程序

有些 Windows 10 体验（例如：启用触摸功能的 UI 页面）必须在新式应用容器内运行。 如果要添加这些体验，请用 UWP 项目和 Windows 运行时组件扩展桌面应用程序。

在许多情况下，你可以直接从桌面应用程序中调用 Windows 运行时 Api，因此在查看本指南中前,，请参阅[适用于 Windows 10 增强](desktop-to-uwp-enhance.md)。

>[!NOTE]
>本指南假定你已为桌面应用程序创建 Windows 应用包。 如果你尚未完成此操作，请参阅[打包桌面应用程序](desktop-to-uwp-root.md)。

如果你已准备就绪，那我们开始吧。

<a id="setup" />

## <a name="first-setup-your-solution"></a>首先，设置你的解决方案

向你的解决方案添加一个或多个 UWP 项目和运行时组件。

从包含 **Windows 应用程序打包项目**（引用你的桌面应用程序）的解决方案开始。

下图显示了一个示例解决方案。

![扩展启动项目](images/desktop-to-uwp/extend-start-project.png)

如果你的解决方案不包含打包项目，请参阅[包使用 Visual Studio 的桌面应用程序](desktop-to-uwp-packaging-dot-net.md)。

### <a name="configure-the-desktop-application"></a>配置的桌面应用程序

请确保你的桌面应用程序具有的文件，则需要调用 Windows 运行时 Api 的参考。

若要执行此操作，请参阅主题[增强 Windows 10 的桌面应用程序](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-enhance#first-set-up-your-project)的[第一次，设置你的项目](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-enhance#first-set-up-your-project)部分。

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

### <a name="build-your-solution"></a>生成解决方案

生成解决方案以确保未显示任何错误。 如果你收到错误，打开**配置管理器**，并确保你的项目面向相同的平台。

![配置管理器](images/desktop-to-uwp/config-manager.png)

以下是可以使用 UWP 项目和运行时组件执行的一些操作。

## <a name="show-a-modern-xaml-ui"></a>显示新式 XAML UI

作为应用程序流的一部分，你可以将基于 XAML 的新式用户界面并入桌面应用程序。 这些用户界面可自然地适应不同屏幕尺寸和分辨率，并支持新式交互模型，如触摸和墨迹。

例如，借助少量 XAML 标记，即可为用户提供与地图相关的强大可视化功能。

此图像显示 Windows 窗体应用程序，该应用程序可打开包含地图控件的、基于 XAML 的新式 UI。

![自适应设计](images/desktop-to-uwp/extend-xaml-ui.png)

>[!NOTE]
>此示例显示了通过向解决方案中添加 UWP 项目的 XAML UI。 这是显示 XAML Ui 中的桌面应用程序的稳定支持的方法。 此方法的替代方法是使用 XAML 岛直接向桌面应用程序添加 UWP XAML 控件。 XAML 群岛目前为开发人员预览。 尽管我们鼓励你试用它们在原型代码中现在，我们不建议你使用它们在生产代码中这一次。 这些 Api 和控件将继续成熟并在将来稳定的 Windows 版本。 若要了解有关 XAML 群岛的详细信息，请参阅[在桌面应用程序的 UWP 控件](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

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

在代码隐藏 XAML 页中，重写``OnNavigatedTo``方法使用参数传递给页面。 在本例中，我们将使用传递给该页面的纬度和经度来在地图中显示一个位置。

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

## <a name="making-your-desktop-application-a-share-target"></a>使桌面应用程序成为共享目标

可使你的桌面应用程序成为共享目标，以使用户能够轻松地共享数据，如来自支持共享的其他应用的图片。

例如，用户可能选择你的应用程序共享来自 Microsoft Edge，照片应用的图片。 下面是具有该功能的 WPF 示例应用程序。

![共享目标](images/desktop-to-uwp/share-target.png).

完整的示例，请参阅[下面](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/ShareTarget)

### <a name="the-design-pattern"></a>设计模式

要使你的应用程序成为共享目标，请执行以下操作：

:one: [添加共享目标扩展](#share-extension)

: two：[覆盖 OnShareTargetActivated 事件处理程序](#override)

: three：[添加桌面扩展到 UWP 项目](#desktop-extensions)

: four：[添加完全信任的进程扩展](#full-trust)

: five：[修改的桌面应用程序来获取共享的文件](#modify-desktop)

<a id="share-extension" />

以下步骤  

### <a name="add-a-share-target-extension"></a>添加共享目标扩展

在**解决方案资源管理器**中，在你的解决方案中打开打包项目的**package.appxmanifest**文件并添加共享目标扩展。

```xml
<Extensions>
      <uap:Extension
          Category="windows.shareTarget"
          Executable="ShareTarget.exe"
          EntryPoint="App">
        <uap:ShareTarget>
          <uap:SupportedFileTypes>
            <uap:SupportsAnyFileType />
          </uap:SupportedFileTypes>
          <uap:DataFormat>Bitmap</uap:DataFormat>
        </uap:ShareTarget>
      </uap:Extension>
</Extensions>  
```

提供 UWP 项目生成的可执行文件的名称以及入口点类的名称。 此标记假定你的 UWP 应用的可执行文件的名称是`ShareTarget.exe`。

你还必须指定可用你的应用共享的文件类型。 在此示例中，我们进行[WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo)桌面应用程序成为共享目标的位图图像，因此我们指定`Bitmap`受支持的文件类型。

<a id="override" />

### <a name="override-the-onsharetargetactivated-event-handler"></a>覆盖 OnShareTargetActivated 事件处理程序

覆盖 UWP 项目的**应用程序**类中**OnShareTargetActivated**事件处理程序。

当用户选择你的应用来共享文件时，将调用此事件处理程序。

```csharp

protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    shareWithDesktopApplication(args.ShareOperation);
}

private async void shareWithDesktopApplication(ShareOperation shareOperation)
{
    if (shareOperation.Data.Contains(StandardDataFormats.StorageItems))
    {
        var items = await shareOperation.Data.GetStorageItemsAsync();
        StorageFile file = items[0] as StorageFile;
        IRandomAccessStreamWithContentType stream = await file.OpenReadAsync();

        await file.CopyAsync(ApplicationData.Current.LocalFolder);
            shareOperation.ReportCompleted();

        await FullTrustProcessLauncher.LaunchFullTrustProcessForCurrentAppAsync();
    }
}
```

在此代码中，我们将保存到应用的本地存储文件夹由用户共享的图像。 更高版本，我们将修改的桌面应用程序到拉取映像从该相同的文件夹。 桌面应用程序可以这样做是因为它包含在与 UWP 应用相同的包。

<a id="desktop-extensions" />

### <a name="add-desktop-extensions-to-the-uwp-project"></a>桌面扩展添加到 UWP 项目

将**适用于 UWP 的 Windows 桌面扩展**扩展添加到 UWP 应用项目。

![桌面扩展](images/desktop-to-uwp/desktop-extensions.png)

<a id="full-trust" />

### <a name="add-the-full-trust-process-extension"></a>添加完全信任的进程扩展

在**解决方案资源管理器**中，在解决方案中，打开打包项目的**package.appxmanifest**文件，然后添加旁边你之前添加此文件共享目标扩展的完全信任过程扩展。

```xml
<Extensions>
  ...
      <desktop:Extension Category="windows.fullTrustProcess" Executable="PhotoStoreDemo\PhotoStoreDemo.exe" />
  ...
</Extensions>  
```

此扩展将使 UWP 应用启动到你想要共享文件的桌面应用程序。 在示例中，我们引用的[WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo)桌面应用程序的可执行文件。

<a id="modify-desktop" />

### <a name="modify-the-desktop-application-to-get-the-shared-file"></a>修改的桌面应用程序来获取共享的文件

修改桌面应用程序以查找并处理共享的文件。 此示例中，在 UWP 应用存储在本地应用数据文件夹中的共享的文件。 因此，我们将修改的[WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo)桌面应用程序向拉取照片从该文件夹。

```csharp
Photos.Path = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
```

对于用户打开已有的桌面应用程序的情况下，我们还可能会处理[FileSystemWatcher](https://docs.microsoft.com/dotnet/api/system.io.filesystemwatcher?view=netframework-4.7.2)事件并在路径中传递到文件位置。 该方法任何打开的实例的桌面应用程序将显示共享的照片。

```csharp
...

   FileSystemWatcher watcher = new FileSystemWatcher(Photos.Path);

...

private void Watcher_Created(object sender, FileSystemEventArgs e)
{
    // new file got created, adding it to the list
    Dispatcher.BeginInvoke(System.Windows.Threading.DispatcherPriority.Normal, new Action(() =>
    {
        if (File.Exists(e.FullPath))
        {
            ImageFile item = new ImageFile(e.FullPath);
            Photos.Insert(0, item);
            PhotoListBox.SelectedIndex = 0;
            CurrentPhoto.Source = (BitmapSource)item.Image;
        }
    }));
}

```

## <a name="create-a-background-task"></a>创建后台任务

你可以添加后台任务，从而在应用挂起后继续运行代码。 后台任务对于不需要用户交互的小任务非常有用。 例如，你的任务可以下载邮件，显示有关传入聊天消息的 Toast 通知，或对系统状况中的更改做出响应。

下面是注册后台任务的 WPF 示例应用程序。

![后台任务](images/desktop-to-uwp/sample-background-task.png)

该任务发出 http 请求并测量从请求到返回响应所用的时间。 你的任务可能会更有意义，但该示例对了解后台任务的基本机制非常有用。

完整的示例，请参阅[下面](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask)。

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

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
