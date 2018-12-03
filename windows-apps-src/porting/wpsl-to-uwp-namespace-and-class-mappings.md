---
description: 本主题提供 WindowsPhone Silverlight Api 到其通用 Windows 平台 (UWP) 等效项的全面映射。
title: WindowsPhone silverlight 移植到 UWP 命名空间和类映射
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0072aa814e0bcb22806cad764b5f365770961ac3
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8458920"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>WindowsPhone silverlight 移植到 UWP API 的映射


本主题提供 WindowsPhone Silverlight Api 到其通用 Windows 平台 (UWP) 等效项的全面映射。 但是，通常不存在一对一的功能映射：任一平台都可能比其在命名空间或类中的对应平台具有更多或更少的功能。

当你要在 UWP 项目中工作并重复使用 WindowsPhone Silverlight 项目中的源代码，映射表将帮助你。 这两个平台之间的命名空间和类（包括 UI 控件）的名称存在差异。 在许多情况下，只需更改命名空间名称，就可以编译代码。 有时，类或 API 名称以及命名空间名称已发生更改。 有时，映射会处理较多工作，在极少数情况下，则需要更改方法。

**如何使用该表：** 首先，搜索要使用的类的名称。 只要进行映射比仅更改命名空间名称更复杂，就会在此处列出类。 如果你的类未列出，则该映射只是命名空间的更改。 因此，找到你的类的命名空间名称后，你将找到等效的 UWP 命名空间名称。 你的类将位于该命名空间中。 如果你的命名空间未列出，则其名称并未更改。

**注意**windows 10 支持更多的.NET framework 比 Windows Phone 应用商店应用的功能。 例如，windows 10 具有多个 System.ServiceModel.\* 命名空间，以及 System.Net、 System.Net.NetworkInformation 和 System.Net.Sockets。
此外，在 windows 10 应用中，你将受益于.NET Native，这是先进障碍的编译技术，可将 MSIL 转换为可本机运行的计算机代码。 .NET Native 应用启动速度更快、使用的内存更少，并且比其对应的 MSIL 更省电。

