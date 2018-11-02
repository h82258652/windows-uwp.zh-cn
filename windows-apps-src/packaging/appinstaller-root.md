---
author: laurenhughes
title: 使用应用安装程序来安装 UWP 应用
description: 此部分包含或链接至有关应用安装程序及其功能如何使用的文章。
ms.author: lahugh
ms.date: 06/05/2018
ms.topic: article
keywords: windows 10, uwp, 应用安装程序, AppInstaller, 旁加载, 相关集, 可选包
ms.localizationpriority: medium
ms.openlocfilehash: f3da1f4f393eea1362b6e59d2ad7e9a2a97bc33b
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5937990"
---
# <a name="install-uwp-apps-with-app-installer"></a>使用应用安装程序来安装 UWP 应用

## <a name="purpose"></a>用途
此部分包含或链接至有关应用安装程序及其功能如何使用的文章。 

利用应用安装程序，可以通过双击应用包来安装 UWP 应用。 这意味着用户无需使用 PowerShell 或其他开发人员工具即可部署 UWP 应用。 应用安装程序还可以从 Web、可选包和相关的集中安装应用。 若要了解如何使用应用安装程序安装应用，请参阅表中的主题。

| 主题 | 说明 |
|-------|-------------|
| [使用 Visual Studio 创建应用安装程序文件](create-appinstallerfile-vs.md)| 了解如何使用 Visual Studio 通过 .appinstaller 文件启用自动更新。 |
| [从网页中安装 UWP 应用](installing-UWP-apps-web.md) | 在此部分中，我们将查看允许用户直接从网页中安装应用所需执行的步骤。 |
| [使用应用安装程序文件安装相关集](install-related-set.md) | 在此部分中，可了解如何通过应用安装程序来安装相关集。 我们还将详述有关构建可定义相关集的应用安装程序文件的相应步骤。 |
| [使用应用安装程序文件遇到的安装问题的疑难解答](troubleshoot-appinstaller-issues.md) | 使用应用安装程序文件旁加载应用程序时的常见问题和解决方案。 |
| [应用安装程序文件 (.appinstaller) 参考](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file) | 查看应用安装程序文件的完整 XML 架构。 |

## <a name="tutorials"></a>教程 

学习下面的教程，了解如何从各种分发平台托管和安装 UWP 应用。 这些教程适用于不希望或不需要将应用发布到 Microsoft Store，但仍希望利用 Windows 10 打包和部署平台的企业和开发人员。

| 教程 | 说明 |
|----------|-------------|
| [从 Azure Web 应用安装 UWP 应用](web-install-azure.md) | 创建 Azure Web 应用并用它来托管和分发 UWP 应用包。 |
| [从 IIS 服务器安装 UWP 应用](web-install-IIS.md) | 安装 IIS 服务器，验证 Web 应用可以托管应用包，并有效使用应用安装程序。 |
| [在 AWS 上为 Web 安装托管 UWP 应用包](web-install-aws.md) | 了解如何设置 Amazon 简单存储服务来从网站托管 UWP 应用包。 |

