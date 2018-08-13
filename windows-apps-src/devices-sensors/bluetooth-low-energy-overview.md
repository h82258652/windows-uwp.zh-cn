---
author: msatranjr
title: 低能耗蓝牙
description: 本主题提供蓝牙 LE UWP 应用程序中的简单的概述。
ms.author: misatran
ms.date: 03/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 蓝牙、 蓝牙 LE、 低能源、 gatt、 间隙、 管理中心、 外围设备、 client、 server、 观察程序、 publisher
ms.localizationpriority: medium
ms.openlocfilehash: 0d6cc1fb5a0b3cb96748b99c490b23a9e1df128f
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "304529"
---
# <a name="bluetooth-low-energy"></a>低能耗蓝牙
蓝牙低能源 (LE) 是定义了发现和高级有效设备之间的通信的协议的规范。 设备发现是通过泛型访问配置文件 （间隙） 协议。 发现后设备的通信是通过泛型属性 (GATT) 协议。 本主题提供蓝牙 LE UWP 应用程序中的简单的概述。 有关蓝牙 LE 的更多详细信息，请参阅[蓝牙核心规格](https://www.bluetooth.com/specifications/bluetooth-core-specification)版本 4.0，蓝牙 LE 引入。 

![蓝牙 LE 角色](images/gatt-roles.png)

*Windows 10 版本 1703年中引入 GATT 和间隙角色*

通过使用以下命名空间，可以 UWP 应用程序中实现 GATT 和间隙协议。
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)

## <a name="central-and-peripheral"></a>管理中心和外围
管理中心和外围称为发现的两个主要角色。 一般情况下，Windows 中央模式中运行并连接到各种外围设备。 

## <a name="attributes"></a>属性
您将看到 Windows 蓝牙 Api 中的常见的首字母缩写是泛型属性 (GATT)。 GATT 配置文件定义的数据结构和操作的两个蓝牙 LE 设备进行通信模式。 属性是主要的构建基块的 GATT。 主要类型的属性是服务、 特征和描述符。 这些属性客户端和服务器之间执行不同，使其更多有用，讨论它们相关的各节中的交互。 

![公共配置文件中的典型属性层次结构](images/gatt-service.png)

*GATT 服务器 API 形式表示的心形速率服务*

## <a name="client-and-server"></a>客户端和服务器
已建立的连接后，会在服务器中称为包含的数据 （通常小型 IoT 传感器或可穿戴） 的设备。 客户端就是使用该数据来执行的功能的设备。 例如，Windows PC （客户端） 中读取数据从心形速率监视器 （服务器），用于跟踪用户以最佳方式使用。 有关详细信息，请参阅[GATT 客户端](gatt-client.md)和[GATT 服务器](gatt-server.md)主题。

## <a name="watchers-and-publishers-beacons"></a>观察者和发布者 （信号）
除了管理中心和外围设备角色中，可以使用观察者和广播者角色。 广播公司常被称作信号，它们不通过通信 GATT 因为它们使用通信广告数据包中提供空间有限。 同样，观察者没有建立连接以接收数据，它所扫描的附近广告。 若要配置窗口观察附近广告，使用[BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher)类。 广播信号负载，才能使用[BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher)类。 有关详细信息，请参阅[广告](ble-beacon.md)主题。

## <a name="see-also"></a>另请参阅
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows.Devices.Bluetooth.Advertisement](https://docs.microsoft.com/en-us/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [蓝牙核心规范](https://www.bluetooth.com/specifications/bluetooth-core-specification)