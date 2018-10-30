---
title: 使用 MDM 部署条形码扫描仪配置文件
author: PatrickFarley
description: 可以使用 MDM 服务器部署条形码扫描仪配置文件。
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.author: pafarley
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cfd9692620273952483ec7da65a69b643cb5bf4f
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5762372"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>使用 MDM 部署条形码扫描仪配置文件

**注意**此功能需要 windows 10 移动版或更高版本。

可以使用 MDM 服务器部署条形码扫描仪配置文件。 若要部署配置文件，请使用 [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025) 中的 *OemProfile* 将配置文件放入 \\Data\\SharedData\\OEM\\Public\\Profile 文件夹中。 然后，驱动程序制造商可以使用这些扫描仪配置文件配置不会通过 API 图面公开的设置。

Microsoft 未定义扫描仪配置文件的细节或实现方法。

## <a name="related-topics"></a>相关主题
- [EnterpriseExtFileSystem 云解决方案提供商](https://msdn.microsoft.com/library/windows/hardware/mt157025)
- [条形码扫描仪设备支持](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)