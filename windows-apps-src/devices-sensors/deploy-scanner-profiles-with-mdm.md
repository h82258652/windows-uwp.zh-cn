---
title: "使用 MDM 部署条形码扫描仪配置文件"
author: PatrickFarley
description: "可以使用 MDM 服务器部署条形码扫描仪配置文件。"
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.openlocfilehash: a63a09e64b6e2b935963a3f49ed7cbc6b82bdcef
ms.sourcegitcommit: d2ec178103f49b198da2ee486f1681e38dcc8e7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2017
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>使用 MDM 部署条形码扫描仪配置文件

**注意**  此功能需要 Windows 10 移动版或更高版本。

可以使用 MDM 服务器部署条形码扫描仪配置文件。 若要部署配置文件，请使用 [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025) 中的 *OemProfile* 将配置文件放入 \\Data\\SharedData\\OEM\\Public\\Profile 文件夹中。 然后，驱动程序制造商可以使用这些扫描仪配置文件配置不会通过 API 图面公开的设置。

Microsoft 未定义扫描仪配置文件的细节或实现方法。

## <a name="related-topics"></a>相关主题
[条形码扫描仪](barcode-scanner.md)

[EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025)