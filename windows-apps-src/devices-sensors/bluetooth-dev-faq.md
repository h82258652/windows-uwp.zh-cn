---
author: msatranjr
title: "蓝牙开发人员常见问题"
description: "本文包含与 UWP 蓝牙 API 相关的常见问题解答。"
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.assetid: e7dee32d-3756-430d-a026-32c1ee288a85
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 7394d211b580ad82689a79e7cbe4eb4dbf545f46
ms.lasthandoff: 02/08/2017

---
# <a name="bluetooth-developer-faq"></a>蓝牙开发人员常见问题

本文包含 UWP 蓝牙 API 的常见问题解答。

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

低耗电蓝牙（GATT 客户端）不支持此功能，因此仍需要通过“设置”页面或使用 [Windows.Devices.Enumeration](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.aspx) API 进行配对才能访问这些设备。


