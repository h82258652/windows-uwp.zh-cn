---
ms.assetid: 25B18BA5-E584-4537-9F19-BB2C8C52DFE1
title: 应用功能声明
description: 功能必须在你的通用 Windows 平台 (UWP) 应用的程序包清单中声明，以便可用于访问某些 API 或资源（如图片、音乐）或者设备（如相机或麦克风）。
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: cc613177f7cdd57691fa261519127e8bbf03e3ad
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319892"
---
# <a name="app-capability-declarations"></a>应用功能声明

功能必须在你的通用 Windows 平台 (UWP) 应用的[程序包清单](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)中声明，以便可用于访问某些 API 或资源（如图片、音乐）或者设备（如相机或麦克风）。

可以通过在应用的[程序包清单](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)中声明功能来请求对特定资源或 API 的访问权限。 可以使用 Microsoft Visual Studio 中的[清单设计器](packaging-uwp-apps.md#configure-an-app-package)来声明常用功能，也可以手动添加它们。 有关详细信息，请参阅[如何在程序包清单中指定功能](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-capabilities-in-a-package-manifest)。 请务必了解，当客户从应用商店获取你的应用时，会向它们告知该应用声明的所有功能。 请避免声明你的应用不需要的功能。

某些功能为应用提供对*敏感资源*的访问权限。 由于这些资源可用于访问用户私人数据或花费用户金钱，因此它们被认为是敏感资源。 由“设置”应用管理的隐私设置允许用户动态控制对敏感资源的访问权限。 因此，你的应用不会假设敏感资源始终可用，这一点很重要。 有关访问敏感资源的详细信息，请参阅[隐私感知应用指南](https://docs.microsoft.com/windows/uwp/security/index)。 向应用提供访问权限的功能*敏感资源*星号进行批注 (\*) 旁边的功能方案。

有几种类型的功能。

- [常规使用功能](#general-use-capabilities)，这适用于最常见的应用方案。
- [设备功能](#device-capabilities)，它允许您的应用程序以访问外围和内部的设备。
- [受限制的功能](#restricted-capabilities)，这需要批准的 Microsoft Store 提交和/或通常是仅供 Microsoft 和某些合作伙伴。
- [自定义功能](#custom-capabilities)。

## <a name="general-use-capabilities"></a>常用功能

常用功能适用于最常见的应用场景。

| 功能应用场景 | 功能用法 |
|---------------------|------------------|
| **音乐**\* | **musicLibrary** 功能提供对用户音乐的编程访问权限，让应用无需用户交互即可枚举和访问库中的所有文件。 此功能通常用在使用整个音乐库的自动唱片点唱机应用中。<br /><br />[  **文件选取器**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)提供了一种强大的 UI 机制，让用户可以打开要通过某个应用处理的文件。 仅当应用需要进行编程访问，而使用**文件选取器**无法实现编程访问时，才声明 **musicLibrary** 功能。<br /><br /> 当在应用的程序包清单中声明 **musicLibrary** 功能时，该功能必须包含 **uap** 命名空间，如下所示。 <br /><br />```<Capabilities><uap:Capability Name="musicLibrary"/></Capabilities>```
| **图片**\* | **picturesLibrary** 功能提供对用户图片的编程访问权限，让应用无需用户交互即可枚举和访问库中的所有文件。 在使用整个图片库的照片应用中，通常会使用此功能。<br /><br />[  **文件选取器**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)提供了一种强大的 UI 机制，让用户可以打开要通过某个应用处理的文件。 仅当应用需要进行编程访问，而使用**文件选取器**无法实现编程访问时，才声明 **picturesLibrary** 功能。<br /><br />当在应用的程序包清单中声明 **picturesLibrary** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<br /><br />```<Capabilities><uap:Capability Name="picturesLibrary"/></Capabilities>```
| **视频**\* | **videosLibrary** 功能提供对用户视频的编程访问权限，让应用无需用户交互即可枚举和访问库中的所有文件。 此功能通常用在使用整个视频库的电影播放应用中。<br /><br />[  **文件选取器**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)提供了一种强大的 UI 机制，让用户可以打开要通过某个应用处理的文件。 仅当应用需要进行编程访问，而使用**文件选取器**无法实现编程访问时，才声明 **videosLibrary** 功能。<br /><br />当在应用的程序包清单中声明 **videosLibrary** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<br /><br />```<Capabilities><uap:Capability Name="videosLibrary"/></Capabilities>```
| **可移动存储** | **removableStorage** 功能提供对可移动存储（如 U 盘和外部硬盘驱动器）中文件的编程访问权限，这些文件将按照程序包清单中声明的文件类型关联进行筛选。 例如，如果某个文档阅读器应用声明了 .doc 文件类型关联，则它可以打开可移动存储设备中的 .doc 文件，但无法打开其他类型的文件。 声明此功能时请务必小心，因为用户的可移动存储设备中可能包含多种信息，他们将期待你的应用提供有效的理由，以便对该可移动存储中属于已声明类型的所有文件进行编程访问。<br /><br />用户将期待你的应用处理你所声明的所有文件关联。 因此，请勿声明你的应用无法可靠处理的文件关联。 [  **文件选取器**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)提供了一种强大的 UI 机制，让用户可以打开要通过某个应用处理的文件。<br /><br />仅当应用需要进行编程访问，而使用[**文件选取器**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)无法实现编程访问时，才声明 **removableStorage** 功能。<br /><br />当在应用的程序包清单中声明 **removableStorage** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<br /><br />```<Capabilities><uap:Capability Name="removableStorage"/></Capabilities>```
| **Internet 和公共网络**\* | 存在两种提供对 Internet 和公共网络的不同级别访问权限的功能。<br /><br />**internetClient** 功能指示应用可以接收从 Internet 传入的数据。 无法充当服务器。 无法访问本地网络。<br />**internetClientServer** 功能指示应用可以接收从 Internet 传入的数据。 可以充当服务器。 无法访问本地网络。<br /><br />具有 Web 服务组件的大多数应用都将使用 **internetClient**。 如果应用支持需要在其中侦听传入的网络连接的对等 (P2P) 方案，则应该使用 **internetClientServer**。 **internetClientServer** 功能包括 **internetClient** 功能所提供的访问权限，因此，在指定 **internetClientServer** 时无需指定 **internetClient**。
| **家庭和工作网络**\* | **privateNetworkClientServer** 功能提供通过防火墙对家庭和工作网络的入站和出站访问权限。 此功能通常用于通过局域网 (LAN) 通信的游戏和在各种本地设备上共享数据的应用。 如果你的应用指定了 **musicLibrary**、**picturesLibrary** 或 **videosLibrary**，则无需使用此功能即可访问家庭组中相应的库。 在 Windows 上，此功能不提供对 Internet 的访问权限。
| **约会** | **appointments** 功能提供对用户的约会存储的访问权限。 此功能允许读取对从同步的网络帐户获得的约会以及写入约会存储的其他应用的访问权限。 通过此功能，你的应用可以创建新的日历并向所创建的日历写入约会。<br /><br />当在应用的程序包清单中声明 **appointments** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<br /><br />```<Capabilities><uap:Capability Name="appointments"/></Capabilities>```
| **联系人**\* | **contacts** 功能提供了对各种联系人存储中的联系人聚合视图的访问权限。 此功能向已在各种网络和本地联系人存储中同步的联系人提供了对该应用的受限访问权限（应用网络许可规则）。<br /><br />当在应用的程序包清单中声明 **contacts** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<br /><br />```<Capabilities><uap:Capability Name="contacts"/></Capabilities>```
| **代码生成** | **codeGeneration** 功能允许应用访问以下功能，可向应用提供 JIT 功能。<br /><br />[**VirtualProtectFromApp**](https://docs.microsoft.com/windows/desktop/api/memoryapi/nf-memoryapi-virtualprotectfromapp)<br />[**CreateFileMappingFromApp**](https://docs.microsoft.com/windows/desktop/api/memoryapi/nf-memoryapi-createfilemappingfromapp)<br />[**OpenFileMappingFromApp**](https://docs.microsoft.com/windows/desktop/api/memoryapi/nf-memoryapi-openfilemappingfromapp)<br />[**MapViewOfFileFromApp**](https://docs.microsoft.com/windows/desktop/api/memoryapi/nf-memoryapi-mapviewoffilefromapp)
| **AllJoyn** | **allJoyn** 功能允许网络上支持 AllJoyn 的应用和设备发现彼此并进行交互。<br /><br />在 [**Windows.Devices.AllJoyn**](https://docs.microsoft.com/uwp/api/Windows.Devices.AllJoyn) 命名空间中访问 API 的所有应用都必须使用此功能。
| **电话呼叫** | **phoneCall** 功能允许应用访问设备上的所有电话线并执行以下功能。<ul><li>通过电话线发出呼叫并显示系统拨号器，而不提示用户。</li><li>访问与线相关的元数据。</li><li>访问与线相关的触发器。</li><li>允许用户选择的垃圾邮件筛选器应用设置并检查阻止列表和呼叫来源信息。</li></ul>当在应用的程序包清单中声明 **phoneCall** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<br /><br />```<Capabilities><uap:Capability Name="phoneCall"/></Capabilities>```<br /><br /><b>PhoneCallHistoryPublic</b>功能允许应用读取移动电话和一些 VoIP 呼叫历史记录信息在设备上的。 此功能还允许应用编写 VoIP 呼叫历史记录条目。 若要访问 <a href="https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls.PhoneCallHistoryStore"><b>PhoneCallHistoryStore</b></a> 类的所有成员，则需要此功能。
| **录制的通话文件夹**\* | **recordedCallsFolder** 设备功能允许应用访问记录的通话文件夹。<br /><br />当在应用的程序包清单中声明 **recordedCallsFolder** 功能时，该功能必须包含 **mobile** 命名空间，如下所示。<br /><br />```<Capabilities><mobile:Capability Name="recordedCallsFolder"/></Capabilities>```
| **用户帐户信息**\* | **userAccountInformation** 功能使应用能够访问用户的名称和头像。<br /><br />若要访问 [**Windows.System.UserProfile**](https://docs.microsoft.com/uwp/api/Windows.System.UserProfile) 命名空间中的某些 API，此功能是必需的。<br /><br />当在应用的程序包清单中声明 **userAccountInformation** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<br /><br />```<Capabilities><uap:Capability Name="userAccountInformation"/></Capabilities>```
| **VoIP 呼叫** | **VoipCall**功能允许应用访问 Api 调用中的 VoIP [ **Windows.ApplicationModel.Calls** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls)命名空间。<br /><br />当在应用的程序包清单中声明 **voipCall** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<br /><br />```<Capabilities><uap:Capability Name="voipCall"/></Capabilities>```
| **三维对象** | **objects3D** 功能允许应用编程访问 3D 对象文件。 此功能通常用在需要访问整个 3D 对象库的 3D 应用和游戏中。<br /><br />若要使用 [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) 命令空间中的 API 访问包含 3D 对象的文件夹，则需要此功能。<br /><br />当在应用的程序包清单中声明 **objects3D** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<br /><br />```<Capabilities><uap:Capability Name="objects3D"/></Capabilities>```
| **读取阻止的消息**\* | **blockedChatMessages** 功能允许应用读取已由“垃圾邮件筛选器”应用阻止的短信和彩信。<br /><br />若要使用 [**Windows.ApplicationModel.Chat**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Chat) 命令空间中的 API 访问已阻止的消息，则需要此功能。<br /><br />当在应用的程序包清单中声明 **blockedChatMessages** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<br /><br />```<Capabilities><uap:Capability Name="blockedChatMessages"/></Capabilities>```
| **自定义设备** | **LowLevelDevices**功能允许应用访问自定义设备时满足其他要求的数量。 不要将此功能与相混淆**lowLevel**允许 GPIO、 I2C、 SPI 和 PWM 设备的访问权限的设备功能。<br /><br /> 如果您开发的自定义驱动程序，公开[设备接口](https://docs.microsoft.com/windows-hardware/drivers/install/device-interface-classes)和你想要打开此设备的句柄并发送 Ioctl，则必须： <ul><li>启用**lowLevelDevices**应用程序清单中的功能： ```<Capabilities><iot:Capability Name="lowLevelDevices"/></Capabilities>```</li><li>启用[嵌入的模式](https://docs.microsoft.com/windows/iot-core/develop-your-app/EmbeddedMode)</li><li>将标记作为设备接口[受限](https://docs.microsoft.com/windows-hardware/drivers/install/devpkey-deviceinterface-restricted)，而是在你[INF](https://docs.microsoft.com/previous-versions/windows/desktop/deviceaccess/register-the-device-interface-class-as-privileged)或通过调用[WdfDeviceAssignInterfaceProperty()](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdevice/nf-wdfdevice-wdfdeviceassigninterfaceproperty)驱动程序中。</ul>然后，可以使用[ **Windows.Devices.Custom.CustomDevice** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Custom.CustomDevice)打开到设备的句柄。 有关详细信息，请参阅[UWP 设备应用程序的内部设备](https://docs.microsoft.com/windows-hardware/drivers/devapps/uwp-device-apps-for-specialized-devices)。
| **IoT 系统管理** | **systemManagement** 功能允许应用具有基本的系统管理权限，例如关机或重启、区域设置和时区。<br /><br />若要访问 [**Windows.System**](https://docs.microsoft.com/uwp/api/Windows.System) 命名空间中的某些 API，则需要此功能。<br /><br />当在应用的程序包清单中声明 **systemManagement** 功能时，该功能必须包含 **iot** 命名空间，如下所示。<br /><br />```<Capabilities><iot:Capability Name="systemManagement"/></Capabilities>```
| **背景媒体播放** | **backgroundMediaPlayback** 功能更改了特定于媒体的 API（如 [**MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer) 和 [**AudioGraph**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph) 类）的行为，支持应用在后台时的媒体播放。 所有活动音频流将不再静音，在应用转换到后台时将继续发声。 此外，播放进行时，应用生存时间将自动延长。
| **远程系统** | **remoteSystem** 功能使应用有权访问与用户的 Microsoft 帐户关联的设备列表。 对于执行在设备间持续的任何操作，需要访问设备列表。 若要访问以下命名空间和方法中的所有成员，此功能是必需的。<ul><li>[Windows.System.RemoteSystems](https://docs.microsoft.com/uwp/api/windows.system.remotesystems)命名空间</li><li>[Windows.System.RemoteLauncher](https://docs.microsoft.com/uwp/api/Windows.System.RemoteLauncher)类</li><li>[AppServiceConnection.OpenRemoteAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection.openremoteasync)方法</li></ul> |
| **空间感知** | **spatialPerception** 功能支持以编程方式访问空间映射数据，为混合现实应用提供有关用户附近空间中应用程序指定区域的表面信息。  请仅在你的应用明确会使用这些表面网格时再声明 spatialPerception 功能，因为混合现实应用在基于用户的头部姿势执行全息呈现时不需要此功能。 |

## <a name="device-capabilities"></a>设备功能

设备功能允许你的应用访问外围设备和内部设备。 可在你的应用包清单中使用 **DeviceCapability** 元素指定设备功能。 此元素可能需要其他子元素，并且某些设备功能需要手动添加到该程序包清单。 有关详细信息，请参阅[如何在程序包清单中指定设备功能](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-in-a-package-manifest)和 [**DeviceCapability Schema reference**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability)。

> [!NOTE]
> 可以有多个**DeviceCapability**并**功能**下的元素**功能**元素，但是所有**DeviceCapability**元素必须出现后**功能**元素。

| 功能应用场景 | 功能用法 |
|---------------------|------------------|
| **位置**\* | **location** 功能提供对定位功能的访问权限，定位功能通常从专用硬件（如电脑中的 GPS 传感器）检索获得或从可用的网络信息中获得。 应用必须处理用户从“设置”  超级按钮禁用定位服务的情况。 |
| **Microphone** | **microphone** 功能提供对麦克风音频源的访问权限，让应用可以录制来自所连接麦克风的音频。 应用必须处理用户从“设置”  超级按钮禁用麦克风的情况。 |
| **邻近** | **proximity** 功能支持临近的多台设备彼此通信。 此功能通常用在一般的多玩家游戏和交换信息的应用中。 设备会尝试使用可提供最佳连接的通信技术，包括蓝牙、Wi-Fi 和 Internet。 此功能仅用于在设备之间发起通信。 |
| **Webcam** | **webcam** 功能提供对内置相机或外部摄像头的视频源的访问权限，这使应用可以捕获照片和视频。 在 Windows 上，应用必须处理用户从“设置”  超级按钮禁用相机的情况。<br/>**webcam** 功能仅授予对视频流的访问权限。 若要也授予对音频流的访问权限，必须添加 **microphone** 功能。 |
| **USB** | **usb** 设备功能允许访问[为 USB 设备更新应用清单程序包](https://go.microsoft.com/fwlink/p/?LinkId=302259)中的 API。 |
| **人机接口设备 (HID)** | **humaninterfacedevice** 设备功能允许访问[如何为 HID 指定设备功能](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-for-hid)中的 API。 |
| **Point of Service (POS)** | **pointOfService** 设备功能允许访问 [**Windows.Devices.PointOfService**](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService) 命名空间中的 API。 该命名空间允许你的应用访问服务点 (POS) 条码扫描仪和磁条阅读器。 该命名空间提供一个独立于供应商的接口，可用于从 UWP 应用访问由各种制造商提供的 POS 设备。 |
| **蓝牙** | **bluetooth** 设备功能允许应用通过通用属性 (GATT) 或经典基本速率 (RFCOMM) 协议与已经配对的蓝牙设备进行通信。<br/>若要使用 [**Windows.Devices.Bluetooth**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth) 命名空间中的某些 API，则需要此功能。 |
| **Wi-fi 网络** | **wiFiControl** 设备功能允许应用扫描并连接到 WLAN 网络。<br/>若要使用 [**Windows.Devices.WiFi**](https://docs.microsoft.com/uwp/api/Windows.Devices.WiFi) 命名空间中的某些 API，则需要此功能。 |
| **单选状态** | **radios** 设备功能允许应用切换 WLAN 和蓝牙无线电收发器。<br/>若要使用 [**Windows.Devices.Radios**](https://docs.microsoft.com/uwp/api/Windows.Devices.Radios) 命名空间中的 API，则需要此功能。  |
| **光盘** | **optical** 设备功能允许应用访问光盘驱动器（如 CD、DVD 和蓝光光盘）上的功能。<br/>若要使用 [**Windows.Devices.Custom**](https://docs.microsoft.com/uwp/api/Windows.Devices.Custom) 命名空间中的某些 API，则需要此功能。 |
| **动态活动** | **activity** 设备功能允许应用检测设备的当前运动。<br/>若要使用 [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) 命名空间中的某些 API，则需要此功能。 |
| **串行通信** | **serialcommunication** 设备功能提供了对于 Windows.Devices.SerialCommunication 命名空间中的 API 的访问，这样 Windows 应用便可与公开了串行端口或串行端口的某些抽象的设备进行通信。 若要使用 [**Windows.Devices.SerialCommnication**](https://docs.microsoft.com/uwp/api/windows.devices.serialcommunication) 命名空间中的 API，则需要该功能。 |
| **关注跟踪器** | 在连接了兼容的目视跟踪设备时，**gazeInput** 功能允许应用在应用程序边界内检测用户正在查看的位置。 此功能需要使用中的某些 Api [ **Windows.Devices.Input.Preview** ](https://docs.microsoft.com/en-us/uwp/api/windows.devices.input.preview)命名空间。 |
| **GPIO、 I2C、 SPI 和 PWM** | **LowLevel**设备功能提供对 GPIO、 I2C、 SPI 和 PWM 设备访问。 此功能需要使用以下命名空间中的 Api:[**Windows.Devices.Gpio**](https://docs.microsoft.com/uwp/api/windows.devices.gpio)， [ **Windows.Devices.I2c**](https://docs.microsoft.com/uwp/api/windows.devices.i2c)， [ **Windows.Devices.Spi**](https://docs.microsoft.com/uwp/api/windows.devices.spi)，[**Windows.Devices.Pwm**](https://docs.microsoft.com/uwp/api/windows.devices.pwm)。<br /><br />```<Capabilities><DeviceCapability Name="lowLevel"/></Capabilities>``` |

<span id="special-and-restricted-capabilities" />

## <a name="restricted-capabilities"></a>受限功能

如果您的应用程序声明了任何受限制的功能，则必须提供信息期间[应用程序提交过程](../publish/app-submissions.md)获得批准将发布到 Microsoft Store 应用所需。 上提供此信息[提交选项](../publish/manage-submission-options.md#restricted-capabilities)你的提交内容，说明您的应用程序如何使用每个页限制它声明的功能。

> [!IMPORTANT]
> 受限制的功能适用于非常特定的方案。 这些功能的使用受严格限制，且受应用商店的其他上架政策和评审的约束。 请注意，可以声明受限制的功能而无需接收任何审批的旁加载应用程序。 只有在将此类应用提交到 Microsoft Store 时才需要获得批准。

请确保未声明这些受到限制功能，除非您的应用程序真正需要的时候。 在某些情况下，这些功能是必需的或适宜的，如在使用双因素身份验证开展银行业务时，用户提供带数字签名的智能卡确认其身份。 其他应用可能主要针对企业客户而设计，并可能需要访问一些必须使用用户的域凭据才能访问的企业资源。

若要声明功能受到限制，请修改你[应用程序包清单](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)源文件 (`Package.appxmanifest`)。 添加**xmlns:rescap** XML 命名空间声明，并用**rescap**前缀声明受限的功能时。 例如，下面演示了如何声明 **appCaptureSettings** 功能。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package
    ...
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="... rescap">
...
<Capabilities>
    <rescap:Capability Name="appCaptureSettings"/>
</Capabilities>
</Package>
```

### <a name="restricted-capability-approval-process"></a>受限功能审核流程

以前，你需要联系支持部门来获得使用某项功能的批准。 我们现在允许你提供此信息在[合作伙伴中心](https://partner.microsoft.com/dashboard)作为的一部分[提交过程](../publish/app-submissions.md)。

当将包上载您的提交时，我们将检测是否声明任何受限制的功能。 如果我们检测到声明了此类功能，则你需要在[提交选项](../publish/manage-submission-options.md#restricted-capabilities)页面上提供有关你的产品如何使用每项功能的详细信息。 请务必提供尽可能详细的信息，以帮助我们了解你的产品需要声明该功能的原因。 请注意，这可能会给你的提交增加一些额外的时间来完成认证过程。

在认证过程中，我们的测试人员将审核你提供的信息，以确定是否批准你的提交使用该功能。 请注意，这可能会给你的提交增加一些额外的时间来完成认证过程。 如果我们批准你使用该功能，你的应用将继续进行认证过程的其余部分。 你向应用提交更新时，通常不必重复功能审批流程（除非你声明了其他功能）。

如果我们不批准使用的功能，你的提交内容将无法通过认证，和我们将提供的认证报告中的反馈。 然后，你可以选择创建新的提交并上传未声明该功能的程序包，或者在适用情况下解决与使用该功能有关的任何问题，然后在新提交中申请批准。

> [!NOTE]
> 如果你的提交内容将在合作伙伴中心中使用开发沙盒 （例如，这是任何游戏与 Xbox Live 集成，这种情况），你必须请求预先批准，而不是提供信息**提交选项**页. 若要执行该操作，请访问 [Windows 开发人员支持页面](https://developer.microsoft.com/windows/support)。 选择开发人员支持主题**仪表板问题**，问题类型**应用程序提交**，和子类别**其他**。 然后介绍如何使用功能以及它为何有必要针对您的产品。 如果不提供所有必要信息，将拒绝你的请求。 还可能会要求你提供更多信息。 请注意，该流程通常需要 5 个工作日或更长时间，因此请提前提交请求。
>
> 此外可以使用此方法的请求审批 （而不提供此信息在你的提交过程），是否要将开发沙盒，如果想要确认您已批准以使用受限制的功能，在开始之前你提交。

<span id="restricted-and-special-use-capability-list" />

### <a name="restricted-capability-list"></a>受限制的功能列表

下表列出了受限制的功能。 你可以按照上述过程为你提交到 Microsoft Store 的应用中的此类功能申请批准。

> [!IMPORTANT]
> 在这些受限功能中，有一些功能只在极其特殊和有限的情况下才获准在提交到 Microsoft Store 的应用中使用。 这些功能在下表中做了特殊标注。 如果你打算通过 Microsoft Store 分发你的应用，我们建议你不要在应用中声明这些功能。

| 功能应用场景 | 功能用法 |
|---------------------|------------------|
| **企业** | Windows 域凭据支持用户使用其凭据登录远程资源，其原理类似于用户已提供其用户名和密码。 **EnterpriseAuthentication**功能通常用于连接到企业内服务器的业务线应用中。 <br /><br />对于一般的 Internet 通信，无需使用此功能。<br /><br />**EnterpriseAuthentication**功能旨在支持常见的业务线应用程序。 不要在无需访问公司资源的应用中声明此功能。 [  **文件选取器**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)提供了一种强大的 UI 机制，让用户可以打开网络共享中要通过某个应用处理的文件。 声明**enterpriseAuthentication**功能仅在您的应用程序的方案需要编程访问且不能使用实现这些时**文件选取器**。<br /><br />当在应用的程序包清单中声明 **enterpriseAuthentication** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<br /><br />```<Capabilities><uap:Capability Name="enterpriseAuthentication"/></Capabilities>```<br /><br />**EnterpriseDataPolicy**功能允许应用程序分别处理企业数据并安全地应用托管使用 Windows 信息保护策略 (例如：移动设备管理和移动应用程序管理系统）。  声明此功能受到限制，如下所示。 <br /><br />```<Capabilities><rescap:Capability Name="enterpriseDataPolicy"/></Capabilities>```<br /><br />若要使用以下类的所有成员，此功能是必需的。<ul><li><a href="https://docs.microsoft.com/uwp/api/Windows.Security.EnterpriseData.FileProtectionManager">FileProtectionManager</a></li><li><a href="https://docs.microsoft.com/uwp/api/Windows.Security.EnterpriseData.DataProtectionManager">DataProtectionManager</a></li><li><a href="https://docs.microsoft.com/uwp/api/Windows.Security.EnterpriseData.ProtectionPolicyManager">ProtectionPolicyManager</a></li></ul> |
| **共享的用户证书** | **SharedUserCertificates**功能，应用程序来添加和访问软件和基于硬件的共享用户证书存储，例如存储在智能卡上的证书。 此功能通常用于需要智能卡来进行身份验证的财经或企业应用。<br /><br />当在应用的程序包清单中声明 **sharedUserCertificates** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<br /><br />```<Capabilities><uap:Capability Name="sharedUserCertificates"/></Capabilities>``` |
|**文档**\* | **DocumentsLibrary**功能提供对用户的 Documents 文件夹中，筛选为包清单中声明的文件类型关联的编程访问。 例如，如果一个文字处理应用程序声明了.doc 文件类型关联，它可以在用户的 Documents 文件夹中打开.doc 文件。 <br /><br />通常情况下，应用应允许用户选择的位置及其文件，使用以下 Api 之一：<ul><li>[FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)打开现有文件。</li><li>[FileSavePicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker)以保存新文件。</li><li>[FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker)以选择要打开 / 保存从其他文件的文件夹。</li></ul>使用这些 Api 允许用户选择最适合他们，例如云同步的帐户 (例如，OneDrive) 的位置。 用户选择了文件或文件夹使用这些 Api 后，您的应用程序可以通过使用获取正在进行位置的访问权限[FutureAccessList](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) API。 此 API 允许应用访问的文件或文件夹在将来无需要求用户再次选择它们。<br /><br />在现有的工作流其中假设文件将位于文档文件夹 （例如，与现有桌面应用程序进行互操作），或者如果不希望用户能够选择位置，您可以声明的情况下**documentsLibrary**为应用程序的功能。 如果您使用**documentsLibrary**为应用程序的功能，建议您还允许用户可以手动选取位置。<br /><br />当在应用的程序包清单中声明 **documentsLibrary** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<br /><br />```<Capabilities><uap:Capability Name="documentsLibrary"/></Capabilities>``` |
| **游戏 DVR 设置** | **appCaptureSettings** 受限功能允许应用控制游戏 DVR 的用户设置。<br /><br />若要使用 [**Windows.Media.Capture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture) 命名空间中的某些 API，则需要此功能。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。  |
| **Cellular** | **cellularDeviceControl** 受限功能允许应用控制手机网络设备。<br /><br />**cellularDeviceIdentity** 功能允许应用访问手机网络标识数据。<br /><br />**cellularMessaging** 功能允许应用使用短信和 RCS。<br /><br />若要使用 [**Windows.Devices.Sms**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sms) 命名空间中的某些 API，这些功能是必需的。  |
| **设备解锁** | **deviceUnlock** 受限功能允许应用解锁用于开发人员和企业旁加载方案的设备。<br /><br /> 我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **双 SIM 磁贴** | **dualSimTiles** 受限功能允许应用在具有多张 SIM 卡的设备上创建额外的应用列表项。<br /><br />若要使用 [**Windows.UI.StartScreen**](https://docs.microsoft.com/uwp/api/Windows.UI.StartScreen) 命名空间中的某些 API，则需要此功能。 |
| **企业共享存储** | **enterpriseDeviceLockdown** 受限功能允许应用使用设备锁定 API 和访问企业共享存储文件夹。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **系统输入的注入** | **InputInjectionBrokered**受限的功能允许应用程序以编程方式插入各种形式的输入如 HID，触摸、 笔、 键盘或鼠标进入系统。 此功能通常用于可控制系统的协作应用。<br /><br />对于电脑，只有位于同一应用容器中的进程才会收到来自应用的具有该功能的输入式注入。<br /><br />```<Capabilities><rescap:Capability Name="inputInjectionBrokered" /></Capabilities>``` |
| **观察输入**\* | **inputObservation** 受限功能允许应用观察要由系统接收的各种形式的原始输入（如 HID、触摸、笔、键盘或鼠标），而不考虑其最终目标。  |
| **禁止输入** | **inputSuppression** 受限功能允许应用隐藏要由系统接收的各种形式的原始输入，例如 HID、触摸、笔、键盘或鼠标。
| **VPN 应用** | **networkingVpnProvider** 受限功能允许应用具有对 VPN 功能的完整访问权限，包括管理连接和提供 VPN 插件功能的能力。<br /><br />若要使用 [**Windows.Networking.Vpn**](https://docs.microsoft.com/uwp/api/Windows.Networking.Vpn) 命名空间中的某些 API，则需要此功能。 |
| **其他应用程序管理** | **packageManagement** 受限功能允许应用直接管理其他应用。<br /><br />**packageQuery** 设备功能允许应用收集有关其他应用的信息。<br /><br />若要访问 [**PackageManager**](https://docs.microsoft.com/uwp/api/Windows.Management.Deployment.PackageManager) 类中的某些方法和属性，这些功能是必需的。 |
| **屏幕投影** | **screenDuplication** 受限功能允许应用将屏幕投影到另一台设备上。<br /><br />若要使用 DirectX 命名空间中的 API，此功能是必需的。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **用户主体名称** | **userPrincipalName** 受限功能允许应用修改和访问照片的缩略图缓存。<br /><br />若要调用 [**GetUserNameEx**](https://docs.microsoft.com/windows/desktop/api/secext/nf-secext-getusernameexa) 函数，则需要此功能。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **Wallet** | **walletSystem** 受限功能允许应用具有对存储的电子钱包卡的完整访问权限。<br /><br />若要使用 [**Windows.ApplicationModel.Wallet.System**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet.System) 命名空间中的 API，则需要此功能。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **位置历史记录** | **locationHistory** 受限功能允许应用访问设备的位置历史记录。<br /><br />若要使用 [**Windows.Devices.Geolocation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation) 命名空间中的 API，则需要此功能。
| **应用关闭确认** | **confirmAppClose** 受限功能允许应用关闭本身及其自己的窗口，以及延迟应用的关闭。<br /><br />应用可以在 Windows 10 版本 1703（内部版本 10.0.15063）及更高版本中请求此功能。 在以前的 Windows 10 版本中，此功能是专用的，会导致应用安装失败，并显示错误消息“无法授权此应用程序访问请求的功能”。 |
| **呼叫历史记录**\* | **phoneCallHistory** 受限功能允许应用读取呼叫历史记录和删除该历史记录中的条目。<br /><br />若要使用 [**Windows.ApplicationModel.Chat**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Chat) 命名空间中的 API，则需要此功能。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **系统级别的约会访问** | **appointmentsSystem** 受限功能允许应用读取和修改用户日历上的所有约会。<br /><br />若要使用 [**Windows.ApplicationModel.Appointment**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments) 命名空间中的 API，则需要此功能。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **系统级别聊天消息访问**\* | **chatSystem** 受限功能允许应用读取和写入所有短信和彩信。<br />若要使用 [**Windows.ApplicationModel.Chat**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Chat) 命名空间中的 API，则需要此功能。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **系统级联系人访问** | **contactsSystem** 受限功能允许应用读取已指定为受限或敏感的联系人信息，以及修改现有的联系人信息。<br /><br />若要使用 [**Windows.ApplicationModel.Chat**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Chat) 命名空间中的 API，则需要此功能。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **电子邮件访问** | **email** 受限功能允许应用读取、整理和发送用户电子邮件。<br /><br />若要使用 [**Windows.ApplicationModel.Email**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email) 命名空间中的 API，则需要此功能。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **系统级别的电子邮件访问**| **emailSystem** 受限功能允许应用读取、整理和发送受限或敏感的电子邮件。 <br /><br />若要使用 [**Windows.ApplicationModel.Email**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email) 命名空间中的 API，则需要此功能。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **系统级别调用历史记录访问** | **phoneCallHistorySystem** 受限功能允许应用通过更改现有条目和编写新条目来完全修改呼叫历史记录。<br /><br />若要使用 [**Windows.ApplicationModel.Calls**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls) 命名空间中的 API，则需要此功能。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **发送文本消息**\* | **smsSend** 受限功能允许应用发送短信和彩信。<br /><br />若要使用 [**Windows.ApplicationModel.Chat**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Chat) 命名空间中的 API，则需要此功能。 |
| **系统级别的访问权限的所有用户数据** | **userDataSystem** 受限功能允许应用访问用户数据系统数据存储。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **应用商店预览功能** | **previewStore** 受限功能允许应用检索和购买应用内产品的 SKU。<br /><br />若要使用 [**Windows.ApplicationModel.Store.Preview**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.Preview) 命名空间中的某些 API，则需要此功能。 |
| **首次登录设置** | **firstSignInSettings** 受限功能允许应用访问用户首次登录到他们的设备时所设置的用户设置。 |
| **Windows 团队体验** | **teamEditionExperience** 受限功能允许应用访问用于控制 Windows 团队会话的多个体验式方面的内部 API。 Windows 团队会话可在诸如 Microsoft Surface Hub 等团队设备上运行。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **远程解锁** | **remotePassportAuthentication** 受限功能允许应用访问可用于解锁远程电脑的凭据。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **预览组合** | **previewUiComposition** 受限功能允许应用在其用户界面上预览 [**Windows.UI.Composition**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition) 命名空间，以便它们可在完成前提供有关该 API 的反馈。 有关详细信息，请联系 wincomposition@microsoft.com。 |
| **安全评估锁定** | **secureAssessment** 受限功能允许应用将 Windows 锁定到单一应用模式以供安全评估。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **预配的连接管理器** | **networkConnectionManagerProvisioning** 受限功能允许应用定义通过 WWAN 与 WLAN 接口连接设备的策略。 使用此功能的应用由移动运营商创建，用来管理连接到其移动网络的设备。 |
| **数据计划预配** | **networkDataPlanProvisioning** 受限功能允许应用收集有关设备上流量套餐的信息并读取网络使用情况。 使用此功能的应用由移动运营商创建，用来将其客户的实际数据使用量集成到操作系统数据使用量设置中。 |
| **软件许可** | **slapiQueryLicenseValue** 受限制功能允许应用查询软件授权策略。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **扩展的执行** | **extendedBackgroundTaskTime** 受限功能可防止后台任务由于执行时间限制而被取消或终止。 它们仍遵循其他所有内存和电量使用限制。 可使用电池使用情况或后台应用隐私设置来限制此功能。 请注意，客户和管理员仍然可以通过组策略设置来控制后台任务。<br /><br />**extendedExecutionBackgroundAudio** 受限功能允许在应用不处于前台时播放音频。<br /><br />**extendedExecutionCritical** 受限功能允许应用开始一个关键扩展执行会话。<br /><br />**extendedExecutionUnconstrained** 受限功能允许应用开始一个不受限制的扩展执行会话。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。<br /><br />请参阅[使用扩展执行来推迟应用挂起](https://docs.microsoft.com/windows/uwp/launch-resume/run-minimized-with-extended-execution)，详细了解如何使用扩展执行在应用挂起时进行推迟。 |
| **移动设备管理** | **deviceManagementDmAccount** 受限功能允许应用预配和配置移动运营商开放移动联盟 - 设备管理 (MO OMA-DM) 帐户。<br /><br />**deviceManagementFoundation** 受限功能允许应用拥有对设备上的移动设备管理 (MDM) 配置服务提供程序 (CSP) 基础结构的基本访问权限。 请注意，其他功能是访问特定 CSP 时所必需的。<br /><br />**deviceManagementWapSecurityPolicies** 受限功能允许应用配置基于无线应用程序协议 (WAP) 的服务，例如彩信、服务指示/服务加载 (SI/SL)，以及开放移动联盟 - 客户端预配 (OMA-CP)。<br /><br />**deviceManagementEmailAccount** 受限功能允许移动运营商所创建的应用在设备上添加和管理其预配给用户的电子邮件帐户。 |
| **包策略控制** | **packagePolicySystem** 受限功能允许应用拥有对与安装在设备上的应用相关的系统策略的控制权。<br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **游戏列表** | **gameList** 受限功能允许应用获取系统上安装的已知游戏列表。<br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **Xbox 附件** | **xboxAccessoryManagement** 受限功能允许应用直接管理符合 Xbox 硬件规范的 Xbox 设备。<br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **附件的语音识别** | **cortanaSpeechAccessory** 受限功能允许应用调用命令并将命令传递给 Cortana。<br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **附件管理** | **accessoryManager** 受限功能允许应用注册为附件应用和选择特定应用通知，以便它们可转发到附件并显示给用户。 |
| **驱动程序访问** | **interopServices** 受限功能允许应用直接与驱动程序交互。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **前景色观察** | **inputForegroundObservation** 受限功能允许前台应用截获键盘输入并绕过所有非应用键盘输入处理。 无法通过此功能截获 SAS 组合。 若要访问 [**KeyboardDeliveryInterceptor**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.KeyboardDeliveryInterceptor) 类的成员，则需要此功能。
| **OEM 和月合作伙伴应用程序** | **oemDeployment** 受限功能允许 Microsoft 合作伙伴创建的应用在设备上安装新应用并查询当前安装的应用。<br /><br />**oemPublicDirectory** 受限功能允许 Microsoft 合作伙伴创建的应用访问共享的应用文件夹。 我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **应用程序授权** | **appLicensing** 受限功能允许应用无需许可证即可运行。 如果你在清单中声明此功能，则无法将应用提交到应用商店。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **位置系统**| **locationSystem** 受限功能允许应用执行某些有特权的位置配置，例如设置设备的默认位置。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **用户数据的帐户提供程序**| **userDataAccountsProvider** 受限功能允许应用完全管理邮件、日历和联系人帐户。 |
| **笔工作区** | **previewPenWorkspace** 功能允许应用访问要作为记住操作处理程序托管在笔工作区内的 Windows.ApplicationModel.Preview.Notes 命名空间。 |
| **第二因素身份** | **secondaryAuthenticationFactor** 功能允许应用通过在附近的配套身份验证设备上传递密钥存储解锁电脑。 例如，配套健身手环可用于解锁电脑。 若要访问 Windows.Security.Authentication.Identity.Provider 命名空间中的 API，则需要此功能。<br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **应用商店许可证管理**| **storeLicenseManagement** 功能允许 Microsoft 合作伙伴中心应用管理设备上的应用商店许可证。 若要访问 Windows.ApplicationModel.Store.LicenseManagement 命令空间中的 API，则需要此功能。 |
| **用户系统 ID**| **userSystemId** 功能允许应用获取特定于用户的标识符。 此标识符可唯一标识特定系统上的当前用户，并且可用于关联应用上的信息。 若要访问 Windows.System.Profile.SystemIdentification 类中的 GetUserSpecificSystemId API，则需要此功能。 |
| **目标的内容**| **targetedContent** 功能使应用程序可检索并使用 [**Windows.Services.TargetedContent**](https://docs.microsoft.com/uwp/api/windows.services.targetedcontent) 命名空间提供的目标订阅内容。<br /><br />若要使用 **Windows.System.Profile.SystemIdentification** 命名空间中的某些 API，则需要该功能。 |
| **UI 自动化**| **uiAutomation** 功能允许 UI 自动化客户端（例如讲述人）连接到 UI 自动化服务器或提供商。<br /><br />若要使用 **Windows.Xbox.Media.Capture.Broadcaster** 命名空间中的某些 API，则需要该功能。 |
|**游戏栏服务**| **gameBarServices** 仅限于第一方应用商店可更新的收件箱 UWA。<br /><br />若要使用 [**Windows.Media.Capture.GameBarsSrvices**](https://docs.microsoft.com/uwp/api/windows.media.capture.gamebarservices) 类，则需要该功能。<br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
|**应用程序捕获服务**| **appCaptureServices** 功能仅限于与 Microsoft 有合同关系的参与方。 这些关系根据合作伙伴协议授予，协议由 Xbox 服务和 bizdev 提供的帮助驱动。<br /><br />若要使用 [**Windows.Media.Capture.AppCaptureServices**](https://docs.microsoft.com/uwp/api/windows.media.capture.appcaptureservices) 类，则需要该功能。 |
|**应用程序广播服务**| **appBroadcastServices** 功能仅限于与 Microsoft 有合同关系的参与方。 这些关系根据合作伙伴协议授予，协议由 Xbox 服务提供的帮助驱动。<br /> <br />若要使用 [**Windows.Media.capture.AppBroadcastServices**](https://docs.microsoft.com/uwp/api/windows.media.capture.appbroadcastservices) 类，则需要该功能。<br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
|**音频设备配置**| **audioDeviceConfiguration** 这一功能允许应用程序查询、配置、启用和禁用音频驱动程序发出的音频效果。 <br /> <br />若要使用 [**Windows.Media.Devices.AudioDeviceModulesManager**](https://docs.microsoft.com/uwp/api/windows.media.devices.audiodevicemodulesmanager) 类，则需要该功能。<br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 这是因为 **AudioDeviceModulesManager** 允许应用程序访问给定系统上的所有音频效果。 还有可能将音频效果设置为对设备上的音频性能造成负面影响。 |
| **背景媒体录制** | **BackgroundMediaRecording**功能更改等的特定于媒体的 Api 的行为[ **MediaCapture** ](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)并[ **AudioGraph** ](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph)类以使媒体录制你的应用处于后台时。 |
|**预览 Ink 工作区**| **previewInkWorkspace** 功能允许应用访问托管在 Ink 工作区内的 Preview Ink 命名空间。 通常情况下，OEM 会使用此功能来替换设备上的白板应用程序。<br /> <br />若要使用 [**Windows.ApplicationModel.Preview.InkWorkspace**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.preview.inkworkspace) 命名空间中的 API，则需要该功能。 |
|**启动屏幕管理**| **startScreenManagement** 功能允许应用自动将磁贴固定到“开始”屏幕。 应用还可以从背景固定。 没有 **startScreenManagement** 功能并不会阻止任何 API；相反，使用 **startScreenManagement** 意味着 Shell 在某个应用使用“固定 API”时不会显示任何 UI。 |
|**Cortana 权限**| **cortanaPermissions** 功能允许应用枚举用户已在设备上授予 Cortana 的权限。 此功能还允许应用授予和撤消设备上的 Cortana 权限。 请注意，使用 **cortanaPermissions** 需要设备在授予权限前显示法律文本。 因此，应用应负责让用户知悉修改权限的法律后果。<br /> <br /><br />此功能需要获得读取访问权限**HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Search**注册表设置。<br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
|**所有应用程序模式**| **allAppMods** 功能允许应用访问所有应用的 AppMods 文件夹。 模型管理实用程序使用 **allAppMods** 在使用 Mod 的游戏或应用外管理 Mod。 |
|**扩展的资源**| **expandedResources** 功能允许应用访问游戏模式资源。 在 Xbox 和充分满足条件的电脑上，游戏模式资源表示保留供应用专用的可用 CPU 核心的子集。 在 Xbox 上，该应用至少还有 4 GB 的内存分区供独占使用。<br /><br />需要该功能才能获得上文所定义的专用 CPU 和内存资源。 |
|**受保护的应用**| **protectedApp** 功能使应用可由应用商店加载到受保护的进程中。 当应用引入应用商店时，应用商店会向可执行文件添加一个 blob。 应用商店还会使用 Microsoft 密钥对可执行文件进行页面签名。 进程加载程序会检查该 blob，而不是强制进行受保护进程的功能，因为 blob 需要 Microsoft 签名。<br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
|**游戏的监视器**| **gameMonitor** 功能会使系统使用活动监视来检测应用的游戏作弊。<br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
|**应用程序诊断**| **appDiagnostics** 功能允许应用获得任何其他正在运行的 UWP 应用的诊断信息（如包信息、内存使用情况和帐户名称）。 返回的信息包括运行应用的域/计算机帐户名称；如果使用管理员权限启动调用应用，则该应用可检索计算机上所有帐户的所有正在运行的应用列表。 <br /><br />需要此功能才可使用 [**Windows.System.AppDiagnosticInfo**](https://docs.microsoft.com/uwp/api/windows.system.appdiagnosticinfo)、**Windows.System.AppDiagnosticInfo.RequestAppDiagnosticInfoAsync** 和 [**Windows.ApplicationModel.AppInfo**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinfo) 类。 |
| **设备门户提供程序** | **devicePortalProvider** 功能允许应用调用 **Windows.System.Diagnostics.DevicePortal** API，并在处于开发人员模式时为诊断工具[充当 Web 服务器](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-plugin)。<br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **企业云上的单一登录** | **enterpriseCloudSSO** 功能允许应用针对托管的 Web 视图控件内的 Azure Active Director (AAD) 资源使用单一登录。 |
| **自动接受 VoIP 呼叫** | **BackgroundVoIP**功能允许你自动接收和接受传入的 VoIP 呼叫而不需要用户显式接受调用。 利用该功能的应用会被授予对相机和麦克风的完全控制，并且可以在后台使用这些资源。<br /><br />我们建议不要声明此功能在提交到 Microsoft Store 应用。 对于大多数开发人员，不会批准使用此功能。 |
| **预留资源 VoIP 呼叫** | **OneProcessVoIP**功能，你可以保留单线程应用程序中的 VoIP 呼叫所需的 CPU 和内存资源。<br /><br />我们建议不要声明此功能在提交到 Microsoft Store 应用。 对于大多数开发人员，不会批准使用此功能。 |
| **开发模式网络** | **developmentModeNetwork** 功能允许应用在调用 C++/CX UWP 应用或 C++ Windows 运行时组件中的 OpenFile Win32 API 时使用已登录用户的凭据访问网络路径。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **广泛的文件系统访问** | **broadFileSystemAccess** 功能允许应用获取与当前运行该应用的用户相同的文件系统访问权限，且在运行期间无需任何额外的文件选取器样式提示符。 请务必注意此功能不需要访问用户已选择使用文件选取器或 FolderPicker 的文件。<br/><br/>此功能适用于 [Windows.Storage](https://docs.microsoft.com/uwp/api/windows.storage) API。 由于用户可以授予或拒绝的权限设置中的任何时间，应确保您的应用程序以适应这些更改。 在 2018 年 4 月更新中，该权限的默认值是上。 在 2018 年 10 月更新中，默认值为 Off。 除该功能外，不声明任何特殊的文件夹功能（如**文档**、**图片**或**视频**）同样很重要。 可以在应用中启用此功能，通过添加**broadFileSystemAccess**到应用清单中。 有关示例，请参阅[文件的访问权限](/windows/uwp/files/file-access-permissions)一文。 |
| **系统固件和 BIOS** | **smbios** 功能允许应用访问 BIOS 数据和系统固件数据。 |
| **完全信任权限级别** | **RunFullTrust**受限的功能允许应用在用户的计算机上运行在完全信任权限级别。 此功能需要使用[FullTrustProcessLauncher](https://docs.microsoft.com/uwp/api/windows.applicationmodel.fulltrustprocesslauncher) API。<br /><br />此功能，还需要为 appx 或 msix 包传递任何桌面应用程序 (如同[桌面桥](https://developer.microsoft.com/windows/bridges/desktop))，并打包这些应用使用桌面应用程序时将自动显示在应用清单转换器 (DAC) 或 Visual Studio。 |
| **提升** | **AllowElevation**受限的功能允许通过 Microsoft 合作伙伴和企业能够保留现有桌面功能，需要自动提升启动时，或在应用程序的生存期内创建的应用。<br/><br/>我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 仅将批准的企业到企业 Microsoft Store 通过其专用应用商店部署业务线应用。  |
| **Windows Team 设备凭据** | **TeamEditionDeviceCredential**受限的功能允许应用访问 Api 的请求的设备运行 Windows 10 版本 1703年或更高版本的 Surface Hub 设备上的帐户凭据。<br/><br/>我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **Windows 团队应用程序视图** | **TeamEditionView**受限的功能允许应用访问 Api，用于托管运行 Windows 10 版本 1703年或更高版本的 Surface Hub 设备上的应用程序视图。<br/><br/>我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **照相机处理扩展插件** | **CameraProcessingExtension**受限的功能允许应用处理而无需直接照相机控件照相机中捕获的映像。<br /><br />此功能需要调用中的 Api [Windows.Devices.PointOfService.Provider](/uwp/api/windows.devices.pointofservice.provider)命名空间。<br /><br />任何人都可以请求访问要提交到应用商店的此功能。 |
| **数据使用情况管理** | **NetworkDataUsageManagement**受限的功能允许应用程序以收集网络数据使用情况信息。<br /><br />此功能需要调用[GetAttributedNetworkUsageAsync](/uwp/api/windows.networking.connectivity.connectionprofile.getattributednetworkusageasync)。<br /><br />任何人都可以请求访问要提交到应用商店的此功能。 |
| **管理电话线路连接** | **PhoneLineTransportManagement**功能允许应用程序管理系统设备负责电话线路连接。<br /><br />此功能需要使用中的 PhoneLineTransportDevice Api [Windows.ApplicationModel.Calls](https://docs.microsoft.com/uwp/api/windows.applicationmodel.calls)命名空间。 |
| **Unvirtualized 的资源** | **UnvirtualizedResources**受限的功能使应用程序可以声明[RegistryWriteVirtualization](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-registrywritevirtualization)并[FileSystemWriteVirtualization](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-filesystemwritevirtualization)若要禁用注册表和文件系统虚拟化其包清单中的元素。 这些声明可防止系统虚拟化到 HKEY_CURRENT_USER 或用户的 AppData 文件夹中，任何写入操作分别。 这是在你的应用程序而需要其他应用程序以读取或写入相同的注册表或文件系统项作为你的应用程序的情况下很有用。<br /><br />此功能适用于某些类型的发布的 Microsoft 和我们的合作伙伴的桌面 PC 游戏。 它不是要用于其他方案，因为它可能会危害系统的功能完全卸载。 |
| **可修改应用程序** | **ModifiableApp**受限的功能使应用程序可以声明[windows.mutablePackageDirectories](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-package-extension)其包清单中的扩展。 这使您可以提供你的应用程序而需要的文件夹的名称修改或添加要查找的文件。 操作系统将创建此文件夹，并使应用程序能够使用此文件夹，而不是 （或除了） 中的文件最初安装的应用程序的文件。<br /><br />此功能适用于某些类型的发布的 Microsoft 和我们的合作伙伴的桌面 PC 游戏。 它将不被授予对于其他情况，因为它可以允许未经签名的代码执行。 |
| **包写入重定向兼容性修补程序** | **PackageWriteRedirectionCompatibilityShim**受限的功能配置你的应用程序在每个用户位置中创建所有新文件。 任何已存在的文件打开以进行写入操作前复制到每用户位置和该位置中的文件执行修改的操作。 此功能可用于创建或修改其安装文件夹中的文件的应用程序。<br /><br />此功能适用于某些类型的发布的 Microsoft 和我们的合作伙伴的桌面 PC 游戏。 但是，也可能适用于其他应用在某些情况下。 |
| **自定义安装操作** | **CustomInstallActions**受限的功能使应用程序可以声明[windows.customInstall](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-package-extension)中其包清单，以便它可以指定一个或多个其他扩展安装程序执行与你的应用程序的文件 （.exe 或.msi）。 这允许您指定的任何标准的部署方案的自定义操作： 安装、 更新、 修复或卸载。 例如，这是有用的应用程序捆绑的第三方的可再发行组件。<br /><br />此功能适用于某些类型的发布的 Microsoft 和我们的合作伙伴的桌面 PC 游戏。 它不会授予的其他方案。 |
| **打包的服务** | **PackagedServices**受限的功能允许应用程序创建的 Microsoft 合作伙伴和企业能够声明[windows.service](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop6-extension)扩展在其包清单，该 it可以安装一个或多个服务和应用程序。 这些服务可以配置为本地服务、 网络服务或本地系统帐户下运行。 本地服务和网络服务的服务仅需要**packagedServices**功能。 本地系统服务，两者都需要**packagedServices**并**localSystemServices**功能。<br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。  |
| **本地系统服务** | **LocalSystemServices**受限的功能允许通过 Microsoft 合作伙伴和企业能够安装一个或多个本地系统服务，以及应用程序创建的应用程序 (即，你的应用程序可以声明StartAccount 服务为本地系统)。 此方案还需要**packagesServices**功能。 <br /><br />我们建议不要声明此功能的应用程序提交到 Microsoft Store 中。 在大多数情况下，不会批准此功能使用。 |
| **背景空间感知** | **BackgroundSpatialPerception**受限的功能允许应用程序访问用户的头节点、 手、 运动控制器和其他跟踪的对象的移动应用在后台运行时。 |

## <a name="custom-capabilities"></a>自定义功能

[受限功能](#restricted-capabilities)上面部分介绍可用于请求批准以使用自定义功能的相同功能审批过程。 [嵌入 SIM](/uwp/api/windows.networking.networkoperators.esim) Api 是需要自定义功能的 Api 的示例。 如果只想要在开发人员模式下本地运行应用程序，则不需要自定义功能。 但您需要它以将您的应用程序发布到 Microsoft Store，或外部开发人员模式下运行。

如果您有 Windows 技术客户经理 (TAM)，然后可以使用您的 TAM 请求的访问权限。 您可以找到更多详细信息[联系你的 Microsoft TAM](/windows-hardware/drivers/mobilebroadband/testing-your-desktop-cosa-apn-database-submission#contact-your-microsoft-tam)。

若要声明自定义功能，请修改你[应用程序包清单](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)源文件 (`Package.appxmanifest`)。 添加**xmlns:uap4** XML 命名空间声明，并用**uap4**前缀声明自定义功能时。 下面提供了一个示例。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package
    ...
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4">
...
<Capabilities>
    <uap4:CustomCapability Name="CompanyName.customCapabilityName_PublisherID"/>
</Capabilities>
</Package>
```

## <a name="related-topics"></a>相关主题

* [提交选项](../publish/manage-submission-options.md)
* [如何在包清单中指定的功能](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-capabilities-in-a-package-manifest)
* [如何在包清单中指定设备功能](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-in-a-package-manifest)
