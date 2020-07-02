---
Description: 使用 Windows UI 和组件扩展桌面应用程序
title: 使用 Windows UI 和组件扩展应用
ms.date: 06/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 73e867071058dfde71979318d6d711d79460f30b
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334556"
---
# <a name="extend-your-desktop-app-with-modern-uwp-components"></a>使用新式 UWP 组件扩展桌面应用

某些 Windows 10 体验（例如，支持触摸的 UI 页面）必须在新式应用容器内部运行。 如果要添加这些体验，请用 UWP 项目和 Windows 运行时组件扩展桌面应用程序。

在许多情况下，你可以直接从桌面应用程序中调用 Windows 运行时 API，因此在查看本指南前，请参阅[面向 Windows 10 的增强](desktop-to-uwp-enhance.md)。

> [!NOTE]
> 本文中所述的功能要求桌面应用具有[程序包标识符](modernize-packaged-apps.md)（通过[在 MSIX 包中打包桌面应用](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)或[使用稀疏包授予应用标识](grant-identity-to-nonpackaged-apps.md)）。

如果你准备好了，那我们就开始吧。

<a id="setup"></a>

## <a name="first-setup-your-solution"></a>首先，设置你的解决方案

向你的解决方案添加一个或多个 UWP 项目和运行时组件。

从包含 Windows 应用程序打包项目  （引用你的桌面应用程序）的解决方案开始。

下图显示了一个示例解决方案。

![扩展启动项目](images/desktop-to-uwp/extend-start-project.png)

如果你的解决方案不包含打包项目，请参阅[使用 Visual Studio 打包桌面应用程序](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)。

### <a name="configure-the-desktop-application"></a>配置桌面应用程序

确保桌面应用程序引用了你调用 Windows 运行时 API 所需的文件。

