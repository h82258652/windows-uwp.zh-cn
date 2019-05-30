---
ms.assetid: D20C8E01-4E78-4115-A2E8-07BB3E67DDDC
description: 本文介绍如何访问和使用设备灯（如果存在）。 灯功能通过设备的相机和相机闪光灯功能单独管理。
title: 独立于相机的手电筒
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ddc7ad87a883c3512c719167975428b6c7746031
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359060"
---
# <a name="camera-independent-flashlight"></a>独立于相机的手电筒



本文介绍如何访问和使用设备灯（如果存在）。 灯功能通过设备的相机和相机闪光灯功能单独管理。 除了获取对灯的引用和调整其设置以外，本文还介绍了在不使用灯资源时如何正确地释放灯资源，以及在灯正由另一个应用使用时，如何检测灯的可用性更改。

## <a name="get-the-devices-default-lamp"></a>获取设备的默认灯

若要获取设备的默认灯设备，请调用 [**Lamp.GetDefaultAsync**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.getdefaultasync)。 灯 API 位于 [**Windows.Devices.Lights**](https://docs.microsoft.com/uwp/api/Windows.Devices.Lights) 命名空间中。 在尝试访问这些 API 之前，请务必为此命名空间添加一个 using 指令。

[!code-cs[LightsNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetLightsNamespace)]


[!code-cs[DeclareLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDeclareLamp)]


[!code-cs[GetDefaultLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetDefaultLamp)]

如果返回的对象是 **null**，则该设备不支持 **Lamp** API。 即使某些设备上实际存在灯，它们也可能不支持 **Lamp** API。

## <a name="get-a-specific-lamp-using-the-lamp-selector-string"></a>使用灯选择器字符串获取特定灯

某些设备可以具有多个灯。 若要获取设备上可用灯的列表，请通过调用 [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.getdeviceselector) 获取设备选择器字符串。 然后，此选择器字符串可以传递到 [**DeviceInformation.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 中。 此方法用于枚举多种不同类型的设备，而选择器字符串可指示该方法仅返回灯设备。 从 **FindAllAsync** 返回的 [**DeviceInformationCollection**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection) 对象是表示设备上的可用灯的 [**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 对象的集合。 选择列表中的某个对象，然后将 [**Id**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 属性传递给 [**Lamp.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.fromidasync)，以获取对请求的灯的引用。 此示例使用 **System.Linq** 命名空间中的 **GetFirstOrDefault** 扩展方法选择 **DeviceInformation** 对象，其中，[**EnclosureLocation.Panel**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.enclosurelocation.panel) 属性的值为 **Back**，可选择位于设备外壳背面的灯（如果存在）。

请注意，[**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation) API 位于 [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) 命名空间中。

[!code-cs[EnumerationNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

[!code-cs[GetLampWithSelectionString](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetLampWithSelectionString)]

## <a name="adjust-lamp-settings"></a>调整灯设置

拥有 [**Lamp**](https://docs.microsoft.com/uwp/api/Windows.Devices.Lights.Lamp) 类的实例后，请通过将 [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.isenabled) 属性设置为 **true** 来打开灯。

[!code-cs[LampSettingsOn](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOn)]

通过将 [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.isenabled) 属性设置为 **false** 来关闭灯。

[!code-cs[LampSettingsOff](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOff)]

某些设备配备支持颜色值的灯。 通过检查 [**IsColorSettable**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.iscolorsettable) 属性来检查灯是否支持颜色。 如果此值为 **true**，你可以使用 [**Color**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.color) 属性设置灯的颜色。

[!code-cs[LampSettingsColor](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsColor)]

## <a name="register-to-be-notified-if-the-lamp-availability-changes"></a>注册，以在灯可用性发生更改时获得通知

灯访问权限将授予最近请求访问的应用。 因此，如果另一个应用已启动并请求使用你的应用当前在使用的灯资源，则在其他应用释放资源之前，你的应用将不再能够控制灯。 若要在灯的可用性发生更改时收到通知，请注册 [**Lamp.AvailabilityChanged**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.availabilitychanged) 事件的处理程序。

[!code-cs[AvailabilityChanged](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChanged)]

在该事件的处理程序中，检查 [**LampAvailabilityChanged.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lampavailabilitychangedeventargs.isavailable) 属性以确定灯是否可用。 此示例根据灯的可用性确定是否启用用于打开或关闭灯的切换开关。

[!code-cs[AvailabilityChangedHandler](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChangedHandler)]

## <a name="properly-dispose-of-the-lamp-resource-when-not-in-use"></a>适当地处理不处于使用状态的灯资源

当你不再使用灯时，你应该将其禁用，然后调用 [**Lamp.Close**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.close) 来释放该资源并允许其他应用访问该灯。 如果你使用的是 C#，此属性将映射到 **Dispose** 方法。 如果你已注册 [**AvailabilityChanged**](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp.availabilitychanged)，则应当在处理灯资源时注销该处理程序。 代码中用于处理灯资源的正确位置取决于你的应用。 若要将灯访问限制到单个页面，请在 [**OnNavigatingFrom**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatingfrom) 事件中释放该资源。

[!code-cs[DisposeLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDisposeLamp)]

## <a name="related-topics"></a>相关主题
- [媒体播放](media-playback.md)

 




