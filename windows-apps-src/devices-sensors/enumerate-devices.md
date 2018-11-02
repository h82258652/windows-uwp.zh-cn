---
author: muhsinking
ms.assetid: 4311D293-94F0-4BBD-A22D-F007382B4DB8
title: 枚举设备
description: 枚举命名空间可以让你找到内部连接到系统的、外部连接的或通过无线或网络协议可检测到的设备。
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: df6082665136442c03273dea4132417b0fd7033c
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5973144"
---
# <a name="enumerate-devices"></a>枚举设备


## <a name="samples"></a>示例

枚举所有可用设备的最简单方法是使用 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx) 命令拍摄快照（会在以下部分进一步介绍）。

```CSharp
async void enumerateSnapshot(){
  DeviceInformationCollection collection = await DeviceInformation.FindAllAsync();
}
```

若要下载演示 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API 的更多高级用法的示例，请单击[此处](http://go.microsoft.com/fwlink/?LinkID=620536)。

## <a name="enumeration-apis"></a>枚举 API

枚举命名空间可以让你找到内部连接到系统的、外部连接的或通过无线或网络协议可检测到的设备。 用于枚举可能的设备而使用的 API 为 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) 命名空间。 使用这些 API 的一些原因包括以下方面。

-   使用你的应用程序查找要连接到的设备。
-   获取连接到系统或易被系统发现的设备的信息。
-   在设备添加、连接、断开连接、更改联机状态或更改其他属性时，通过应用接收通知。
-   在设备连接、断开连接、更改联机状态或更改其他属性时，通过应用接收后台触发器。

假如运行该应用的单个设备和系统支持该项技术，则这些 API 可通过以下任何协议和总线来枚举设备。 这不是详尽列表，并且特定设备可能支持其他协议。

