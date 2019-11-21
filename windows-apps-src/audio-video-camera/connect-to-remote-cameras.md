---
ms.assetid: ''
description: 本文说明如何连接到远程照相机，并从每个照相机获取 MediaFrameSourceGroup 来检索帧。
title: 连接到远程摄像头
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 253eea00ba6c4188197224111909c28a53932b88
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257352"
---
# <a name="connect-to-remote-cameras"></a>连接到远程摄像头

本文说明如何连接到一个或多个远程照相机，并获取一个[**MediaFrameSourceGroup**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup)对象，该对象允许您从每个照相机读取帧。 有关从媒体源中读取帧的详细信息，请参阅[使用 MediaFrameReader 处理媒体帧](process-media-frames-with-mediaframereader.md)。 有关与设备配对的详细信息，请参阅[配对设备](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices)。

> [!NOTE] 
> 本文中所述的功能仅适用于 Windows 10 1903 版。

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>创建用于监视可用远程相机的 DeviceWatcher 类

[**DeviceWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher)类监视应用可用的设备，并在添加或删除设备时通知应用。 通过调用[**DeviceInformation**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_)获取**DeviceWatcher**的实例，传递标识要监视的设备类型的高级查询语法（AQS）字符串。 指定网络摄像机设备的 AQS 字符串如下：

```
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> Helper 方法[**MediaFrameSourceGroup**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector)返回一个 AQS 字符串，该字符串将监视本地连接的和远程网络摄像机。 若要仅监视网络摄像机，应使用上面所示的 AQS 字符串。


当你通过调用[**start**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.start)方法来启动返回的**DeviceWatcher**时，它将为当前可用的每个网络相机引发[**添加**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added)的事件。 在通过调用[**stop**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop)停止观察程序之前，当新的网络照相机设备可用时，将引发**添加**的事件，并且当照相机设备不可用时，将引发[**已删除**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.devicewatcher.removed)的事件。

传入**添加**和**移除**事件处理程序的事件参数分别是[**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)或[**DeviceInformationUpdate**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.deviceinformationupdate)对象。 其中每个对象都有一个**Id**属性，该属性是触发事件的网络摄像机的标识符。 将此 ID 传递到[**MediaFrameSourceGroup. FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync)方法，以获取可用于从相机检索帧的[**MediaFrameSourceGroup**](https://docs.microsoft.com/en-us/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync)对象。

## <a name="remote-camera-pairing-helper-class"></a>远程相机配对帮助程序类

下面的示例演示一个帮助器类，该类使用**DeviceWatcher**创建和更新**MediaFrameSourceGroup**对象的**ObservableCollection** ，以支持与照相机列表的数据绑定。 典型应用会在自定义模型类中包装**MediaFrameSourceGroup** 。 请注意，帮助器类维护对应用程序的[**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher)的引用，并在对[**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync)的调用中更新照相机的集合，以确保在 ui 线程上更新绑定到该集合的 ui。

此外，此示例还处理**已添加**和**已删除**事件之外的[**DeviceWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated)事件。 在**更新**的处理程序中，将从中移除关联的远程照相机设备，然后将其添加回集合。

[!code-cs[SnippetRemoteCameraPairingHelper](./code/Frames_Win10/Frames_Win10/RemoteCameraPairingHelper.cs#SnippetRemoteCameraPairingHelper)]


## <a name="related-topics"></a>相关主题

* [摄像头](camera.md)
* [带有 MediaCapture 的基本照片、视频和音频捕获](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [照相机框架示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [用 MediaFrameReader 处理媒体帧](process-media-frames-with-mediaframereader.md)
 

 




