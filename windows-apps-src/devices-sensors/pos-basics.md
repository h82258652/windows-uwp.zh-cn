---
author: TerryWarwick
title: 服务点入门
description: 本文包含有关 PointOfService UWP API 入门的信息。
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: a0583adbcef9e45dfe0b2e56e03ce7e0451ac5bb
ms.sourcegitcommit: ce45a2bc5ca6794e97d188166172f58590e2e434
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2018
ms.locfileid: "1983540"
---
# <a name="getting-started-with-point-of-service"></a>服务点入门

此部分包含所有服务点设备类别的通用主题。

|主题 |描述 |
|------|------------|
| [功能声明](pos-basics-capability.md)      | 了解如何将 **pointOfService** 功能添加到应用程序清单。  使用 Windows.Devices.PointOfService 命名空间需要有此功能。  |
| [枚举设备](pos-basics-enumerating.md)        | 了解如何定义用于向系统提供的查询设备的设备选择器，以及如何使用此选择器枚举服务点设备。  |
| [创建设备对象](pos-basics-deviceobject.md)  | 了解如何创建 PointOfService 设备对象（将授予你访问外围设备的只读属性的访问权限）以及如何声明外围设备的独占使用。 |
| [声明独占使用设备 ](pos-basics-claim.md)  | 了解如何通过 PointOfService 声明模型保留 PointOfService 外围设备以独占使用，同时允许同一台计算机上的其他应用程序在需要独占使用时访问 PointOfService 外围设备。  |
|

## <a name="see-also"></a>另请参阅
[Windows.Devices.PointOfService 入门](pos-get-started.md)


## <a name="sample-code"></a>示例代码
+ [条形码扫描仪示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [收银机示例]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [行显示示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [磁条阅读器示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [POSPrinter 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

