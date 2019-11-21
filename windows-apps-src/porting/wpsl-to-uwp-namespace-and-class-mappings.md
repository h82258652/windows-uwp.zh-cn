---
description: This topic provides a comprehensive mapping of Windows Phone Silverlight APIs to their Universal Windows Platform (UWP) equivalents.
title: Windows Phone Silverlight to UWP namespace and class mappings
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1af36b43e02c9ee08373098f57ba29c10badb6c8
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259069"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>Windows Phone Silverlight to UWP API mappings


This topic provides a comprehensive mapping of Windows Phone Silverlight APIs to their Universal Windows Platform (UWP) equivalents. 但是，通常不存在一对一的功能映射：任一平台都可能比其在命名空间或类中的对应平台具有更多或更少的功能。

The mapping table will help you when you're working in a UWP project and you're re-using source code from a Windows Phone Silverlight project. 这两个平台之间的命名空间和类（包括 UI 控件）的名称存在差异。 在许多情况下，只需更改命名空间名称，就可以编译代码。 有时，类或 API 名称以及命名空间名称已发生更改。 有时，映射会处理较多工作，在极少数情况下，则需要更改方法。

**How to use the table:  ** First, search for the name of the class you're using. 只要进行映射比仅更改命名空间名称更复杂，就会在此处列出类。 如果你的类未列出，则该映射只是命名空间的更改。 因此，找到你的类的命名空间名称后，你将找到等效的 UWP 命名空间名称。 你的类将位于该命名空间中。 如果你的命名空间未列出，则其名称并未更改。

**Note**  Windows 10 supports much more of the .NET Framework than a Windows Phone Store app does. For example, Windows 10 has several System.ServiceModel.\* namespaces as well as System.Net, System.Net.NetworkInformation, and System.Net.Sockets.
Also, in a Windows 10 app, you will benefit from .NET Native, which an ahead-of-time compilation technology that converts MSIL into natively-runnable machine code. .NET Native 应用启动速度更快、使用的内存更少，并且比其对应的 MSIL 更省电。

