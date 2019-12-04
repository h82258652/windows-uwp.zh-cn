---
title: 服务要点
description: 本文包含有关 PointOfService UWP API 入门的信息。
ms.date: 12/3/2019
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 4749b66cc5ce2593aead75d65993f70106da7c8b
ms.sourcegitcommit: 2d709ddcc31f52d2a4ace1134aea45057d99a615
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2019
ms.locfileid: "74782577"
---
# <a name="point-of-service-basics"></a>服务要点

此部分包含所有服务点设备类别的通用主题。

|主题 |描述 |
|------|------------|
| [功能声明](pos-basics-capability.md)      | 了解如何将 **pointOfService** 功能添加到应用程序清单。  使用 Windows.Devices.PointOfService 命名空间需要有此功能。  |
| [枚举设备](pos-basics-enumerating.md)        | 了解如何定义用于向系统提供的查询设备的设备选择器，以及如何使用此选择器枚举服务点设备。  |
| [创建设备对象](pos-basics-deviceobject.md)  | 了解如何创建 PointOfService 设备对象（将授予你访问外围设备的只读属性的访问权限）以及如何声明外围设备的独占使用。 |
| [声明和启用](pos-basics-claim.md)  | 了解如何为独占使用和启用 i/o 操作保留 PointOfService 外设。  |
| [与他人共享外围设备](pos-basics-sharing.md) | 了解如何在多台电脑依赖于共享外围设备（而不是连接到每台计算机的专用外围设备）的环境中，与其他计算机共享网络或蓝牙连接的外围网络。
| [PointOfService 端到端](pos-get-started.md)  | 这是一个端到端示例，介绍了如何使用以上示例与 PointOfService 外设交互。 |
|

## <a name="see-also"></a>另请参阅

| 主题   | 描述 |
|:--------|:------------|
| [应用程序分发](../publish/distribute-lob-apps-to-enterprises.md) | 了解将应用分发给企业客户的选项。 |
| [应用程序生命周期](../launch-resume/app-lifecycle.md) | 了解 UWP 应用程序的生命周期，以及 Windows 启动、挂起和恢复应用时会发生的情况。 |
| [应用程序资源](../app-resources/index.md) | 了解如何创作、打包和使用您的应用程序的字符串、图像和文件资源。 |
| [数据绑定](../data-binding/index.md) | 了解如何使用数据绑定在应用的 UI 中显示数据。 |
| [设备枚举](enumerate-devices.md) | 了解如何使用高级枚举技术查找外围设备。|
| [版本自适应应用程序](../debug-test-perf/version-adaptive-apps.md) | 了解如何设计应用程序，使其在多个版本的 Windows 10 上运行。|
|


## <a name="sample-code"></a>示例代码
+ [条形码扫描器示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [银箱示例]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [行显示示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [磁条阅读器示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [POSPrinter 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)
