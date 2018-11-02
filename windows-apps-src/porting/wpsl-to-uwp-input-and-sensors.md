---
author: stevewhims
description: 与设备本身及其传感器集成的代码涉及到与用户之间的输入和输出。
title: 针对 I/O、 设备和应用模型进行移植到 UWP 的 WindowsPhone Silverlight
ms.assetid: bf9f2c03-12c1-49e4-934b-e3fa98919c53
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a0041fd154a4ce32930e10e21175706e8e7ad988
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5934925"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-io-device-and-app-model"></a>针对 I/O、 设备和应用模型进行移植 WindowsPhone silverlight 移植到 UWP


上一主题是[移植 XAML 和 UI](wpsl-to-uwp-porting-xaml-and-ui.md)。

与设备本身及其传感器集成的代码涉及到与用户之间的输入和输出。 它还可以涉及处理数据。 但是通常不将此代码视为 UI 层或数据层。 此代码包含与振动控制器、加速计、陀螺仪、麦克风和扬声器（与语音识别和合成交叉）、（地理）位置和输入形式（例如触摸、鼠标、键盘和笔）的集成。

## <a name="application-lifecycle-process-lifetime-management"></a>应用程序生命周期（进程周期管理）

WindowsPhone Silverlight 应用包含代码，用于保存和还原其应用程序状态及其视图状态才可支持进行逻辑删除并随后进行重新激活。 通用 Windows 平台 (UWP) 应用的应用生命周期有诸多与 WindowsPhone Silverlight 应用，因为它们都旨在最大程度地可用的资源相同的目标应用到用户已选择都使用在任何时刻的前景色。 你会发现你的代码可以相当轻松地适应新系统。

**注意**自动按下硬件**后退**按钮终止 WindowsPhone Silverlight 应用。 按下移动设备上的硬件 **“后退”** 按钮*不会*自动终止 UWP 应用。 但是，它将处于暂停状态，然后可能会被终止。 但是，对于对应用程序生命周期事件做出相应响应的应用，这些详细信息是透明的。

在应用处于非活动状态以及系统引发暂停事件期间，将出现一个“防止误动作窗口”。 对于 UWP 应用而言，并不存在防止误动作窗口；因为只要应用处于非活动状态，便会引发暂停事件。

