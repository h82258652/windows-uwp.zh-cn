---
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: 校准传感器
description: 基于磁力计的设备中的传感器（指南针、测斜仪和方向传感器）因环境因素需要校准。
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7a93d59a00630c240e74049a9fd98d50f285b0dd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634672"
---
# <a name="calibrate-sensors"></a>校准传感器


**重要的 Api**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Windows.Devices.Sensors.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn895032)

基于磁力计的设备中的传感器（指南针、测斜仪和方向传感器）因环境因素需要校准。 [  **MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552) 枚举可帮助确定设备需要校准时的做法。

## <a name="when-to-calibrate-the-magnetometer"></a>何时校准磁力计

[  **MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552) 枚举包含四个值，它们可帮助你确定运行应用的设备是否需要校准。 如果需要校准设备，则让用户知道需要校准。 但是，不能过于频繁地提示用户校准。 建议仅每 10 分钟一次。

| 值           | 描述    |
| ----------------- | ------------------- |
| **Unknown**     | 传感器驱动程序无法报告当前准确性。 这并不表示不校准设备。 由应用来确定返回 **Unknown** 时的最佳做法。 如果应用依赖于准确的传感器读数，可能需要提示用户校准设备。 |
| **不可靠**  | 目前磁力计高度不准确。 在首次返回此值时，应用应始终要求用户校准。 |
| **估计** | 对于某些应用，数据很准确。 虚拟现实应用（仅需要知道用户是否上下或左右移动设备）可以继续使用而不用校准。 需要绝对方向的应用（如需要知道你的驾驶方向才能提供路线的导航应用）需要校准。 |
| **高**        | 数据精确。 不需要校准，即使对于需要知道绝对方向的应用（如增强现实或导航应用），也是如此。 |