为此，请参阅[设置项目](desktop-to-uwp-enhance.md#set-up-your-project)部分。

### <a name="add-a-uwp-project"></a>添加 UWP 项目

向你的解决方案中添加一个空白应用（通用 Windows）  。

这是你将构建新式 XAML UI 或使用只在 UWP 进程中运行的 API 的位置。

![UWP 项目](images/desktop-to-uwp/add-uwp-project-to-solution.png)

在打包项目中，右键单击“应用程序”节点，然后单击“添加引用”   。

![引用 UWP 项目](images/desktop-to-uwp/add-uwp-project-reference.png)

然后，添加对该 UWP 项目的引用。

![引用 UWP 项目](images/desktop-to-uwp/choose-uwp-project.png)

你的解决方案将如下所示：

![包含 UWP 项目的解决方案](images/desktop-to-uwp/uwp-project-reference.png)

### <a name="optional-add-a-windows-runtime-component"></a>（可选）添加 Windows 运行时组件

要实现某些方案，你需要向 Windows 运行时组件添加代码。

![运行时组件应用服务](images/desktop-to-uwp/add-runtime-component.png)

然后，在 UWP 项目中添加对运行时组件的引用。 你的解决方案将如下所示：

![运行时组件引用](images/desktop-to-uwp/runtime-component-reference.png)

### <a name="build-your-solution"></a>生成解决方案

生成解决方案以确保无错误出现。 如果收到错误，请打开**配置管理器**并确保你的项目以同一平台为应用目标。

![配置管理器](images/desktop-to-uwp/config-manager.png)

以下是可以使用 UWP 项目和运行时组件执行的一些操作。

## <a name="show-a-modern-xaml-ui"></a>显示新式 XAML UI

作为应用程序流的一部分，你可以将基于 XAML 的新式用户界面并入桌面应用程序。 这些用户界面可自然地适应不同屏幕尺寸和分辨率，并支持新式交互模型，例如触摸和墨迹。

例如，借助少量 XAML 标记，你可以为用户提供与地图相关的强大可视化功能。

此图像显示 Windows 窗体应用程序，该应用程序可打开包含地图控件的、基于 XAML 的新式 UI。

![自适应设计](images/desktop-to-uwp/extend-xaml-ui.png)

>[!NOTE]
>本示例通过向解决方案中添加一个 UWP 项目来展示了一个 XAML UI。 这是支持在桌面应用程序中显示 XAML UI 的一种稳定方法。 此方法的另一种替代方法是使用 XAML 岛直接向桌面应用程序添加 UWP XAML 控件。 XAML 岛当前以开发者预览版的形式提供。 现在，尽管我们鼓励在自己的原型代码中试用，但不建议在生产代码中使用它们。 在未来的 Windows 版本中，这些 API 和控件将继续趋于成熟和稳定。 要详细了解 XAML 岛，请参阅[桌面应用程序中的 UWP 控件](xaml-islands.md)

### <a name="the-design-pattern"></a>设计模式

要显示基于 XAML 的 UI，请执行以下操作：

:one:[设置解决方案](#solution-setup)

:two:[创建 XAML UI](#xaml-UI)

:three:[向 UWP 项目添加协议扩展](#add-a-protocol-extension)

:four:[从桌面应用启动 UWP 应用](#start)

:five:[在 UWP 项目中，显示所需页面](#parse)

<a id="solution-setup"></a>

### <a name="setup-your-solution"></a>设置解决方案

有关如何设置解决方案的一般指导，请参阅本指南开头的[首先，设置你的解决方案](#setup)部分。

解决方案将如下所示：

![XAML UI 解决方案](images/desktop-to-uwp/xaml-ui-solution.png)

在该示例中，Windows 窗体项目被命名为“Landmarks”  ，包含 XAML UI 的 UWP 项目被命名为“MapUI”  。

<a id="xaml-UI"></a>

### <a name="create-a-xaml-ui"></a>创建 XAML UI

向 UWP 项目添加 XAML UI。 以下是一个基本地图的 XAML。

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

在“解决方案资源管理器”  中，打开解决方案中打包项目的“package.appxmanifest”文件  并添加该扩展。

```xml
<Extensions>
  <uap:Extension Category="windows.protocol" Executable="MapUI.exe" EntryPoint="MapUI.App">
    <uap:Protocol Name="xamluidemo" />
  </uap:Extension>
</Extensions>
```

为协议命名，并提供 UWP 项目生成的可执行文件的名称以及入口点类的名称。

还可以在设计器中打开 package.appxmanifest  ，选择“声明”  选项卡，然后在其中添加扩展。

![“声明”选项卡](images/desktop-to-uwp/protocol-properties.png)

> [!NOTE]
> 地图控件可从 Internet 下载数据，因此如果你使用一个地图控件，则还必须要向清单中添加“Internet 客户端”功能。

<a id="start"></a>

### <a name="start-the-uwp-app"></a>启动 UWP 应用

首先，在桌面应用程序中，创建一个 [URI](https://docs.microsoft.com/dotnet/api/system.uri)，其中包含协议名称和要传入 UWP 应用的任何参数。 然后，调用 [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) 方法。

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

<a id="parse"></a>

### <a name="parse-parameters-and-show-a-page"></a>分析参数并显示页面

在 UWP 项目的“应用”  类中，覆盖 OnActivated  事件处理程序。 如果应用已通过协议激活，则分析参数并打开所需页面。

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

在 XAML 页面的代码中，覆盖 ``OnNavigatedTo`` 方法以使用传递到该页面的参数。 在本例中，我们将使用传递给该页面的纬度和经度来在地图中显示一个位置。

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

你可以使你的桌面应用程序成为共享目标，这样一来，用户就能够轻松地共享数据，例如来自支持共享的其他应用的图片。

例如，用户可以选择你的应用程序来共享来自 Microsoft Edge 的图片，即照片应用。 下面是具有该功能的 WPF 示例应用程序。

![共享目标](images/desktop-to-uwp/share-target.png).

有关完整示例，请参阅[此处](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/ShareTarget)

### <a name="the-design-pattern"></a>设计模式

要使你的应用程序成为共享目标，请执行以下操作：

:one:[添加共享目标扩展](#share-extension)

:two:[覆盖 OnShareTargetActivated 事件处理程序](#override)

:three:[将桌面扩展添加到 UWP 项目](#desktop-extensions)

:four:[添加完全信任的进程扩展](#full-trust)

:five:[修改桌面应用程序以获取共享文件](#modify-desktop)

<a id="share-extension"></a>

具体步骤如下  

### <a name="add-a-share-target-extension"></a>添加共享目标扩展

在“解决方案资源管理器”  中，打开解决方案中打包项目的“package.appxmanifest”文件  并添加此共享目标扩展。

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

提供 UWP 项目生成的可执行文件的名称以及入口点类的名称。 此标记假设 UWP 应用的可执行文件的名称是 `ShareTarget.exe`。

你还需要指定可与你的应用共享的文件类型。 在此示例中，我们将 [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) 桌面应用程序作为位图图像的共享目标，因此对于支持的文件类型，我们指定 `Bitmap`。

<a id="override"></a>

### <a name="override-the-onsharetargetactivated-event-handler"></a>覆盖 OnShareTargetActivated 事件处理程序

在 UWP 项目的“应用”  类中，覆盖 OnShareTargetActivated  事件处理程序。

此事件处理程序会在用户选择你的应用来共享他们的文件时被调用。

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

在本代码中，我们将用户正在共享的图像保存到应用的本地存储文件夹中。 稍后，我们将修改桌面应用程序以从同一文件夹中拉取图像。 由于桌面应用程序包含在 UWP 应用所在的同一包中，因此可以执行此操作。

<a id="desktop-extensions"></a>

### <a name="add-desktop-extensions-to-the-uwp-project"></a>将桌面扩展添加到 UWP 项目

将适用于 UWP 的 Windows 桌面扩展添加到 UWP 应用项目中  。

![桌面扩展](images/desktop-to-uwp/desktop-extensions.png)

<a id="full-trust"></a>

### <a name="add-the-full-trust-process-extension"></a>添加完全信任的进程扩展

在“解决方案资源管理器”  中，打开解决方案中打包项目的 package.appxmanifest  文件，然后添加之前添加此文件的共享目标扩展旁的完全信任的进程扩展。

```xml
<Extensions>
  ...
      <desktop:Extension Category="windows.fullTrustProcess" Executable="PhotoStoreDemo\PhotoStoreDemo.exe" />
  ...
</Extensions>  
```

此扩展将允许 UWP 应用启动你要与之共享文件的桌面应用程序。 在本例中，我们引用 [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) 桌面应用程序的可执行文件。

<a id="modify-desktop"></a>

### <a name="modify-the-desktop-application-to-get-the-shared-file"></a>修改桌面应用程序以获取共享文件

修改桌面应用程序以找到并处理共享文件。 在此示例中，UWP 应用将共享文件存储在本地应用数据文件夹中。 因此，我们将修改 [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) 桌面应用程序以从该文件夹拉取照片。

```csharp
Photos.Path = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
```

对于用户已打开的桌面应用程序的实例，我们还可以处理 [FileSystemWatcher](https://docs.microsoft.com/dotnet/api/system.io.filesystemwatcher) 事件，并传入文件位置的路径。 这样，任何打开的桌面应用程序实例都会显示共享照片。

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

你可以添加后台任务来运行代码，即使应用已挂起时也是如此。 后台任务对于不需要用户交互的小任务非常有用。 例如，你的任务可以下载邮件，显示有关传入聊天消息的 Toast 通知，或对系统状况中的更改做出响应。

下面是一个注册后台任务的 WPF 示例应用程序。

![后台任务](images/desktop-to-uwp/sample-background-task.png)

该任务发出 http 请求并测量从请求到返回响应所用的时间。 你的任务可能会更有趣，但该示例对了解后台任务的基本机制非常有用。

有关完整示例，请参阅[此处](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask)。

### <a name="the-design-pattern"></a>设计模式

要创建后台服务，请执行以下操作：

:one:[实现后台任务](#implement-task)

:two:[配置后台任务](#configure-background-task)

:three:[注册后台任务](#register-background-task)

<a id="implement-task"></a>

### <a name="implement-the-background-task"></a>实施后台任务

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

<a id="configure-background-task"></a>

### <a name="configure-the-background-task"></a>配置后台任务

在清单设计器中，打开解决方案中打包项目的 package.appxmanifest  文件。

在“声明”  选项卡中，添加一个“后台任务”  声明。

![后台任务选项](images/desktop-to-uwp/background-task-option.png)

然后，选择所需属性。 我们的示例使用 Timer  属性。

![Timer 属性](images/desktop-to-uwp/timer-property.png)

在 Windows 运行时组件中提供实现后台任务的类的完全限定名称。

![Timer 属性](images/desktop-to-uwp/background-task-entry-point.png)

<a id="register-background-task"></a>

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

## <a name="find-answers-to-your-questions"></a>查找问题的答案

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。
