---
author: PatrickFarley
ms.assetid: 374D1983-60E0-4E18-ABBB-04775BAA0F0D
title: 从应用扫描
description: 在此处了解如何通过使用平板扫描仪、送纸器或自动配置的扫描源从你的应用扫描内容。
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f9128056cbb3b9218d164b243948d9dd16af0786
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7300752"
---
# <a name="scan-from-your-app"></a>从应用扫描


**重要的 API**

-   [**Windows.Devices.Scanners**](https://msdn.microsoft.com/library/windows/apps/Dn264250)
-   [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393)
-   [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381)

在此处了解如何通过使用平板扫描仪、送纸器或自动配置的扫描源从你的应用扫描内容。

**重要** [**Windows.Devices.Scanners**](https://msdn.microsoft.com/library/windows/apps/Dn264250) Api 是桌面[设备系列](https://msdn.microsoft.com/library/windows/apps/Dn894631)的一部分。 应用可以使用这些 Api 仅在 windows 10 桌面版上。

若要从你的应用进行扫描，你必须首先声明一个新的 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 对象并获取 [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381) 类型，以此来列出可用的扫描仪。 仅列出并向应用提供带有 WIA 驱动程序的本地安装的扫描仪。

在应用列出可用的扫描仪后，它可以使用基于扫描仪类型的自动配置的扫描设置，或使用可用的平板或送纸器扫描源进行扫描。 要使用自动配置的设置，扫描仪必须启用自动配置，并且不可同时配备平板和送纸器扫描仪。 有关详细信息，请参阅[自动配置的扫描](https://msdn.microsoft.com/library/windows/hardware/Ff539393)。

## <a name="enumerate-available-scanners"></a>枚举可用扫描仪

Windows 不会自动检测扫描仪。 你必须执行此步骤以使应用与该扫描仪进行通信。 在本示例中，使用 [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) 命名空间进行扫描仪设备枚举。

1.  首先，将这些 using 语句添加到你的类定义文件。

``` csharp
    using Windows.Devices.Enumeration;
    using Windows.Devices.Scanners;
```

2.  接着，实现一个设备查看器以开始枚举扫描仪。 有关详细信息，请参阅[枚举类](enumerate-devices.md)。

```csharp
    void InitDeviceWatcher()
    {
       // Create a Device Watcher class for type Image Scanner for enumerating scanners
       scannerWatcher = DeviceInformation.CreateWatcher(DeviceClass.ImageScanner);

       scannerWatcher.Added += OnScannerAdded;
       scannerWatcher.Removed += OnScannerRemoved;
       scannerWatcher.EnumerationCompleted += OnScannerEnumerationComplete;
    }
```

3.  为添加扫描仪创建事件处理程序。

```csharp
    private async void OnScannerAdded(DeviceWatcher sender,  DeviceInformation deviceInfo)
    {
       await
       MainPage.Current.Dispatcher.RunAsync(
             Windows.UI.Core.CoreDispatcherPriority.Normal,
             () =>
             {
                MainPage.Current.NotifyUser(String.Format("Scanner with device id {0} has been added", deviceInfo.Id), NotifyType.StatusMessage);

                // search the device list for a device with a matching device id
                ScannerDataItem match = FindInList(deviceInfo.Id);

                // If we found a match then mark it as verified and return
                if (match != null)
                {
                   match.Matched = true;
                   return;
                }

                // Add the new element to the end of the list of devices
                AppendToList(deviceInfo);
             }
       );
    }
```

## <a name="scan"></a>扫描

1.  **获取 ImageScanner 对象**

对于每个 [**ImageScannerScanSource**](https://msdn.microsoft.com/library/windows/apps/Dn264238) 枚举类型，无论它是 **Default**、**AutoConfigured**、**Flatbed** 还是 **Feeder**，必须首先通过调用 [**ImageScanner.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.scanners.imagescanner.fromidasync) 方法来创建 [**ImageScanner**](https://msdn.microsoft.com/library/windows/apps/Dn263806) 对象，如下所示。

 ```csharp
    ImageScanner myScanner = await ImageScanner.FromIdAsync(deviceId);
 ```

2.  **仅扫描**

要以默认设置进行扫描，你的应用将依靠 [**Windows.Devices.Scanners**](https://msdn.microsoft.com/library/windows/apps/Dn264250) 命名空间选择一个扫描仪并从该来源进行扫描。 未更改扫描设置。 可能的扫描仪为自动配置、平板或送纸器。 此类型的扫描最有可能产生成功的扫描操作，即使它从错误的来源进行扫描，如从平板扫描仪而不是从送纸器。

**注意**如果用户送纸器中放置要扫描的文档，扫描仪将从平板改为扫描。 如果用户尝试从空的送纸器进行扫描，扫描作业将不会产生任何扫描后的文件。
 
```csharp
    var result = await myScanner.ScanFilesToFolderAsync(ImageScannerScanSource.Default,
        folder).AsTask(cancellationToken.Token, progress);
```

3.  **从自动配置、平板或送纸器来源进行扫描**

应用可以使用设备的[自动配置的扫描](https://msdn.microsoft.com/library/windows/hardware/Ff539393)来以最佳的扫描设置进行扫描。 使用此选项，设备本身可以根据要扫描的内容确定最佳扫描设置，例如颜色模式和扫描分辨率。 设备在运行时为每个新的扫描作业选择扫描设置。

**注意**并非所有扫描仪都支持此功能，因此应用必须检查扫描仪是否支持使用此设置前此功能。

在本示例中，应用首先检查扫描仪是否可以进行自动配置，然后进行扫描。 要指定平板扫描仪或送纸器扫描仪，只需使用 **Flatbed** 或 **Feeder** 替换 **AutoConfigured**。

```csharp
    if (myScanner.IsScanSourceSupported(ImageScannerScanSource.AutoConfigured))
    {
        ...
        // Scan API call to start scanning with Auto-Configured settings.
        var result = await myScanner.ScanFilesToFolderAsync(
            ImageScannerScanSource.AutoConfigured, folder).AsTask(cancellationToken.Token, progress);
        ...
    }
```

## <a name="preview-the-scan"></a>预览扫描

你可以在扫描到文件夹之前添加代码以预览该扫描。 在以下示例中，应用检查 **Flatbed** 扫描仪是否支持预览，然后预览该扫描。

```csharp
if (myScanner.IsPreviewSupported(ImageScannerScanSource.Flatbed))
{
    rootPage.NotifyUser("Scanning", NotifyType.StatusMessage);
                // Scan API call to get preview from the flatbed.
                var result = await myScanner.ScanPreviewToStreamAsync(
                    ImageScannerScanSource.Flatbed, stream);
```

## <a name="cancel-the-scan"></a>取消扫描

你可以让用户在扫描中途取消扫描作业，如下所示。

```csharp
void CancelScanning()
{
    if (ModelDataContext.ScenarioRunning)
    {
        if (cancellationToken != null)
        {
            cancellationToken.Cancel();
        }                
        DisplayImage.Source = null;
        ModelDataContext.ScenarioRunning = false;
        ModelDataContext.ClearFileList();
    }
}
```

## <a name="scan-with-progress"></a>显示进度的扫描

1.  创建 **System.Threading.CancellationTokenSource** 对象。

```csharp
cancellationToken = new CancellationTokenSource();
```

2.  设置进度事件处理程序并获取扫描的进度。

```csharp
    rootPage.NotifyUser("Scanning", NotifyType.StatusMessage);
    var progress = new Progress<UInt32>(ScanProgress);
```

## <a name="scanning-to-the-pictures-library"></a>扫描到图片库

用户可以使用 [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/BR207881) 类动态扫描到任何文件夹，但是你必须在清单中声明*图片库*功能以允许用户扫描到该文件夹。 有关应用功能的详细信息，请参阅[应用功能声明](https://msdn.microsoft.com/library/windows/apps/Mt270968)。
