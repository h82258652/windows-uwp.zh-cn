---
title: 蓝牙开发人员常见问题
description: 本文包含与 UWP 蓝牙 API 相关的常见问题解答。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
ms.localizationpriority: medium
ms.openlocfilehash: 03b72b5722a3ece0165fc63e7ce4abc1238bc135
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8467780"
---
# <a name="bluetooth-developer-faq"></a>蓝牙开发人员常见问题

本文包含 UWP 蓝牙 API 的常见问题解答。

## <a name="what-apis-do-i-use-bluetooth-classic-rfcomm-or-bluetooth-low-energy-gatt"></a>是否使用哪些 Api？ 蓝牙经典 (RFCOMM) 或低能耗蓝牙 (GATT)？
让我们保留此应答完全相对于窗口的区别有各种讨论联机围绕此常规主题。 下面是一些一般指南：

### <a name="bluetooth-le-windowsdevicesbluetoothgenericattributeprofile"></a>蓝牙 LE (Windows.Devices.Bluetooth.GenericAttributeProfile)

当你正在与支持蓝牙低功耗设备通信时，请使用 GATT Api。 如果你正在使用情况少数情况下，低带宽或低功耗越低耗电 Bluetooth 答案。 包含此功能的主要命名空间是[Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)。 

**何时不使用蓝牙 LE**
- 高带宽，高频方案。 如果你需要不断保持同步大量数据，请考虑使用蓝牙经典或甚至 WiFi。 

### <a name="bluetooth-classic-windowsdevicesbluetoothrfcomm"></a>蓝牙经典 (Windows.Devices.Bluetooth.Rfcomm)

RFCOMM Api 为开发人员提供了套接字来执行 bi 方向串行端口样式的通信。 一旦你已将套接字，写入和读取它的方法是相当标准。 此实现的[Rfcomm 聊天示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat)中显示。 

**何时不使用蓝牙 Rfcomm** 
- 通知。 蓝牙 GATT 协议具有特定命令为此，将导致显著减少功耗以及更快的响应时间。 
- 检查邻近感应或不存在的检测。 最好使用[广告 Api](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement) ，并通过蓝牙 LE 连接。 


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

**（14393 和更低版本）** 此功能不可用的低耗电蓝牙 （GATT 客户端），因此仍需要与对通过设置页面，或使用[Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) Api 在订单访问这些设备。

**（15030 和更高版本）** 不再需要配对蓝牙设备。 使用新的异步 Api 将 GetGattServicesAsync 和 GetCharacteristicsAsync 等才能查询远程设备的当前状态。 查看更多详细信息，[客户端文档](gatt-client.md)。 

## <a name="when-should-i-pair-with-a-device-before-communicating-with-it"></a>何时应我与设备配对与之通信前？
通常情况下，如果你需要与设备的受信任的、 长期的证券，与之配对 （定向至设置页面或使用设备枚举和配对 Api）。 如果你只需要关闭阅读信息的设备公开公开 （温度传感器或信标），然后连接或侦听广告而不进行任何工作以与设备配对。 这将从长远来看防止互操作性问题，因为主机的设备不支持配对。 

## <a name="do-all-windows-devices-support-peripheral-role"></a>所有 Windows 设备是否都支持外设角色？

没有 – 这是一种依赖于硬件功能，但一种方法提供 (BluetoothAdapter.IsPeripheralRoleSupported) 查询是否或不受支持。  当前受支持的设备包括 Windows Phone 上 8992 + 和 RPi3 (Windows IoT)。 

## <a name="can-i-access-these-apis-from-win32"></a>我是否可以从 Win32 访问这些 Api？

是的应在所有这些 Api。 此博客详细介绍的方法调用[从桌面应用程序的 Windows Api](https://blogs.windows.com/buildingapps/2017/01/25/calling-windows-10-apis-desktop-application/)。 
## <a name="is-this-functionality-supposed-to-exist-on--insert-sku-here-"></a>此功能应当存在于 *-此处插入 SKU-*？

**蓝牙 LE**： 所有功能是，在 OneCore 并应为带有正常运行的蓝牙 LE 堆栈的最新设备上可用。 
> 警告： 外围设备的角色是取决于硬件和某些 Windows Server 版本不支持蓝牙。 

**蓝牙 BR/EDR （传统）**： 存在一些变体，但一般说来，它们具有非常类似的配置文件级别支持。 对于[电脑](https://support.microsoft.com/en-us/help/10568/windows-10-supported-bluetooth-profiles)和[手机](https://support.microsoft.com/en-us/help/10569/windows-10-mobile-supported-bluetooth-profiles)上[RFCOMM](send-or-receive-files-with-rfcomm.md)和这些受支持的配置文件文档查看文档。

