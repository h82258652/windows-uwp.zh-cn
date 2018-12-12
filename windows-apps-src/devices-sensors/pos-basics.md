---
title: 服务点入门
description: 本文包含有关 PointOfService UWP API 入门的信息。
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 1b4ff9443c40cf44e171bf898b627de3e2819034
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8939925"
---
# <a name="getting-started-with-point-of-service"></a>服务点入门

## <a name="pointofservice-basics"></a>PointOfService 基础知识

此部分包含所有服务点设备类别的通用主题。

|主题 |描述 |
|------|------------|
| [功能声明](pos-basics-capability.md)      | 了解如何将 **pointOfService** 功能添加到应用程序清单。  使用 Windows.Devices.PointOfService 命名空间需要有此功能。  |
| [枚举设备](pos-basics-enumerating.md)        | 了解如何定义用于向系统提供的查询设备的设备选择器，以及如何使用此选择器枚举服务点设备。  |
| [创建设备对象](pos-basics-deviceobject.md)  | 了解如何创建 PointOfService 设备对象（将授予你访问外围设备的只读属性的访问权限）以及如何声明外围设备的独占使用。 |
| [声明和启用 ](pos-basics-claim.md)  | 了解如何保留 PointOfService 外围设备以独占使用，并启用 I/O 操作。  |
| [与他人共享外设](pos-basics-sharing.md) | 了解如何在多台电脑依赖于共享外围设备而不是专用的外设连接到每台计算机的环境中的其他计算机共享网络或连接蓝牙外围设备。
| [PointOfService 端到端](pos-get-started.md)  | 这是如何与利用以上示例中的 PointOfService 外设进行交互的端到端示例。 |
|

## <a name="see-also"></a>另请参阅

| 主题   | 描述 |
|:--------|:------------|
| [应用程序的分发](../publish/distribute-lob-apps-to-enterprises.md) | 了解有关分发你的企业客户的应用的选项。 |
| [应用程序的生命周期](../launch-resume/app-lifecycle.md) | 了解如何在 UWP 应用程序生命周期和 Windows 启动、 暂停、 和恢复你的应用时会发生什么情况。 |
| [应用程序资源](../app-resources/index.md) | 了解如何创作、 打包和使用你的应用的字符串、 图像和文件资源。 |
| [数据绑定](../data-binding/index.md) | 了解如何使用数据绑定以在你的应用的 UI 中显示数据。 |
| [设备枚举](enumerate-devices.md) | 了解使用高级枚举技术，以查找外围设备。|
| [版本自适应应用程序](../debug-test-perf/version-adaptive-apps.md) | 了解如何设计你的应用，以便它在多个版本的 Windows 10 上运行。|
|


## <a name="sample-code"></a>示例代码
+ [条形码扫描仪示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [收银机示例]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [行显示示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [磁条阅读器示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [POSPrinter 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

