---
ms.assetid: 404783BA-8859-4BFB-86E3-3DD2042E66F5
title: 蓝牙
description: 本部分包含有关如何将蓝牙集成到通用 Windows 平台 (UWP) 应用的文章，包括如何使用 RFCOMM、GATT 和低功耗 (LE) 广告。
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: aad368adf500c5968bf6374a5855a7e0dab0a044
ms.sourcegitcommit: 015291bdf2e7d67076c1c85fc025f49c840ba475
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469562"
---
# <a name="bluetooth"></a>蓝牙
本节包含有关如何将蓝牙集成到通用 Windows 平台 (UWP) 应用的文章。 可以选择在应用中实现两种不同的蓝牙技术。

> [!Important]
> 必须在*appxmanifest.xml*中声明 "蓝牙" 功能。
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

## <a name="classic-bluetooth-rfcomm"></a>经典蓝牙 (RFCOMM)
出现蓝牙 LE 之前，设备常使用此协议进行蓝牙通信。 此协议十分简单，可用于设备到设备的通信而无需节能。 有关此协议的详细信息（包括代码示例），请参阅[蓝牙 RFCOMM](send-or-receive-files-with-rfcomm.md) 主题。

## <a name="bluetooth-low-energy-le"></a>低耗电蓝牙 (LE)
低耗电蓝牙 (LE) 是一种规范，定义具有高能效使用要求的设备之间的发现和通信协议。 有关详细信息（包括代码示例），请参阅[低耗电蓝牙](bluetooth-low-energy-overview.md)主题。

## <a name="see-also"></a>另请参阅
- [蓝牙开发人员常见问题](bluetooth-dev-faq.md)