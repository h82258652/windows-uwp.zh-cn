---
title: 开发人员计划概述
author: KevinAsgari
description: 了解可供使用 Xbox Live 的各种开发人员计划。
ms.assetid: 1166308a-4079-41b4-8550-ce04b82b4f72
ms.author: kevinasg
ms.date: 5/30/2018
ms.topic: article
keywords: xbox live, xbox, games, uwp, windows 10, xbox one, 开发人员计划, 创意者
ms.localizationpriority: medium
ms.openlocfilehash: faf010e2a61199d56a1ae5b9ed08fd373f89cd80
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6996516"
---
# <a name="developer-program-overview"></a>开发人员计划概述

如果你要开发支持 Xbox Live 的作品，则可以在若干选项中选择。 对于每种选项，你都可以选择不同级别的时间投入、可供你使用的功能以及支持选项。

## <a name="xbox-live-creators-program"></a>Xbox Live 创意者计划

如果你希望熟悉 Xbox Live 开发，最好将 Xbox Live 创意者计划作为使用 Xbox Live 的起点。 此计划无需经过 Microsoft 的审批流程即可加入，并且认证和发布要求也最低。

Xbox Live 创意者计划仅支持创建适用于[通用 Windows 平台](https://msdn.microsoft.com/en-us/windows/uwp/get-started/universal-application-platform-guide)(UWP) 的作品。  这些作为 UWP 游戏创建的作品可在 Windows 10 电脑和 Xbox One 主机上运行。  有关在 Xbox One 上运行 UWP 游戏的更多详细信息，请参阅 [Xbox One 上的 UWP](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/index)。  

Xbox One 为玩家提供策展式 Microsoft Store 体验，玩家可以用 Xbox 在 Microsoft Store 全新创意者集锦版块中购买通过 Xbox Live 创意者计划发布的游戏。 这样，便在确保人人都能基于开放式平台开发并交付游戏与主机玩家逐步了解并期待获得特选 Microsoft Store 体验之间达成平衡。 在 Windows 10 上，你的作品将与其他所有 Xbox Live 游戏一起发布在 Microsoft Store 中。

### <a name="publishing-and-certification"></a>发布和认证
你必须在[合作伙伴中心开发人员计划](https://developer.microsoft.com/store/register)中发布游戏的 Xbox Live 创意者计划中注册。 你的游戏必须符合下面两组要求：

1. 集成 Xbox Live 登录并显示用户身份（玩家代号、玩家头像等）。 所有其他 Xbox Live 服务都是可选的。
2. 遵循标准的 [Microsoft Store 策略](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx)。

### <a name="supported-xbox-live-services"></a>支持的 Xbox Live 服务
通过 Xbox Live 创意者计划创建的作品，可以使用排行榜、特别推荐的统计数据、作品存储、连接存储，以及一组受限的社交功能。 通过 Xbox Live 创意者计划创建的作品**不**支持成就、多人在线游戏和多项社交功能。

有关支持的服务的完整列表，请参阅[功能表](#feature-table)。

### <a name="supported-third-party-game-development-engines"></a>受支持的第三方游戏开发引擎
Xbox Live 创意者计划作品是可以通过一些热门游戏引擎构建的 UWP 游戏。 Microsoft 提供了一个文档，介绍如何将 Xbox Live 服务集成到用 [Unity 游戏引擎](https://unity.com)构建的 UWP 游戏中。 你可以在本网站上找到详细介绍 Xbox Live 与 Unity 游戏集成的[文档](get-started-with-creators/develop-creators-title-with-unity.md)，还可以下载和了解 Microsoft 构建的 [Xbox Live Unity 插件](https://github.com/Microsoft/xbox-live-unity-plugin)。

Xbox Live 创意者计划作品也可以使用游戏引擎 [Construct（2 和 3）](https://www.scirra.com/construct2)和 [GameMaker Studio 2](https://www.yoyogames.com/gamemaker) 构建。 这两个游戏引擎都增加了 Xbox Live 支持，但是，该支持由游戏引擎创建者而非 Microsoft 提供。 有关将 Xbox Live 添加到你的 Construct 或 GameMaker Studio 2 项目中的详细信息和支持，需要分别参阅每个游戏引擎文档。

[了解如何将 Xbox Live 集成到 Construct 项目。](https://www.scirra.com/tutorials/9540/using-xbox-live-in-uwp-apps)

[了解如何将 Xbox Live 集成到 GameMaker Studio 2 项目。](https://www.yoyogames.com/gamemaker/xblc)

对于不支持 Xbox Live 功能或插件的其他游戏开发引擎，如 [MonoGame](http://www.monogame.net/) 或 [Xenko](https://xenko.com/)，仍然可以使用 Xbox Live API 将 Xbox Live 添加到你的作品中。 要从你的项目使用 Xbox Live API，可以通过 NuGet 程序包添加对二进制文件的引用或者添加 API 源。 添加 NuGet 程序包可加快编译速度，而添加源可简化调试。

### <a name="support-and-feedback"></a>支持和反馈
你可能遇到的任何问题都可以在 [MSDN 论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev)上得到解答。  你也可以使用“xbox-live”标记在 [Stack Overflow](http://stackoverflow.com/questions/tagged/xbox-live) 上提出编程相关问题。  Xbox Live 团队将与社区合作，并且将根据在此处收到的反馈不断改进我们的 API、工具和文档。

对于 Xbox Live 创意者计划中的开发人员，你可以在我们的 [Xbox 用户反馈](https://xbox.uservoice.com/forums/363186--new-ideas)中[提交新的想法](https://xbox.uservoice.com/forums/363186--new-ideas?category_id=196261)或[对现有想法进行表决](https://xbox.uservoice.com/forums/251649?category_id=210838)。

## <a name="idxbox"></a>ID@Xbox

Xbox Live 创意者计划非常适用于大量游戏和开发人员。 但是，如果你希望访问完整的 Xbox Live 堆栈，包括多人在线游戏、成就和游戏得分，或者想要利用硬件开发工具包获得 Xbox One 系列设备的完整功能，则 [ID@Xbox](http://www.xbox.com/en-US/developers/id) 计划适合你。

ID@Xbox 计划中的游戏必须是获批的概念，并通过基于 Xbox One 和 Windows 10 的完整认证，这需要贵方投入更多的时间。
相对于 Creators Collection，ID@Xbox 作品放置在应用商店的主要版块中，这样做可以增加客户看到的机会。

ID@Xbox 计划中的开发人员还可以获得 Microsoft 提供的开发人员支持和促销帮助，以及一整套免费的专属白皮书，并访问开发人员技术论坛。 你可以继续使用 [MSDN 论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev)，也可以使用“xbox live”标记在 [Stack Overflow](http://stackoverflow.com/questions/tagged/xbox-live) 上提出编程相关问题（如果你愿意）。

## <a name="microsoft-partners"></a>Microsoft 合作伙伴

开发人员如果与作为 Microsoft 合作伙伴的游戏发布者合作，则可以获得一整套 Xbox Live 功能，并联系专门的 Microsoft 代表，为你的开发、认证和发布过程提供帮助。

## <a name="feature-table"></a>功能表

下表说明了适用于 Xbox Live 创意者计划和 [ID@Xbox](http://www.xbox.com/en-US/developers/id) 计划的功能。  

<table>

<tr>
<th>功能区域</th>
<th>功能</th>
<th>描述</th>
<th> ID@Xbox </th>
<th>Xbox Live 创意者计划</th>
</tr>

<tr>
<td rowspan="2" class="dev-program-feature-name">标识</td>
<td>登录/注册</td>
<td>允许玩家在你的作品中登录到 Xbox Live，或根据需要创建新的 Xbox Live 帐户</td>
<td class="xbl-features-required">必填</td>
<td class="xbl-features-required">必填</td>
</tr>

<tr>
<td>用户标识</td>
<td>通过显示玩家代号和玩家头像等利用 Xbox Live 标识</td>
<td class="xbl-features-required">必填</td>
<td class="xbl-features-required">必填</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="13" class="dev-program-feature-name">社交</td>

<td>基本状态</td>
<td>显示基本状态字符串，说明作品中的用户活动。  例如：“Steve 正在玩‘我的世界’”</td>
<td class="xbl-features-automatic">自动</td>
<td class="xbl-features-automatic">自动</td>
</tr>

<tr>
<td>最近玩过</td>
<td>显示在 Xbox 应用或 Xbox One 的最近玩过的作品中</td>
<td class="xbl-features-automatic">自动</td>
<td class="xbl-features-automatic">自动</td>
</tr>

<tr>
<td>活动源</td>
<td>显示在 Xbox 应用或 Xbox One 的活动源中</td>
<td class="xbl-features-automatic">自动</td>
<td class="xbl-features-automatic">自动</td>
</tr>

<tr>
<td>游戏中心</td>
<td>使游戏中心与你的作品关联，以显示统计数据、视频和特定于作品的源中的其他内容</td>
<td class="xbl-features-automatic">自动</td>
<td class="xbl-features-automatic">自动</td>
</tr>

<tr>
<td>俱乐部</td>
<td>玩家可以使用 Xbox 应用或 Xbox One 创建俱乐部，这些俱乐部可有选择地与你的作品关联。</td>
<td class="xbl-features-automatic">自动</td>
<td class="xbl-features-automatic">自动</td>
</tr>

<tr>
<td>查找组 (LFG)</td>
<td>LFG 允许玩家查找游戏外的其他人，以计划多人游戏。</td>
<td class="xbl-features-automatic">自动</td>
<td class="xbl-features-automatic">自动</td>
</tr>

<tr>
<td>GameDVR</td>
<td>玩家可以捕获其游戏玩法会话视频，并在活动源中共享这些内容。</td>
<td class="xbl-features-automatic">自动</td>
<td class="xbl-features-automatic">自动</td>
</tr>

<tr>
<td>广播</td>
<td>玩家可以通过 Mixer 和 Twitch 等流服务实时广播其游戏玩法</td>
<td class="xbl-features-automatic">自动</td>
<td class="xbl-features-automatic">自动</td>
</tr>

<tr>
<td>完整状态</td>
<td>显示与你的作品中的玩家有关的更多详细信息。  Basic Presence 可显示“用户正在玩赛车游戏”，而 Rich Presence 用于指定更详细的字符串，例如“用户正驾驶超级跑车穿越雨林”</td>
<td class="xbl-features-required">必填</td>
<td class="xbl-features-notavailable">不支持</td>
</tr>

<tr>
<td>好友</td>
<td>检索登录用户的好友列表，以支持你的作品中的社交游戏玩法方案。</td>
<td class="xbl-features-required">必填</td>
<td class="xbl-features-limited">可选/受限（仅显示玩你的作品的好友）</td>
</tr>

<tr>
<td>隐私</td>
<td>允许玩家对其他玩家进行静音或阻止</td>
<td class="xbl-features-optional">可选</td>
<td class="xbl-features-optional">可选</td>
</tr>

<tr>
<td>信誉</td>
<td>玩家因其行为获得或丧失信誉。 行为在匹配中使用，并可供你的作品以自定义方式使用。</td>
<td class="xbl-features-optional">可选</td>
<td class="xbl-features-notavailable">不支持</td>
</tr>

<tr>
<td>社交管理器</td>
<td>高效检索有关玩家的社交图片的信息</td>
<td class="xbl-features-optional">可选</td>
<td class="xbl-features-limited">可选/受限（仅显示玩你的作品的好友）</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="4" class="dev-program-feature-name">数据平台</td>

<td>玩家统计数据</td>
<td>上传有关玩家的统计数据，这些统计数据可在排行榜中使用。</td>
<td class="xbl-features-optional">可选</td>
<td class="xbl-features-optional">可选（仅限 Data Platform 2017）</td>
</tr>

<tr>
<td>特别推荐的统计数据</td>
<td>将某些统计数据指定为将在游戏中心显示的“特别推荐的统计数据”。</td>
<td class="xbl-features-required">必填</td>
<td class="xbl-features-optional">可选（仅限 Data Platform 2017）</td>
</tr>

<tr>
<td>排行榜</td>
<td>检索玩家统计数据，并以排序方式显示，以鼓励竞争。</td>
<td class="xbl-features-optional">可选</td>
<td class="xbl-features-optional">可选（仅限 Data Platform 2017）</td>
</tr>

<tr>
<td>成就与玩家分数</td>
<td>将某些统计数据指定为将在游戏中心显示的“特别推荐的统计数据”。</td>
<td class="xbl-features-required">必填</td>
<td class="xbl-features-notavailable">不支持</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="1" class="dev-program-feature-name">媒体</td>

<td>上下文搜索</td>
<td>使用关键字为 GameDVR 剪辑添加注释，以使玩家更容易找到与他们要关注的内容对应的剪辑。</td>
<td class="xbl-features-optional">可选</td>
<td class="xbl-features-notavailable">不支持</td>
</tr>


<tr class="dev-program-feature-start">
<td rowspan="2" class="dev-program-feature-name">存储</td>

<td>连接存储</td>
<td>在 Xbox One 主机和电脑之间漫游游戏保存内容</td>
<td class="xbl-features-required">必填</td>
<td class="xbl-features-optional">可选</td>
</tr>

<tr>
<td>作品存储</td>
<td>用于存储大量每用户或每作品数据的云存储。</td>
<td class="xbl-features-optional">可选</td>
<td class="xbl-features-optional">可选</td>
</tr>

<tr class="dev-program-feature-start">
<td rowspan="6" class="dev-program-feature-name">多人在线游戏</td>

<td>多人游戏会话目录 (MPSD)</td>
<td>存储与多人游戏会话有关的信息，例如玩家列表和状态等</td>
<td class="xbl-features-optional">必填</td>
<td class="xbl-features-notavailable">不支持</td>
</tr>

<tr>
<td>匹配</td>
<td>Xbox Live 可将多人游戏会话中的不同玩家匹配在一起。</td>
<td class="xbl-features-optional">可选</td>
<td class="xbl-features-notavailable">不支持</td>
</tr>

<tr>
<td>Arena</td>
<td>玩家可以在锦标赛中互相竞争。</td>
<td class="xbl-features-optional">可选</td>
<td class="xbl-features-notavailable">不支持</td>
</tr>

<tr>
<td>游戏聊天</td>
<td>多人游戏中的玩家可进行语音聊天</td>
<td class="xbl-features-optional">可选</td>
<td class="xbl-features-notavailable">不支持</td>
</tr>

<tr>
<td>Xbox Live Compute</td>
<td>部署可与你的作品进行通信的可执行文件和资产，以解除客户端的计算工作。</td>
<td class="xbl-features-optional">可选</td>
<td class="xbl-features-notavailable">不支持</td>
</tr>

</table>
