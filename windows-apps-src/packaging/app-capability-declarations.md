---
author: msatranjr
ms.assetid: 25B18BA5-E584-4537-9F19-BB2C8C52DFE1
title: 应用功能声明
description: 功能必须在你的通用 Windows 平台 (UWP) 应用的程序包清单中声明，以便可用于访问某些 API 或资源（如图片、音乐）或者设备（如相机或麦克风）。
ms.author: misatran
ms.date: 09/20/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3d6cce2a38624339016acbad51693de1ade50678
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5473862"
---
# <a name="app-capability-declarations"></a>应用功能声明

功能必须在你的通用 Windows 平台 (UWP) 应用的[程序包清单](https://msdn.microsoft.com/library/windows/apps/BR211474)中声明，以便可用于访问某些 API 或资源（如图片、音乐）或者设备（如相机或麦克风）。

可以通过在应用的[程序包清单](https://msdn.microsoft.com/library/windows/apps/BR211474)中声明功能来请求对特定资源或 API 的访问权限。 可以使用 Microsoft Visual Studio 中的[清单设计器](packaging-uwp-apps.md#configure-an-app-package)来声明常用功能，也可以手动添加它们。 有关详细信息，请参阅[如何在程序包清单中指定功能](https://msdn.microsoft.com/library/windows/apps/BR211477)。 请务必了解，当客户从应用商店获取你的应用时，会向它们告知该应用声明的所有功能。 请避免声明你的应用不需要的功能。

某些功能为应用提供对*敏感资源*的访问权限。 由于这些资源可用于访问用户私人数据或花费用户金钱，因此它们被认为是敏感资源。 由“设置”应用管理的隐私设置允许用户动态控制对敏感资源的访问权限。 因此，你的应用不会假设敏感资源始终可用，这一点很重要。 有关访问敏感资源的详细信息，请参阅[隐私感知应用指南](https://msdn.microsoft.com/library/windows/apps/Hh768223)。 已注释用于为应用提供对*敏感资源* 的访问权限的功能，即此类功能应用场景的旁边会有一个星号 (*)。

有几种类型的功能。

- [常用功能](#general-use-capabilities)，适用于大多数常见应用场景。
- [设备功能](#device-capabilities)，这允许应用访问外围设备和内部设备。
- [受限的功能](#restricted-capabilities)，Microsoft Store 提交需要获得批准才能和/或通常只适用于 Microsoft 及某些合作伙伴。
- [自定义功能](#custom-capabilities)。

## <a name="general-use-capabilities"></a>常用功能

常用功能适用于最常见的应用场景。

| 功能应用场景 | 功能用法 |
|---------------------|------------------|
| **音乐**\* | **musicLibrary** 功能提供对用户音乐的编程访问权限，让应用无需用户交互即可枚举和访问库中的所有文件。 此功能通常用在使用整个音乐库的自动唱片点唱机应用中。<br /><br />[**文件选取器**](https://msdn.microsoft.com/library/windows/apps/BR207847)提供了一种强大的 UI 机制，让用户可以打开要通过某个应用处理的文件。 仅当应用需要进行编程访问，而使用**文件选取器**无法实现编程访问时，才声明 **musicLibrary** 功能。<br /><br /> 当在应用的程序包清单中声明 **musicLibrary** 功能时，该功能必须包含 **uap** 命名空间，如下所示。 <table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="musicLibrary"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **图片**\* | **picturesLibrary** 功能提供对用户图片的编程访问权限，让应用无需用户交互即可枚举和访问库中的所有文件。 在使用整个图片库的照片应用中，通常会使用此功能。<br /><br />[**文件选取器**](https://msdn.microsoft.com/library/windows/apps/BR207847)提供了一种强大的 UI 机制，让用户可以打开要通过某个应用处理的文件。 仅当应用需要进行编程访问，而使用**文件选取器**无法实现编程访问时，才声明 **picturesLibrary** 功能。<br /><br />当在应用的程序包清单中声明 **picturesLibrary** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="picturesLibrary"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **视频**\* | **videosLibrary** 功能提供对用户视频的编程访问权限，让应用无需用户交互即可枚举和访问库中的所有文件。 此功能通常用在使用整个视频库的电影播放应用中。<br /><br />[**文件选取器**](https://msdn.microsoft.com/library/windows/apps/BR207847)提供了一种强大的 UI 机制，让用户可以打开要通过某个应用处理的文件。 仅当应用需要进行编程访问，而使用**文件选取器**无法实现编程访问时，才声明 **videosLibrary** 功能。<br /><br />当在应用的程序包清单中声明 **videosLibrary** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="videosLibrary"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **可移动存储** | **removableStorage** 功能提供对可移动存储（如 U 盘和外部硬盘驱动器）中文件的编程访问权限，这些文件将按照程序包清单中声明的文件类型关联进行筛选。 例如，如果某个文档阅读器应用声明了 .doc 文件类型关联，则它可以打开可移动存储设备中的 .doc 文件，但无法打开其他类型的文件。 声明此功能时请务必小心，因为用户的可移动存储设备中可能包含多种信息，他们将期待你的应用提供有效的理由，以便对该可移动存储中属于已声明类型的所有文件进行编程访问。<br /><br />用户将期待你的应用处理你所声明的所有文件关联。 因此，请勿声明你的应用无法可靠处理的文件关联。 [**文件选取器**](https://msdn.microsoft.com/library/windows/apps/BR207847)提供了一种强大的 UI 机制，让用户可以打开要通过某个应用处理的文件。<br /><br />仅当应用需要进行编程访问，而使用[**文件选取器**](https://msdn.microsoft.com/library/windows/apps/BR207847)无法实现编程访问时，才声明 **removableStorage** 功能。<br /><br />当在应用的程序包清单中声明 **removableStorage** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="removableStorage"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **Internet 和公共网络**\* | 存在两种提供对 Internet 和公共网络的不同级别访问权限的功能。<br /><br />**internetClient** 功能指示应用可以接收从 Internet 传入的数据。 无法充当服务器。 无法访问本地网络。<br />**internetClientServer** 功能指示应用可以接收从 Internet 传入的数据。 可以充当服务器。 无法访问本地网络。<br /><br />具有 Web 服务组件的大多数应用都将使用 **internetClient**。 如果应用支持需要在其中侦听传入的网络连接的对等 (P2P) 方案，则应该使用 **internetClientServer**。 **internetClientServer** 功能包括 **internetClient** 功能所提供的访问权限，因此，在指定 **internetClientServer** 时无需指定 **internetClient**。
| **家庭和工作网络**\* | **privateNetworkClientServer** 功能提供通过防火墙对家庭和工作网络的入站和出站访问权限。 此功能通常用于通过局域网 (LAN) 通信的游戏和在各种本地设备上共享数据的应用。 如果你的应用指定了 **musicLibrary**、**picturesLibrary** 或 **videosLibrary**，则无需使用此功能即可访问家庭组中相应的库。 在 Windows 上，此功能不提供对 Internet 的访问权限。
| **约会** | **appointments** 功能提供对用户的约会存储的访问权限。 此功能允许读取对从同步的网络帐户获得的约会以及写入约会存储的其他应用的访问权限。 通过此功能，你的应用可以创建新的日历并向所创建的日历写入约会。<br /><br />当在应用的程序包清单中声明 **appointments** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="appointments"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **联系人**\* | **contacts** 功能提供了对各种联系人存储中的联系人聚合视图的访问权限。 此功能向已在各种网络和本地联系人存储中同步的联系人提供了对该应用的受限访问权限（应用网络许可规则）。<br /><br />当在应用的程序包清单中声明 **contacts** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="contacts"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **代码生成** | **codeGeneration** 功能允许应用访问以下功能，可向应用提供 JIT 功能。<br /><br />[**VirtualProtectFromApp**](https://msdn.microsoft.com/library/windows/desktop/Mt169846)<br />[**CreateFileMappingFromApp**](https://msdn.microsoft.com/library/windows/desktop/Hh994453)<br />[**OpenFileMappingFromApp**](https://msdn.microsoft.com/library/windows/desktop/Mt169844)<br />[**MapViewOfFileFromApp**](https://msdn.microsoft.com/library/windows/desktop/Hh994454)
| **AllJoyn** | **allJoyn** 功能允许网络上支持 AllJoyn 的应用和设备发现彼此并进行交互。<br /><br />在 [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971) 命名空间中访问 API 的所有应用都必须使用此功能。
| **电话呼叫** | **phoneCall** 功能允许应用访问设备上的所有电话线并执行以下功能。<br /><br />通过电话线发出呼叫并显示系统拨号器，而不提示用户。<br />访问与线相关的元数据。<br />访问与线相关的触发器。<br />允许用户选择的垃圾邮件筛选器应用设置并检查阻止列表和呼叫来源信息。<br /><br />当在应用的程序包清单中声明 **phoneCall** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="phoneCall"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table><b>phoneCallHistoryPublic</b> 功能允许应用读取设备上的手机网络和某些 VOIP 呼叫历史记录信息。 此功能还允许应用编写 VOIP 呼叫历史记录项。 若要访问 <a href="https://msdn.microsoft.com/library/windows/apps/Dn705931"><b>PhoneCallHistoryStore</b></a> 类的所有成员，则需要此功能。
| **录制的通话文件夹**\* | **recordedCallsFolder** 设备功能允许应用访问记录的通话文件夹。<br /><br />当在应用的程序包清单中声明 **recordedCallsFolder** 功能时，该功能必须包含 **mobile** 命名空间，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;mobile:Capability Name="recordedCallsFolder"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **用户帐户信息**\* | **userAccountInformation** 功能使应用能够访问用户的名称和头像。<br /><br />若要访问 [**Windows.System.UserProfile**](https://msdn.microsoft.com/library/windows/apps/Windows.System.UserProfile) 命名空间中的某些 API，此功能是必需的。<br /><br />当在应用的程序包清单中声明 **userAccountInformation** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="userAccountInformation"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **VOIP 呼叫** | **voipCall** 功能允许应用访问 [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) 命名空间中的 VOIP 呼叫 API。<br /><br />当在应用的程序包清单中声明 **voipCall** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="voipCall"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **3D 对象** | **objects3D** 功能允许应用编程访问 3D 对象文件。 此功能通常用在需要访问整个 3D 对象库的 3D 应用和游戏中。<br /><br />若要使用 [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) 命令空间中的 API 访问包含 3D 对象的文件夹，则需要此功能。<br /><br />当在应用的程序包清单中声明 **objects3D** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="objects3D"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **读取阻止的消息**\* | **blockedChatMessages** 功能允许应用读取已由“垃圾邮件筛选器”应用阻止的短信和彩信。<br /><br />若要使用 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 命令空间中的 API 访问已阻止的消息，则需要此功能。<br /><br />当在应用的程序包清单中声明 **blockedChatMessages** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;uap:Capability Name="blockedChatMessages"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **自定义设备** | **LowLevelDevices**功能允许应用访问自定义设备满足其他要求的大量时。 不要将此功能通过**lowLevel**设备功能，它允许访问 GPIO、 I2C、 SPI 和 PWM 设备相混淆。<br /><br /> 如果你想要打开此设备的句柄和发送 Ioctl 开发公开[设备接口](https://docs.microsoft.com/windows-hardware/drivers/install/device-interface-classes)的自定义驱动程序，你必须<ul><li>启用应用程序清单中的**lowLevelDevices**功能： <table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;iot:Capability Name="lowLevelDevices"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table></li><li>启用[嵌入的模式](https://docs.microsoft.com/windows/iot-core/develop-your-app/EmbeddedMode)</li><li>将设备接口标记为[受限制](https://docs.microsoft.com/windows-hardware/drivers/install/devpkey-deviceinterface-restricted)，在[INF](https://msdn.microsoft.com/library/windows/desktop/hh404264(v=vs.85).aspx)中或通过调用[WdfDeviceAssignInterfaceProperty()](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/wdfdevice/nf-wdfdevice-wdfdeviceassigninterfaceproperty)中你的驱动程序。</ul>  <br /><br />然后可以使用[**Windows.Devices.Custom.CustomDevice**](https://docs.microsoft.com/uwp/api/Windows.Devices.Custom.CustomDevice)打开到你的设备的句柄。 有关详细信息，请参阅[适用于内部设备的 UWP 设备应用](https://docs.microsoft.com/windows-hardware/drivers/devapps/uwp-device-apps-for-specialized-devices)。
| **IoT 系统管理** | **systemManagement** 功能允许应用具有基本的系统管理权限，例如关机或重启、区域设置和时区。<br /><br />若要访问 [**Windows.System**](https://msdn.microsoft.com/library/windows/apps/BR241814) 命名空间中的某些 API，则需要此功能。<br /><br />当在应用的程序包清单中声明 **systemManagement** 功能时，该功能必须包含 **iot** 命名空间，如下所示。<table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;iot:Capability Name="systemManagement"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>
| **后台媒体播放** | **backgroundMediaPlayback** 功能更改了特定于媒体的 API（如 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.aspx) 和 [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/windows.media.audio.audiograph.aspx) 类）的行为，支持应用在后台时的媒体播放。 所有活动音频流将不再静音，在应用转换到后台时将继续发声。 此外，播放进行时，应用生存时间将自动延长。
| **远程系统** | **remoteSystem** 功能使应用有权访问与用户的 Microsoft 帐户关联的设备列表。 对于执行在设备间持续的任何操作，需要访问设备列表。 若要访问以下命名空间和方法中的所有成员，此功能是必需的。<br /><br />Windows.System.RemoteSystems 命名空间<br />Windows.System.RemoteLauncher 命名空间<br />AppServiceConnection.OpenRemoteAsync 方法 |
| **空间感知** | **spatialPerception** 功能支持以编程方式访问空间映射数据，为混合现实应用提供有关用户附近空间中应用程序指定区域的表面信息。  请仅在你的应用明确会使用这些表面网格时再声明 spatialPerception 功能，因为混合现实应用在基于用户的头部姿势执行全息呈现时不需要此功能。 |


## <a name="device-capabilities"></a>设备功能

设备功能允许你的应用访问外围设备和内部设备。 可在你的应用包清单中使用 **DeviceCapability** 元素指定设备功能。 此元素可能需要其他子元素，并且某些设备功能需要手动添加到该程序包清单。 有关详细信息，请参阅[如何在程序包清单中指定设备功能](https://msdn.microsoft.com/library/windows/apps/Dn263092)和 [**DeviceCapability Schema reference**](https://msdn.microsoft.com/library/windows/apps/BR211430)。

| 功能应用场景 | 功能用法 |
|---------------------|------------------|
| **位置**\* | **location** 功能提供对定位功能的访问权限，定位功能通常从专用硬件（如电脑中的 GPS 传感器）检索获得或从可用的网络信息中获得。 应用必须处理用户从**设置**超级按钮禁用定位服务的情况。 |
| **麦克风** | **microphone** 功能提供对麦克风音频源的访问权限，让应用可以录制来自所连接麦克风的音频。 应用必须处理用户从**设置**超级按钮禁用麦克风的情况。 |
| **邻近感应** | **proximity** 功能支持临近的多台设备彼此通信。 此功能通常用在一般的多玩家游戏和交换信息的应用中。 设备会尝试使用可提供最佳连接的通信技术，包括蓝牙、Wi-Fi 和 Internet。 此功能仅用于在设备之间发起通信。 |
| **摄像头** | **webcam** 功能提供对内置相机或外部摄像头的视频源的访问权限，这使应用可以捕获照片和视频。 在 Windows 上，应用必须处理用户从**设置**超级按钮禁用相机的情况。<br/>**webcam** 功能仅授予对视频流的访问权限。 若要也授予对音频流的访问权限，必须添加 **microphone** 功能。 |
| **USB** | **usb** 设备功能允许访问[为 USB 设备更新应用清单程序包](http://go.microsoft.com/fwlink/p/?LinkId=302259)中的 API。 |
| **人体学接口设备 (HID)** | **humaninterfacedevice** 设备功能允许访问[如何为 HID 指定设备功能](https://msdn.microsoft.com/library/windows/apps/Dn263091)中的 API。 |
| **服务点 (POS)** | **pointOfService** 设备功能允许访问 [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071) 命名空间中的 API。 该命名空间允许你的应用访问服务点 (POS) 条码扫描仪和磁条阅读器。 该命名空间提供一个独立于供应商的接口，可用于从 UWP 应用访问由各种制造商提供的 POS 设备。 |
| **蓝牙** | **bluetooth** 设备功能允许应用通过通用属性 (GATT) 或经典基本速率 (RFCOMM) 协议与已经配对的蓝牙设备进行通信。<br/>若要使用 [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413) 命名空间中的某些 API，则需要此功能。 |
| **WLAN 网络** | **wiFiControl** 设备功能允许应用扫描并连接到 WLAN 网络。<br/>若要使用 [**Windows.Devices.WiFi**](https://msdn.microsoft.com/library/windows/apps/Dn975224) 命名空间中的某些 API，则需要此功能。 |
| **无线电收发器状态** | **radios** 设备功能允许应用切换 WLAN 和蓝牙无线电收发器。<br/>若要使用 [**Windows.Devices.Radios**](https://msdn.microsoft.com/library/windows/apps/Dn996447) 命名空间中的 API，则需要此功能。  |
| **光盘** | **optical** 设备功能允许应用访问光盘驱动器（如 CD、DVD 和蓝光光盘）上的功能。<br/>若要使用 [**Windows.Devices.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn263667) 命名空间中的某些 API，则需要此功能。 |
| **运动活动** | **activity** 设备功能允许应用检测设备的当前运动。<br/>若要使用 [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408) 命名空间中的某些 API，则需要此功能。 |
| **串行通信** | **serialcommunication** 设备功能提供了对于 Windows.Devices.SerialCommunication 命名空间中的 API 的访问，这样 Windows 应用便可与公开了串行端口或串行端口的某些抽象的设备进行通信。 若要使用 [**Windows.Devices.SerialCommnication**](https://docs.microsoft.com/uwp/api/windows.devices.serialcommunication) 命名空间中的 API，则需要该功能。 |
| **目视跟踪器** | 在连接了兼容的目视跟踪设备时，**gazeInput** 功能允许应用在应用程序边界内检测用户正在查看的位置。 若要使用[**Windows.Devices.Input.Preview**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.input.preview)命名空间中的某些 Api，则需要此功能。 |
| **GPIO、 I2C、 SPI 和 PWM** | **LowLevel**设备功能提供了访问 GPIO、 I2C、 SPI 和 PWM 设备。 此功能是必需使用以下命名空间中的 Api: [**Windows.Devices.Gpio**](https://docs.microsoft.com/uwp/api/windows.devices.gpio)、 [**Windows.Devices.I2c**](https://docs.microsoft.com/uwp/api/windows.devices.i2c)、 [**Windows.Devices.Spi**](https://docs.microsoft.com/uwp/api/windows.devices.spi)、[**Windows.Devices.Pwm**](https://docs.microsoft.com/uwp/api/windows.devices.pwm)。 <table><thead><tr><th>XML</th></tr></thead><tbody><tr><td><pre><code>&lt;Capabilities&gt;&lt;DeviceCapability Name="lowLevel"/&gt;&lt;/Capabilities&gt;</code></pre></td></tr></tbody></table>|


<span id="special-and-restricted-capabilities" />

## <a name="restricted-capabilities"></a>受限功能

如果你的应用声明所有受限的功能，必须在[应用提交过程](../publish/app-submissions.md)提供信息，以便批准要将应用发布到 Microsoft Store。 你提供此信息的提交，[提交选项](../publish/manage-submission-options.md#restricted-capabilities)页面上说明你的应用如何使用它声明每个受限的功能。

> [!IMPORTANT]
> 受限的功能专用于非常特定的场景。 这些功能的使用受严格限制，且受 Microsoft Store 的其他上架政策和评审的约束。 请注意，你可以旁加载声明受限的功能，而无需任何批准的应用。 只有在将此类应用提交到 Microsoft Store 时才需要获得批准。

请确保不要将其声明这些受限功能，除非你的应用确实需要它们。 在某些情况下，这些功能是必需的或适宜的，如在使用双因素身份验证开展银行业务时，用户提供带数字签名的智能卡确认其身份。 其他应用可能主要针对企业客户而设计，并可能需要访问一些必须使用用户的域凭据才能访问的企业资源。

若要声明受限的功能，请修改[应用包清单](https://msdn.microsoft.com/library/windows/apps/BR211474)源文件 (`Package.appxmanifest`)。 添加**xmlns: rescap** XML 命名空间声明，并声明受限的功能时使用的**rescap**前缀。 例如，下面演示了如何声明 **appCaptureSettings** 功能。

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

以前，你需要联系支持部门来获得使用某项功能的批准。 现在，你可以在开发人员中心仪表板中作为[提交过程](../publish/app-submissions.md)的一部分提供该信息。

当你为提交上传程序包时，我们将检测是否声明所有受限的功能。 如果我们检测到声明了此类功能，则你需要在[提交选项](../publish/manage-submission-options.md#restricted-capabilities)页面上提供有关你的产品如何使用每项功能的详细信息。 请务必提供尽可能详细的信息，以帮助我们了解你的产品需要声明该功能的原因。 请注意，这可能会给你的提交增加一些额外的时间来完成认证过程。

在认证过程中，我们的测试人员将审核你提供的信息，以确定是否批准你的提交使用该功能。 请注意，这可能会给你的提交增加一些额外的时间来完成认证过程。 如果我们批准你使用该功能，你的应用将继续进行认证过程的其余部分。 你向应用提交更新时，通常不必重复功能审批流程（除非你声明了其他功能）。

如果我们不批准你使用该功能，你的提交将无法通过认证，并且我们将提供在认证报告中的反馈。 然后，你可以选择创建新的提交并上传未声明该功能的程序包，或者在适用情况下解决与使用该功能有关的任何问题，然后在新提交中申请批准。

> [!NOTE]
> 如果你的提交在开发人员中心中使用了开发沙盒（例如，任何与 Xbox Live 集成的游戏都是这种情况），则你必须提前申请批准，而不是在**提交选项**页面上提供信息。 若要执行该操作，请访问 [Windows 开发人员支持页面](https://developer.microsoft.com/windows/support)。 选择开发人员支持主题**仪表板问题**、 问题类型**应用提交**和子类别**其他**。 然后介绍如何使用该功能，以及为何需要你的产品。 如果不提供所有必要信息，将拒绝你的请求。 我们也可能会要求你提供更多信息。 请注意，该流程通常需要 5 个工作日或更长时间，因此请提前提交请求。
>
> 你也可以使用此方法的请求审批 （而不提供此信息在你的提交过程），指示是否你使用的开发沙盒，如果你想要确认，你已被批准使用受限的功能，在开始之前你提交。

<span id="restricted-and-special-use-capability-list" />

### <a name="restricted-capability-list"></a>受限的功能列表

下表列出了受限的功能。 你可以按照上述过程为你提交到 Microsoft Store 的应用中的此类功能申请批准。

> [!IMPORTANT]
> 在这些受限功能中，有一些功能只在极其特殊和有限的情况下才获准在提交到 Microsoft Store 的应用中使用。 这些功能在下表中做了特殊标注。 如果你打算通过 Microsoft Store 分发你的应用，我们建议你不要在应用中声明这些功能。

| 功能应用场景 | 功能用法 |
|---------------------|------------------|
| **企业** | Windows 域凭据支持用户使用其凭据登录远程资源，其原理类似于用户已提供其用户名和密码。 **EnterpriseAuthentication**功能通常用在连接到企业内服务器的业务线应用。 <br /><br />对于一般的 Internet 通信，无需使用此功能。<br /><br />**EnterpriseAuthentication**功能旨在支持常见业务线应用。 不要在无需访问公司资源的应用中声明此功能。 [**文件选取器**](https://msdn.microsoft.com/library/windows/apps/BR207847)提供了一种强大的 UI 机制，让用户可以打开网络共享中要通过某个应用处理的文件。 **EnterpriseAuthentication**功能仅在声明为你的应用的方案需要以编程方式访问权限，并且无法通过使用**文件选取器**实现。<br /><br />当在应用的程序包清单中声明 **enterpriseAuthentication** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<br /><br />```<Capabilities><uap:Capability Name="enterpriseAuthentication"/></Capabilities>```<br /><br />**EnterpriseDataPolicy**功能允许应用单独处理企业数据和安全应用管理与 Windows 信息保护策略 (例如： 移动设备管理和移动应用程序管理系统)。  声明此受限的功能，如下所示。 <br /><br />```<Capabilities><rescap:Capability Name="enterpriseDataPolicy"/></Capabilities>```<br /><br />若要使用以下类的所有成员，此功能是必需的。<ul><li><a href="https://msdn.microsoft.com/library/windows/apps/Dn705151">FileProtectionManager</a></li><li><a href="https://msdn.microsoft.com/library/windows/apps/Dn706017">DataProtectionManager</a></li><li><a href="https://msdn.microsoft.com/library/windows/apps/Dn705170">ProtectionPolicyManager</a></li></ul> |
| **共享用户证书** | **SharedUserCertificates**功能支持应用添加和访问软件和应用商店中共享的用户的基于硬件的证书，例如存储在智能卡上的证书。 此功能通常用于需要智能卡来进行身份验证的财经或企业应用。<br /><br />当在应用的程序包清单中声明 **sharedUserCertificates** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<br /><br />```<Capabilities><uap:Capability Name="sharedUserCertificates"/></Capabilities>``` |
|**文档**\* | **DocumentsLibrary**功能提供对用户的文档，在包清单中声明以支持脱机访问 OneDrive 的文件类型关联进行筛选以编程方式访问。 例如，如果某个 DOC 阅读器应用仅声明了一个 .doc 文件类型关联，则它可以打开文档中的 .doc 文件，却无法打开其他类型的文件。 <br /><br />声明**documentsLibrary**功能的应用无法访问家庭组的计算机上的文档。 [文件选取器](https://msdn.microsoft.com/library/windows/apps/Hh465174)提供了一种强大的 UI 机制，让用户可以打开要通过某个应用处理的文件。 仅当你不能使用文件选取器时，请声明**documentsLibrary**功能。<br /><br />若要使用**documentsLibrary**功能，应用必须：<ul><li>使用有效的 OneDrive URL 或资源 ID 促进跨平台脱机访问特定 OneDrive 内容</li><li>在脱机时将打开的文件自动保存到用户的 OneDrive</li></ul>对于这两个目的使用**documentsLibrary**功能的应用对象也可能会使用该功能打开另一文档中的嵌入的内容。 接受仅**documentsLibrary**功能的以上用途。<ul><li>你的应用无法访问手机的内部存储中的文档库。 但是，如果另一个应用在可选 SD 卡上创建“文档”文件夹，你的应用可以看到该文件夹。</li></ul>当在应用的程序包清单中声明 **documentsLibrary** 功能时，该功能必须包含 **uap** 命名空间，如下所示。<br /><br />```<Capabilities><uap:Capability Name="documentsLibrary"/></Capabilities>``` |
| **游戏 DVR 设置** | **appCaptureSettings** 受限功能允许应用控制游戏 DVR 的用户设置。<br /><br />若要使用 [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/BR226738) 命名空间中的某些 API，则需要该功能。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。  |
| **手机网络** | **cellularDeviceControl** 受限功能允许应用控制手机网络设备。<br /><br />**cellularDeviceIdentity** 功能允许应用访问手机网络标识数据。<br /><br />**cellularMessaging** 功能允许应用使用短信和 RCS。<br /><br />若要使用 [**Windows.Devices.Sms**](https://msdn.microsoft.com/library/windows/apps/BR206567) 命名空间中的某些 API，这些功能是必需的。  |
| **设备解锁** | **deviceUnlock** 受限功能允许应用解锁用于开发人员和企业旁加载方案的设备。<br /><br /> 我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **双重 SIM 卡磁贴** | **dualSimTiles** 受限功能允许应用在具有多张 SIM 卡的设备上创建额外的应用列表项。<br /><br />若要使用 [**Windows.UI.StartScreen**](https://msdn.microsoft.com/library/windows/apps/BR242235) 命名空间中的某些 API，则需要该功能。 |
| **企业共享的存储** | **enterpriseDeviceLockdown** 受限功能允许应用使用设备锁定 API 和访问企业共享存储文件夹。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **系统输入式注入** | **InputInjectionBrokered**受限功能允许应用以编程方式将各种形式的输入，例如 HID、 触摸、 笔、 键盘或鼠标到系统。 该功能通常用于可控制系统的协作应用。<br /><br />对于电脑，只有位于同一应用容器中的进程才会收到来自应用的具有该功能的输入式注入。<br /><br />```<Capabilities><rescap:Capability Name="inputInjectionBrokered" /></Capabilities>``` |
| **观察输入**\* | **inputObservation** 受限功能允许应用观察要由系统接收的各种形式的原始输入（如 HID、触摸、笔、键盘或鼠标），而不考虑其最终目标。  |
| **禁止显示输入** | **inputSuppression** 受限功能允许应用隐藏要由系统接收的各种形式的原始输入，例如 HID、触摸、笔、键盘或鼠标。
| **VPN 应用** | **networkingVpnProvider** 受限功能允许应用具有对 VPN 功能的完整访问权限，包括管理连接和提供 VPN 插件功能的能力。<br /><br />若要使用 [**Windows.Networking.Vpn**](https://msdn.microsoft.com/library/windows/apps/Dn434040) 命名空间中的某些 API，则需要该功能。 |
| **其他应用管理** | **packageManagement** 受限功能允许应用直接管理其他应用。<br /><br />**packageQuery** 设备功能允许应用收集有关其他应用的信息。<br /><br />若要访问 [**PackageManager**](https://msdn.microsoft.com/library/windows/apps/BR240960) 类中的某些方法和属性，这些功能是必需的。 |
| **屏幕投影** | **screenDuplication** 受限功能允许应用将屏幕投影到另一台设备上。<br /><br />若要使用 DirectX 命名空间中的 API，该功能是必需的。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **用户主体名称** | **userPrincipalName** 受限功能允许应用修改和访问照片的缩略图缓存。<br /><br />若要调用 [**GetUserNameEx**](https://msdn.microsoft.com/library/windows/desktop/ms724435) 函数，则需要该功能。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **电子钱包** | **walletSystem** 受限功能允许应用具有对存储的电子钱包卡的完整访问权限。<br /><br />若要使用 [**Windows.ApplicationModel.Wallet.System**](https://msdn.microsoft.com/library/windows/apps/Mt171610) 命名空间中的 API，则需要该功能。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **位置历史记录** | **locationHistory** 受限功能允许应用访问设备的位置历史记录。<br /><br />若要使用 [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/BR225603) 命名空间中的 API，则需要该功能。
| **应用关闭确认** | **confirmAppClose** 受限功能允许应用关闭本身及其自己的窗口，以及延迟应用的关闭。<br /><br />应用可以在 Windows 10 版本 1703（内部版本 10.0.15063）及更高版本中请求此功能。 在以前的 Windows 10 版本中，此功能是专用的，会导致应用安装失败，并显示错误消息“无法授权此应用程序访问请求的功能”。 |
| **呼叫历史记录**\* | **phoneCallHistory** 受限功能允许应用读取呼叫历史记录和删除该历史记录中的条目。<br /><br />若要使用 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 命名空间中的 API，则需要该功能。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **系统级别约会访问** | **appointmentsSystem** 受限功能允许应用读取和修改用户日历上的所有约会。<br /><br />若要使用 [**Windows.ApplicationModel.Appointment**](https://msdn.microsoft.com/library/windows/apps/Dn263359) 命名空间中的 API，则需要该功能。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **系统级别聊天消息访问**\* | **chatSystem** 受限功能允许应用读取和写入所有短信和彩信。<br />若要使用 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 命名空间中的 API，则需要该功能。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **系统级别联系人访问** | **contactsSystem** 受限功能允许应用读取已指定为受限或敏感的联系人信息，以及修改现有的联系人信息。<br /><br />若要使用 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 命名空间中的 API，则需要该功能。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **电子邮件访问*** | **email** 受限功能允许应用读取、整理和发送用户电子邮件。<br /><br />若要使用 [**Windows.ApplicationModel.Email**](https://msdn.microsoft.com/library/windows/apps/Dn631285) 命名空间中的 API，则需要该功能。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **系统级别电子邮件访问**| **emailSystem** 受限功能允许应用读取、整理和发送受限或敏感的电子邮件。 <br /><br />若要使用 [**Windows.ApplicationModel.Email**](https://msdn.microsoft.com/library/windows/apps/Dn631285) 命名空间中的 API，则需要该功能。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **系统级别呼叫历史记录访问** | **phoneCallHistorySystem** 受限功能允许应用通过更改现有条目和编写新条目来完全修改呼叫历史记录。<br /><br />若要使用 [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) 命名空间中的 API，则需要该功能。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **发送文本消息**\* | **smsSend** 受限功能允许应用发送短信和彩信。<br /><br />若要使用 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 命名空间中的 API，则需要该功能。 |
| **对所有用户数据的系统级别访问权限** | **userDataSystem** 受限功能允许应用访问用户数据系统数据存储。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **Microsoft Store 预览功能** | **previewStore** 受限功能允许应用检索和购买应用内产品的 SKU。<br /><br />若要使用 [**Windows.ApplicationModel.Store.Preview**](https://msdn.microsoft.com/library/windows/apps/Mt185546) 命名空间中的某些 API，则需要该功能。 |
| **首次登录设置** | **firstSignInSettings** 受限功能允许应用访问用户首次登录到他们的设备时所设置的用户设置。 |
| **Windows 团队体验** | **teamEditionExperience** 受限功能允许应用访问用于控制 Windows 团队会话的多个体验式方面的内部 API。 Windows 团队会话可在诸如 Microsoft Surface Hub 等团队设备上运行。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **远程解锁** | **remotePassportAuthentication** 受限功能允许应用访问可用于解锁远程电脑的凭据。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **预览合成** | **previewUiComposition** 受限功能允许应用在其用户界面上预览 [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Dn706878) 命名空间，以便它们可在完成前提供有关该 API 的反馈。 有关详细信息，请联系 wincomposition@microsoft.com。 |
| **安全评估锁定** | **secureAssessment** 受限功能允许应用将 Windows 锁定到单一应用模式以供安全评估。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **连接管理器预配** | **networkConnectionManagerProvisioning** 受限功能允许应用定义通过 WWAN 与 WLAN 接口连接设备的策略。 使用该功能的应用由移动运营商创建，用来管理连接到其移动网络的设备。 |
| **流量套餐预配** | **networkDataPlanProvisioning** 受限功能允许应用收集有关设备上流量套餐的信息并读取网络使用情况。 使用此功能的应用由移动运营商创建，用来将其客户的实际数据使用量集成到操作系统数据使用量设置中。 |
| **软件授权** | **slapiQueryLicenseValue** 受限制功能允许应用查询软件授权策略。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **扩展执行** | **extendedBackgroundTaskTime** 受限功能可防止后台任务由于执行时间限制而被取消或终止。 它们仍遵循其他所有内存和电量使用限制。 可使用电池使用情况或后台应用隐私设置来限制此功能。 请注意，客户和管理员仍然可以通过组策略设置来控制后台任务。<br /><br />**extendedExecutionBackgroundAudio** 受限功能允许应用在后台运行时播放音频。<br /><br />**extendedExecutionCritical** 受限功能允许应用开始一个关键扩展执行会话。<br /><br />**extendedExecutionUnconstrained** 受限功能允许应用开始一个不受限制的扩展执行会话。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。<br /><br />请参阅[使用扩展执行来推迟应用挂起](https://docs.microsoft.com/windows/uwp/launch-resume/run-minimized-with-extended-execution)，详细了解如何使用扩展执行在应用挂起时进行推迟。 |
| **移动设备管理** | **deviceManagementDmAccount** 受限功能允许应用预配和配置移动运营商开放移动联盟 - 设备管理 (MO OMA-DM) 帐户。<br /><br />**deviceManagementFoundation** 受限功能允许应用拥有对设备上的移动设备管理 (MDM) 配置服务提供程序 (CSP) 基础结构的基本访问权限。 请注意，其他功能是访问特定 CSP 时所必需的。<br /><br />**deviceManagementWapSecurityPolicies** 受限功能允许应用配置基于无线应用程序协议 (WAP) 的服务，例如彩信、服务指示/服务加载 (SI/SL)，以及开放移动联盟 - 客户端预配 (OMA-CP)。<br /><br />**deviceManagementEmailAccount** 受限功能允许移动运营商所创建的应用在设备上添加和管理其预配给用户的电子邮件帐户。 |
| **程序包策略控制** | **packagePolicySystem** 受限功能允许应用拥有对与安装在设备上的应用相关的系统策略的控制权。<br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **游戏列表** | **gameList** 受限功能允许应用获取系统上安装的已知游戏列表。<br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **Xbox 附件** | **xboxAccessoryManagement** 受限功能允许应用直接管理符合 Xbox 硬件规范的 Xbox 设备。<br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **附件的语音识别** | **cortanaSpeechAccessory** 受限功能允许应用调用命令并将命令传递给 Cortana。<br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **附件管理** | **accessoryManager** 受限功能允许应用注册为附件应用和选择特定应用通知，以便它们可转发到附件并显示给用户。 |
| **驱动程序访问** | **interopServices** 受限功能允许应用直接与驱动程序交互。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **前台观察** | **inputForegroundObservation** 受限功能允许前台应用截获键盘输入并绕过所有非应用键盘输入处理。 无法通过此功能截获 SAS 组合。 若要访问 [**KeyboardDeliveryInterceptor**](https://msdn.microsoft.com/library/windows/apps/Mt608395) 类的成员，则需要该功能。
| **OEM 和 MO 合作伙伴应用** | **oemDeployment** 受限功能允许 Microsoft 合作伙伴创建的应用在设备上安装新应用并查询当前安装的应用。<br /><br />**oemPublicDirectory** 受限功能允许 Microsoft 合作伙伴创建的应用访问共享的应用文件夹。 我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **应用许可** | **appLicensing** 受限功能允许应用无需许可证即可运行。 如果你在清单中声明该功能，则无法将应用提交到 Microsoft Store。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **位置系统**| **locationSystem** 受限功能允许应用执行某些有特权的位置配置，例如设置设备的默认位置。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **用户数据帐户提供程序**| **userDataAccountsProvider** 受限功能允许应用完全管理邮件、日历和联系人帐户。 |
| **笔工作区** | **previewPenWorkspace** 功能允许应用访问要作为记住操作处理程序托管在笔工作区内的 Windows.ApplicationModel.Preview.Notes 命名空间。 |
| **辅助身份验证因素** | **secondaryAuthenticationFactor** 功能允许应用通过在附近的配套身份验证设备上传递密钥存储解锁电脑。 例如，配套健身手环可用于解锁电脑。 若要访问 Windows.Security.Authentication.Identity.Provider 命名空间中的 API，则需要该功能。<br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **Microsoft Store 许可证管理**| **storeLicenseManagement** 功能允许 Microsoft 合作伙伴中心应用管理设备上的应用商店许可证。 若要访问 Windows.ApplicationModel.Store.LicenseManagement 命令空间中的 API，则需要该功能。 |
| **用户系统 ID**| **userSystemId** 功能允许应用获取特定于用户的标识符。 此标识符可唯一标识特定系统上的当前用户，并且可用于关联应用上的信息。 若要访问 Windows.System.Profile.SystemIdentification 类中的 GetUserSpecificSystemId API，则需要该功能。 |
| **目标内容**| **targetedContent** 功能使应用程序可检索并使用 [**Windows.Services.TargetedContent**](https://docs.microsoft.com/uwp/api/windows.services.targetedcontent) 命名空间提供的目标订阅内容。<br /><br />若要使用 **Windows.System.Profile.SystemIdentification** 命名空间中的某些 API，则需要该功能。 |
| **UI 自动化**| **uiAutomation** 功能允许 UI 自动化客户端（例如讲述人）连接到 UI 自动化服务器或提供商。<br /><br />若要使用 **Windows.Xbox.Media.Capture.Broadcaster** 命名空间中的某些 API，则需要该功能。 |
|**游戏栏服务**| **gameBarServices** 仅限于第一方应用商店可更新的收件箱 UWA。<br /><br />若要使用 [**Windows.Media.Capture.GameBarsSrvices**](https://docs.microsoft.com/uwp/api/windows.media.capture.gamebarservices) 类，则需要该功能。<br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
|**应用捕获服务**| **appCaptureServices** 功能仅限于与 Microsoft 有合同关系的参与方。 这些关系根据合作伙伴协议授予，协议由 Xbox 服务和 bizdev 提供的帮助驱动。<br /><br />若要使用 [**Windows.Media.Capture.AppCaptureServices**](https://docs.microsoft.com/uwp/api/windows.media.capture.appcaptureservices) 类，则需要该功能。 |
|**应用广播服务**| **appBroadcastServices** 功能仅限于与 Microsoft 有合同关系的参与方。 这些关系根据合作伙伴协议授予，协议由 Xbox 服务提供的帮助驱动。<br /> <br /><br />若要使用 [**Windows.Media.capture.AppBroadcastServices**](https://docs.microsoft.com/uwp/api/windows.media.capture.appbroadcastservices) 类，则需要该功能。<br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
|**音频设备配置**| **audioDeviceConfiguration** 这一功能允许应用程序查询、配置、启用和禁用音频驱动程序发出的音频效果。 <br /> <br />若要使用 [**Windows.Media.Devices.AudioDeviceModulesManager**](https://docs.microsoft.com/uwp/api/windows.media.devices.audiodevicemodulesmanager) 类，则需要该功能。<br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 这是因为 **AudioDeviceModulesManager** 允许应用程序访问给定系统上的所有音频效果。 还有可能将音频效果设置为对设备上的音频性能造成负面影响。 |
|**Preview Ink 工作区**| **previewInkWorkspace** 功能允许应用访问托管在 Ink 工作区内的 Preview Ink 命名空间。 通常情况下，OEM 会使用此功能来替换设备上的白板应用程序。<br /> <br />若要使用 [**Windows.ApplicationModel.Preview.InkWorkspace**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.preview.inkworkspace) 命名空间中的 API，则需要该功能。 |
|**“开始”屏幕管理**| **startScreenManagement** 功能允许应用自动将磁贴固定到“开始”屏幕。 应用还可以从背景固定。 没有 **startScreenManagement** 功能并不会阻止任何 API；相反，使用 **startScreenManagement** 意味着 Shell 在某个应用使用“固定 API”时不会显示任何 UI。 |
|**Cortana 权限**| **cortanaPermissions** 功能允许应用枚举用户已在设备上授予 Cortana 的权限。 此功能还允许应用授予和撤消设备上的 Cortana 权限。 请注意，使用 **cortanaPermissions** 需要设备在授予权限前显示法律文本。 因此，应用应负责让用户知悉修改权限的法律后果。<br /> <br /><br />若要获取对 **HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Search\(*)** 注册表设置的读取访问权限，则需要该功能。<br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
|**所有应用模型**| **allAppMods** 功能允许应用访问所有应用的 AppMods 文件夹。 模型管理实用程序使用 **allAppMods** 在使用 Mod 的游戏或应用外管理 Mod。 |
|**扩展资源**| **expandedResources** 功能允许应用访问游戏模式资源。 在 Xbox 和充分满足条件的电脑上，游戏模式资源表示保留供应用专用的可用 CPU 核心的子集。 在 Xbox 上，该应用至少还有 4 GB 的内存分区供独占使用。<br /><br />需要该功能才能获得上文所定义的专用 CPU 和内存资源。 |
|**受保护的应用**| **protectedApp** 功能使应用可由应用商店加载到受保护的进程中。 当应用引入应用商店时，应用商店会向可执行文件添加一个 blob。 应用商店还会使用 Microsoft 密钥对可执行文件进行页面签名。 进程加载程序会检查该 blob，而不是强制进行受保护进程的功能，因为 blob 需要 Microsoft 签名。<br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
|**游戏监视器**| **gameMonitor** 功能会使系统使用活动监视来检测应用的游戏作弊。<br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
|**应用诊断**| **appDiagnostics** 功能允许应用获得任何其他正在运行的 UWP 应用的诊断信息（如包信息、内存使用情况和帐户名称）。 返回的信息包括运行应用的域/计算机帐户名称；如果使用管理员权限启动调用应用，则该应用可检索计算机上所有帐户的所有正在运行的应用列表。 <br /><br />需要此功能才可使用 [**Windows.System.AppDiagnosticInfo**](https://docs.microsoft.com/uwp/api/windows.system.appdiagnosticinfo)、**Windows.System.AppDiagnosticInfo.RequestAppDiagnosticInfoAsync** 和 [**Windows.ApplicationModel.AppInfo**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appinfo) 类。 |
| **设备门户提供程序** | **devicePortalProvider** 功能允许应用调用 **Windows.System.Diagnostics.DevicePortal** API，并在处于开发人员模式时为诊断工具[充当 Web 服务器](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-plugin)。<br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **企业云单一登录** | **enterpriseCloudSSO** 功能允许应用针对托管的 Web 视图控件内的 Azure Active Director (AAD) 资源使用单一登录。 |
| **自动接受 VoIP 呼叫** | **backgroundVoIP** 功能允许开发人员自动接收并接受传入的 VoIP 呼叫，而无需用户明确接受呼叫。 利用该功能的应用会被授予对相机和麦克风的完全控制，并且可以在后台使用这些资源。<br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **开发模式网络** | **developmentModeNetwork** 功能允许应用在调用 C++/CX UWP 应用或 C++ Windows 运行时组件中的 OpenFile Win32 API 时使用已登录用户的凭据访问网络路径。 <br /><br />我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **广泛的文件系统访问** | **broadFileSystemAccess** 功能允许应用获取与当前运行该应用的用户相同的文件系统访问权限，且在运行期间无需任何额外的文件选取器样式提示符。 请务必注意，不需要此功能即可访问用户具有已选择使用 FilePicker 或 FolderPicker 的文件。<br/><br/>此功能适用于 [Windows.Storage](https://docs.microsoft.com/uwp/api/windows.storage) API。 请务必注意，第一次通过此功能（已在应用程序包清单中声明）使用任何 **Windows.Storage** API 都将触发用户同意提示，用户可以在此授予或拒绝权限。 用户也可以在任何设置切换点允许或拒绝权限。 除该功能外，不声明任何特殊的文件夹功能（如**文档**、**图片**或**视频**）同样很重要。 |
| **系统固件和 BIOS** | **smbios** 功能允许应用访问 BIOS 数据和系统固件数据。 |
| **完全信任权限级别** | **RunFullTrust**受限功能允许应用在用户计算机上运行完全信任权限级别。 若要使用[FullTrustProcessLauncher](https://docs.microsoft.com/uwp/api/windows.applicationmodel.fulltrustprocesslauncher) API，则需要此功能。<br /><br />此功能也是必需的 appx 或 msix 包作为提供任何桌面应用程序 （与[桌面桥](https://developer.microsoft.com/windows/bridges/desktop)），打包这些应用使用 Desktop App Converter (DAC) 时将自动显示在你的清单或Visual Studio。 |
| **提升权限** | **AllowElevation**受限功能允许通过 Microsoft 合作伙伴和企业可保留现有的桌面功能需要自动提升启动或在应用的生命周期期间创建的应用。<br/><br/>我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 它将仅批准通过适用于企业的 Microsoft 应用商店其专用应用商店向企业部署的业务线应用。  |
| **Windows 团队设备的凭据** | **TeamEditionDeviceCredentials**受限功能允许应用访问请求运行 Windows 10 版本 1703年或更高版本的 Surface Hub 设备上的设备帐户凭据的 Api。<br/><br/>我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |
| **Windows 团队应用程序视图** | **TeamEditionView**受限功能允许应用访问托管在运行 Windows 10 版本 1703年或更高版本的 Surface Hub 设备上的应用程序视图的 Api。<br/><br/>我们不建议在提交到 Microsoft Store 的应用中声明该功能。 对于大多数开发人员，我们不会批准其使用该功能。 |

## <a name="custom-capabilities"></a>自定义功能

上面的[受限的功能](#restricted-capabilities)部分介绍了相同的功能审批流程，你可以使用申请批准才能使用自定义功能。 [嵌入的 sim 卡](/uwp/api/windows.networking.networkoperators.esim)Api 是需要自定义功能的 Api 的示例。 如果你仅想要在开发人员模式下本地运行你的应用程序，则不需要自定义功能。 但你需要将应用发布到 Microsoft Store，或外部开发人员模式下运行它。

如果你有 Windows 技术帐户经理 (TAM)，则可以使用你 TAM 请求访问权限。 你可以在[接触 Microsoft TAM](/windows-hardware/drivers/mobilebroadband/testing-your-desktop-cosa-apn-database-submission#contact-your-microsoft-tam)时找到更多详细信息。

若要声明自定义功能，请修改[应用包清单](https://msdn.microsoft.com/library/windows/apps/BR211474)源文件 (`Package.appxmanifest`)。 添加**xmlns:uap4** XML 命名空间声明，并声明自定义功能时使用**uap4**前缀。 下面提供了一个示例。

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
* [如何在程序包清单中指定功能](https://msdn.microsoft.com/library/windows/apps/BR211477)
* [如何在包清单中指定设备功能](https://msdn.microsoft.com/library/windows/apps/Dn263092)