-   以物理方式连接的总线。 这包括 PCI 和 USB。 例如，你可以在“设备管理器”**** 中看到的任何内容。
-   [UPnP](https://msdn.microsoft.com/library/windows/desktop/Aa382303)
-   数字生活网络联盟 (DLNA)
-   [**发现和启动 (DIAL)**](https://msdn.microsoft.com/library/windows/apps/Dn946818)
-   [**DNS 服务发现 (DNS-SD)**](https://msdn.microsoft.com/library/windows/apps/Dn895183)
-   [基于设备的 Web 服务 (WSD)](https://msdn.microsoft.com/library/windows/desktop/Aa826001)
-   [蓝牙](https://msdn.microsoft.com/library/windows/desktop/Aa362932)
-   [**WLAN Direct**](https://msdn.microsoft.com/library/windows/apps/Dn297687)
-   WiGig
-   [**服务点**](https://msdn.microsoft.com/library/windows/apps/Dn298071)

在许多情况下，你都无需担心如何使用枚举 API。 这是因为，使用设备的许多 API 将会自动选择适当的默认设备或提供更简单的枚举 API。 例如，[**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926) 将自动使用默认音频呈现器设备。 只要你的应用可以使用默认设备，就无需在应用程序中使用枚举 API。 枚举 API 提供了一种用于发现和连接到可用设备的常规而灵活的方法。 本主题提供了有关枚举设备的信息并介绍了枚举设备的四种常见方法。

-   使用 [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) UI
-   枚举系统当前可发现的设备快照
-   枚举当前可发现的设备并观察变化
-   枚举当前可发现的设备并观察后台任务中的变化

## <a name="deviceinformation-objects"></a>DeviceInformation 对象


如果使用枚举 API，你将经常需要使用 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 对象。 这些对象包含大多数可用的设备信息。 下表将介绍你感兴趣的一些 **DeviceInformation** 属性。 有关完整列表，请参阅 **DeviceInformation** 引用页。

| 属性                         | 备注                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **DeviceInformation.Id**         | 这是设备的唯一标识符，作为字符串变量提供。 在大多数情况下，这是你将从一种方法传递到另一种方法的模糊值，用于指示你感兴趣的特定设备。 你还可以在关闭应用并重新打开它后使用此属性和 **DeviceInformation.Kind** 属性。 这将确保你可以恢复并重新使用同一 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 对象。 |
| **DeviceInformation.Kind**       | 这指示了使用 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 对象表示的设备对象类型。 这并不是设备类别或设备类型。 单个设备可使用若干个不同的 **DeviceInformation** 对象（不同种类）来表示。 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) 中列出了此属性可能具有的值以及这些值相互关联的方式。                           |
| **DeviceInformation.Properties** | 此属性包包含了为 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 对象而请求的信息。 可以轻松地将最常见的属性引用为 **DeviceInformation** 对象的属性，如 [**DeviceInformation.Name**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.name)。 有关详细信息，请参阅[设备信息属性](device-information-properties.md)。                                                                |

 

## <a name="devicepicker-ui"></a>DevicePicker UI


[**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) 是 Windows 所提供的控件，用于创建使用户能够从列表中选择设备的较小 UI。 你可以使用多种方法自定义 **DevicePicker** 窗口。

-   你可以通过将 [**SupportedDeviceSelectors**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors.aspx) 或 [**SupportedDeviceClasses**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses.aspx) 或两者一起添加到 [**DevicePicker.Filter**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.filter) 来控制在 UI 中显示的设备。 在大多数情况下，你仅需要添加一个选择器或类，但如果确实需要多个，也可以添加多个。 如果你确实添加了多个选择器或类，则将 OR 逻辑函数连接它们。
-   你可以指定要为设备检索的属性。 你可以通过将属性添加到 [**DevicePicker.RequestedProperties**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.requestedproperties) 来完成。
-   你可以使用 [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) 更改 [**Appearance**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.appearance) 的外观。
-   你可以指定 [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) 显示时的大小和位置。

当 [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) 处于显示状态时，如果添加、删除或更新设备，则 UI 内容将自动更新。

**注意**不能指定使用[**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) 。 如果想要拥有特定的 **DeviceInformationKind** 设备，你将需要生成一个 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) 并提供你自己的 UI。

 

强制转换媒体内容和 DIAL 各自还提供其自带的选取器（如果你想要使用它们）。 它们分别是 [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn972525) 和 [**DialDevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn946783)。

## <a name="enumerate-a-snapshot-of-devices"></a>枚举设备快照


在某些情形下，[**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) 将不能满足你的需求，你需要更灵活的东西。 也许你想要生成自己的 UI 或需要枚举设备而无需向用户显示 UI。 在这些情况中，你可以枚举设备快照。 这需要仔细查看当前已连接到系统或已与系统配对的设备。 然而，需要注意的是此方法只是查看可用设备的快照，因此在你枚举列表后，你将无法找到连接的设备。 如果更新或删除了设备，你也将收不到通知。 要注意的另一个可能的缺点是，在完成整个枚举之前，此方法将保留所有结果。 因此，如果你对 **AssociationEndpoint**、**AssociationEndpointContainer** 或 **AssociationEndpointService** 对象感兴趣，则不应使用此方法，因为是通过网络或无线协议才找到了它们。 这最多可能需要 30 秒才能完成。 在这种情况下，你应使用 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) 对象来枚举可能的设备。

若要枚举设备快照，请使用 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx) 方法。 此方法会一直等待直到整个枚举进程完成并且会将所有的结果返回为一个 [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationcollection.aspx) 对象。 此方法还将重载，为你提供多种选项以便进行筛选结果并将其限制到你感兴趣的设备。 你可以通过提供 [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381) 或在设备选择器中传递来执行此操作。 设备选择器是一个 AQS 字符串，可以指定你想要枚举的设备。 有关详细信息，请参阅[生成设备选择器](build-a-device-selector.md)。

提供的设备枚举快照的示例如下所示：



除了限制结果以外，你还可以指定要为设备检索的属性。 如果你执行了此操作，则指定的属性将会在每个 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 对象（返回在集合中）的属性包中可用。 请务必注意并不是所有的属性都可用于所有的设备种类。 要查看哪些属性可用于哪些设备种类，请参阅[设备信息属性](device-information-properties.md)。



## <a name="enumerate-and-watch-devices"></a>枚举并监视设备


一种更强大更灵活的枚举设备的方法就是创建 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446)。 选择此种方法可为你枚举设备提供最大的灵活性。 它不仅可让你枚举当前存在的设备，而且在添加、删除与你的设备选择器相匹配的设备时或在属性发生更改时，还会收到通知。 当创建 **DeviceWatcher** 后，你将会提供一个设备选择器。 有关设备选择器的详细信息，请参阅[生成设备选择器](build-a-device-selector.md)。 创建观察程序后，针对与你提供的标准相匹配的任何设备，你将会收到以下通知。

-   在添加新设备时添加通知。
-   在更改你感兴趣的属性时更新通知。
-   在设备不再可用或不再与你的筛选器相匹配时删除通知。

在使用 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) 的大多数情况下，你是在维护设备列表并在其中添加项目、删除项目，或者在你的观察程序从你监视的设备中收到更新时更新项目。 当你收到更新通知时，更新的信息将会作为 [**DeviceInformationUpdate**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationupdate.aspx) 对象可用。 若要更新你的设备列表，请首先找到已发生更改的相应的 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)。 然后为该对象调用 [**Update**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.update) 方法，从而提供 **DeviceInformationUpdate** 对象。 这是一个便利函数，将自动更新你的 **DeviceInformation** 对象。

