---
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: 校准传感器
description: 基于磁力计的设备中的传感器（指南针、测斜仪和方向传感器）因环境因素需要校准。
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 21e902daac01d8ed2645625320dec27bf7805fba
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370315"
---
# <a name="calibrate-sensors"></a>校准传感器


**重要的 Api**

-   [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors)
-   [**Windows.Devices.Sensors.Custom**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Custom)

基于磁力计的设备中的传感器（指南针、测斜仪和方向传感器）因环境因素需要校准。 [  **MagnetometerAccuracy**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) 枚举可帮助确定设备需要校准时的做法。

## <a name="when-to-calibrate-the-magnetometer"></a>何时校准磁力计

[  **MagnetometerAccuracy**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) 枚举包含四个值，它们可帮助你确定运行应用的设备是否需要校准。 如果需要校准设备，则让用户知道需要校准。 但是，不能过于频繁地提示用户校准。 建议仅每 10 分钟一次。

| ReplTest1           | 描述    |
| ----------------- | ------------------- |
| **Unknown**     | 传感器驱动程序无法报告当前准确性。 这并不表示不校准设备。 由应用来确定返回 **Unknown** 时的最佳做法。 如果应用依赖于准确的传感器读数，可能需要提示用户校准设备。 |
| **不可靠**  | 目前磁力计高度不准确。 在首次返回此值时，应用应始终要求用户校准。 |
| **Approximate** | 对于某些应用，数据很准确。 虚拟现实应用（仅需要知道用户是否上下或左右移动设备）可以继续使用而不用校准。 需要绝对方向的应用（如需要知道你的驾驶方向才能提供路线的导航应用）需要校准。 |
| **高**        | 数据精确。 不需要校准，即使对于需要知道绝对方向的应用（如增强现实或导航应用），也是如此。 |