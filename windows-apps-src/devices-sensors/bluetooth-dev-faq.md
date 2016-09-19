---
author: msatranjr
title: "蓝牙开发人员常见问题"
description: "本文包含与 UWP 蓝牙 API 相关的常见问题解答。"
translationtype: Human Translation
ms.sourcegitcommit: e4c95448262c6c62956fcb50581c98d8c34d6dc0
ms.openlocfilehash: 2afc1250aa9d7a6cf6c9c8cb45dd2379b9d36984

---
# 蓝牙开发人员常见问题

本文包含 UWP 蓝牙 API 的常见问题解答。

## 为什么我的蓝牙 LE 设备在断开连接后会停止响应？

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

## 在使用蓝牙设备前是否要先配对？

蓝牙 RFCOMM（传统）设备无需执行此操作。 从 Windows 10 版本 1607 开始，只需查询附近设备即可连接。 更新后的 [RFCOMM 聊天示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BluetoothRfcommChat)展示了此项功能。 

低耗电蓝牙（GATT 客户端）不支持此功能，因此仍需要通过“设置”页面或使用 [Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) API 进行配对才能访问这些设备。




<!--HONumber=Aug16_HO3-->


