---
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: 打包应用
description: 本部分包含或链接到有关打包通用 Windows 平台 (UWP) 应用的文章。
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp, 打包
ms.localizationpriority: medium
ms.openlocfilehash: 5d18f67ca9088c227052a2222f798cadff6df75c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372823"
---
# <a name="packaging-apps"></a>打包应用


## <a name="purpose"></a>用途

本部分包含或链接到有关打包通用 Windows 平台 (UWP) 应用的文章。

| 主题 | 描述 |
|-------|-------------|
| [使用 Visual Studio 打包 UWP 应用](packaging-uwp-apps.md) | 要分发或销售你的通用 Windows 平台 (UWP) 应用，你需要为其创建一个应用包。 |
| [手动打包应用](manual-packaging-root.md) | 如果你想要创建应用包并对其进行签名，但你未使用 Visual Studio 开发你的应用，那么你将需要使用手动应用打包工具。 |
| [应用包体系结构](device-architecture.md) | 详细了解在生成你的 UWP 应用包时应使用哪些处理器体系结构。 |
| [UWP 应用流式安装](streaming-install.md) | 借助通用 Windows 平台 (UWP) 应用流式安装，可指定希望 Microsoft Store 首先下载应用的哪些部分。 当首先下载了该应用的基本文件后，用户就可以启动该应用并与其进行交互，而应用的其余部分则会在后台完成下载。 |
| [可选包和相关集创作](optional-packages.md) | 可选包中包含可与主要包相集成的内容。 这些内容可用于可下载内容 (DLC)，因为大小限制而划分大型应用，或者用于随附从原始应用中单独分隔出来的任何其他内容。 |
| [包含可执行代码的可选包](optional-packages-with-executable-code.md) | 了解如何使用 Visual Studio 创建包含可执行代码的可选包。 |
| [使用应用安装程序安装 Windows 10 应用](https://docs.microsoft.com/windows/msix/app-installer/app-installer-root) | 利用应用安装程序，可以通过双击应用包来安装 Windows 10 应用。 |
| [使用 WinAppDeployCmd.exe 工具安装应用](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Windows 应用程序部署 (WinAppDeployCmd.exe) 是一个命令行工具，可用于将 UWP 应用从 Windows 10 计算机部署到任意 Windows 10 移动版设备。 当 Windows 10 移动版设备通过 USB 进行连接或无需 Microsoft Visual Studio 或该应用的解决方案即可连接到同一子网时，可使用此工具部署应用包。 本文介绍如何使用此工具安装 UWP 应用。 |
| [设置 UWP 应用的自动生成](auto-build-package-uwp-apps.md) | 如果你希望在自动生成过程中打包应用，本主题介绍如何使用 Visual Studio Team Services (VSTS) 执行此操作。 |
| [应用功能声明](app-capability-declarations.md) | 功能必须在你的 UWP 应用的[程序包清单](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)中声明，以便可用于访问某些 API 或资源（如图片、音乐）或者设备（如相机或麦克风）。 |
| [从 Microsoft Store 下载并安装程序包更新](self-install-package-updates.md) | 你的 UWP 应用可以以编程方式检查程序包更新，并安装这些更新。 你的应用还可以查询已在合作伙伴中心内标记为必需的程序包，以及完成安装该必需更新前禁用功能。  |
