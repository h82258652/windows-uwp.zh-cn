---
title: 相机条形码扫描仪入门
description: 了解如何使用相机条形码扫描仪
ms.date: 09/02/2019
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: b35ff6b183a6344fbc8da6b44a6cb81ea695a1c9
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243294"
---
# <a name="getting-started-with-a-camera-barcode-scanner"></a>相机条形码扫描仪入门

此处使用的代码段仅用于演示目的。 有关工作示例，请参阅[条形码扫描器示例](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)。

## <a name="step-1-add-capability-declarations-to-your-app-manifest"></a>步骤 1：向应用程序清单添加功能声明

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

## <a name="step-3-define-your-device-selector"></a>步骤 3：定义设备选择器

### <a name="option-a-find-all-barcode-scanners"></a>**选项 A：查找所有条形码扫描器**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector();
```

### <a name="option-b-scoping-device-selector-to-connection-type"></a>**选项 B：将设备选择器作用域连接到连接类型**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

## <a name="step-4-enumerate-all-barcode-scanners"></a>步骤 4：枚举所有条形码扫描器

如果你不希望在应用程序的生命周期内更改设备的列表，则可以使用[DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)枚举快照一次，但如果你认为条码扫描器列表在你的应用程序应改用[DeviceWatcher](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) 。  

> [!Important]
> 使用 GetDefaultAsync 枚举 PointOfService 设备可能导致不一致的行为，因为它只返回在类中找到的第一个设备，这可能随会话的变化而改变。

### <a name="option-a-enumerate-a-snapshot-of-barcode-scanners"></a>**选项 A：枚举条形码扫描器的快照**

```Csharp
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

> [!TIP]
> 请参阅[*枚举设备快照*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-a-snapshot-of-devices)了解有关使用 *FindAllAsync* 的详细信息。

### <a name="option-b-enumerate-available-barcode-scanners-and-watch-for-changes-to-the-available-scanners"></a>**选项 B：枚举可用的条形码扫描器并监视对可用扫描仪的更改**

```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
watcher.Added += Watcher_Added;
watcher.Removed += Watcher_Removed;
watcher.Updated += Watcher_Updated;
watcher.Start();
```

> [!TIP]
> 请参阅[*枚举和观察设备更改*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices)和 [*DeviceWatcher*](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) 了解详细信息。

## <a name="step-5-identify-camera-barcode-scanners"></a>步骤 5：识别相机条形码扫描器

相机条形码扫描仪在 Windows 将连接到计算机的相机与软件解码器配对时动态创建。  每个相机 - 解码器对都是一个功能完全的条形码扫描仪。

对于生成的设备集合中的每个条形码扫描器，可以通过检查[*BarcodeScanner. VideoDeviceID*](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.videodeviceid#Windows_Devices_PointOfService_BarcodeScanner_VideoDeviceId)属性来区分照相机条形码扫描器和物理条形码扫描仪。  非 NULL VideoDeviceID 表示设备集合中的条形码扫描器对象是相机条形码扫描器。  如果有多个相机条形码扫描器，你可能想要生成一个不包括物理条形码扫描器的单独集合。

使用随 Windows 附带的解码器的相机条形码扫描器标识为：

> Microsoft BarcodeScanner（*你的相机名称在这里*）

如果有多个照相机并且它们内置于计算机的机箱中，则该名称可能会区分*前*和*后*两个照相机。

> [!NOTE]
> 将来可能会发布具有不同命名方案的其他软件解码器。

当 DeviceWatcher 启动时（步骤4），它会通过每个连接的设备进行枚举。 此处，我们将可用的扫描仪添加到条形码扫描器集合，并将该集合绑定到 ListBox。

```csharp
ObservableCollection<BarcodeScannerInfo> barcodeScanners = new ObservableCollection<BarcodeScannerInfo>();

private async void Watcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        barcodeScanners.Add(new BarcodeScannerInfo(args.Name, args.Id));

        // Select the first scanner by default.
        if (barcodeScanners.Count == 1)
        {
            ScannerListBox.SelectedIndex = 0;
        }
    });
}
```

当 ListBox 的 SelectedIndex 更改时（默认情况下，在上一个代码段中选择第一项），将查询设备信息。

```csharp
private async void ScannerSelection_Changed(object sender, SelectionChangedEventArgs args)
{
    var selectedScannerInfo = (BarcodeScannerInfo)args.AddedItems[0];
    var deviceId = selectedScannerInfo.DeviceId;

    await SelectScannerAsync(deviceId);
}
```

## <a name="step-6-claim-the-camera-barcode-scanner"></a>步骤 6：声明照相机条形码扫描器

使用 [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) 来获取对相机条形码扫描仪的独占使用。

```csharp
private async Task SelectScannerAsync(string scannerDeviceId)
{
    selectedScanner = await BarcodeScanner.FromIdAsync(scannerDeviceId);

    if (selectedScanner != null)
    {
        claimedScanner = await selectedScanner.ClaimScannerAsync();
        if (claimedScanner != null)
        {
            await claimedScanner.EnableAsync();
        }
        else
        {
            rootPage.NotifyUser("Failed to claim the selected barcode scanner", NotifyType.ErrorMessage);
        }
    }
    else
    {
        rootPage.NotifyUser("Failed to create a barcode scanner object", NotifyType.ErrorMessage);
    }
}
```

## <a name="step-7-system-provided-preview"></a>步骤 7：系统提供的预览版

用户要成功扫描相机的条形码需要使用相机预览。  Windows 提供了一个简单的相机预览，用于启动用于基本控制相机条形码扫描器的对话框。  调用 [ClaimedBarcodeScanner.ShowVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.showvideopreviewasync) 即可打开对话框，完成后调用 [ClaimedBarcodeScanner.HideVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.hidevideopreview) 将其关闭。

> [!TIP]
> 请参阅[托管预览](pos-camerabarcode-hosting-preview.md)了解如何托管应用程序中相机条形码扫描仪的预览。

## <a name="step-8-initiate-scan"></a>步骤 8：启动扫描

你可以通过调用 [**StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync) 来启动扫描过程。

根据[**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived)的值，扫描程序可能只扫描一个条形码，然后在调用[**StopSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync)之前停止或扫描。

设置所需的 [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 值来在解码条形码时控制扫描仪行为。

| ReplTest1 | 描述 |
| ----- | ----------- |
| True   | 在扫描一个条形码后停止 |
| False  | 持续扫描条形码而不停止 |

## <a name="see-also"></a>请参阅

### <a name="samples"></a>示例

- [条形码扫描器示例](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
