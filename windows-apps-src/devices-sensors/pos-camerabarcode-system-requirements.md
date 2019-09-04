---
title: 相机条形码扫描仪的系统要求
description: 本文列出了从 UWP 应用使用相机条形码扫描仪的要求。
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 4eed59e302bc34e93d21adef794de02427f2933e
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243273"
---
# <a name="camera-barcode-scanner-system-requirements"></a>相机条形码扫描仪的系统要求
从 Windows 10 版本 1803 开始，你可以通过来自通用 Windows 应用程序的标准相机镜头来读取条形码。

## <a name="supported-windows-editions"></a>受支持的 Windows 版本
- 处于 S 模式的 Windows 10 专业版
- Windows 10 专业版
- Windows 10 企业版
- Windows 10 IOT 核心版


## <a name="webcam-requirements"></a>网络摄像头要求
| 类别      | 建议           | 备注 |
| ------------- | ------------------------ | -------- |
| Focus         | 自动对焦               | 不建议使用固定对焦 |
| 分辨率    | 1920 x 1440 或更高    | 使用 1920 x 1440 或更高分辨率的相机能够达到最佳体验。  如果条形码打印得足够大，一些分辨率较低的相机可以读取标准条形码。 宽度更窄的条形码可能需要使用分辨率更高的相机。 |
|

## <a name="see-also"></a>请参阅

### <a name="samples"></a>示例

- [条形码扫描器示例](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
