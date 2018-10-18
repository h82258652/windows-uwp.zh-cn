---
author: laurenhughes
ms.assetid: ee51eae3-ed55-419e-ad74-6adf1e1fb8b9
title: 手动打包应用
description: 本节包含或链接到有关手动打包通用 Windows 平台 (UWP) 应用的文章。
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 打包
ms.localizationpriority: medium
ms.openlocfilehash: fcd6d937c7261b5cfa8af954eb5d2ec2869d8afd
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2018
ms.locfileid: "4752415"
---
# <a name="manual-app-packaging"></a>手动打包应用

如果你想要创建应用包并对其进行签名，但你未使用 Visual Studio 开发你的应用，那么你将需要使用手动应用打包工具。

> [!IMPORTANT] 
> 如果你使用 Visual Studio 开发你的应用，建议使用 Visual Studio 向导创建应用包并对其进行签名。 有关详细信息，请参阅[使用 Visual Studio 打包 UWP 应用](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)。

## <a name="purpose"></a>用途

本节包含或链接到有关手动打包通用 Windows 平台 (UWP) 应用的文章。

| 主题 | 说明 |
|-------|-------------|
| [使用 MakeAppx.exe 工具创建应用包](create-app-package-with-makeappx-tool.md) | MakeAppx.exe 创建、加密、解密应用程序包和捆绑包，并从中提取文件。 |
| [为程序包签名创建证书](create-certificate-package-signing.md) | 使用 PowerShell 工具为应用包签名创建和导出证书。 |
| [使用 SignTool 对应用包进行签名](sign-app-package-using-signtool.md) | 使用带证书的 SignTool 手动对应用包签名。 |

### <a name="advanced-topics"></a>高级主题

本部分包含更多高级主题，用于对大型或复杂应用进行组件化，从而提高打包和安装效率。 

> [!IMPORTANT]
> 如果你打算将应用提交到 Microsoft Store，你需要联系 [Windows 开发人员支持](https://developer.microsoft.com/windows/support)并获得批准才能使用本部分中的任何高级功能。


| 主题 | 说明 |
|-------|-------------|
| [资产包简介](asset-packages.md) | 资产包是一种用作应用程序公用文件集中存放位置的程序包，可有效消除整个体系结构程序包中的重复文件。 |
| [用资产包和包折叠进行开发](package-folding.md) | 了解如何使用资产包和包折叠高效组织你的应用。 |
| [平面捆绑应用包](flat-bundles.md) | 介绍如何创建平面捆绑包的应用的程序包文件。 |
| [使用包布局创建包](packaging-layout.md) | 包布局是用于描述应用的包结构的单个文档。 它指定应用中的捆绑（主要和可选）、捆绑中的包以及包中的文件。 |
