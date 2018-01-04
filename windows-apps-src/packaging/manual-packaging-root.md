---
author: laurenhughes
ms.assetid: ee51eae3-ed55-419e-ad74-6adf1e1fb8b9
title: "手动打包应用"
description: "本节包含或链接到有关手动打包通用 Windows 平台 (UWP) 应用的文章。"
ms.author: lahugh
ms.date: 03/07/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 打包"
ms.openlocfilehash: 8eca88588b2e444450daccd997aebb9e838a90d4
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: HT
ms.contentlocale: zh-CN
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