| Windows Phone Silverlight | Windows 运行时 |
| ------------------------- | --------------- |
| 广告 | |
| **Microsoft.Advertising.Mobile.UI.AdControl** 类 | [AdControl](https://docs.microsoft.com/windows/uwp/monetize/display-ads-using-the-microsoft-advertising-libraries) 类 |
| 警报、提醒和后台代理程序 | |
| **Microsoft.Phone.BackgroundAgent** 类 | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) class |
| **Microsoft.Phone.Scheduler** 命名空间 | [**Windows.ApplicationModel.Background**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background) namespace |
| **Microsoft.Phone.Scheduler.Alarm** 类 | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) and [**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) classes |
| **Microsoft.Phone.Scheduler.PeriodicTask**、**ScheduledAction**、**ScheduledActionService**、**ScheduledTask**、**ScheduledTaskAgent** 类 | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) class |
| **Microsoft.Phone.Scheduler.Reminder** 类 | [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) and [**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) classes |
| **Microsoft.Phone.PictureDecoder** 类 | [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) class |
| **Microsoft.Phone.BackgroundAudio** 命名空间 | [**Windows.Media.Playback**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback) namespace |
| **Microsoft.Phone.BackgroundTransfer** 命名空间 | [**Windows.Networking.BackgroundTransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer) namespace |
| 应用模型和环境 | |
| **System.AppDomain** 类 | 无直接等效项。 请参阅 [**Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application)、[**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication) 类 |
| **System.Environment** 类 | 无直接等效项 |
| **System.ComponentModel.Annotations** 类  | 无直接等效项 |
| **System.ComponentModel.BackgroundWorker** 类 | [**ThreadPool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) class |
| **System.ComponentModel.DesignerProperties** 类 | [**DesignMode**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DesignMode) class |
| **System.Threading.Thread**、**System.Threading.ThreadPool** 类 | [**ThreadPool**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) class |
| (ST = **System.Threading**) <br/> **ST.Thread.MemoryBarrier** 方法 | (ST = **System.Threading**) <br/> **ST.Interlocked.MemoryBarrier** 方法 |
| (ST = **System.Threading**) <br/> **ST.Thread.ManagedThreadId** 属性 | (S = **System**) <br/> **S.Environment.ManagedThreadId** 属性 |
| **System.Threading.Timer** 类 | [**ThreadPoolTimer**](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer) class |
| (SWT = **System.Windows.Threading**) <br/> **SWT.Dispatcher** 类 | [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) class |
| (SWT = **System.Windows.Threading**) <br/> **SWT.DispatcherTimer** 类 | [**DispatcherTimer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DispatcherTimer) class |
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
| **Microsoft.Phone.UserData** 命名空间 | [**Windows.ApplicationModel.Contacts**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts), [**Windows.ApplicationModel.Appointments**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments) namespaces |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Account**、**ContactAddress**、**ContactCompanyInformation**、**ContactEmailAddress**、**ContactPhoneNumber** 类 | [**Contact**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) class |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Appointments** 类 | [**AppointmentCalendar**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentCalendar) class |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Contacts** 类 | [**ContactStore**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactStore) class |
| 控件和 UI 基础结构 | |
| **ControlTiltEffect.TiltEffect** 类 | Windows 运行时动画库中的动画内置于常用控件的默认样式中。 请参阅[动画](wpsl-to-uwp-porting-xaml-and-ui.md)。 |
| **Microsoft.Phone.Controls** 命名空间 | [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) namespace |
| (MPC = **Microsoft.Phone.Controls**) <br/> **MPC.ContextMenu** 类 | [**PopupMenu**](https://docs.microsoft.com/uwp/api/Windows.UI.Popups.PopupMenu) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.DatePickerPage** 类 | [**DatePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.GestureListener** 类 | [**GestureRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.LongListSelector** 类 | [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.ObscuredEventArgs** 类 | [**SystemProtection**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.SystemProtection), [**WindowActivatedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.WindowActivatedEventArgs) classes |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.Panorama** 类 | [**Hub**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationFrame**、<br/>(SWN = **System.Windows.Navigation**) <br/>**SWN.NavigationService** 类 | [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationPage** 类 | [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TiltEffect** 类 | [**PointerDownThemeAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TimePickerPage** 类 | [**TimePickerFlyout**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowser** 类 | [**WebView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) class |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowserExtensions** 类 | 无直接等效项 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WrapPanel** 类 | 无常规布局用途的直接等效项。 [**ItemsWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) and [**WrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) can be used in the items panel template of an items control. |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq** 命名空间 | 无直接等效项 |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq.Mapping** 命名空间 | 无直接等效项 |
| **Microsoft.Phone.Globalization** 命名空间 | 无直接等效项 |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.DeviceExtendedProperties**、**DeviceStatus** 类 | [**EasClientDeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Security.ExchangeActiveSyncProvisioning.EasClientDeviceInformation), [**MemoryManager**](https://docs.microsoft.com/uwp/api/Windows.System.MemoryManager) classes. 有关详细信息，请参阅[设备状态](wpsl-to-uwp-input-and-sensors.md)。 |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.MediaCapabilities** 类 | 无直接等效项 |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.UserExtendedProperties** 类 | [**AdvertisingManager**](https://docs.microsoft.com/uwp/api/Windows.System.UserProfile.AdvertisingManager) class |
| **System.Windows** 命名空间 | [**Windows.UI.Xaml**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml) namespace |
| **System.Windows.Automation** 命名空间 | [**Windows.UI.Xaml.Automation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation) namespace |
| **System.Windows.Controls**、**System.Windows.Input** 命名空间 | [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) namespaces |
| **System.Windows.Controls.DrawingSurface**、**DrawingSurfaceBackgroundGrid** 类 | [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) class |
| **System.Windows.Controls.RichTextBox** 类 | [**RichEditBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) class |
| **System.Windows.Controls.WrapPanel** 类 | 无常规布局用途的直接等效项。 [**ItemsWrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) and [**WrapGrid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) can be used in the items panel template of an items control. |
| **System.Windows.Controls.Primitives** 命名空间 | [**Windows.UI.Xaml.Controls.Primitives**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives) namespace |
| **System.Windows.Controls.Shapes** 命名空间 | [**Windows.UI.Xaml.Controls.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) namespace |
| **System.Windows.Data** 命名空间 | [**Windows.UI.Xaml.Data**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data) namespace |
| **System.Windows.Documents** 命名空间 | [**Windows.UI.Xaml.Documents**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents) namespace |
| **System.Windows.Ink** 命名空间 | 无直接等效项 |
| **System.Windows.Markup** 命名空间 | [**Windows.UI.Xaml.Markup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup) namespace | 
| **System.Windows.Navigation** 命名空间 | [**Windows.UI.Xaml.Navigation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation) namespace |
| **System.Windows.UIElement.Tap** 事件、**EventHandler&lt;GestureEventArgs&gt;** 委托 | [**Tapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped) event, [**TappedEventHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.tappedeventhandler) delegate |
| 数据和服务 |  |
| **System.Data.Linq.DataContext** 类 | 无直接等效项 |
| **System.Data.Linq.Mapping.ColumnAttribute** 类 | 无直接等效项 |
| **System.Data.Linq.SqlClient.SqlHelpers** 类 | 无直接等效项 |
| “设备” | |
| **Microsoft.Devices**、**Microsoft.Devices.Sensors** 命名空间 | [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration), [**Windows.Devices.Enumeration.Pnp**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.Pnp), [**Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input), [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) namespaces |
| **Microsoft.Devices.Camera**、**Microsoft.Devices.PhotoCamera** 类 | [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) class. 此外，[**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 类（仅限 Windows）。 |
| **Microsoft.Devices.CameraButtons** 类 | [**HardwareButtons**](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons) class |
| **Microsoft.Devices.CameraVideoBrushExtensions** 类 | [**CaptureElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) class |
| **Microsoft.Devices.Environment** 类 | 无直接等效项。 使用条件编译并定义自定义符号作为解决方法。 或者你可以使用 [IsAttached](https://docs.microsoft.com/dotnet/api/system.diagnostics.debugger.isattached#System_Diagnostics_Debugger_IsAttached) 属性设计一个解决方法。 |
| **Microsoft.Devices.MediaHistory** 类 | 无直接等效项 |
| **Microsoft.Devices.VibrateController** 类 | [**VibrationDevice**](https://docs.microsoft.com/uwp/api/Windows.Phone.Devices.Notification.VibrationDevice) class |
| **Microsoft.Devices.Radio.FMRadio** 类 | 无直接等效项 |
| **Microsoft.Devices.Sensors.Accelerometer**、**Compass** 类 | 在 [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) 命名空间中 |
| **Microsoft.Devices.Sensors.Gyroscope** 类 | [**Gyrometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Gyrometer) class |
| **Microsoft.Devices.Sensors.Motion** 类 | [**Inclinometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer) class |
| 全球化 | |
| **System.Globalization** 命名空间 | [**Windows.Globalization**](https://docs.microsoft.com/uwp/api/Windows.Globalization) namespace |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentCulture** 属性 | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentCulture** 属性 |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentUICulture** 属性 | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentUICulture** 属性 |
| 图形和动画 | |
| **Microsoft.Xna.Framework.\*** namespaces, [XNA Framework Class Library](https://msdn.microsoft.com/library/bb203940.aspx), [Content Pipeline Class Library](https://msdn.microsoft.com/library/bb195587(v=XNAGameStudio.40).aspx) | 无直接等效项。 通常情况下，结合使用 [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) 和 C++。 请参阅[开发游戏](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10))和 [DirectX 和 XAML 互操作](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10))。 |
| **Microsoft.Xna.Framework.Audio.Microphone** 类 | [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) class |
| **Microsoft.Xna.Framework.Audio.SoundEffect** 类 | [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) class |
| **Microsoft.Xna.Framework.GamerServices** 命名空间 | (WPS = **Windows.Phone.System**) <br/> [**WPS.UserProfile.GameServices.Core**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.UserProfile.GameServices.Core) namespace |
| **Microsoft.Xna.Framework.GamerServices.Guide** 类 | 无直接等效项 |
| **Microsoft.Xna.Framework.Input.GamePad** 类 | [**HardwareButtons**](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons) class |
| **Microsoft.Xna.Framework.Input.Touch.TouchPanel** 类 | [**GestureRecognizer**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer) class |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> **MXFM.MediaLibrary**、**MXFM.PhoneExtensions.MediaLibraryExtensions** 类 | [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) class |
| **Microsoft.Xna.Framework.Media.MediaQueue** 类 | [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls) class |
| **Microsoft.Xna.Framework.Media.Playlist** 类 | [**BackgroundMediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.BackgroundMediaPlayer) class |
| **System.Windows.Media** 命名空间 | [**Windows.UI.Xaml.Media**](/uwp/api/Windows.UI.Xaml.Media) namespace |
| **System.Windows.Media.RadialGradientBrush** 类 | 无直接等效项。 请参阅[媒体和图形](wpsl-to-uwp-porting-xaml-and-ui.md)。 |
| **System.Windows.Media.Animation** 命名空间 | [**Windows.UI.Xaml.Media.Animation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation) namespace |
| **System.Windows.Media.Effects** 命名空间 | 无直接等效项 |
| **System.Windows.Media.Imaging** 命名空间 | [**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging) namespace |
| **System.Windows.Media.Media3D** 命名空间 | [**Windows.UI.Xaml.Media.Media3D**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D) namespace |
| **System.Windows.Shapes** 命名空间 | [**Windows.UI.Xaml.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) namespace |
| 启动器和选择器 | |
| **Microsoft.Phone.Tasks.AddressChooserTask**、**EmailAddressChooserTask**、**PhoneNumberChooserTask** 类 | [**ContactPicker**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactPicker) class |
| **Microsoft.Phone.Tasks.AddWalletItemTask**、**AddWalletItemResult** 类 | [**Windows.ApplicationModel.Wallet**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet) namespace |
| **Microsoft.Phone.Tasks.BingMapsDirectionsTask**、**BingMapsTask** 类 | 无直接等效项 |
| **Microsoft.Phone.Tasks.CameraCaptureTask** 类 | [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) class. 此外，[**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) 类（仅限 Windows）。 |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) class ([**RequestAppPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) method) |
| **Microsoft.Phone.Tasks.ConnectionSettingsTask**、**MarketplaceHubTask**、**MarketplaceReviewTask**、**MarketplaceSearchTask**、**MediaPlayerLauncher**、**SearchTask**、**SmsComposeTask**、**WebBrowserTask** 类 | [**Launcher**](https://docs.microsoft.com/uwp/api/Windows.System.Launcher) class |
| **Microsoft.Phone.Tasks.EmailComposeTask** 类 | [**EmailMessage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email.EmailMessage) class |
| **Microsoft.Phone.Tasks.GameInviteTask** 类 | 无直接等效项 |
| **Microsoft.Phone.Tasks.MapDownloaderTask**、**MapsDirectionsTask**、**MapsTask**、**MapUpdaterTask** 类 | 无直接等效项 |
| **Microsoft.Phone.Tasks.PhoneCallTask** 类 | [**PhoneCallManager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls.PhoneCallManager) class |
| **Microsoft.Phone.Tasks.PhotoChooserTask** 类 | [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) class |
| **Microsoft.Phone.Tasks.SaveAppointmentTask** 类 | [**AppointmentManager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentManager) class |
| **Microsoft.Phone.Tasks.SaveContactTask**、**SaveEmailAddressTask**、**SavePhoneNumberTask** 类 | [**StoredContact**](https://docs.microsoft.com/uwp/api/Windows.Phone.PersonalInformation.StoredContact) class (Windows Phone only) | 
| **Microsoft.Phone.Tasks.SaveRingtoneTask** 类 | 无直接等效项 |
| **Microsoft.Phone.Tasks.ShareLinkTask**、**ShareMediaTask**、**ShareStatusTask** 类 | [**DataPackage**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) class |
| 位置 | |
| **System.Device.Location** 命名空间 | [**Windows.Devices.Geolocation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation) namespace |
| **System.Device.GeoCoordinateWatcher** 类 | [**Geolocator**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) class |
| 地图 | |
| **Microsoft.Phone.Maps** 命名空间 | [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) namespace |
| **Microsoft.Phone.Maps.Controls** 命名空间 | [**Windows.UI.Xaml.Controls.Maps**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps) namespace |
| **Microsoft.Phone.Maps.Controls.Map** 类 | [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) class |
| **Microsoft.Phone.Maps.Services** 命名空间 | [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) namespace |
| **Microsoft.Phone.Maps.Services.GeocodeQuery**、**ReverseGeocodeQuery** 类 | [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) class |
| **System.Device.Location.GeoCoordinate** 类 | [**Geopoint**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint) class |
| **Microsoft.Phone.Maps.Services.Route** 类 | [**MapRoute**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) class |
| **Microsoft.Phone.Maps.Services.RouteQuery** 类 | [**MapRouteFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinder) class |
| 盈利 | |
| **Microsoft.Phone.Marketplace** 命名空间 | [**Windows.ApplicationModel.Store**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) namespace |
| Media | |
| **Microsoft.Phone.Media** 命名空间 | [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) class |
| 网络 | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.DeviceNetworkInformation** 类 | [**Hostname**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName), [**NetworkInformation**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) classes |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterface** 类 | [**NetworkInformation**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) class |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceInfo** 类 | [**ConnectionProfile**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) class |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceList** 类 | [**NetworkInformation**](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) class |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.SocketExtensions** 类 | 无直接等效项 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.WebRequestExtensions** 类 | 无直接等效项 |
| **Microsoft.Phone.Networking.Voip** 命名空间 | 无直接等效项 |
| **System.Net.CookieCollection** 类 | 仍受支持，但部分属性已丢失（例如 IsReadOnly） |
| **System.Net.DownloadProgressChangedEventArgs** 类，以及与 **System.Net.WebClient** 相关的类似的类 | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) class (or [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))). 派生自 [System.Net.Http.StreamContent](https://docs.microsoft.com/previous-versions/visualstudio/hh138119(v=vs.118))，用于测量进度。 |
| **System.Net.DnsEndPoint**、**IPAddress** 类 | 这些类仍受支持，但部分属性已丢失。 或者，移植到 [**HostName**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName) 类。 |
| **System.Net.HttpUtility** 类 | [**HtmlFormatHelper**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.HtmlFormatHelper) class |
| **System.Net.HttpWebRequest** 类 | 部分支持，但推荐的预期备用项为 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 类（或 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))）。 这些 API 使用 [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) 来表示 HTTP 请求。 |
| **System.Net.HttpWebResponse** 类 | 仍受支持，但使用的是 Dispose() 而不是 Close()。 但是，推荐的预期备用项为 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 类（或 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))）。 这些 API 使用 [System.Net.Http.HttpResponseMessage](https://docs.microsoft.com/dotnet/api/system.net.http.httpresponsemessage) 表示 HTTP 响应。 |
| (SNN = **System.Net.NetworkInformation**) <br/> **SNN.NetworkChange** 类 | 仍受支持，但构造函数除外。 |
| **System.Net.OpenReadCompletedEventArgs** 类，以及与 **System.Net.WebClient** 相关的类似的类 | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) class (or [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.Sockets.Socket** 类 | 仍受支持，但使用的是 Dispose() 而不是 Close()。 或者，移植到 [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) 类。 |
| **System.Net.Sockets.SocketException** 类 | 仍受支持，但使用的是 SocketErrorCode 属性而不是 ErrorCode。 |
| **System.Net.Sockets.UdpAnySourceMulticastClient**、**UdpSingleSourceMulticastClient** 类 | [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket) class |
| **System.Net.UploadProgressChangedEventArgs** 类，以及与 **System.Net.WebClient** 相关的类似的类 | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) class (or [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.WebClient** 类 | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) class (or [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.WebRequest** 类 | 部分支持（另一组属性），但推荐的预期备用项为 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 类（或 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))）。 这些 API 使用 [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) 来表示 HTTP 请求。 |
| **System.Net.WebResponse** 类 | 仍受支持，但使用的是 Dispose() 而不是 Close()。 但是，推荐的预期备用项为 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 类（或 [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))）。 这些 API 使用 [System.Net.Http.HttpResponseMessage](https://docs.microsoft.com/dotnet/api/system.net.http.httpresponsemessage) 表示 HTTP 响应。 |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventArgs** 类 | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) class (or [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventHandler** 类 | [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) class (or [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.UriFormatException** 类 | **System.FormatException** 类 |
| 通知 | |
| MPN = **Microsoft.Phone.Notification** 命名空间 | [**Windows.UI.Notifications**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications), [**Windows.Networking.PushNotifications**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications) namespaces |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotification** 类 | [**TileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification) class |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotificationChannel** 类 | [**PushNotificationChannel**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel) class |
| 编程 | |
| **System** 命名空间 | [**Windows.Foundation**](https://docs.microsoft.com/uwp/api/Windows.Foundation) namespace |
| **System.Diagnostics.StackFrame**、**StackTrace** 类 | 无直接等效项 |
| **System.Diagnostics** 命名空间 | [**Windows.Foundation.Diagnostics**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Diagnostics) namespace |
| **System.ICloneable** 接口 | 可返回相应类型的自定义方法。 |
| **System.Reflection.Emit.ILGenerator** 类 | 无直接等效项 |
| 反应性扩展框架 | |
| **Microsoft.Phone.Reactive** 命名空间 | 无直接等效项 |
| 反射 | |
| **System.Type** 类 | **System.Reflection.TypeInfo** 类。 请参阅[.NET Framework 中适用于 UWP 应用的反射](https://docs.microsoft.com/dotnet/framework/reflection-and-codedom/reflection-for-windows-store-apps)。 |
| 资源 | |
| **System.Resources.ResourceManager** 类 | (WA = **Windows.ApplicationModel**)<br/>[**WA.Resources.Core**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core) and [**WA.Resources**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources) namespaces, [**ResourceManager**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) class. 请参阅[在 Windows 运行时应用中创建和检索资源](https://docs.microsoft.com/previous-versions/windows/apps/hh694557(v=vs.140))。 |
| 安全元素 | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementChannel**、**MPS.SecureElementSession** 类 | [**SmartCardConnection**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardConnection) class |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementReader** 类 | [**SmartCardReader**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardReader) class |
| 安全 | |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.Aes**、**SSC.RSA** 类 | [**CryptographicEngine**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) class |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.HMACSHA256**、**SSC.SHA256** 类 | [**HashAlgorithmProvider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider) class |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.ProtectedData** 类 | [**DataProtectionProvider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider) class |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.RandomNumberGenerator** 类 | [**CryptographicBuffer**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.CryptographicBuffer) class |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.X509Certificates.X509Certificate** 类 | [**CertificateEnrollmentManager**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Certificates.CertificateEnrollmentManager) class |
| 壳体 | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBar** 类 | [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarIconButton** 类 | [**AppBarButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) class (when used inside the [**PrimaryCommands**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands) property) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarMenuItem** 类 | [**AppBarButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) class (when used inside the [**SecondaryCommands**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands) property) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.CycleTileData**、**MPSh.FlipTileData**、**MPSh.IconicTileData**、**MPSh.ShellTileData**、**MPSh.StandardTileData** 类 | [**TileTemplateType**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileTemplateType) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.PhoneApplicationService** 类 | [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication), [**DisplayRequest**](https://docs.microsoft.com/uwp/api/Windows.System.Display.DisplayRequest) classes |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ProgressIndicator** 类 | [**StatusBarProgressIndicator**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBarProgressIndicator) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTile** 类 | [**SecondaryTile**](https://docs.microsoft.com/uwp/api/Windows.UI.StartScreen.SecondaryTile) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTileSchedule** 类 | [**TileUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellToast** 类 | [**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.SystemTray** 类 | [**StatusBar**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBar) class |
| 存储和 I/O | |
| **Microsoft.Phone.Storage.ExternalStorage**、**ExternalStorageDevice**、**ExternalStorageFile**、**ExternalStorageFolder** 类 | [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) class |
| **System.IO** 命名空间 | [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage), [**Windows.Storage.Streams**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams) namespaces |
| **System.IO.Directory** 类 | [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) class |
| **System.IO.File** 类 | [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) and [**PathIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.PathIO) classes
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageFile** 类 |[**ApplicationData.LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder) property |
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageSettings** 类 | [**ApplicationData.LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings) property |
| **System.IO.Stream** 类 | 仍受支持，但使用的是 ReadAsync() 和 WriteAsync()，而不是 BeginRead()/EndRead() 和 BeginWrite()/EndWrite()。 |
| 电子钱包 | |
| **Microsoft.Phone.Wallet** 命名空间 | [**Windows.ApplicationModel.Wallet**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet) namespace |
| Xml | |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTime** 方法 |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTimeOffset** 方法 |


下一主题是[移植项目](wpsl-to-uwp-porting-to-a-uwp-project.md)。