| WindowsPhone Silverlight | Windows 运行时 |
| ------------------------- | --------------- |
| 广告 | |
| **Microsoft.Advertising.Mobile.UI.AdControl** 类 | [AdControl](http://msdn.microsoft.com/library/advertising-windows-sdk-api-reference-adcontrol.aspx) 类 |
| 警报、提醒和后台代理程序 | |
| **Microsoft.Phone.BackgroundAgent** 类 | [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 类 |
| **Microsoft.Phone.Scheduler** 命名空间 | [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847) 命名空间 |
| **Microsoft.Phone.Scheduler.Alarm** 类 | [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 和 [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642) 类 |
| **Microsoft.Phone.Scheduler.PeriodicTask**、**ScheduledAction**、**ScheduledActionService**、**ScheduledTask**、**ScheduledTaskAgent** 类 | [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 类 |
| **Microsoft.Phone.Scheduler.Reminder** 类 | [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 和 [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642) 类 |
| **Microsoft.Phone.PictureDecoder** 类 | [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) 类 |
| **Microsoft.Phone.BackgroundAudio** 命名空间 | [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) 命名空间 |
| **Microsoft.Phone.BackgroundTransfer** 命名空间 | [**Windows.Networking.BackgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) 命名空间 |
| 应用模型和环境 | |
| **System.AppDomain** 类 | 无直接等效项。 请参阅 [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324)、[**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016) 类 |
| **System.Environment** 类 | 无直接等效项 |
| **System.ComponentModel.Annotations** 类  | 无直接等效项 |
| **System.ComponentModel.BackgroundWorker** 类 | [**ThreadPool**](https://msdn.microsoft.com/library/windows/apps/br229621) 类 |
| **System.ComponentModel.DesignerProperties** 类 | [**DesignMode**](https://msdn.microsoft.com/library/windows/apps/br224664) 类 |
| **System.Threading.Thread**、**System.Threading.ThreadPool** 类 | [**ThreadPool**](https://msdn.microsoft.com/library/windows/apps/br229621) 类 |
| (ST = **System.Threading**) <br/> **ST.Thread.MemoryBarrier** 方法 | (ST = **System.Threading**) <br/> **ST.Interlocked.MemoryBarrier** 方法 |
| (ST = **System.Threading**) <br/> **ST.Thread.ManagedThreadId** 属性 | (S = **System**) <br/> **S.Environment.ManagedThreadId** 属性 |
| **System.Threading.Timer** 类 | [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/br230587) 类 |
| (SWT = **System.Windows.Threading**) <br/> **SWT.Dispatcher** 类 | [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 类 |
| (SWT = **System.Windows.Threading**) <br/> **SWT.DispatcherTimer** 类 | [**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250) 类 |
| Blend for Visual Studio | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> **MEDC.GeometryHelper** 类 | 无直接等效项 |
| **Microsoft.Expression.Interactivity** 命名空间 | [Microsoft.Xaml.Interactivity](http://go.microsoft.com/fwlink/p/?LinkId=328776) 命名空间 |
| **Microsoft.Expression.Interactivity.Core** 命名空间 | [Microsoft.Xaml.Interactions.Core](http://go.microsoft.com/fwlink/p/?LinkId=328773) 命名空间 |
| (MEIC = **Microsoft.Expression.Interactivity.Core**) <br/> **MEIC.ExtendedVisualStateManager** 类 | 无直接等效项 |
| **Microsoft.Expression.Interactivity.Input** 命名空间 | 无直接等效项 |
| **Microsoft.Expression.Interactivity.Media** 命名空间 | [Microsoft.Xaml.Interactions.Media](http://go.microsoft.com/fwlink/p/?LinkId=328775) 命名空间 |
| **Microsoft.Expression.Shapes** 命名空间 | 无直接等效项 |
| (MI = **Microsoft.Internal**) <br/> **MI.IManagedFrameworkInternalHelper** 接口 | 无直接等效项 |
| 联系人和日历数据 | |
| **Microsoft.Phone.UserData** 命名空间 | [**Windows.ApplicationModel.Contacts**](https://msdn.microsoft.com/library/windows/apps/br225002)、[**Windows.ApplicationModel.Appointments**](https://msdn.microsoft.com/library/windows/apps/dn263359) 命名空间 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Account**、**ContactAddress**、**ContactCompanyInformation**、**ContactEmailAddress**、**ContactPhoneNumber** 类 | [**Contact**](https://msdn.microsoft.com/library/windows/apps/br224849) 类 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Appointments** 类 | [**AppointmentCalendar**](https://msdn.microsoft.com/library/windows/apps/dn596134) 类 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Contacts** 类 | [**ContactStore**](https://msdn.microsoft.com/library/windows/apps/dn624859) 类 |
| 控件和 UI 基础结构 | |
| **ControlTiltEffect.TiltEffect** 类 | Windows 运行时动画库中的动画内置于常用控件的默认样式中。 请参阅[动画](wpsl-to-uwp-porting-xaml-and-ui.md)。 |
| **Microsoft.Phone.Controls** 命名空间 | [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) 命名空间 |
| (MPC = **Microsoft.Phone.Controls**) <br/> **MPC.ContextMenu** 类 | [**PopupMenu**](https://msdn.microsoft.com/library/windows/apps/br208693) 类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.DatePickerPage** 类 | [**DatePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn625013) 类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.GestureListener** 类 | [**GestureRecognizer**](https://msdn.microsoft.com/library/windows/apps/br241937) 类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.LongListSelector** 类 | [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) 类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.ObscuredEventArgs** 类 | [**SystemProtection**](https://msdn.microsoft.com/library/windows/apps/jj585394)、[**WindowActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208377) 类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.Panorama** 类 | [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) 类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationFrame**、<br/>(SWN = **System.Windows.Navigation**) <br/>**SWN.NavigationService** 类 | [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationPage** 类 | [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) 类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TiltEffect** 类 | [**PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164) 类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TimePickerPage** 类 | [**TimePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn608313) 类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowser** 类 | [**WebView**](https://msdn.microsoft.com/library/windows/apps/br227702) 类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowserExtensions** 类 | 无直接等效项 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WrapPanel** 类 | 无常规布局用途的直接等效项。 [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/dn298849) 和 [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227717) 可以在项目控件的项目面板模板中使用。 |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq** 命名空间 | 无直接等效项 |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq.Mapping** 命名空间 | 无直接等效项 |
| **Microsoft.Phone.Globalization** 命名空间 | 无直接等效项 |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.DeviceExtendedProperties**、**DeviceStatus** 类 | [**EasClientDeviceInformation**](https://msdn.microsoft.com/library/windows/apps/hh701390)、[**MemoryManager**](https://msdn.microsoft.com/library/windows/apps/dn633831) 类 有关详细信息，请参阅[设备状态](wpsl-to-uwp-input-and-sensors.md)。 |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.MediaCapabilities** 类 | 无直接等效项 |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.UserExtendedProperties** 类 | [**AdvertisingManager**](https://msdn.microsoft.com/library/windows/apps/dn363391) 类 |
| **System.Windows** 命名空间 | [**Windows.UI.Xaml**](https://msdn.microsoft.com/library/windows/apps/br209045) 命名空间 |
| **System.Windows.Automation** 命名空间 | [**Windows.UI.Xaml.Automation**](https://msdn.microsoft.com/library/windows/apps/br209179) 命名空间 |
| **System.Windows.Controls**、**System.Windows.Input** 命名空间 | [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)、[**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)、[**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) 命名空间 |
| **System.Windows.Controls.DrawingSurface**、**DrawingSurfaceBackgroundGrid** 类 | [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) 类 |
| **System.Windows.Controls.RichTextBox** 类 | [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548) 类 |
| **System.Windows.Controls.WrapPanel** 类 | 无常规布局用途的直接等效项。 [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/dn298849) 和 [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227717) 可以在项目控件的项目面板模板中使用。 |
| **System.Windows.Controls.Primitives** 命名空间 | [**Windows.UI.Xaml.Controls.Primitives**](https://msdn.microsoft.com/library/windows/apps/br209818) 命名空间 |
| **System.Windows.Controls.Shapes** 命名空间 | [**Windows.UI.Xaml.Controls.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) 命名空间 |
| **System.Windows.Data** 命名空间 | [**Windows.UI.Xaml.Data**](https://msdn.microsoft.com/library/windows/apps/br209917) 命名空间 |
| **System.Windows.Documents** 命名空间 | [**Windows.UI.Xaml.Documents**](https://msdn.microsoft.com/library/windows/apps/br209984) 命名空间 |
| **System.Windows.Ink** 命名空间 | 无直接等效项 |
| **System.Windows.Markup** 命名空间 | [**Windows.UI.Xaml.Markup**](https://msdn.microsoft.com/library/windows/apps/br228046) 命名空间 | 
| **System.Windows.Navigation** 命名空间 | [**Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300) 命名空间 |
| **System.Windows.UIElement.Tap** 事件、**EventHandler&lt;GestureEventArgs&gt;** 委托 | [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985) 事件、[**TappedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br227993) 委托 |
| 数据和服务 |  |
| **System.Data.Linq.DataContext** 类 | 无直接等效项 |
| **System.Data.Linq.Mapping.ColumnAttribute** 类 | 无直接等效项 |
| **System.Data.Linq.SqlClient.SqlHelpers** 类 | 无直接等效项 |
| 设备 | |
| **Microsoft.Devices**、**Microsoft.Devices.Sensors** 命名空间 | [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459)、[**Windows.Devices.Enumeration.Pnp**](https://msdn.microsoft.com/library/windows/apps/br225517)、[**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)、[**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/br206408) 命名空间 |
| **Microsoft.Devices.Camera**、**Microsoft.Devices.PhotoCamera** 类 | [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 类。 此外，[**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 类（仅限 Windows）。 |
| **Microsoft.Devices.CameraButtons** 类 | [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557) 类 |
| **Microsoft.Devices.CameraVideoBrushExtensions** 类 | [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) 类 |
| **Microsoft.Devices.Environment** 类 | 无直接等效项。 使用条件编译并定义自定义符号作为解决方法。 或者你可以使用 [IsAttached](http://msdn.microsoft.com/library/e299w87h.aspx) 属性设计一个解决方法。 |
| **Microsoft.Devices.MediaHistory** 类 | 无直接等效项 |
| **Microsoft.Devices.VibrateController** 类 | [**VibrationDevice**](https://msdn.microsoft.com/library/windows/apps/jj207230) 类 |
| **Microsoft.Devices.Radio.FMRadio** 类 | 无直接等效项 |
| **Microsoft.Devices.Sensors.Accelerometer**、**Compass** 类 | 在 [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/br206408) 命名空间中 |
| **Microsoft.Devices.Sensors.Gyroscope** 类 | [**Gyrometer**](https://msdn.microsoft.com/library/windows/apps/br225718) 类 |
| **Microsoft.Devices.Sensors.Motion** 类 | [**Inclinometer**](https://msdn.microsoft.com/library/windows/apps/br225766) 类 |
| 全球化 | |
| **System.Globalization** 命名空间 | [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) 命名空间 |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentCulture** 属性 | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentCulture** 属性 |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentUICulture** 属性 | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentUICulture** 属性 |
| 图形和动画 | |
| **Microsoft.Xna.Framework.\*** 命名空间、[XNA Framework 类库](http://go.microsoft.com/fwlink/p/?LinkId=263769)、[Content Pipeline 类库](http://go.microsoft.com/fwlink/p/?LinkId=263770) | 无直接等效项。 通常情况下，结合使用 [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) 和 C++。 请参阅[开发游戏](https://msdn.microsoft.com/library/windows/apps/hh452744)和 [DirectX 和 XAML 互操作](https://msdn.microsoft.com/library/windows/apps/hh825871)。 |
| **Microsoft.Xna.Framework.Audio.Microphone** 类 | [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 类 |
| **Microsoft.Xna.Framework.Audio.SoundEffect** 类 | [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 类 |
| **Microsoft.Xna.Framework.GamerServices** 命名空间 | (WPS = **Windows.Phone.System**) <br/> [**WPS.UserProfile.GameServices.Core**](https://msdn.microsoft.com/library/windows/apps/jj207609) 命名空间 |
| **Microsoft.Xna.Framework.GamerServices.Guide** 类 | 无直接等效项 |
| **Microsoft.Xna.Framework.Input.GamePad** 类 | [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557) 类 |
| **Microsoft.Xna.Framework.Input.Touch.TouchPanel** 类 | [**GestureRecognizer**](https://msdn.microsoft.com/library/windows/apps/br241937) 类 |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> **MXFM.MediaLibrary**、**MXFM.PhoneExtensions.MediaLibraryExtensions** 类 | [**KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151) 类 |
| **Microsoft.Xna.Framework.Media.MediaQueue** 类 | [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 类 |
| **Microsoft.Xna.Framework.Media.Playlist** 类 | [**BackgroundMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652527) 类 |
| **System.Windows.Media** 命名空间 | [**Windows.UI.Xaml.Media**](/uwp/api/Windows.UI.Xaml.Media) 命名空间 |
| **System.Windows.Media.RadialGradientBrush** 类 | 无直接等效项。 请参阅[媒体和图形](wpsl-to-uwp-porting-xaml-and-ui.md)。 |
| **System.Windows.Media.Animation** 命名空间 | [**Windows.UI.Xaml.Media.Animation**](https://msdn.microsoft.com/library/windows/apps/br243232) 命名空间 |
| **System.Windows.Media.Effects** 命名空间 | 无直接等效项 |
| **System.Windows.Media.Imaging** 命名空间 | [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258) 命名空间 |
| **System.Windows.Media.Media3D** 命名空间 | [**Windows.UI.Xaml.Media.Media3D**](https://msdn.microsoft.com/library/windows/apps/br243274) 命名空间 |
| **System.Windows.Shapes** 命名空间 | [**Windows.UI.Xaml.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) 命名空间 |
| 启动器和选择器 | |
| **Microsoft.Phone.Tasks.AddressChooserTask**、**EmailAddressChooserTask**、**PhoneNumberChooserTask** 类 | [**ContactPicker**](https://msdn.microsoft.com/library/windows/apps/br224913) 类 |
| **Microsoft.Phone.Tasks.AddWalletItemTask**、**AddWalletItemResult** 类 | [**Windows.ApplicationModel.Wallet**](https://msdn.microsoft.com/library/windows/apps/dn631399) 命名空间 |
| **Microsoft.Phone.Tasks.BingMapsDirectionsTask**、**BingMapsTask** 类 | 无直接等效项 |
| **Microsoft.Phone.Tasks.CameraCaptureTask** 类 | [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 类。 此外，[**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 类（仅限 Windows）。 |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 类（[**RequestAppPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/hh967813) 方法） |
| **Microsoft.Phone.Tasks.ConnectionSettingsTask**、**MarketplaceHubTask**、**MarketplaceReviewTask**、**MarketplaceSearchTask**、**MediaPlayerLauncher**、**SearchTask**、**SmsComposeTask**、**WebBrowserTask** 类 | [**Launcher**](https://msdn.microsoft.com/library/windows/apps/br241801) 类 |
| **Microsoft.Phone.Tasks.EmailComposeTask** 类 | [**EmailMessage**](https://msdn.microsoft.com/library/windows/apps/dn631270) 类 |
| **Microsoft.Phone.Tasks.GameInviteTask** 类 | 无直接等效项 |
| **Microsoft.Phone.Tasks.MapDownloaderTask**、**MapsDirectionsTask**、**MapsTask**、**MapUpdaterTask** 类 | 无直接等效项 |
| **Microsoft.Phone.Tasks.PhoneCallTask** 类 | [**PhoneCallManager**](https://msdn.microsoft.com/library/windows/apps/dn624832) 类 |
| **Microsoft.Phone.Tasks.PhotoChooserTask** 类 | [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 类 |
| **Microsoft.Phone.Tasks.SaveAppointmentTask** 类 | [**AppointmentManager**](https://msdn.microsoft.com/library/windows/apps/dn297254) 类 |
| **Microsoft.Phone.Tasks.SaveContactTask**、**SaveEmailAddressTask**、**SavePhoneNumberTask** 类 | [**StoredContact**](https://msdn.microsoft.com/library/windows/apps/jj207727) 类（仅限 Windows Phone） | 
| **Microsoft.Phone.Tasks.SaveRingtoneTask** 类 | 无直接等效项 |
| **Microsoft.Phone.Tasks.ShareLinkTask**、**ShareMediaTask**、**ShareStatusTask** 类 | [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/br205873) 类 |
| 位置 | |
| **System.Device.Location** 命名空间 | [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603) 命名空间 |
| **System.Device.GeoCoordinateWatcher** 类 | [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 类 |
| 地图 | |
| **Microsoft.Phone.Maps** 命名空间 | [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空间 |
| **Microsoft.Phone.Maps.Controls** 命名空间 | [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751) 命名空间 |
| **Microsoft.Phone.Maps.Controls.Map** 类 | [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 类 |
| **Microsoft.Phone.Maps.Services** 命名空间 | [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空间 |
| **Microsoft.Phone.Maps.Services.GeocodeQuery**、**ReverseGeocodeQuery** 类 | [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 类 |
| **System.Device.Location.GeoCoordinate** 类 | [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675) 类 |
| **Microsoft.Phone.Maps.Services.Route** 类 | [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) 类 |
| **Microsoft.Phone.Maps.Services.RouteQuery** 类 | [**MapRouteFinder**](https://msdn.microsoft.com/library/windows/apps/dn636938) 类 |
| 盈利 | |
| **Microsoft.Phone.Marketplace** 命名空间 | [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197) 命名空间 |
| 媒体 | |
| **Microsoft.Phone.Media** 命名空间 | [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 类 |
| 网络 | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.DeviceNetworkInformation** 类 | [**Hostname**](https://msdn.microsoft.com/library/windows/apps/br207113)、[**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293) 类 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterface** 类 | [**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293) 类 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceInfo** 类 | [**ConnectionProfile**](https://msdn.microsoft.com/library/windows/apps/br207249) 类 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceList** 类 | [**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293) 类 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.SocketExtensions** 类 | 无直接等效项 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.WebRequestExtensions** 类 | 无直接等效项 |
| **Microsoft.Phone.Networking.Voip** 命名空间 | 无直接等效项 |
| **System.Net.CookieCollection** 类 | 仍受支持，但部分属性已丢失（例如 IsReadOnly） |
| **System.Net.DownloadProgressChangedEventArgs** 类，以及与 **System.Net.WebClient** 相关的类似的类 | [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 类（或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)）。 派生自 [System.Net.Http.StreamContent](https://msdn.microsoft.com/library/system.net.http.streamcontent.aspx)，用于测量进度。 |
| **System.Net.DnsEndPoint**、**IPAddress** 类 | 这些类仍受支持，但部分属性已丢失。 或者，移植到 [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113) 类。 |
| **System.Net.HttpUtility** 类 | [**HtmlFormatHelper**](https://msdn.microsoft.com/library/windows/apps/hh738437) 类 |
| **System.Net.HttpWebRequest** 类 | 部分支持，但推荐的预期备用项为 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 类（或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)）。 这些 API 使用 [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) 来表示 HTTP 请求。 |
| **System.Net.HttpWebResponse** 类 | 仍受支持，但使用的是 Dispose() 而不是 Close()。 但是，推荐的预期备用项为 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 类（或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)）。 这些 API 使用 [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx) 表示 HTTP 响应。 |
| (SNN = **System.Net.NetworkInformation**) <br/> **SNN.NetworkChange** 类 | 仍受支持，但构造函数除外。 |
| **System.Net.OpenReadCompletedEventArgs** 类，以及与 **System.Net.WebClient** 相关的类似的类 | [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 类（或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)） |
| **System.Net.Sockets.Socket** 类 | 仍受支持，但使用的是 Dispose() 而不是 Close()。 或者，移植到 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 类。 |
| **System.Net.Sockets.SocketException** 类 | 仍受支持，但使用的是 SocketErrorCode 属性而不是 ErrorCode。 |
| **System.Net.Sockets.UdpAnySourceMulticastClient**、**UdpSingleSourceMulticastClient** 类 | [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 类 |
| **System.Net.UploadProgressChangedEventArgs** 类，以及与 **System.Net.WebClient** 相关的类似的类 | [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 类（或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)） |
| **System.Net.WebClient** 类 | [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 类（或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)） |
| **System.Net.WebRequest** 类 | 部分支持（另一组属性），但推荐的预期备用项为 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 类（或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)）。 这些 API 使用 [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) 来表示 HTTP 请求。 |
| **System.Net.WebResponse** 类 | 仍受支持，但使用的是 Dispose() 而不是 Close()。 但是，推荐的预期备用项为 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 类（或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)）。 这些 API 使用 [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx) 表示 HTTP 响应。 |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventArgs** 类 | [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 类（或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)） |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventHandler** 类 | [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 类（或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)） |
| **System.UriFormatException** 类 | **System.FormatException** 类 |
| 通知 | |
| MPN = **Microsoft.Phone.Notification** 命名空间 | [**Windows.UI.Notifications**](https://msdn.microsoft.com/library/windows/apps/br208661)、[**Windows.Networking.PushNotifications**](https://msdn.microsoft.com/library/windows/apps/br241307) 命名空间 |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotification** 类 | [**TileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616) 类 |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotificationChannel** 类 | [**PushNotificationChannel**](https://msdn.microsoft.com/library/windows/apps/br241283) 类 |
| 编程 | |
| **System** 命名空间 | [**Windows.Foundation**](https://msdn.microsoft.com/library/windows/apps/br226021) 命名空间 |
| **System.Diagnostics.StackFrame**、**StackTrace** 类 | 无直接等效项 |
| **System.Diagnostics** 命名空间 | [**Windows.Foundation.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/br206677) 命名空间 |
| **System.ICloneable** 接口 | 可返回相应类型的自定义方法。 |
| **System.Reflection.Emit.ILGenerator** 类 | 无直接等效项 |
| 反应性扩展框架 | |
| **Microsoft.Phone.Reactive** 命名空间 | 无直接等效项 |
| 反射 | |
| **System.Type** 类 | **System.Reflection.TypeInfo** 类。 请参阅[.NET Framework 中适用于 UWP 应用的反射](https://msdn.microsoft.com/library/hh535795.aspx)。 |
| 资源 | |
| **System.Resources.ResourceManager** 类 | (WA = **Windows.ApplicationModel**)<br/>[**WA.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) 以及 [**WA.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022) 命名空间、[**ResourceManager**](https://msdn.microsoft.com/library/windows/apps/br206078) 类。 请参阅[在 Windows 运行时应用中创建和检索资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh694557.aspx)。 |
| 安全元素 | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementChannel**、**MPS.SecureElementSession** 类 | [**SmartCardConnection**](https://msdn.microsoft.com/library/windows/apps/dn608002) 类 |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementReader** 类 | [**SmartCardReader**](https://msdn.microsoft.com/library/windows/apps/dn263857) 类 |
| 安全 | |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.Aes**、**SSC.RSA** 类 | [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) 类 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.HMACSHA256**、**SSC.SHA256** 类 | [**HashAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241511) 类 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.ProtectedData** 类 | [**DataProtectionProvider**](https://msdn.microsoft.com/library/windows/apps/br241559) 类 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.RandomNumberGenerator** 类 | [**CryptographicBuffer**](https://msdn.microsoft.com/library/windows/apps/br227092) 类 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.X509Certificates.X509Certificate** 类 | [**CertificateEnrollmentManager**](https://msdn.microsoft.com/library/windows/apps/br212075) 类 |
| Shell | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBar** 类 | [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/dn279427) 类 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarIconButton** 类 | [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) 类（当在 [**PrimaryCommands**](https://msdn.microsoft.com/library/windows/apps/dn279430) 属性内部使用时） |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarMenuItem** 类 | [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) 类（当在 [**SecondaryCommands**](https://msdn.microsoft.com/library/windows/apps/dn279434) 属性内部使用时） |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.CycleTileData**、**MPSh.FlipTileData**、**MPSh.IconicTileData**、**MPSh.ShellTileData**、**MPSh.StandardTileData** 类 | [**TileTemplateType**](https://msdn.microsoft.com/library/windows/apps/br208621) 类 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.PhoneApplicationService** 类 | [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016)、[**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) 类 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ProgressIndicator** 类 | [**StatusBarProgressIndicator**](https://msdn.microsoft.com/library/windows/apps/dn633865) 类 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTile** 类 | [**SecondaryTile**](https://msdn.microsoft.com/library/windows/apps/br242183) 类 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTileSchedule** 类 | [**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628) 类 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellToast** 类 | [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642) 类 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.SystemTray** 类 | [**StatusBar**](https://msdn.microsoft.com/library/windows/apps/dn633864) 类 |
| 存储和 I/O | |
| **Microsoft.Phone.Storage.ExternalStorage**、**ExternalStorageDevice**、**ExternalStorageFile**、**ExternalStorageFolder** 类 | [**KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151) 类 |
| **System.IO** 命名空间 | [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346)、[**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791) 命名空间 |
| **System.IO.Directory** 类 | [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 类 |
| **System.IO.File** 类 | [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 和 [**PathIO**](https://msdn.microsoft.com/library/windows/apps/hh701663) 类
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageFile** 类 |[**ApplicationData.LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) 属性 |
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageSettings** 类 | [**ApplicationData.LocalSettings**](https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.localsettings.aspx) 属性 |
| **System.IO.Stream** 类 | 仍受支持，但使用的是 ReadAsync() 和 WriteAsync()，而不是 BeginRead()/EndRead() 和 BeginWrite()/EndWrite()。 |
| 电子钱包 | |
| **Microsoft.Phone.Wallet** 命名空间 | [**Windows.ApplicationModel.Wallet**](https://msdn.microsoft.com/library/windows/apps/dn631399) 命名空间 |
| Xml | |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTime** 方法 |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTimeOffset** 方法 |


下一主题是[移植项目](wpsl-to-uwp-porting-to-a-uwp-project.md)。