由于 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) 会在设备到达以及它们发生更改时发送通知，因此如果你对 **AssociationEndpoint**、**AssociationEndpointContainer** 或 **AssociationEndpointService** 对象感兴趣，应使用此设备枚举方法，即通过网络或无线协议进行枚举。

若要创建 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446)，请使用一种 [**CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createwatcher.aspx) 方法。 这些方法会重载以便让你能够指定你感兴趣的设备。 你可以通过提供 [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381) 或在设备选择器中传递来执行此操作。 设备选择器是一个 AQS 字符串，可以指定你想要枚举的设备。 有关详细信息，请参阅[生成设备选择器](build-a-device-selector.md)。 你还可以指定你想要为设备检索的和感兴趣的属性。 如果你执行了此操作，则指定的属性将会在每个 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 对象（返回在集合中）的属性包中可用。 请务必注意并不是所有的属性都可用于所有的设备种类。 要查看哪些属性可用于哪些设备种类，请参阅[设备信息属性](device-information-properties.md)

## <a name="watch-devices-as-a-background-task"></a>将设备作为后台任务监视


将设备作为后台任务监视与上述的创建 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) 非常类似。 实际上，你仍需要先创建一个常规的 **DeviceWatcher** 对象（如前一部分中所述）。 创建后，你就可以调用 [**GetBackgroundTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.enumerationcompleted.aspx) 而不是 [**DeviceWatcher.Start**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.start)。 如果你调用 **GetBackgroundTrigger**，则必须指定哪些是你感兴趣的通知：添加、删除或更新。 如果不请求添加，你也无法请求更新或删除。 一旦你注册了触发器，**DeviceWatcher** 将立刻开始在后台运行。 从此刻开始，只要收到匹配条件的应用程序新通知，就会触发后台任务并向你提供自其上次触发应用程序以来最新的更改。

**重要** [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838)触发你的应用程序的第一次将观察程序一时间**EnumerationCompleted**状态。 这意味着它将包含所有的初始结果。 不管将来何时触发你的应用程序，它将仅包含自上次触发就已出现的添加、更新和删除通知。 这与前台的 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) 对象略有不同，因为初始结果并不是一个个地依次而来，而是在达到 **EnumerationCompleted** 后以捆绑的形式传送。

 

