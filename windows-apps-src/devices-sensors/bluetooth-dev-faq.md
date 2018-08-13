---
author: msatranjr
title: 蓝牙开发人员常见问题
description: 本文包含与 UWP 蓝牙 API 相关的常见问题解答。
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: c0af6a19e17a62ed82c32e68ea1732e1f51d4641
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "301923"
---
# <a name="bluetooth-developer-faq"></a>蓝牙开发人员常见问题

本文包含 UWP 蓝牙 API 的常见问题解答。

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>是否使用哪些 Api？ 蓝牙经典 (RFCOMM) 或蓝牙低能源 (GATT)？
我们保留此答案典型相对于 Windows 的区别有各种论述联机了本常规的主题。 下面是一些一般原则：

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>蓝牙 LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

您的支持蓝牙低能源设备与之通信时，请使用 GATT Api。 如果您正在使用案例少数情况下，低带宽或需要低电源，蓝牙低能源答案。 主命名空间包含此功能是[Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)。 

**何时不使用蓝牙 LE**
- 高带宽，高频率方案。 如果您需要不断地保留与大量数据同步，请考虑使用蓝牙经典或甚至 WiFi。 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>蓝牙经典 (Windows.Devices.Bluetooth.Rfcomm)

RFCOMM Api 为开发人员提供了执行 bi 方向串行端口样式通信套接字。 一旦您套接字，写入和读取从它的方法是相当标准配置。 [Rfcomm 聊天示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat)中，将显示此实现。 

**何时不使用蓝牙 Rfcomm** 
- 通知。 蓝牙 GATT 协议具有特定命令为此，并且将导致显著减少电源绘制和更快地响应时间。 
- 检查的邻近度或状态检测。 最好使用[广告 Api](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement) ，并通过蓝牙 LE 连接。 


## <a name="why-does-my-bluetooth-le-device-stop-responding-after-a-disconnect"></a>为什么我的蓝牙 LE 设备在断开连接后会停止响应？

发生这种情况的常见原因是远程设备丢失了配对信息。 许多较早的蓝牙设备不需要身份验证。 为了保护用户，在“设置”应用中执行的所有配对规则都需要身份验证，但某些设备不知道如何处理。 

从 Windows 10 版本 1511 开始，开发人员可以控制配对规则。 [设备枚举和配对示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing)详细介绍了关联新设备的各个方面。

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

蓝牙 RFCOMM（传统）设备无需执行此操作。 从 Windows 10 版本 1607 开始，只需查询附近设备即可连接。 更新后的 [RFCOMM 聊天示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat)展示了此项功能。 

**（14393 和下）** 此功能不可用的蓝牙低能源 （GATT 客户端），因此您仍然可以进行配对通过设置页或使用[Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) Api 中订单访问这些设备。

**（15030 及以上）** 不再需要配对蓝牙设备。 使用新的异步 Api 以查询远程设备的当前状态，如 GetGattServicesAsync 和 GetCharacteristicsAsync。 [客户端文档](gatt-client.md)的详细信息，请参阅。 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>时应我与设备配对之前与之通信？
通常，如果您需要的受信任的长期债券与设备时，配对与之 （定向至设置页或使用设备枚举和配对 Api）。 如果您只需关闭阅读信息的设备的公开公开 （温度感应器或信号），然后连接或侦听广告不做任何努力与设备。 因为配对不支持的设备的主机，这将从长远来看防止互操作性问题。 

## <a name="do-all-windows-devices-support-peripheral-role"></a>是否所有 Windows 设备都支持外围角色？

没有 – 这是一个硬件相关功能，但提供方法 (BluetoothAdapter.IsPeripheralRoleSupported) 查询是否受或不支持。  当前受支持的设备包括 Windows Phone 上 8992 + 和 RPi3 (Windows IoT)。 

## <a name="can-i-access-these-apis-from-win32"></a>可以从 Win32 访问这些 Api？

是，所有这些 Api 应该会正常工作。 此博客详细调用[Windows Api 从桌面应用程序](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/)的方式。 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>是否应该 *-此处插入 SKU-* 上存在此功能？

**蓝牙 LE**: 是，所有功能处于 OneCore 和可正常运行的蓝牙 LE 堆栈具有最新的设备上。 
> 警告： 外围角色是相关的硬件和某些 Windows Server 版本不支持蓝牙。 

**蓝牙 BR/EDR （经典）**： 存在一些变体，但一般说来，它们具有非常类似的配置文件级别的支持。 请参阅文档上[RFCOMM](send-or-receive-files-with-rfcomm.md)和这些支持的配置文件的文档的[PC](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles)和[电话](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles)

