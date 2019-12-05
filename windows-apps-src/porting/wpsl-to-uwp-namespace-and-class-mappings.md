---
description: 本主题提供 Windows Phone Silverlight Api 到通用 Windows 平台（UWP）等效项的综合性映射。
title: WPSL 到 UWP 命名空间和类映射
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fdb1dc8ad4b4e61e1ffec294cfbf17e8abcc8586
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2019
ms.locfileid: "74735052"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>Windows Phone Silverlight 到 UWP API 映射


本主题提供 Windows Phone Silverlight Api 到通用 Windows 平台（UWP）等效项的综合性映射。 但是，通常不存在一对一的功能映射：任一平台都可能比其在命名空间或类中的对应平台具有更多或更少的功能。

当你在 UWP 项目中工作，并且要从 Windows Phone Silverlight 项目中重新使用源代码时，映射表将会有帮助。 这两个平台之间的命名空间和类（包括 UI 控件）的名称存在差异。 在许多情况下，只需更改命名空间名称，就可以编译代码。 有时，类或 API 名称以及命名空间名称已发生更改。 有时，映射会处理较多工作，在极少数情况下，则需要更改方法。

**如何使用表：  **首先，搜索正在使用的类的名称。 只要进行映射比仅更改命名空间名称更复杂，就会在此处列出类。 如果你的类未列出，则该映射只是命名空间的更改。 因此，找到你的类的命名空间名称后，你将找到等效的 UWP 命名空间名称。 你的类将位于该命名空间中。 如果你的命名空间未列出，则其名称并未更改。

**请注意**  Windows 10 比 Windows Phone 应用商店应用支持更多的 .NET Framework。 例如，Windows 10 有多个 System.servicemodel。\* 命名空间以及 System.Net、System.net.networkinformation 和系统 .Net。
此外，在 Windows 10 应用程序中，你将从 .NET Native 中获益，这是一项预编译技术，可将 MSIL 转换为可运行的本机代码。 .NET Native 应用启动速度更快、使用的内存更少，并且比其对应的 MSIL 更省电。