对于某些无线协议，如果它们正在进行后台和前台扫描，或它们根本就不支持后台扫描，则这些无线协议就会有不同的行为。 有三种与后台扫描相关的可能性。 下表列出了这些可能性以及这可能会对你的应用程序产生的影响。 例如，蓝牙和 Wi-Fi Direct 不支持后台扫描，即便通过扩展，它们也不支持 [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838)。

| 行为                                  | 影响                                                                                                                                  |
|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| 后台中相同的行为               | 无                                                                                                                                    |
| 后台中可能只有被动扫描 | 等待出现被动扫描的同时，可能需要更长的时间来发现设备。                                                           |
| 不支持后台扫描            | [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838) 将不会检测到任何设备，也不会报告任何更新。 |

 

如果你的 [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838) 包含了不支持作为后台任务进行扫描的协议，则你的触发器仍会起作用。 不过，你将不能通过该协议来获取任何更新或结果。 正常情况下，仍可检测到其他协议或设备的更新。

## <a name="using-deviceinformationkind"></a>使用 DeviceInformationKind


在大多数情况下，你无需担心 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) 对象的 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)。 这是因为你正使用的设备 API 所返回的设备选择器将会保证你能够获取设备对象的正确类型以便与其 API 结合使用。 然而，在某些情况下，你将需要获取设备的 **DeviceInformation**，但是没有相应的设备 API 可以提供设备选择器。 在这些情况下，你将需要生成自己的选择器。 例如，设备上的 Web 服务没有专用的 API，不过你可以通过使用 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) API 来发现这些设备以及获取它们的相关信息，并且通过使用套接字 API 来使用这些设备。

如果你要生成自己的设备选择器来枚举设备对象，请务必要了解 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx)。 在 **DeviceInformationKind** 的引用页面中描述了所有可能的类型及其相互关联的方式。 使用 **DeviceInformationKind** 的最常见的一种情形是在将查询连同设备选择器一起提交时指定你要搜索的设备类型。 通过执行此操作，可确保你仅枚举与提供的 **DeviceInformationKind** 匹配的设备。 例如，你可以查找 **DeviceInterface** 对象，然后运行查询以获取父 **Device** 对象的信息。 该父对象可能包含其他信息。

请务必注意，属性包中提供的 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 对象的属性因设备的 [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) 而异。 某些属性仅可用于某些种类。 有关哪些属性适用于哪些种类的详细信息，请参阅[设备信息属性](device-information-properties.md)。 因此，在以上示例中，搜索父 **Device** 将会使你能够获取 **DeviceInterface** 设备对象中未提供的详细信息。 正因为如此，当你创建 AQS 筛选器字符串时，请务必确保所请求的属性可用于你要枚举的 **DeviceInformationKind** 对象。 有关生成筛选器的详细信息，请参阅[生成设备选择器](build-a-device-selector.md)。

在枚举 **AssociationEndpoint**、**AssociationEndpointContainer** 或 **AssociationEndpointService** 对象时，将通过无线或网络协议进行枚举。 在这些情况下，我们建议你不要使用 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx) 而改用 [**CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createwatcher.aspx)。 这是因为在网络上进行搜索通常会导致在生成 [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.enumerationcompleted.aspx) 之前，搜索操作超时不会超过 10 秒或以上。 直到触发 **EnumerationCompleted**，**FindAllAsync** 才会完成其操作。 如果你在使用 [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446)，则无论何时调用 **EnumerationCompleted**，你都将获得更接近实际时间的结果。

## <a name="save-a-device-for-later-use"></a>保存设备供以后使用


任何 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 对象都可由两条信息的组合来唯一地标识：[**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id) 和 [**DeviceInformation.Kind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.kind.aspx)。 如果保留了这两条信息，可以在 **DeviceInformation** 对象丢失之后，通过向 [**CreateFromIdAsync**](https://msdn.microsoft.com/library/windows/apps/br225425.aspx) 提供此信息以重新创建它。 如果执行此操作，则可以保存与你的应用集成的设备的用户首选项。


 

 




