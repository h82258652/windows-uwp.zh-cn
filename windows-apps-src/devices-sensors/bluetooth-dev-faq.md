---
title: 蓝牙开发人员常见问题
description: 本文包含与 UWP 蓝牙 API 相关的常见问题解答。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 584d327fc4882db6d3bf8d0cfd2a84b13023c6f4
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684843"
---
# <a name="bluetooth-developer-faq"></a>蓝牙开发人员常见问题

本文包含 UWP 蓝牙 API 的常见问题解答。

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>使用哪些 API？ 经典蓝牙 (RFCOMM) 还是低耗电蓝牙 (GATT)？
围绕这一常规主题存在多种线上讨论，因此我们直接从与 Windows 相关的差异出发来寻找答案。 下面是一些常规指南：

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>蓝牙 LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

使用支持低耗电蓝牙的设备进行通信时，请使用 GATT API。 如果用例不常发生、带宽较低或需要电量低，则会出现蓝牙低能量问题。 包含此功能的主命名空间是 [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)。 

**何时不使用蓝牙 LE**
- 高带宽、高频率条件下。 如果需要持续与大量数据保持同步，请考虑使用经典蓝牙，甚至可以使用 WiFi。 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>经典蓝牙 (Windows.Devices.Bluetooth.Rfcomm)

RFCOMM Api 为开发人员提供了用于执行双向串行端口样式通信的套接字。 获取套接字后，编写和读取的方法相当标准。 [Rfcomm 聊天示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat)中演示了此套接字的实现。 

**何时不使用 Bluetooth Rfcomm** 
- 通知。 蓝牙 GATT 协议具有用于此方面的特定命令，功耗显著降低并且响应时间更快。 
- 邻近感应检查或存在检测。 更好的做法是使用[播发 API](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement) 并通过蓝牙 LE 进行连接。 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>为什么我的蓝牙 LE 设备在断开连接后会停止响应？

出现这种情况的最常见原因是远程设备丢失了配对信息。 大量旧的蓝牙设备不需要进行身份验证。 若要保护用户，从设置应用程序中执行的所有配对事务都需要进行身份验证，而某些设备并不考虑这一点。 

从 Windows 10 版本1511开始，开发人员可以控制配对握手。 [设备枚举和配对示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing)详细介绍了关联新设备的各个方面。

在本示例中，我们首先配对不加密的设备。 请注意，这仅在远程设备不需要加密或身份验证也可运行时才有效。

```csharp
// Get ceremony type and protection level selections
// You must select at least ConfirmOnly or the pairing attempt will fail
    DevicePairingKinds ceremonySelected = DevicePairingKinds.ConfirmOnly;

//  Workaround remote devices losing pairing information
    DevicePairingProtectionLevel protectionLevel = DevicePairingProtectionLevel.None

    DeviceInformationCustomPairing customPairing = deviceInfoDisp.DeviceInformation.Pairing.Custom;

// Declare an event handler - you don't need to do much in PairingRequestedHandler since the ceremony is "None"
    customPairing.PairingRequested += PairingRequestedHandler;
    DevicePairingResult result = await customPairing.PairAsync(ceremonySelected, protectionLevel);
```

## <a name="do-i-have-to-pair-bluetooth-devices-before-using-them"></a>在使用蓝牙设备前是否要先配对？

如果使用 Bluetooth RFCOMM （经典），则无需对设备进行配对。 从 Windows 10 版本 1607 开始，只需查询附近设备即可连接。 更新后的 [RFCOMM 聊天示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat)展示了此项功能。 

**（14393 及更低版本）** 低耗电蓝牙（GATT 客户端）不支持此功能，因此仍需要通过“设置”页面或使用 [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) API 进行配对才能访问这些设备。

**（15030 及更高版本）** 不再需要对蓝牙设备进行配对。 使用新的异步 API（如 GetGattServicesAsync 和 GetCharacteristicsAsync）查询远程设备的当前状态。 有关更多详细信息，请参阅[客户端文档](gatt-client.md)。 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>在与设备进行通信之前，应何时与它配对？
通常，如果需要与设备进行受信任的长期绑定，请将用户定向到 "设置" 页或使用设备枚举和配对 Api，使其与设备配对。 如果只需从公开公开的设备读取信息（温度传感器或信标），请连接或侦听广告，无需对设备进行配对。 这会阻止长时间运行中的互操作性问题，因为大量设备不支持配对。 

## <a name="do-all-windows-devices-support-peripheral-role"></a>所有 Windows 设备是否都支持外设角色？

不相同。 这是一个与硬件相关的功能，但提供了一个方法 BluetoothAdapter. IsPeripheralRoleSupported，用于查询是否支持此功能。  当前支持的设备包括 8992+ 上的 Windows Phone 和 RPi3 (Windows IoT)。 

## <a name="can-i-access-these-apis-from-win32"></a>是否可以从 Win32 访问这些 API？

可以，所有这些 API 都应正常工作。 本博客详细介绍了[从桌面应用程序调用 Windows API](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/) 的方式。 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>此功能是否在 *-在此处插入 SKU-* 上存在？

**蓝牙 LE**：是的，所有功能都处于 OneCore 中，应在带有正常运行的蓝牙 LE 堆栈的最新设备上可用。 
> 警告：外围角色依赖于硬件，某些 Windows Server 版本不支持蓝牙。 

**蓝牙 BR/EDR （经典）** ：存在一些变体，但它们主要具有非常相似的配置文件级别支持。 请参阅[RFCOMM](send-or-receive-files-with-rfcomm.md)上的文档和适用于[电脑](https://support.microsoft.com/help/10568/windows-10-supported-bluetooth-profiles)和[手机](https://support.microsoft.com/help/10569/windows-10-mobile-supported-bluetooth-profiles)的这些受支持的配置文件文档
