---
author: msatranjr
title: "蓝牙广告"
description: "本部分包含有关如何通过 AdvertisementWatcher 和 AdvertisementPublisher API 的用户将蓝牙低功耗 (LE) 广告集成到通用 Windows 平台 (UWP) 应用的文章。"
ms.sourcegitcommit: 62e97bdb8feb78981244c54c76a00910a8442532
ms.openlocfilehash: a419ad04fe4f21867f2f1bd1664fbce39a7da792

---

# 蓝牙广告

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** 重要的 API ** 

-   [**Windows.Devices.Bluetooth.Advertisement**](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.aspx)

本文概述了适用于通用 Windows 平台 (UWP) 应用的蓝牙广告（信标）。  

## 概述

开发人员可以使用广告 API 执行以下两项主要功能：

-   [广告观察程序](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.aspx)：侦听附近信标并根据负载或邻近感应将它们过滤掉。  
-   [广告发布程序](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher.aspx)：定义负载使 Windows 能够以开发人员的名义发布广告。  

Github 上的[蓝牙广告示例](http://go.microsoft.com/fwlink/p/?LinkId=619990)中有完整的示例代码。



<!--HONumber=Jun16_HO4-->


