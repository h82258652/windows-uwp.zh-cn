---
title: 服务点入门
description: 本文包含有关 PointOfService UWP API 入门的信息。
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: f1e58dbf8bae22df0652ada6ff8aafff6d30e8aa
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63817394"
---
# <a name="getting-started-with-point-of-service"></a>服务点入门

## <a name="pointofservice-basics"></a>PointOfService 基础知识

此部分包含所有服务点设备类别的通用主题。

|主题 |描述 |
|------|------------|
| [功能声明](pos-basics-capability.md)      | 了解如何将 **pointOfService** 功能添加到应用程序清单。  使用 Windows.Devices.PointOfService 命名空间需要有此功能。  |
| [枚举设备](pos-basics-enumerating.md)        | 了解如何定义用于向系统提供的查询设备的设备选择器，以及如何使用此选择器枚举服务点设备。  |
| [创建设备对象](pos-basics-deviceobject.md)  | 了解如何创建 PointOfService 设备对象（将授予你访问外围设备的只读属性的访问权限）以及如何声明外围设备的独占使用。 |
| [声明和启用 ](pos-basics-claim.md)  | 了解如何保留 PointOfService 外围供独占使用，并启用 I/O 操作。  |
| [与其他人共享外围设备](pos-basics-sharing.md) | 了解如何与多台 Pc，依赖于共享外围设备而不是附加到每台计算机的专用外围设备的环境中的其他计算机共享网络或连接的蓝牙外围设备。
| [PointOfService 端到端](pos-get-started.md)  | 这是如何与 PointOfService 外围设备利用上面的示例进行交互的端到端示例。 |
|

## <a name="see-also"></a>请参阅

| 主题   | 描述 |
|:--------|:------------|
| [应用程序分发](../publish/distribute-lob-apps-to-enterprises.md) | 了解分发到企业客户应用的选项。 |
| [应用程序生命周期](../launch-resume/app-lifecycle.md) | 了解有关 UWP 应用程序的生命周期和在 Windows 中启动、 挂起，并恢复您的应用程序时，会发生什么情况。 |
| [应用程序资源](../app-resources/index.md) | 了解如何创作、 设置包，并使用您的应用程序的字符串、 图像和文件资源。 |
| [数据绑定](../data-binding/index.md) | 了解如何使用数据绑定来在应用程序的 UI 中显示数据。 |
| [设备枚举](enumerate-devices.md) | 了解使用高级枚举方法来查找你外围设备。|
| [版本自适应应用程序](../debug-test-perf/version-adaptive-apps.md) | 了解如何设计您的应用程序，以便使其在多个版本的 Windows 10 上运行。|
|


## <a name="sample-code"></a>示例代码
+ [条形码扫描程序示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [现金抽屉示例]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [行显示示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [磁条读取器示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [POSPrinter 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

