---
ms.assetid: ''
description: 本文介绍如何连接到远程摄像头并获取 MediaFrameSourceGroup 帧检索每个照相机。
title: 连接到远程摄像头
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bc719b8dad2adef0542edf284d257846052eac21
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63789587"
---
# <a name="connect-to-remote-cameras"></a>连接到远程摄像头

本文介绍如何连接到一个或多个远程摄像头并获取[ **MediaFrameSourceGroup** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup)对象，它使你可以读取帧中每个照相机。 从媒体源读取帧的详细信息，请参阅[处理媒体帧与 MediaFrameReader](process-media-frames-with-mediaframereader.md)。 与设备配对的详细信息，请参阅[配对设备](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices)。

> [!NOTE] 
> 在本文中讨论的功能才可从 Windows 10，版本 1903年开始。

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>创建要监视的可用远程摄像头 DeviceWatcher 类

[ **DeviceWatcher** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher)类监视您的应用程序可供设备并在添加或删除设备时通知您的应用程序。 获取的实例**DeviceWatcher**通过调用[ **DeviceInformation.CreateWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_)、 传入标识的类型的高级查询语法 (AQS) 字符串你想要监视的设备。 指定网络摄像机设备 AQS 字符串如下所示：

```
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> 帮助器方法[ **MediaFrameSourceGroup.GetDeviceSelector** ](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector)返回 AQS 字符串并将监视其本地连接和远程网络摄像机。 若要监视仅网络摄像机，应使用上面所示的 AQS 字符串。


在开始返回**DeviceWatcher**通过调用[**启动**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.start)方法，它将引发[ **Added** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added)目前提供每个网络摄像机的事件。 直到您停止观察程序通过调用[**停止**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop)，则**Added**新网络摄像机设备变得可用时，将引发事件和[ **已删除**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.devicewatcher.removed)照相机设备变得不可用时，将引发事件。

事件参数传递到**Added**并**已移除**事件处理程序[ **DeviceInformation** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)或[ **DeviceInformationUpdate** ](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.deviceinformationupdate)对象，分别。 每个对象具有**Id**是网络摄像头触发该事件的标识符的属性。 传递到此 ID [ **MediaFrameSourceGroup.FromIdAsync** ](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync)方法以获取[ **MediaFrameSourceGroup** ](https://docs.microsoft.com/en-us/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) ，可以使用的对象从照相机中检索的帧。

## <a name="remote-camera-pairing-helper-class"></a>遥控照相机配对的帮助器类

下面的示例演示使用的帮助器类**DeviceWatcher**创建和更新**ObservableCollection**的**MediaFrameSourceGroup**对象以支持数据绑定到相机的列表。 典型的应用程序会将包装**MediaFrameSourceGroup**中自定义模型类。 请注意，帮助器类保留到应用程序的引用[ **CoreDispatcher** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) ，并更新对的调用中的相机的集合[ **RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync)以确保在 UI 线程更新 UI 绑定到集合。

此外，此示例处理[ **DeviceWatcher.Updated** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated)除了事件**Added**并**已删除**事件。 在中**Updated**从删除处理程序相关联的远程照相机设备并将其然后重新添加到集合。

[!code-cs[SnippetRemoteCameraPairingHelper](./code/Frames_Win10/Frames_Win10/RemoteCameraPairingHelper.cs#SnippetRemoteCameraPairingHelper)]


## <a name="related-topics"></a>相关主题

* [摄像头](camera.md)
* [基本的照片、 视频和音频捕获与 MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [相机帧示例](https://go.microsoft.com/fwlink/?LinkId=823230)
* [处理媒体帧与 MediaFrameReader](process-media-frames-with-mediaframereader.md)
 

 




