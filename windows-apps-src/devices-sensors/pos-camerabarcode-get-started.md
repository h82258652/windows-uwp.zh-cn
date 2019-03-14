---
title: 相机条形码扫描仪入门
description: 了解如何使用相机条形码扫描仪
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 66dc3d9e12f6ef73e5461b8fe0064f21a0848c7e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614072"
---
# <a name="getting-started-with-a-camera-barcode-scanner"></a>相机条形码扫描仪入门
## <a name="step-1-add-capability-declarations-to-your-app-manifest"></a>第 1 步：将功能声明添加到应用程序清单
1. 在 Microsoft Visual Studio 的“解决方案资源管理器”中，通过双击“package.appxmanifest”项，打开应用程序清单的设计器。
2. 选择**功能**选项卡
3. 选中**网络摄像头**和 **PointOfService** 对应的框 

>[!NOTE] 
> 软件解码器从相机接收要解码的帧并从你的应用程序提供预览时需要使用**网络摄像头**功能

## <a name="step-2-add-using-directives"></a>步骤 2：添加 using 指令

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;
```
## <a name="step-3-define-your-device-selector"></a>步骤 3:定义设备选择器

### <a name="option-a-find-all-barcode-scanners"></a>**选项 a:查找所有条码扫描器**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector();       
```

### <a name="option-b-scoping-device-selector-to-connection-type"></a>**选项 b:为连接类型作用域的设备选择器**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

## <a name="step-4-enumerate-barcode-scanners"></a>步骤 4：枚举条形码扫描程序
如果你预计设备列表在应用程序的生命周期内不会更改，你可以使用 *FindAllAsync* 只枚举快照一次，但如果你认为条形码扫描仪列表可能会在应用程序的生命周期内更改，则应改为使用 *DeviceWatcher*。  

> [!Important] 
> 使用 GetDefaultAsync 枚举 PointOfService 设备可能导致不一致的行为，因为它只返回在类中找到的第一个设备，这可能随会话的变化而改变。

### <a name="option-a-enumerate-a-snapshot-of-barcode-scanners"></a>**选项 a:枚举的条码扫描器的快照**
```Csharp
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

> [!TIP]
> 请参阅[*枚举设备快照*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-a-snapshot-of-devices)了解有关使用 *FindAllAsync* 的详细信息。

### <a name="option-b-enumerate-and-watch-for-changes-in-available-barcode-scanners"></a>**选项 b:枚举并监视中可用的条码扫描器的更改**
```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);

// TODO: Add Event Listeners and Handlers
```
> [!TIP]
> 请参阅[*枚举和观察设备更改*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices)和 [*DeviceWatcher*](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) 了解详细信息。

## <a name="step-5-identify-camera-barcode-scanners"></a>步骤 5：识别照相机条形码扫描程序
相机条形码扫描仪在 Windows 将连接到计算机的相机与软件解码器配对时动态创建。  每个相机 - 解码器对都是一个功能完全的条形码扫描仪。

对于生成的设备集合中的每个条形码扫描仪，你可以通过检查 [*BarcodeScanner.VideoDeviceID*](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.videodeviceid#Windows_Devices_PointOfService_BarcodeScanner_VideoDeviceId) 属性来确定哪些是相机条形码扫描仪，哪些是物理条形码扫描仪。  非 NULL VideoDeviceID 将指示来自设备集合的条形码扫描仪对象是相机条形码扫描仪。  如果你有多个相机条形码扫描仪，你可能需要生成一个单独的集合，将物理条形码扫描仪排除在外。 

使用 Windows 附带的解码器的相机条形码扫描仪将显示为： 

> Microsoft BarcodeScanner（*你的相机名称在这里*）

如果你的相机已内置于计算机机箱，在你有多个相机时，名称中可能会使用 *front* 和 *rear* 加以区分。  将来，可能会提供其他软件解码器，使用不同的命名方案。

## <a name="step-6-claim-the-camera-barcode-scanner"></a>步骤 6：声明照相机条形码扫描程序 
使用 [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) 来获取对相机条形码扫描仪的独占使用。

## <a name="step-7-system-provided-preview"></a>步骤 7：系统提供预览
用户要成功扫描相机的条形码需要使用相机预览。  Windows 提供简单的相机预览，它将启动用于基本控制相机条形码扫描仪的对话框。  调用 [ClaimedBarcodeScanner.ShowVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.showvideopreviewasync) 即可打开对话框，完成后调用 [ClaimedBarcodeScanner.HideVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.hidevideopreview) 将其关闭。

> [!TIP]
> 请参阅[托管预览](pos-camerabarcode-hosting-preview.md)了解如何托管应用程序中相机条形码扫描仪的预览。

## <a name="step-8-initiate-scan"></a>步骤 8：启动扫描 
你可以通过调用 [**StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync) 来启动扫描过程。  
根据 [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 的值，扫描仪可能在扫描一个条形码后即停止，或持续扫描直到你调用 [**StopSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync)。

设置所需的 [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 值来在解码条形码时控制扫描仪行为。

| 值 | 描述 |
| ----- | ----------- |
| True   | 在扫描一个条形码后停止 |
| False  | 持续扫描条形码而不停止 |