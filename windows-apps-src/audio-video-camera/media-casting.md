---
author: drewbatgit
ms.assetid: 40B97E0C-EB1B-40C2-A022-1AB95DFB085E
description: 本文向你演示了如何将媒体从通用 Windows 应用转换到远程设备。
title: 媒体转换
---

# 媒体转换

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本文向你演示了如何将媒体从通用 Windows 应用转换到远程设备。

## 通过 MediaElement 的内置媒体转换

从通用 Windows 应用转换媒体的最简单方法是使用 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 控件的内置转换功能。

若要允许用户打开一个要在 **MediaElement** 控件中播放的视频文件，请将以下命名空间添加到你的项目。

[!code-cs[BuiltInCastingUsing](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetBuiltInCastingUsing)]

在你的应用的 XAML 文件中，添加 **MediaElement** 并将 [**AreTransportControlsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn298977) 设置为 true。

[!code-xml[MediaElement](./code/MediaCastingWin10/cs/MainPage.xaml#SnippetMediaElement)]

添加按钮以让用户启动选取文件操作。

[!code-xml[OpenButton](./code/MediaCastingWin10/cs/MainPage.xaml#SnippetOpenButton)]

在按钮的 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 事件处理程序中，创建 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 的新实例、将视频文件类型添加到 [**FileTypeFilter**](https://msdn.microsoft.com/library/windows/apps/br207850) 集合，并将起始位置设置为用户的视频库。

调用 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) 以启动“文件选取器”对话框。 当此方法返回时，结果是表示视频文件的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 对象。 检查以确保文件不是 null，如果用户取消选取操作，该文件将是 null。 调用文件的 [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227221.aspx) 方法来获取文件 [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731)。 最后，调用 **MediaElement** 对象的 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338) 方法，以使视频文件成为该控件的视频源。

[!code-cs[OpenButtonClick](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetOpenButtonClick)]

在 **MediaElement** 中加载视频后，用户只需按传输控件上的转换按钮，即可启动内置对话框，他们可以在该对话框中选择已加载媒体将转换到的设备。

![MediaElement 转换按钮](images/media-element-casting-button.png)

## 通过 CastingDevicePicker 的媒体转换

将媒体转换到设备的第二个方法是使用 [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/dn972525)。 若要使用此类型，请在项目中包括 [**Windows.Media.Casting**](https://msdn.microsoft.com/library/windows/apps/dn972568) 命名空间。

[!code-cs[CastingNamespace](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetCastingNamespace)]

声明 **CastingDevicePicker** 对象的成员变量。

[!code-cs[DeclareCastingPicker](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetDeclareCastingPicker)]

初始化你的页面时，创建转换选取器的新实例并将 [**Filter**](https://msdn.microsoft.com/library/windows/apps/dn972540) 设置为 [**SupportsVideo**](https://msdn.microsoft.com/library/windows/apps/dn972526) 属性，以指示由选取器列出的转换设备应支持视频。 为 [**CastingDeviceSelected**](https://msdn.microsoft.com/library/windows/apps/dn972539) 事件注册处理程序，它将在用户选取某台设备进行转换时引发。

[!code-cs[InitCastingPicker](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetInitCastingPicker)]

在你的 XAML 文件中，添加按钮以允许用户启动选取器。

[!code-xml[CastPickerButton](./code/MediaCastingWin10/cs/MainPage.xaml#SnippetCastPickerButton)]

在按钮的 **Click** 事件处理程序中，调用 [**TransformToVisual**](https://msdn.microsoft.com/library/windows/apps/br208986) 以获取相对于另一个元素的 UI 元素转换。 在此示例中，转换是相对于应用程序窗口的可视根的转换选取器按钮的位置。 调用 [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/dn972525) 对象的 [**Show**](https://msdn.microsoft.com/library/windows/apps/dn972542) 方法，以启动“转换选取器”对话框。 指定转换选取器按钮的位置和尺寸，以便系统可以使对话框在用户按下该按钮时弹出。

[!code-cs[CastPickerButtonClick](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetCastPickerButtonClick)]

在 **CastingDeviceSelected** 事件处理程序中，调用事件参数的 [**SelectedCastingDevice**](https://msdn.microsoft.com/library/windows/apps/dn972546) 属性的 [**CreateCastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972547) 方法，该方法表示用户所选的转换设备。 为 [**ErrorOccurred**](https://msdn.microsoft.com/library/windows/apps/dn972519) 和 [**StateChanged**](https://msdn.microsoft.com/library/windows/apps/dn972523) 事件注册处理程序。 最后，调用 [**RequestStartCastingAsync**](https://msdn.microsoft.com/library/windows/apps/dn972520) 以开始转换，从而传入 **MediaElement** 对象的 [**GetAsCastingSource**](https://msdn.microsoft.com/library/windows/apps/dn920012) 方法的结果，以便指明要转换的媒体是 **MediaElement** 的内容。

**注意** 必须在 UI 线程上启动转换连接。 因为未在 UI 线程上调用 **CastingDeviceSelected**，因此你必须将这些调用放置在对 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317)（可使这些调用在 UI 线程上调用）的调用内。

[!code-cs[CastingDeviceSelected](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetCastingDeviceSelected)]

在 **ErrorOccurred** 和 **StateChanged** 事件处理程序中，应更新 UI 以通知用户当前转换状态。 在有关创建自定义转换设备选取器的下一节中详细讨论了这些事件。

[!code-cs[EmptyStateHandlers](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetEmptyStateHandlers)]

## 通过自定义设备选取器的媒体转换

下一节介绍了如何通过从你的代码枚举转换设备和启动连接，创建你自己的转换设备选取器 UI。

若要枚举可用的转换设备，请在你的项目中包括 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459) 命名空间。

[!code-cs[EnumerationNamespace](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

将以下控件添加到 XAML 页面，以实现此示例的基本 UI：

-   用于启动查找可用转换设备的设备观察程序的按钮。
-   用于向正在运行转换枚举的用户提供反馈的 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) 控件。
-   用于列出发现的转换设备的 [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868)。 为控件定义 [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/br242830)，以便我们可以将转换设备对象直接分配给该控件，并仍然显示 [**FriendlyName**](https://msdn.microsoft.com/library/windows/apps/dn972549) 属性。
-   允许用户断开转换设备连接的按钮。

[!code-xml[CustomPickerXAML](./code/MediaCastingWin10/cs/MainPage.xaml#SnippetCustomPickerXAML)]

在代码隐藏中，声明 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/br225446) 和 [**CastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972510) 的成员变量。

[!code-cs[DeclareDeviceWatcher](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetDeclareDeviceWatcher)]

在 *startWatcherButton* 的 **Click** 处理程序中，首先通过禁用该按钮，并在正在运行设备枚举时激活进度环来更新 UI。 清除转换设备的列表框。

接下来，通过调用 [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/br225427) 创建设备观察程序。 此方法可用于观察许多不同类型的设备。 指明你想要通过使用 [**CastingDevice.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn972551) 返回的设备选择器字符串，观察支持视频转换的设备。

最后，为 [**Added**](https://msdn.microsoft.com/library/windows/apps/br225450)、[**Removed**](https://msdn.microsoft.com/library/windows/apps/br225453)、[**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/br225451) 和 [**Stopped**](https://msdn.microsoft.com/library/windows/apps/br225457) 事件注册事件处理程序。

[!code-cs[StartWatcherButtonClick](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetStartWatcherButtonClick)]

当观察程序发现新设备时，将引发 **Added** 事件。 在此事件的处理程序中，通过调用 [**CastingDevice.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn972550) 并传入发现的转换设备的 ID 来创建新的 [**CastingDevice**](https://msdn.microsoft.com/library/windows/apps/dn972524) 对象，它包含在已传入到处理程序中的 **DeviceInformation** 对象中。

将 **CastingDevice** 添加到转换设备 **ListBox**，以便用户可以选择它。 由于在 XAML 中定义的 [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/br242830)，[**FriendlyName**](https://msdn.microsoft.com/library/windows/apps/dn972549) 属性将用作列表框中的项文本。 因为未在 UI 线程上调用此事件处理程序，因此你必须在对 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 的调用内更新 UI。

[!code-cs[WatcherAdded](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetWatcherAdded)]

当观察程序检测到转换设备不再存在时，将引发 **Removed** 事件。 比较已传入到处理程序中 **Added** 对象的 ID 属性与列表框的 [**Items**](https://msdn.microsoft.com/library/windows/apps/br242823) 集合中每个 **Added** 的 ID。 如果 ID 匹配，则从集合中删除该对象。 同样，因为 UI 已得到更新，因此必须从 **RunAsync** 调用内进行此调用。

[!code-cs[WatcherRemoved](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetWatcherRemoved)]

当观察程序完成设备检测时，将引发 **EnumerationCompleted** 事件。 在此事件的处理程序中，更新 UI 以让用户知道设备枚举已完成，并通过调用 [**Stop**](https://msdn.microsoft.com/library/windows/apps/br225456) 停止设备观察程序。

[!code-cs[WatcherEnumerationCompleted](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetWatcherEnumerationCompleted)]

当设备观察程序已停止时，应引发 Stopped 事件。 在此事件的处理程序中，停止 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) 控件并重新启用 *startWatcherButton*，以便用户可以重新启动设备枚举过程。

[!code-cs[WatcherStopped](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetWatcherStopped)]

当用户从列表框中选择转换设备之一时，将引发 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) 事件。 它位于将创建转换连接并且将启动转换的这一处理程序中。

首先，请确保设备观察程序已停止，以便设备枚举不会干扰媒体转换。 通过在用户所选的 **CastingDevice** 对象上调用 [**CreateCastingConnection**](https://msdn.microsoft.com/library/windows/apps/dn972547) 来创建转换连接。 为 [**StateChanged**](https://msdn.microsoft.com/library/windows/apps/dn972523) 和 [**ErrorOccurred**](https://msdn.microsoft.com/library/windows/apps/dn972519) 事件添加事件处理程序。

通过调用 [**RequestStartCastingAsync**](https://msdn.microsoft.com/library/windows/apps/dn972520) 启动媒体转换，从而传入通过调用 **MediaElement** 方法 [**GetAsCastingSource**](https://msdn.microsoft.com/library/windows/apps/dn920012) 返回的转换源。 最后，使断开连接按钮可见，以允许用户停止媒体转换。

[!code-cs[SelectionChanged](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetSelectionChanged)]

在状态发生更改的处理程序中，要采取的操作取决于转换连接的新状态：

-   如果状态为 **Connected** 或 **Rendering**，确保 **ProgressRing** 控件处于非活动状态，并且断开连接按钮可见。
-   如果状态为 **Disconnected**，取消选择列表框中的当前转换设备、使 **ProgressRing** 控件处于非活动状态，并隐藏断开连接按钮。
-   如果状态为 **Connecting**，使 **ProgressRing** 控件处于活动状态，并隐藏断开连接按钮。
-   如果状态为 **Disconnecting**，使 **ProgressRing** 控件处于活动状态，并隐藏断开连接按钮。

[!code-cs[StateChanged](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetStateChanged)]

在 **ErrorOccurred** 事件的处理程序中，更新你的 UI 以让用户知道发生了转换错误，并取消选择列表框中的当前 **CastingDevice** 对象。

[!code-cs[ErrorOccurred](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetErrorOccurred)]

最后，实现断开连接按钮的处理程序。 通过调用 **CastingConnection** 对象的 [**DisconnectAsync**](https://msdn.microsoft.com/library/windows/apps/dn972518) 方法，停止媒体转换并从转换设备断开连接。 此调用必须已通过调用 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) 调度到 UI 线程。

[!code-cs[DisconnectButton](./code/MediaCastingWin10/cs/MainPage.xaml.cs#SnippetDisconnectButton)]

 

 






<!--HONumber=May16_HO2-->


