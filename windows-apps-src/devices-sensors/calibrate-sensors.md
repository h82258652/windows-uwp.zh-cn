---
author: DBirtolo
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: "校准传感器"
description: "基于磁力计的设备中的传感器（指南针、测斜仪和方向传感器）因环境因素需要校准。"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 8b669d7f939da9ee93e5a49d2f6434d5573e23c0

---
# 校准传感器

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** 重要的 API **

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Windows.Devices.Sensors.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn895032)

基于磁力计的设备中的传感器（指南针、测斜仪和方向传感器）因环境因素需要校准。 [**MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552) 枚举可帮助确定设备需要校准时的做法。

## 何时校准磁力计

[**MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552) 枚举包含四个值，它们可帮助你确定运行应用的设备是否需要校准。 如果需要校准设备，则让用户知道需要校准。 但是，不能过于频繁地提示用户校准。 建议仅每 10 分钟一次。

| 值 | 描述 |-----------------|-------------------| | **Unknown** | 传感器驱动程序无法报告当前准确性。 这并不表示不校准设备。 由应用来确定返回 **Unknown** 时的最佳做法。 如果应用依赖于准确的传感器读数，可能需要提示用户校准设备。 | | **Unreliable** | 目前磁力计高度不准确。 在首次返回此值时，应用应始终要求用户校准。 | | **Approximate** | 对于某些应用程序，数据足够准确。 虚拟现实应用（仅需要知道用户是否上下或左右移动设备）可以继续使用而不用校准。 需要绝对方向的应用（如需要知道你的驾驶方向才能提供路线的导航应用）需要校准。 | | **High** | 数据精确。 不需要校准，即使对于需要知道绝对方向的应用（如增强现实或导航应用），也是如此。 |

## 如何校准磁力计

此简短视频概述了如何校准磁力计。<iframe src="https://hubs-video.ssl.catalog.video.msn.com/embed/727bd0e3-9116-49c3-8af6-0b4339324b71/IA?csid=ux-en-us&MsnPlayerLeadsWith=html&PlaybackMode=Inline&MsnPlayerDisplayShareBar=false&MsnPlayerDisplayInfoButton=false&iframe=true&QualityOverride=HD" width="720" height="405" allowFullScreen="true" frameBorder="0" scrolling="no">一份开发人员备忘录 - 传感器校准</iframe>






<!--HONumber=Aug16_HO3-->


