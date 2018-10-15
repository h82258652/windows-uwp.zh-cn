---
title: 适用于 Xbox Live 的开发工具
author: StaceyHaffner
description: 了解用于帮助开发和测试支持 Xbox Live 的游戏的工具。
ms.assetid: 380a29bf-41a7-4817-9c57-f48f2b824b52
ms.author: kevinasg
ms.date: 6/13/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 工具, 玩家重置, live 跟踪分析器, LTA, xbox live 帐户工具
ms.localizationpriority: medium
ms.openlocfilehash: 98b21eda55c6122104c9ec79cda10708e362f3a4
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2018
ms.locfileid: "4614943"
---
# <a name="development-tools-for-xbox-live"></a>适用于 Xbox Live 的开发工具

本部分介绍可用于帮助进行 Xbox Live 开发的各种工具。 [Xbox Live 开发人员工具 GitHub](https://github.com/Microsoft/xbox-live-developer-tools) 存储库中提供了其中许多工具。 也可使用[开发工具库](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools)创建自己的自定义工具。 可在 [https://aka.ms/xboxliveuwptools](https://aka.ms/xboxliveuwptools) 下载所有独立开发人员工具。

> [!NOTE]
> 下载中包含的 MatchSim 和 XboxLiveCompute 工具仅可由托管的合作伙伴，或在注册合作伙伴[ID@Xbox](http://www.xbox.com/Developers/id)计划。 若要了解有关的可用的开发人员计划的详细信息，请参阅[开发人员计划概述](https://docs.microsoft.com/windows/uwp/xbox-live/developer-program-overview)。 

## <a name="global-storage"></a>全局存储
全局标题存储用于存储每个人均可读取的数据，如名单、地图、挑战，或艺术资源。 它是一种[标题存储](../storage-platform/xbox-live-title-storage/xbox-live-title-storage.md)。 全局存储工具用于管理测试沙盒中的全局标题存储。 数据仍须通过 Windows 开发人员中心或 Xbox 开发人员门户 (XDP) 发布到 RETAIL。 该工具在 [开发工具] (https://aka.ms/xboxliveuwptools) 压缩包中，通过命令行使用。 可使用[开发工具库](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools)创建自定义工具。

## <a name="multiplayer-session-history-viewer"></a>多人游戏会话历史记录查看器
通过多人游戏会话历史记录查看器，可查看多人游戏会话文档历史记录（包括已删除文档）的所有更改的历史时间线。 通过此工具可更深入地了解 MPSD 会话文档随时间推移发生的变化。 该工具在 [开发工具](https://aka.ms/xboxliveuwptools) 压缩包中作为独立工具提供。

## <a name="player-data-reset"></a>玩家数据重置
玩家数据重置工具可用于重置测试沙盒中的玩家数据。 你可重置成就、排行榜、统计数据和游戏历史记录等数据。 该工具在 [开发工具](https://aka.ms/xboxliveuwptools) 压缩包中，通过命令行使用。 可使用[开发工具库](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools)创建自定义工具。

## <a name="xbox-live-developer-account"></a>Xbox Live 开发人员帐户
Xbox Live 开发人员帐户工具可用于管理开发人员帐户的身份验证。 该工具需与需要开发人员凭证的其他开发人员工具（例如玩家重置和全局存储）配合使用。 该工具在 [开发工具](https://aka.ms/xboxliveuwptools) 压缩包中，通过命令行使用。

## <a name="xbox-live-trace-analyzer"></a>Xbox Live 跟踪分析器
使用 [Xbox Live 跟踪分析器](analyze-service-calls.md)，可捕获所有服务调用，然后对其进行离线分析，以了解调用模式中是否存在任何违规。 可使用 xbtrace 命令行工具或通过协议激活更高级方案来激活服务调用跟踪。 此外，也支持通过游戏代码直接激活服务调用跟踪。 该工具在 [开发工具](https://aka.ms/xboxliveuwptools) 压缩包中，通过命令行使用。

## <a name="xbox-live-account-tool"></a>Xbox Live 帐户工具  
[Xbox Live 帐户工具](xbox-live-account-tool.md)旨在帮助你为测试游戏方案设置现有测试帐户。 例如，可使用 Xbox Live 帐户工具更改帐户的玩家代号或为帐户的好友列表快速添加 1000 位关注者。 该工具在 [开发工具](https://aka.ms/xboxliveuwptools) 压缩包中，通过命令行使用。

## <a name="config-as-source"></a>配置为源
[配置为源](https://github.com/Microsoft/xbox-live-developer-tools/blob/master/CONFIGASSOURCE.md)是 Microsoft 为满足高级用户需求开发的一套工具，为集成到我们的配置服务提供官方支持的工具和 API。 这些 Xbox Live 服务通常在开发人员中心为你的作品配置，其中包括从排行榜到成就，再到 Web 服务和依赖方等多种服务。 对于许多游戏开发人员而言，使用开发人员中心就足够了。 但对于高级用户，他们有将常见配置任务集成到自有过程和工具的需求。  “配置为源”旨在通过提供命令行工具和新的 API 支持到现有工作流和管道的集成，从而支持这些场景。 该工具在 [开发工具](https://aka.ms/xboxliveuwptools) 压缩包中，通过命令行使用。
