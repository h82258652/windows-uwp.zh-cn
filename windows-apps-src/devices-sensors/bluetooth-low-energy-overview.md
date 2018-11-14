---
author: msatranjr
title: 低能耗蓝牙
description: 本主题提供在 UWP 应用中的蓝牙 LE 概览。
ms.author: misatran
ms.date: 03/15/2017
ms.topic: article
keywords: windows 10，uwp，蓝牙、 蓝牙 LE、 低功耗、 gatt、 间隙、 中央、 外围设备，客户端、 服务器、 观察程序，发布者
ms.localizationpriority: medium
ms.openlocfilehash: 9e5bef16c76ee14c2abb7a5a41ab02d150a97333
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6263041"
---
# <a name="bluetooth-low-energy"></a>低能耗蓝牙
蓝牙低功耗 (LE) 是定义用于发现和高效的设备之间通信协议的规范。 发现设备通过通用的访问权限配置文件 （间隙） 协议完成。 后发现，通过通用属性 (GATT) 协议完成设备到设备通信。 本主题提供在 UWP 应用中的蓝牙 LE 概览。 蓝牙 LE 有关的更多详细信息，请参阅的[蓝牙核心规范](https://www.bluetooth.com/specifications/bluetooth-core-specification)版本 4.0，蓝牙 LE 引入。 

![蓝牙 LE 角色](images/gatt-roles.png)

*Windows 10 版本 1703年中引入了 GATT 和间隙角色*

GATT 和间隙协议可以通过使用以下命名空间实现 UWP 应用中。
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>中心和外围设备
发现的两个主要角色称为中亚和外围设备。 一般情况下，Windows 在中央模式下运行，并连接到各种外围设备。 

## <a name="attributes"></a>属性
常见的首字母缩略词你将看到 Windows 蓝牙 api 是通用特性 (GATT)。 GATT 配置文件定义的数据结构和模式的操作的两个蓝牙 LE 设备进行通信。 该属性是 GATT 的主要构建基块。 主要类型的属性是服务、 特征和描述符。 这些属性的客户端和服务器之间执行不同的方式，使其更有用，以讨论其相关的部分中的交互。 

![常见的配置文件中的典型属性层次结构](images/gatt-service.png)

*GATT 服务器 API 形式表示心率服务*

## <a name="client-and-server"></a>客户端和服务器
建立连接后，包含的数据 （通常较小 IoT 传感器或可穿戴设备） 的设备称为服务器。 使用该数据来执行某项功能的设备称为客户端。 例如，在 Windows 电脑 （客户端） 读取数据从心率显示器 （服务器） 来跟踪用户以最佳方式处理。 有关详细信息，请参阅的[GATT 客户端](gatt-client.md)和[GATT 服务器](gatt-server.md)主题。

## <a name="watchers-and-publishers-beacons"></a>观察程序和发布者 （信标）
除了中亚和外围设备的角色，还有观察者和广播角色。 广播公司通常称为信标，它们不会传达通过 GATT 因为它们使用有限的空间中的广告数据包提供进行通信。 同样，观察者不需要建立连接以接收数据，它会扫描附近的广告。 若要将 Windows 配置为观察附近的广告，使用[BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher)类。 为了广播信标负载，使用[BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher)类。 有关详细信息，请参阅[广告](ble-beacon.md)主题。

## <a name="see-also"></a>另请参阅
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [蓝牙核心规范](https://www.bluetooth.com/specifications/bluetooth-core-specification)