| Windows Phone Silverlight | Windows 运行时 |
| ------------------------- | --------------- |
| 广告 | |
| **Microsoft.Advertising.Mobile.UI.AdControl** 类 | [AdControl](https://docs.microsoft.com/windows/uwp/monetize/display-ads-using-the-microsoft-advertising-libraries) 类 |
| 警报、提醒和后台代理程序 | |
| **Microsoft.Phone.BackgroundAgent** 类 | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)类 |
| **Microsoft.Phone.Scheduler** 命名空间 | [**Windows.applicationmodel.resources.core**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background)命名空间 |
| **Microsoft.Phone.Scheduler.Alarm** 类 | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)和[**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager)类 |
| **Microsoft.Phone.Scheduler.PeriodicTask**、**ScheduledAction**、**ScheduledActionService**、**ScheduledTask**、**ScheduledTaskAgent** 类 | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)类 |
| **Microsoft.Phone.Scheduler.Reminder** 类 | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)和[**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager)类 |
| **Microsoft.Phone.PictureDecoder** 类 | [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder)类 |
| **Microsoft.Phone.BackgroundAudio** 命名空间 | [**Windows Media！播放**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback)命名空间 |
| **Microsoft.Phone.BackgroundTransfer** 命名空间 | [**Windows.networking.backgroundtransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer)命名空间 |
| 应用模型和环境 | |
| **System.AppDomain** 类 | 无直接等效项。 请参阅 [**Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application)、[**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication) 类 |
| **System.Environment** 类 | 无直接等效项 |
| **System.ComponentModel.Annotations** 类  | 无直接等效项 |
| **System.ComponentModel.BackgroundWorker** 类 | [**ThreadPool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool)类 |
| **System.ComponentModel.DesignerProperties** 类 | [**DesignMode**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DesignMode)类 |
| **System.Threading.Thread**、**System.Threading.ThreadPool** 类 | [**ThreadPool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool)类 |
| (ST = **System.Threading**) <br/> **ST.Thread.MemoryBarrier** 方法 | (ST = **System.Threading**) <br/> **ST.Interlocked.MemoryBarrier** 方法 |
| (ST = **System.Threading**) <br/> **ST.Thread.ManagedThreadId** 属性 | (S = **System**) <br/> **S.Environment.ManagedThreadId** 属性 |
| **System.Threading.Timer** 类 | [**Windows.system.threading.threadpooltimer**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer)类 |
| (SWT = **System.Windows.Threading**) <br/> **SWT.Dispatcher** 类 | [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher)类 |
| (SWT = **System.Windows.Threading**) <br/> **SWT.DispatcherTimer** 类 | [**DispatcherTimer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DispatcherTimer)类 |
| Blend for Visual Studio | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> **MEDC.GeometryHelper** 类 | 无直接等效项 |
| **Microsoft.Expression.Interactivity** 命名空间 | [Microsoft.Xaml.Interactivity](https://msdn.microsoft.com/library/windows/apps/microsoft.xaml.interactivity.aspx) 命名空间 |
| **Microsoft.Expression.Interactivity.Core** 命名空间 | [Microsoft.Xaml.Interactions.Core](https://msdn.microsoft.com/library/windows/apps/microsoft.xaml.interactions.core.aspx) 命名空间 |
| (MEIC = **Microsoft.Expression.Interactivity.Core**) <br/> **MEIC.ExtendedVisualStateManager** 类 | 无直接等效项 |
| **Microsoft.Expression.Interactivity.Input** 命名空间 | 无直接等效项 |
| **Microsoft.Expression.Interactivity.Media** 命名空间 | [Microsoft.Xaml.Interactions.Media](https://msdn.microsoft.com/library/windows/apps/microsoft.xaml.interactions.media.aspx) 命名空间 |
| **Microsoft.Expression.Shapes** 命名空间 | 无直接等效项 |
| (MI = **Microsoft.Internal**) <br/> **MI.IManagedFrameworkInternalHelper** 接口 | 无直接等效项 |
| 联系人和日历数据 | |
| **Microsoft.Phone.UserData** 命名空间 | [**Windows.applicationmodel.resources.core**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts)、 [**windows.applicationmodel.resources.core**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments) 、命名空间 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Account**、**ContactAddress**、**ContactCompanyInformation**、**ContactEmailAddress**、**ContactPhoneNumber** 类 | [**Contact**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact)类 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Appointments** 类 | [**AppointmentCalendar**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentCalendar)类 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Contacts** 类 | [**ContactStore**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactStore)类 |
| 控件和 UI 基础结构 | |
| **ControlTiltEffect.TiltEffect** 类 | Windows 运行时动画库中的动画内置于常用控件的默认样式中。 请参阅[动画](wpsl-to-uwp-porting-xaml-and-ui.md)。 |
| **Microsoft.Phone.Controls** 命名空间 | [**WINDOWS UI. Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls)命名空间 |
| (MPC = **Microsoft.Phone.Controls**) <br/> **MPC.ContextMenu** 类 | [**PopupMenu**](https://docs.microsoft.com/uwp/api/Windows.UI.Popups.PopupMenu)类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.DatePickerPage** 类 | [**DatePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout)类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.GestureListener** 类 | [**GestureRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer)类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.LongListSelector** 类 | [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom)类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.ObscuredEventArgs** 类 | [**SystemProtection**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.SystemProtection)， [**WindowActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.WindowActivatedEventArgs)类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.Panorama** 类 | [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub)类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationFrame**、<br/>(SWN = **System.Windows.Navigation**) <br/>**SWN.NavigationService** 类 | [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationPage** 类 | [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TiltEffect** 类 | [**PointerDownThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TimePickerPage** 类 | [**TimePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout)类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowser** 类 | [**Web 视图**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView)类 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowserExtensions** 类 | 无直接等效项 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WrapPanel** 类 | 无常规布局用途的直接等效项。 [**ItemsWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid)和[**WrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid)可用于 items 控件的 "项" 面板模板。 |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq** 命名空间 | 无直接等效项 |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq.Mapping** 命名空间 | 无直接等效项 |
| **Microsoft.Phone.Globalization** 命名空间 | 无直接等效项 |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.DeviceExtendedProperties**、**DeviceStatus** 类 | [**EasClientDeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Security.ExchangeActiveSyncProvisioning.EasClientDeviceInformation)、 [**MemoryManager**](https://docs.microsoft.com/uwp/api/Windows.System.MemoryManager)类。 有关详细信息，请参阅[设备状态](wpsl-to-uwp-input-and-sensors.md)。 |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.MediaCapabilities** 类 | 无直接等效项 |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.UserExtendedProperties** 类 | [**AdvertisingManager**](https://docs.microsoft.com/uwp/api/Windows.System.UserProfile.AdvertisingManager)类 |
| **System.Windows** 命名空间 | [**WINDOWS UI .xaml**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml)命名空间 |
| **System.Windows.Automation** 命名空间 | [**WINDOWS UI .xaml**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation)命名空间 |
| **System.Windows.Controls**、**System.Windows.Input** 命名空间 | " [**Windows ui"、** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core)"windows [ **"、"** ](https://docs.microsoft.com/uwp/api/Windows.UI.Input)ui" [](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) 、" |
| **System.Windows.Controls.DrawingSurface**、**DrawingSurfaceBackgroundGrid** 类 | [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)类 |
| **System.Windows.Controls.RichTextBox** 类 | [**RichEditBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)类 |
| **System.Windows.Controls.WrapPanel** 类 | 无常规布局用途的直接等效项。 [**ItemsWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid)和[**WrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid)可用于 items 控件的 "项" 面板模板。 |
| **System.Windows.Controls.Primitives** 命名空间 | [**WINDOWS UI. .xaml**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives)命名空间 |
| **System.Windows.Controls.Shapes** 命名空间 | [**Windows**](/uwp/api/Windows.UI.Xaml.Shapes)命名空间命名空间 |
| **System.Windows.Data** 命名空间 | [**WINDOWS UI. .xaml**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data)命名空间 |
| **System.Windows.Documents** 命名空间 | [**WINDOWS UI. .xaml**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents)命名空间 |
| **System.Windows.Ink** 命名空间 | 无直接等效项 |
| **System.Windows.Markup** 命名空间 | [**Windows. .xaml**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup)命名空间 | 
| **System.Windows.Navigation** 命名空间 | [**WINDOWS UI .xaml**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation)命名空间 |
| **System.Windows.UIElement.Tap** 事件、**EventHandler&lt;GestureEventArgs&gt;** 委托 | [**攻丝**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped)事件， [**TappedEventHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.tappedeventhandler)委托 |
| 数据和服务 |  |
| **System.Data.Linq.DataContext** 类 | 无直接等效项 |
| **System.Data.Linq.Mapping.ColumnAttribute** 类 | 无直接等效项 |
| **System.Data.Linq.SqlClient.SqlHelpers** 类 | 无直接等效项 |
| “设备” | |
| **Microsoft.Devices**、**Microsoft.Devices.Sensors** 命名空间 | " [**Windows**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)"、 [ **"windows"** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.Pnp)、"windows"、"windows"、"windows"、"windows" 和 "设备"。 [](https://docs.microsoft.com/uwp/api/Windows.Devices.Input) [](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) |
| **Microsoft.Devices.Camera**、**Microsoft.Devices.PhotoCamera** 类 | [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture)类。 此外，[**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 类（仅限 Windows）。 |
| **Microsoft.Devices.CameraButtons** 类 | [**HardwareButtons**](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons)类 |
| **Microsoft.Devices.CameraVideoBrushExtensions** 类 | [**CaptureElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement)类 |
| **Microsoft.Devices.Environment** 类 | 无直接等效项。 使用条件编译并定义自定义符号作为解决方法。 或者你可以使用 [IsAttached](https://docs.microsoft.com/dotnet/api/system.diagnostics.debugger.isattached#System_Diagnostics_Debugger_IsAttached) 属性设计一个解决方法。 |
| **Microsoft.Devices.MediaHistory** 类 | 无直接等效项 |
| **Microsoft.Devices.VibrateController** 类 | [**VibrationDevice**](https://docs.microsoft.com/uwp/api/Windows.Phone.Devices.Notification.VibrationDevice)类 |
| **Microsoft.Devices.Radio.FMRadio** 类 | 无直接等效项 |
| **Microsoft.Devices.Sensors.Accelerometer**、**Compass** 类 | 在 [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) 命名空间中 |
| **Microsoft.Devices.Sensors.Gyroscope** 类 | [**陀螺测试仪**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Gyrometer)类 |
| **Microsoft.Devices.Sensors.Motion** 类 | [**倾斜仪**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer)类 |
| 全球化 | |
| **System.Globalization** 命名空间 | [**Windows**](https://docs.microsoft.com/uwp/api/Windows.Globalization)命名空间命名空间 |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentCulture** 属性 | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentCulture** 属性 |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentUICulture** 属性 | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentUICulture** 属性 |
| 图形和动画 | |
| **\*** 命名空间、带空间[框架](https://msdn.microsoft.com/library/bb203940.aspx)类库、[内容管道](https://msdn.microsoft.com/library/bb195587(v=XNAGameStudio.40).aspx)类库 | 无直接等效项。 通常情况下，结合使用 [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) 和 C++。 请参阅[开发游戏](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10))和 [DirectX 和 XAML 互操作](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10))。 |
| **Microsoft.Xna.Framework.Audio.Microphone** 类 | [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture)类 |
| **Microsoft.Xna.Framework.Audio.SoundEffect** 类 | [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement)类 |
| **Microsoft.Xna.Framework.GamerServices** 命名空间 | (WPS = **Windows.Phone.System**) <br/> [**WPS。GameServices**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.UserProfile.GameServices.Core)命名空间 |
| **Microsoft.Xna.Framework.GamerServices.Guide** 类 | 无直接等效项 |
| **Microsoft.Xna.Framework.Input.GamePad** 类 | [**HardwareButtons**](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons)类 |
| **Microsoft.Xna.Framework.Input.Touch.TouchPanel** 类 | [**GestureRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer)类 |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> **MXFM.MediaLibrary**、**MXFM.PhoneExtensions.MediaLibraryExtensions** 类 | [**Knownfolders.h**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)类 |
| **Microsoft.Xna.Framework.Media.MediaQueue** 类 | [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls)类 |
| **Microsoft.Xna.Framework.Media.Playlist** 类 | [**BackgroundMediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.BackgroundMediaPlayer)类 |
| **System.Windows.Media** 命名空间 | [**WINDOWS UI .xaml**](/uwp/api/Windows.UI.Xaml.Media)命名空间 |
| **System.Windows.Media.RadialGradientBrush** 类 | 无直接等效项。 请参阅[媒体和图形](wpsl-to-uwp-porting-xaml-and-ui.md)。 |
| **System.Windows.Media.Animation** 命名空间 | [**Windows. .xaml. Media**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation)命名空间 |
| **System.Windows.Media.Effects** 命名空间 | 无直接等效项 |
| **System.Windows.Media.Imaging** 命名空间 | [**Windows. .xaml**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging)命名空间 |
| **System.Windows.Media.Media3D** 命名空间 | [**System.windows.media.media3d.model3d>** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D)命名空间。 |
| **System.Windows.Shapes** 命名空间 | [**WINDOWS UI .xaml**](/uwp/api/Windows.UI.Xaml.Shapes)命名空间 |
| 启动器和选择器 | |
| **Microsoft.Phone.Tasks.AddressChooserTask**、**EmailAddressChooserTask**、**PhoneNumberChooserTask** 类 | [**Windows.contactpicker**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactPicker)类 |
| **Microsoft.Phone.Tasks.AddWalletItemTask**、**AddWalletItemResult** 类 | [**Windows.applicationmodel.resources.core**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet)命名空间 |
| **Microsoft.Phone.Tasks.BingMapsDirectionsTask**、**BingMapsTask** 类 | 无直接等效项 |
| **Microsoft.Phone.Tasks.CameraCaptureTask** 类 | [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture)类。 此外，[**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 类（仅限 Windows）。 |
| **MarketplaceDetailTask。** | [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp)类（[**RequestAppPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync)方法） |
| **Microsoft.Phone.Tasks.ConnectionSettingsTask**、**MarketplaceHubTask**、**MarketplaceReviewTask**、**MarketplaceSearchTask**、**MediaPlayerLauncher**、**SearchTask**、**SmsComposeTask**、**WebBrowserTask** 类 | [**启动器**](https://docs.microsoft.com/uwp/api/Windows.System.Launcher)类 |
| **Microsoft.Phone.Tasks.EmailComposeTask** 类 | [**EmailMessage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email.EmailMessage)类 |
| **Microsoft.Phone.Tasks.GameInviteTask** 类 | 无直接等效项 |
| **Microsoft.Phone.Tasks.MapDownloaderTask**、**MapsDirectionsTask**、**MapsTask**、**MapUpdaterTask** 类 | 无直接等效项 |
| **Microsoft.Phone.Tasks.PhoneCallTask** 类 | [**PhoneCallManager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls.PhoneCallManager)类 |
| **Microsoft.Phone.Tasks.PhotoChooserTask** 类 | [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)类 |
| **Microsoft.Phone.Tasks.SaveAppointmentTask** 类 | [**AppointmentManager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentManager)类 |
| **Microsoft.Phone.Tasks.SaveContactTask**、**SaveEmailAddressTask**、**SavePhoneNumberTask** 类 | [**StoredContact**](https://docs.microsoft.com/uwp/api/Windows.Phone.PersonalInformation.StoredContact)类（仅 Windows Phone） | 
| **Microsoft.Phone.Tasks.SaveRingtoneTask** 类 | 无直接等效项 |
| **Microsoft.Phone.Tasks.ShareLinkTask**、**ShareMediaTask**、**ShareStatusTask** 类 | [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage)类 |
| 位置 | |
| **System.Device.Location** 命名空间 | [**Windows. 地理位置**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation)命名空间 |
| **System.Device.GeoCoordinateWatcher** 类 | [**定位**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator)类 |
| 地图 | |
| **Microsoft.Phone.Maps** 命名空间 | [**Windows. Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps)命名空间 |
| **Microsoft.Phone.Maps.Controls** 命名空间 | [**WINDOWS UI. .xaml**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps)命名空间 |
| **Microsoft.Phone.Maps.Controls.Map** 类 | [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)类 |
| **Microsoft.Phone.Maps.Services** 命名空间 | [**Windows. Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps)命名空间 |
| **Microsoft.Phone.Maps.Services.GeocodeQuery**、**ReverseGeocodeQuery** 类 | [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)类 |
| **System.Device.Location.GeoCoordinate** 类 | [**Geopoint**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint)类 |
| **Microsoft.Phone.Maps.Services.Route** 类 | [**Maproute.html**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute)类 |
| **Microsoft.Phone.Maps.Services.RouteQuery** 类 | [**MapRouteFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinder)类 |
| 盈利 | |
| **Microsoft.Phone.Marketplace** 命名空间 | [**Windows.applicationmodel.resources.core**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store)命名空间 |
| Media | |
| **Microsoft.Phone.Media** 命名空间 | [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement)类 |
| 网络 | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.DeviceNetworkInformation** 类 | [**Hostname**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName)、 [**system.net.networkinformation**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation)类 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterface** 类 | [**System.net.networkinformation**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation)类 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceInfo** 类 | [**ConnectionProfile**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile)类 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceList** 类 | [**System.net.networkinformation**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation)类 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.SocketExtensions** 类 | 无直接等效项 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.WebRequestExtensions** 类 | 无直接等效项 |
| **Microsoft.Phone.Networking.Voip** 命名空间 | 无直接等效项 |
| **System.Net.CookieCollection** 类 | 仍受支持，但部分属性已丢失（例如 IsReadOnly） |
| **System.Net.DownloadProgressChangedEventArgs** 类，以及与 **System.Net.WebClient** 相关的类似的类 | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)类（或[HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))）。 派生自 [System.Net.Http.StreamContent](https://docs.microsoft.com/previous-versions/visualstudio/hh138119(v=vs.118))，用于测量进度。 |
| **System.Net.DnsEndPoint**、**IPAddress** 类 | 这些类仍受支持，但部分属性已丢失。 或者，移植到 [**HostName**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName) 类。 |
| **System.Net.HttpUtility** 类 | [**HtmlFormatHelper**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.HtmlFormatHelper)类 |
| **System.Net.HttpWebRequest** 类 | 部分支持，但推荐的预期备用项为 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 类（或 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))）。 这些 API 使用 [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) 来表示 HTTP 请求。 |
| **System.Net.HttpWebResponse** 类 | 仍受支持，但使用的是 Dispose() 而不是 Close()。 但是，推荐的预期备用项为 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 类（或 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))）。 这些 API 使用 [System.Net.Http.HttpResponseMessage](https://docs.microsoft.com/dotnet/api/system.net.http.httpresponsemessage) 表示 HTTP 响应。 |
| (SNN = **System.Net.NetworkInformation**) <br/> **SNN.NetworkChange** 类 | 仍受支持，但构造函数除外。 |
| **System.Net.OpenReadCompletedEventArgs** 类，以及与 **System.Net.WebClient** 相关的类似的类 | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)类（或[HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))） |
| **System.Net.Sockets.Socket** 类 | 仍受支持，但使用的是 Dispose() 而不是 Close()。 或者，移植到 [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) 类。 |
| **System.Net.Sockets.SocketException** 类 | 仍受支持，但使用的是 SocketErrorCode 属性而不是 ErrorCode。 |
| **System.Net.Sockets.UdpAnySourceMulticastClient**、**UdpSingleSourceMulticastClient** 类 | [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket)类 |
| **System.Net.UploadProgressChangedEventArgs** 类，以及与 **System.Net.WebClient** 相关的类似的类 | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)类（或[HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))） |
| **System.Net.WebClient** 类 | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)类（或[HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))） |
| **System.Net.WebRequest** 类 | 部分支持（另一组属性），但推荐的预期备用项为 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 类（或 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))）。 这些 API 使用 [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) 来表示 HTTP 请求。 |
| **System.Net.WebResponse** 类 | 仍受支持，但使用的是 Dispose() 而不是 Close()。 但是，推荐的预期备用项为 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 类（或 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))）。 这些 API 使用 [System.Net.Http.HttpResponseMessage](https://docs.microsoft.com/dotnet/api/system.net.http.httpresponsemessage) 表示 HTTP 响应。 |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventArgs** 类 | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)类（或[HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))） |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventHandler** 类 | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)类（或[HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))） |
| **System.UriFormatException** 类 | **System.FormatException** 类 |
| 通知 | |
| MPN = **Microsoft.Phone.Notification** 命名空间 | [**Windows.networking.pushnotifications**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications)命名[**空间（&** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications)u） |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotification** 类 | [**TileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification)类 |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotificationChannel** 类 | [**PushNotificationChannel**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel)类 |
| 编程 | |
| **System** 命名空间 | [**Windows Foundation**](https://docs.microsoft.com/uwp/api/Windows.Foundation)命名空间 |
| **System.Diagnostics.StackFrame**、**StackTrace** 类 | 无直接等效项 |
| **System.Diagnostics** 命名空间 | " [**Windows. 诊断**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Diagnostics)命名空间" |
| **System.ICloneable** 接口 | 可返回相应类型的自定义方法。 |
| **System.Reflection.Emit.ILGenerator** 类 | 无直接等效项 |
| 反应性扩展框架 | |
| **Microsoft.Phone.Reactive** 命名空间 | 无直接等效项 |
| 反射 | |
| **System.Type** 类 | **System.Reflection.TypeInfo** 类。 请参阅[.NET Framework 中适用于 UWP 应用的反射](https://docs.microsoft.com/dotnet/framework/reflection-and-codedom/reflection-for-windows-store-apps)。 |
| 资源 | |
| **System.Resources.ResourceManager** 类 | (WA = **Windows.ApplicationModel**)<br/>[**WA。Resources**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core)和[**WA。资源**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources)命名空间， [**ResourceManager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager)类。 请参阅[在 Windows 运行时应用中创建和检索资源](https://docs.microsoft.com/previous-versions/windows/apps/hh694557(v=vs.140))。 |
| 安全元素 | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementChannel**、**MPS.SecureElementSession** 类 | [**SmartCardConnection**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardConnection)类 |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementReader** 类 | [**SmartCardReader**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardReader)类 |
| 安全 | |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.Aes**、**SSC.RSA** 类 | [**CryptographicEngine**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine)类 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.HMACSHA256**、**SSC.SHA256** 类 | [**HashAlgorithmProvider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider)类 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.ProtectedData** 类 | [**DataProtectionProvider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider)类 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.RandomNumberGenerator** 类 | [**CryptographicBuffer**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.CryptographicBuffer)类 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.X509Certificates.X509Certificate** 类 | [**CertificateEnrollmentManager**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Certificates.CertificateEnrollmentManager)类 |
| 壳体 | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBar** 类 | [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar)类 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarIconButton** 类 | [**AppBarButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)类（在[**PrimaryCommands**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands)属性中使用时） |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarMenuItem** 类 | [**AppBarButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)类（在[**SecondaryCommands**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands)属性中使用时） |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.CycleTileData**、**MPSh.FlipTileData**、**MPSh.IconicTileData**、**MPSh.ShellTileData**、**MPSh.StandardTileData** 类 | [**TileTemplateType**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileTemplateType)类 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.PhoneApplicationService** 类 | [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication)， [**DisplayRequest**](https://docs.microsoft.com/uwp/api/Windows.System.Display.DisplayRequest)类 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ProgressIndicator** 类 | [**StatusBarProgressIndicator**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBarProgressIndicator)类 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTile** 类 | [**SecondaryTile**](https://docs.microsoft.com/uwp/api/Windows.UI.StartScreen.SecondaryTile)类 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTileSchedule** 类 | [**TileUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater)类 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellToast** 类 | [**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager)类 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.SystemTray** 类 | [**状态栏**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBar) |
| 存储和 I/O | |
| **Microsoft.Phone.Storage.ExternalStorage**、**ExternalStorageDevice**、**ExternalStorageFile**、**ExternalStorageFolder** 类 | [**Knownfolders.h**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders)类 |
| **System.IO** 命名空间 | [**Windows storage**](https://docs.microsoft.com/uwp/api/Windows.Storage)、 [**windows. storage**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams)命名空间 |
| **System.IO.Directory** 类 | [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder)类 |
| **System.IO.File** 类 | [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)和[**windows.storage.pathio**](https://docs.microsoft.com/uwp/api/Windows.Storage.PathIO)类
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageFile** 类 |[**ApplicationData. LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder)属性 |
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageSettings** 类 | [**ApplicationData. LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings)属性 |
| **System.IO.Stream** 类 | 仍受支持，但使用的是 ReadAsync() 和 WriteAsync()，而不是 BeginRead()/EndRead() 和 BeginWrite()/EndWrite()。 |
| 电子钱包 | |
| **Microsoft.Phone.Wallet** 命名空间 | [**Windows.applicationmodel.resources.core**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet)命名空间 |
| Xml | |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTime** 方法 |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTimeOffset** 方法 |


下一主题是[移植项目](wpsl-to-uwp-porting-to-a-uwp-project.md)。

