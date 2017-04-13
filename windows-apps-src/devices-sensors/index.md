---
author: DBirtolo
ms.assetid: 0b891f63-02fa-4c30-b307-9fbcccac5caa
title: "设备、传感器和电源"
description: "为了向用户提供丰富的体验，你可能会发现有必要将外部设备或传感器集成到你的应用中。"
ms.author: dbirtolo
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 7915f626d5e86aed0c47f4d2f1efc61348385ba8
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="devices-sensors-and-power"></a>设备、传感器和电源

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

为了向用户提供丰富的体验，你可能会发现有必要将外部设备或传感器集成到你的应用中。 下面是一些功能示例，你可以使用本节所述技术将其添加到你的应用中。

-   提供增强的打印体验
-   将动作和方向传感器集成到你的游戏中
-   直接连接到设备或通过网络协议连接

| 主题 | 说明 |
|-------|-------------|
| [启用设备功能](enable-device-capabilities.md) | 本教程介绍如何在 Microsoft Visual Studio 中声明设备功能。 这允许你的应用使用相机、麦克风、位置传感器以及其他设备。 | 
| [为 Windows IoT 启用用户模式访问](enable-usermode-access.md) | 本教程介绍如何对 Windows 10 IoT 核心版上的 GPIO、I2C、SPI 和 UART 启用用户模式访问。 |
| [枚举设备](enumerate-devices.md) | 枚举命名空间可以让你找到内部连接到系统的、外部连接的或通过无线或网络协议可检测到的设备。 |
| [设备配对](pair-devices.md) | 某些设备需要先进行配对，然后才能使用。 [<strong>Windows.Devices.Enumeration</strong>](https://msdn.microsoft.com/library/windows/apps/BR225459) 命名空间支持三种配对设备的不同方式。 |
| [带外配对](out-of-band-pairing.md) | 本部分介绍了带外配对如何支持应用在无需发现的情况下也能连接到某些设备。 | 
| [传感器](sensors.md) | 传感器使你的应用了解它周围的设备和外界之间的关系。 传感器可以告知你的应用设备的方向、定位和移动。 |
| [蓝牙](bluetooth.md) | 本部分包含有关如何将蓝牙集成到通用 Windows 平台 (UWP) 应用的文章，包括如何使用 RFCOMM、GATT 和低功耗 (LE) 广告。 | 
| [打印和扫描](printing-and-scanning.md) | 本部分介绍了如何从你的通用 Windows 应用进行打印和扫描。 | 
| [3D 打印](3d-printing.md) | 本部分介绍了如何在通用 Windows 应用中使用 3D 打印功能。 |
| [创建 NFC 智能卡应用](host-card-emulation.md) | Windows Phone 8.1 支持的 NFC 卡仿真应用使用基于 SIM 卡的安全元素，但该模型需要安全付款应用与移动网络运营商 (MNO) 进行密切合作。 这限制了未与 MNO 密切合作的其他商户或开发人员提供的各种可能的支付解决方案。 在 Windows 10 移动版中，我们引入了称为主机卡仿真 (HCE) 的新的卡仿真技术。 HCE 技术使你的应用可以直接与 NFC 读卡器通信。 本主题演示了主机卡仿真 (HCE) 在 Windows 10 移动版设备上的工作原理以及如何开发 HCE 应用，以便你的客户无需与 MNO 协作即可通过他们的手机而非物理卡访问你的服务。 |
| [获取电池信息](get-battery-info.md) | 了解如何使用 [<strong>Windows.Devices.Power</strong>](https://msdn.microsoft.com/library/windows/apps/Dn895017) 命名空间中的 API 获取电池的详细信息。 |

