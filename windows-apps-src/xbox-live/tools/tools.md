---
title: 适用于 Xbox Live 的开发工具
author: StaceyHaffner
description: 了解用于帮助开发和测试支持 Xbox Live 的游戏的工具。
ms.assetid: 380a29bf-41a7-4817-9c57-f48f2b824b52
ms.author: kevinasg
ms.date: 2/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 工具, 玩家重置, live 跟踪分析器, LTA, xbox live 帐户工具
ms.localizationpriority: low
ms.openlocfilehash: bbd17f419f1e830359cc1f1698021889f2b20e51
ms.sourcegitcommit: 01760b73fa8cdb423a9aa1f63e72e70647d8f6ab
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/24/2018
---
# <a name="development-tools-for-xbox-live"></a>适用于 Xbox Live 的开发工具

本部分介绍可用于帮助进行 Xbox Live 开发的各种工具。 [Xbox Live 开发人员工具 GitHub](https://github.com/Microsoft/xbox-live-developer-tools) 存储库中提供了其中许多工具。 也可使用[开发工具库](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools)创建自己的自定义工具。 可在 [https://aka.ms/xboxliveuwptools](https://aka.ms/xboxliveuwptools) 下载所有独立开发人员工具。

## <a name="global-storage"></a>全局存储
全局标题存储用于存储每个人均可读取的数据，如名单、地图、挑战，或艺术资源。 它是一种[标题存储](../storage-platform/xbox-live-title-storage/xbox-live-title-storage.md)。 全局存储工具用于管理测试沙盒中的全局标题存储。 数据仍须通过 Windows 开发人员中心或 Xbox 开发人员门户 (XDP) 发布到 RETAIL。 可通过命令行将该工具作为 [开发工具] (https://aka.ms/xboxliveuwptools) 压缩包的一部分提供。 可使用[开发工具库](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools)创建自定义工具。

## <a name="multiplayer-session-history-viewer"></a>多人游戏会话历史记录查看器
通过多人游戏会话历史记录查看器，可查看多人游戏会话文档历史记录（包括已删除文档）的所有更改的历史时间线。 通过此工具可更深入地了解 MPSD 会话文档随时间推移发生的变化。 该工具作为 [开发工具] (https://aka.ms/xboxliveuwptools) 压缩包中的独立工具提供。

## <a name="player-reset"></a>玩家重置
玩家重置工具可用于重置测试沙盒中的玩家数据。 你可重置成就、排行榜、统计数据和游戏历史记录等数据。 可通过命令行将该工具作为 [开发工具] (https://aka.ms/xboxliveuwptools) 压缩包的一部分提供。 可使用[开发工具库](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools)创建自定义工具。

## <a name="xbox-live-developer-account"></a>Xbox Live 开发人员帐户
Xbox Live 开发人员帐户工具可用于管理开发人员帐户的身份验证。 该工具需与需要开发人员凭证的其他开发人员工具（例如玩家重置和全局存储）配合使用。 可通过命令行将该工具作为 [开发工具] (https://aka.ms/xboxliveuwptools) 压缩包的一部分提供。

## <a name="xbox-live-trace-analyzer"></a>Xbox Live 跟踪分析器
使用 [Xbox Live 跟踪分析器](analyze-service-calls.md)，可捕获所有服务调用，然后对其进行离线分析，以了解调用模式中是否存在任何违规。 可使用 xbtrace 命令行工具或通过协议激活更高级方案来激活服务调用跟踪。 此外，也支持通过游戏代码直接激活服务调用跟踪。 可通过命令行将该工具作为 [开发工具] (https://aka.ms/xboxliveuwptools) 压缩包的一部分提供。

## <a name="xbox-live-account-tool"></a>Xbox Live 帐户工具  
[Xbox Live 帐户工具](xbox-live-account-tool.md)旨在帮助你为测试游戏方案设置现有测试帐户。 例如，可使用 Xbox Live 帐户工具更改帐户的玩家代号或为帐户的好友列表快速添加 1000 位关注者。 可通过命令行将该工具作为 [开发工具] (https://aka.ms/xboxliveuwptools) 压缩包的一部分提供。

