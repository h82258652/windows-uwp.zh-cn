---
description: 与设备本身及其传感器集成的代码涉及到与用户之间的输入和输出。
title: 针对 I/O、设备和应用模型将 Windows 运行时 8.x 移植到 UWP
ms.assetid: bb13fb8f-bdec-46f5-8640-57fb0dd2d85b
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5847553bed563b724bb142f7abe62403fa8ec097
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8922328"
---
# <a name="porting-windows-runtime-8x-to-uwp-for-io-device-and-app-model"></a>针对 I/O、设备和应用模型将 Windows 运行时 8.x 移植到 UWP




上一主题是[移植 XAML 和 UI](w8x-to-uwp-porting-xaml-and-ui.md)。

与设备本身及其传感器集成的代码涉及到与用户之间的输入和输出。 它还可以涉及处理数据。 但是通常不会将此代码视为 UI 层*或*数据层。 此代码包含与振动控制器、加速计、陀螺仪、麦克风和扬声器（与语音识别和合成交叉）、（地理）位置和输入形式（例如触摸、鼠标、键盘和笔）的集成。

## <a name="application-lifecycle-process-lifetime-management"></a>应用程序生命周期（进程周期管理）


对于通用 8.1 应用，在该应用处于非活动状态以及系统引发暂停事件期间会出现 2 秒的“防止误动作窗口”。 将此防止误动作窗口的出现时间用作暂停状态的额外时间并不安全；而对于通用 Windows 平台 (UWP) 应用而言，根本不会存在防止误动作窗口；因为只要某个应用处于非活动状态，便会引发暂停事件。

有关详细信息，请参阅[应用生命周期](https://msdn.microsoft.com/library/windows/apps/mt243287)。

## <a name="background-audio"></a>后台音频


对于[**MediaElement.AudioCategory**](https://msdn.microsoft.com/library/windows/apps/br227352)属性中，适用于 windows 10 应用弃用**ForegroundOnlyMedia**和**BackgroundCapableMedia** 。 改用 Windows Phone 应用商店应用模型。 有关详细信息，请参阅[后台音频](https://msdn.microsoft.com/library/windows/apps/mt282140)。

## <a name="detecting-the-platform-your-app-is-running-on"></a>检测正运行你的应用的平台


应用面向 windows 10 的更改所做的方式。 新增的概念模型是，应用面向通用 Windows 平台 (UWP)，并且可跨所有 Windows 设备运行。 这样它便可以选择充分利用特定设备系列所独有的功能。 特别是，该应用还可以选择自行限制为面向一个或多个设备系列（如果需要）。 有关具体设备系列（以及如何确定要面向哪一个设备系列）的详细信息，请参阅 [UWP 应用指南](https://msdn.microsoft.com/library/windows/apps/dn894631)。

如果你在通用 8.1 应用中具有代码（可检测到运行它的操作系统），你可能需要做一些更改，具体取决于理性逻辑。 如果应用正在通过操作系统传递值，但没有在该系统上执行任何操作，你可能需要继续收集该操作系统的相关信息。

**注意**我们建议你不使用操作系统或设备系列来检测某些功能是否存在。 通常情况下，标识当前操作系统或设备系列并不是确定是否存在特定的操作系统或设备系列功能的最佳方式。 与其检测操作系统或设备系列（和版本号），不如自行测试功能是否存在（请参阅[条件编译和自适应代码](w8x-to-uwp-porting-to-a-uwp-project.md)）。 如果你必须请求某个特定操作系统或设备系列，请确保将其用作受支持的最低版本，而不是针对某一版本设计相应测试。

 

若要定制你的应用的 UI 以适应不同的设备，可以使用我们建议的多种技术。 不过，你也可以像往常那样继续使用可自动调整大小的元素和动态布局面板。 在 XAML 标记中，元素大小继续以有效像素（之前称为视图像素）为单位，以便 UI 能适应不同的分辨率和比例因子（请参阅[有效像素、观看距离和比例因子](w8x-to-uwp-porting-xaml-and-ui.md)）。 并且，通过使用视觉状态管理器的自适应触发器和设置器，让 UI 能适应相应的窗口大小（请参阅 [UWP App 指南](https://msdn.microsoft.com/library/windows/apps/dn894631)）。

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

另请参阅[条件编译和自适应代码](w8x-to-uwp-porting-to-a-uwp-project.md)。

## <a name="location"></a>位置


声明了位置功能其应用包清单中的应用上运行时 windows 10，则系统将提示最终用户同意。 这是 true 指示应用是否在 Windows Phone 应用商店应用或 windows 10 应用。 因此，如果你的应用显示自己的自定义许可提示，或者如果它提供了一个开/关切换开关，则需要删除它以便仅提示最终用户一次。

 

 