有关详细信息，请参阅[应用生命周期](https://msdn.microsoft.com/library/windows/apps/mt243287)。

## <a name="camera"></a>相机

WindowsPhone Silverlight 相机捕获代码使用**Microsoft.Devices.Camera**、 **Microsoft.Devices.PhotoCamera**或**Microsoft.Phone.Tasks.CameraCaptureTask**类。 若要将该代码移植到通用 Windows 平台 (UWP)，你可以使用 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 类。 [**CapturePhotoToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh700836) 主题中提供一个代码示例。 该方法使你可以将照片捕获到存储文件，而且它要求**麦克风**和**摄像头**[**设备功能**](https://msdn.microsoft.com/library/windows/apps/dn934747)将应用包清单中。

另一个选项是[**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030)类，这也需要**麦克风**和**摄像头**的[**设备功能**](https://msdn.microsoft.com/library/windows/apps/dn934747)。

镜头应用不受 UWP 应用支持。

## <a name="detecting-the-platform-your-app-is-running-on"></a>检测正运行你的应用的平台

做更改的应用面向 windows 10 的方法。 新增的概念模型是，应用面向通用 Windows 平台 (UWP)，并且可跨所有 Windows 设备运行。 这样它便可以选择充分利用特定设备系列所独有的功能。 特别是，该应用还可以选择自行限制为面向一个或多个设备系列（如果需要）。 有关具体设备系列（以及如何确定要面向哪一个设备系列）的详细信息，请参阅 [UWP 应用指南](https://msdn.microsoft.com/library/windows/apps/dn894631)。

**注意**我们建议你不使用操作系统或设备系列来检测某些功能是否存在。 通常情况下，标识当前操作系统或设备系列并不是确定是否存在特定的操作系统或设备系列功能的最佳方式。 与其检测操作系统或设备系列（和版本号），不如自行测试功能是否存在（请参阅[条件编译和自适应代码](wpsl-to-uwp-porting-to-a-uwp-project.md)）。 如果你必须请求某个特定操作系统或设备系列，请确保将其用作受支持的最低版本，而不是针对某一版本设计相应测试。

若要定制你的应用的 UI 以适应不同的设备，可以使用我们建议的多种技术。 可以像往常那样继续使用可自动调整大小的元素和动态布局面板。 在 XAML 标记中，继续使用以有效像素（之前称为视图像素）为单位的大小，以便 UI 能适应不同的分辨率和比例系数（请参阅[视图/有效像素、观看距离和比例系数](wpsl-to-uwp-porting-xaml-and-ui.md)）。 并且，通过使用视觉状态管理器的自适应触发器和设置器，让 UI 能适应相应的窗口大小（请参阅 [UWP App 指南](https://msdn.microsoft.com/library/windows/apps/dn894631)）。

但是，在遇到必须检测设备系列的情况时，你可以执行此操作。 在本示例中，我们将使用 [**AnalyticsVersionInfo**](https://msdn.microsoft.com/library/windows/apps/dn960165) 类导航到为移动设备系列定制的页面（如果适用），并且我们保证可通过其他方式回退到默认页面。

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

你的应用还可以通过有效的资源选择因素，确定正在运行它的设备系列。 下面的示例演示了如何强制执行此操作；[**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/br206071) 主题描述了在加载特定于设备系列的资源（基于设备系列规格）时有关该类的更为典型的用例。

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

另请参阅[条件编译和自适应代码](wpsl-to-uwp-porting-to-a-uwp-project.md)。

## <a name="device-status"></a>设备状态

WindowsPhone Silverlight 应用可以使用**Microsoft.Phone.Info.DeviceStatus**类来获取有关在应用运行所在的设备的信息。 尽管 **Microsoft.Phone.Info** 命名空间没有直接的 UWP 等效项，但下面提供了一些可在 UWP 应用中使用的属性和事件，从而无需调用 **DeviceStatus** 类的成员。

| Windows Phone Silverlight                                                               | UWP                                                                                                                                                                                                                                                                                                                                |
|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ApplicationCurrentMemoryUsage** 和 **ApplicationCurrentMemoryUsageLimit** 属性 | [**MemoryManager.AppMemoryUsage**](https://msdn.microsoft.com/library/windows/apps/dn633832) 和 [**AppMemoryUsageLimit**](https://msdn.microsoft.com/library/windows/apps/dn633836) 属性                                                                                                                                    |
| **ApplicationPeakMemoryUsage** 属性                                                 | 使用 Visual Studio 中的内存分析工具。 有关详细信息，请参阅[分析内存使用量](http://msdn.microsoft.com/library/windows/apps/dn645469.aspx)。                                                                                                                                                                          |
| **DeviceFirmwareVersion** 属性                                                      | [**EasClientDeviceInformation.SystemFirmwareVersion**](https://msdn.microsoft.com/library/windows/apps/dn608144) 属性（仅桌面设备系列）                                                                                                                                                                             |
| **DeviceHardwareVersion** 属性                                                      | [**EasClientDeviceInformation.SystemHardwareVersion**](https://msdn.microsoft.com/library/windows/apps/dn608145) 属性（仅桌面设备系列）                                                                                                                                                                             |
| **DeviceManufacturer** 属性                                                         | [**EasClientDeviceInformation.SystemManufacturer**](https://msdn.microsoft.com/library/windows/apps/hh701398) 属性（仅桌面设备系列）                                                                                                                                                                                |
| **DeviceName** 属性                                                                 | [**EasClientDeviceInformation.SystemProductName**](https://msdn.microsoft.com/library/windows/apps/hh701401) 属性（仅桌面设备系列）                                                                                                                                                                                 |
| **DeviceTotalMemory** 属性                                                          | 无等效项                                                                                                                                                                                                                                                                                                                      |
| **IsKeyboardDeployed** 属性                                                         | 无等效项。 此属性为移动设备提供了有关不常用的硬件键盘的信息。                                                                                                                                                                                                        |
| **IsKeyboardPresent** 属性                                                          | 无等效项。 此属性为移动设备提供了有关不常用的硬件键盘的信息。                                                                                                                                                                                                        |
| **KeyboardDeployedChanged** 事件                                                       | 无等效项。 此属性为移动设备提供了有关不常用的硬件键盘的信息。                                                                                                                                                                                                        |
| **PowerSource** 属性                                                                | 无等效项                                                                                                                                                                                                                                                                                                                      |
| **PowerSourceChanged** 事件                                                            | 处理 [**RemainingChargePercentChanged**](https://msdn.microsoft.com/library/windows/apps/jj207240) 事件（仅移动设备系列）。 当 [**RemainingChargePercent**](https://msdn.microsoft.com/library/windows/apps/jj207239) 属性的值 （仅移动设备系列）下降 1% 时引发该事件。 |

## <a name="location"></a>位置

声明了位置功能其应用包清单中的应用上运行时 windows 10，则系统将提示最终用户同意。 因此，如果你的应用显示自己的自定义许可提示，或者如果它提供了一个开/关切换开关，则需要删除它以便仅提示最终用户一次。

## <a name="orientation"></a>方向

**PhoneApplicationPage.SupportedOrientations** 和 **Orientation** 属性的 UWP 应用等效项是应用包清单中的 [**uap:InitialRotationPreference**](https://msdn.microsoft.com/library/windows/apps/dn934798) 元素。 选择 **“应用程序”** 选项卡（如果未选中），并选中 **“支持的旋转”** 下的一个或多个复选框以记录你的首选项。

不过，我们鼓励你设计自己的 UWP 应用的 UI，无论设备方向和屏幕大小如何，都应使其保持美观。 再下个主题[针对外形规格和用户体验进行移植](wpsl-to-uwp-form-factors-and-ux.md)中提供了有关该内容的详细信息。

下一主题是[移植业务和数据层](wpsl-to-uwp-business-and-data.md)。

