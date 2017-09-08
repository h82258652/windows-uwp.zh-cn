---
title: "工具"
author: KevinAsgari
description: "了解用于帮助开发和测试支持 Xbox Live 的游戏的工具。"
ms.assetid: 380a29bf-41a7-4817-9c57-f48f2b824b52
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "xbox live, xbox, 游戏, uwp, windows 10, xbox one, 工具"
ms.openlocfilehash: 0f41f81596ece36a03605ca58e5f521c036f4e0e
ms.sourcegitcommit: 90fbdc0e25e0dff40c571d6687143dd7e16ab8a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2017
---
# <a name="tools"></a>工具

本部分介绍你可用于帮助你使用 Xbox Live 的各种工具。

[Xbox Live 跟踪分析器](analyze-service-calls.md)  
现在，游戏开发人员可以通过 Xbox Live 服务 API 捕获所有服务调用，然后对其进行离线分析，以了解调用模式中是否存在任何违规。 可以使用 xbtrace 命令行工具中的新功能或者通过协议激活更高级方案来激活服务调用跟踪。 此外，也支持通过游戏代码直接激活服务调用跟踪。 离线分析工具（称为 Xbox Live 跟踪分析器，XBLTraceAnalyzer.exe）可用于游戏开发人员网络。

[Xbox Live 帐户工具](xbox-live-account-tool.md)   
Xbox Live 帐户工具是一款设计用于帮助开发人员为测试游戏方案设置现有开发人员帐户的工具。 例如，你可以使用 Xbox Live 帐户工具更改开发人员帐户的玩家代号或为帐户的好友列表快速添加 1000 位关注者。

[Xbox Live PowerShell 模块](https://github.com/Microsoft/xbox-live-powershell-module/blob/master/docs/XboxLivePsModule.md)  
XboxlivePSModule 包含各种用于帮助 Xbox Live 开发的实用程序。
* 若要从 [PowerShell 库](https://www.powershellgallery.com/packages/XboxlivePSModule)使用该模块，请打开 PowerShell 窗口：
    1. 下载并安装该模块： `Install-Module XboxlivePSModule -Scope CurrentUser`
    2. 通过运行开始使用 `Import-Module XboxlivePSModule`
    3. 运行 cmdlet，即 Set-XblSandbox XDKS.1 或 Get-XblSandbox

* 若要通过已下载的压缩文件使用它，请打开一个 PowerShell 窗口，
    1. 运行 `Import-Module <path to unzipped folder>\XboxLivePsModule\XboxLivePsModule.psd1`
    2. 运行 cmdlet，即 Set-XblSandbox XDKS.1 或 Get-XblSandbox

有关可用 cmdlet 及其用法的更多详细信息，请访问 [github](https://github.com/Microsoft/xbox-live-powershell-module/blob/master/docs/XboxLivePsModule.md) 上的在线文档
