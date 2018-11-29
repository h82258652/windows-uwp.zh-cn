---
title: 使用 MDM 部署条形码扫描仪配置文件
description: 可以使用 MDM 服务器部署条形码扫描仪配置文件。
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dbcaa683e2c7a2bb18d88fcba03e10fa951d4459
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2018
ms.locfileid: "8191650"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>使用 MDM 部署条形码扫描仪配置文件

**注意**此功能需要 windows 10 移动版或更高版本。

可以使用 MDM 服务器部署条形码扫描仪配置文件。 若要部署配置文件，请使用 [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025) 中的 *OemProfile* 将配置文件放入 \\Data\\SharedData\\OEM\\Public\\Profile 文件夹中。 然后，驱动程序制造商可以使用这些扫描仪配置文件配置不会通过 API 图面公开的设置。

Microsoft 未定义扫描仪配置文件的细节或实现方法。

## <a name="related-topics"></a>相关主题
- [EnterpriseExtFileSystem 云解决方案提供商](https://msdn.microsoft.com/library/windows/hardware/mt157025)
- [条形码扫描仪设备支持](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)