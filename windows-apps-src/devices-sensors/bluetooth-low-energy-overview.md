---
title: 低耗电蓝牙
description: 本主题提供 UWP 应用中的蓝牙 LE 的快速概述。
ms.date: 03/19/2017
ms.topic: article
keywords: Windows 10, uwp, 蓝牙, 蓝牙 LE, 低耗电, gatt, gap, 中央, 外围, 客户端, 服务器, 观察程序, 发布者
ms.localizationpriority: medium
ms.openlocfilehash: 4859dfb540b252f379a0ec3cbfe52985c0776fd9
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684847"
---
# <a name="bluetooth-low-energy"></a>低耗电蓝牙
低耗电蓝牙 (LE) 是一种规范，定义高能效设备之间的发现和通信协议。 设备的发现通过通用访问配置文件 (GAP) 协议进行。 发现之后，设备到设备之间的通信通过通用属性 (GATT) 协议进行。 本主题提供 UWP 应用中的蓝牙 LE 的快速概述。 若要查看有关蓝牙 LE 的更多详细信息，请参阅[蓝牙核心规范](https://www.bluetooth.com/specifications/bluetooth-core-specification/)版本 4.0，其中介绍了蓝牙 LE。 

![蓝牙 LE 角色](images/gatt-roles.png)

*Windows 10 1703 版中引入了 GATT 和 GAP 角色*

可以使用以下命名空间在 UWP 应用中实现 GATT 和 GAP 协议。
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows。](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement)

## <a name="central-and-peripheral"></a>中央和外围
发现的两个主要角色称为中央和外围。 一般情况下，Windows 在中央模式下运行，并连接到不同的外围设备。 

## <a name="attributes"></a>属性
在 Windows 蓝牙 API 中看到的一个常见首字母缩写是通用属性 (GATT)。 GATT 配置文件定义两个蓝牙 LE 设备用于进行通信的数据结构和操作模式。 属性是 GATT 的主要构建基块。 属性的主要类型是服务、特征和描述符。 这些属性在客户端和服务器之间以不同方式执行，因此在相关章节中讨论其交互会更加有意义。 

![常用配置文件中的典型属性层次结构](images/gatt-service.png)

*心率服务以 GATT 服务器 API 格式表示*

## <a name="client-and-server"></a>客户端和服务器
建立连接之后，包含数据的设备（通常是小型 IoT 传感器或可穿戴设备）称为服务器。 使用该数据执行功能的设备称为客户端。 例如，Windows 电脑（客户端）从心率监视器（服务器）读取数据以跟踪用户是否以最佳方式进行锻炼。 有关详细信息，请参阅 [GATT 客户端](gatt-client.md)和 [GATT 服务器](gatt-server.md)主题。

## <a name="watchers-and-publishers-beacons"></a>观察程序和发布者（信标）
除了中央和外围角色之外，还有观察者和广播者角色。 广播者通常称为信标，它们不通过 GATT 进行通信，因为它们使用播发数据包中提供的有限空间进行通信。 同样，观察者不必建立连接来接收数据，它会扫描附近的播发。 若要配置 Windows 以观察附近的播发，请使用 [BluetoothLEAdvertisementWatcher](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher) 类。 若要广播信标有效负载，请使用 [BluetoothLEAdvertisementPublisher](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher) 类。 有关详细信息，请参阅[播发](ble-beacon.md)主题。

## <a name="see-also"></a>另请参阅
- [Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile)
- [Windows。](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.advertisement)
- [蓝牙核心规范](https://www.bluetooth.com/specifications/bluetooth-core-specification)
