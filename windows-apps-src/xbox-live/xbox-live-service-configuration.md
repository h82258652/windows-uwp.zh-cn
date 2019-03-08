---
title: Xbox Live 服务配置
description: 了解如何为你的游戏配置 Xbox Live 服务。
ms.assetid: 631c415b-5366-4a84-ba4f-4f513b229c32
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 服务配置
ms.localizationpriority: medium
ms.openlocfilehash: d12c66e61a189c13ddbcd96dd99caa351206ecf6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640902"
---
# <a name="xbox-live-service-configuration"></a>Xbox Live 服务配置

## <a name="what-is-service-configuration"></a>什么是服务配置？

你可能熟悉 Xbox Live 的一些功能，例如[成就](achievements-2017/achievements.md)、[排行榜](leaderboards-and-stats-2017/leaderboards.md)和[匹配](multiplayer/multiplayer-concepts.md#smartmatch-matchmaking)。

如果不熟悉的话，我们将以“排行榜”为例简要介绍一下。 排行榜让玩家可以查看表示自己相比其他玩家取得的成就的值。  例如，街机游戏中的高分、赛车游戏中的单圈时间，或第一人称射击游戏中的爆头数。 但不同的是，街机只显示在该物理计算机上玩过的玩家所取得的最高分，而 Xbox Live 可显示来自世界各地的玩家取得的高分。

不过，要做到这一点，你需要执行某种一次性配置，使 Xbox Live 了解你的排行榜。 例如，值应以升序还是降序排序，以及应对哪部分数据进行排序。

此配置上发生的情况[合作伙伴中心](https://partner.microsoft.com/dashboard)大部分时间。 但是，某些开发人员将使用 [Xbox 开发人员门户 (XDP)](https://xdp.xboxlive.com)。

如果你是 Xbox Live Creators 计划的一部分开发你的标题，则使用[合作伙伴中心](https://partner.microsoft.com/dashboard)，并可以读取[获取启动与 Xbox Live](get-started-with-creators/get-started-with-xbox-live-creators.md)若要了解如何进行设置。

如果你是 ID@Xbox 开发人员，或要与作为 Microsoft 合作伙伴的发布者合作，请阅读下文。

## <a name="choose-your-development-portal"></a>选择你的开发门户

如上所述，有两个不同的门户可用于配置 Xbox Live 服务。 在合作伙伴中心[ https://partner.microsoft.com/dashboard ](https://partner.microsoft.com/dashboard)和 Xbox 开发门户 (XDP) 处[ https://xdp.xboxlive.com ](https://xdp.xboxlive.com)。

合作伙伴中心建议的所有标题，但对于某些功能，您可能仍想要使用 XDP。 此部分将帮助并建议你从何处配置你的作品。

具体取决于你所选的门户，可以找到有关特定服务配置页的信息：

* [合作伙伴中心配置](configure-xbl/windows-dev-center.md)
* [Xbox 开发门户配置](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/atoc-service-configuration)-若要访问此链接，您必须具有 Microsoft 帐户 (MSA) 的已启用的 Xbox Live 的完全访问权限。

如果你已经配置了作品，则可以向下滚动到[获取你的 ID](#get_ids)，以了解如何获取设置你的作品所需的各种标识符。

### <a name="pcmobile-uwp-game-only"></a>仅限电脑/手机 UWP 游戏
合作伙伴中心建议用于配置和管理仅在 Windows 10 电脑和/或 Windows 10 移动版设备运行的 UWP 游戏。

#### <a name="using-xdp-to-configure-uwp-titles"></a>使用 XDP 配置 UWP 作品

如果你有以下要求之一，则可能需要使用 XDP 配置 UWP 作品：

1. 你使用的是 Arena。
2. 你已在要继续使用的 XDP 上设置现有的用户、组和权限。
3. 你目前使用的工具仅适用于 XDP，例如锦标赛工具或多人游戏会话目录会话历史记录查看器。
4. 你要开发的作品将是在基于 Xbox One XDK 的游戏与该游戏的 UWP 电脑/手机版之间进行的跨平台游戏。

如果不属于这些类别之一，则应使用合作伙伴中心。 否则，你可以参阅下文，了解如何使用 XDP 配置 UWP 作品。

使用 XDP 为 UWP 应用程序配置 Xbox Live 服务时，应注意以下几个重要事项：

* **后向 CERT/零售 XDP 中发布游戏的 Xbox Live 服务配置时，没有任何追溯 ！** 该游戏的 Xbox Live 服务配置需要在游戏作品的生命周期内一直保留在 XDP 中。
* **从 XDP 到合作伙伴中心没有迁移路径。** 如果 XDP 中启动你的 Xbox Live 配置，你必须手动重新创建它在合作伙伴中心如果你想要将其移动。

给定这些两个注意事项，我们建议使用合作伙伴中心 PC/移动游戏，除非属于上述类别之一。

### <a name="cross-play-between-xbox-one-and-pcmobile-games"></a>Xbox One 与电脑/手机之间的跨平台联机游戏 ###
Xbox One 与电脑之间的跨设备游戏称为跨平台联机游戏，是对 Windows 10 体验的完美展示。 在这种情况下，Xbox One 和电脑版游戏共用相同的 Xbox Live 配置。

这种方案还包括以下情况：你目前拥有 Xbox One XDK 游戏，并且想要为 UWP 版游戏提供跨平台联机支持。

为了实现跨平台联机游戏，请执行以下操作：

* **使用 XDP 若要配置和发布 XDK 游戏。** 合作伙伴中心不支持这一次的 Xbox One XDK 游戏。
* **使用单个 Xbox Live 服务配置为 XDK 和游戏的 UWP 版本 XDP 中创建。** 现在，XDP 的新增功能允许游戏在 XDK 版和 UWP 版游戏之间共享单个 Xbox Live 服务配置。
* **使用合作伙伴中心来引入和发布 UWP 游戏。** 但是，不要使用合作伙伴中心来配置 Xbox Live 服务，因为您的游戏将使用在 XDP 中创建的服务配置。
* **不拆分 XDP 与合作伙伴中心之间的 Xbox Live 服务配置。** XDP 和合作伙伴中心并不知道的并发布一个源中的服务配置将覆盖发布从其他源的配置。 这可能会导致数据丢失（缺少成就、擦除游戏保存内容等），从而带来糟糕的用户体验。 出于这个原因，**我们要求，对于 XDK 和 UWP 跨平台联机游戏，Xbox Live 服务配置必须全部在 XDP 中完成。**

[跨平台联机游戏入门](get-started-with-partner/get-started-with-cross-play-games.md)指南中提供了有关此过程的更多详细信息，包括*不*属于自助服务的项目

### <a name="separate-versions-of-xbox-one-and-pcmobile-games-that-are-not-cross-play"></a>不属于跨平台联机游戏的 Xbox One 和电脑/手机游戏的单独版本
你可以决定使游戏的 Xbox One 版本与电脑/手机版本分隔开来。 在这种情况下，你可以创建两个单独的产品，并按照指南分别仅对 Xbox One XDK 和电脑/手机 UWP 游戏执行操作。

不能在这种情况下，这两个版本使用相同的服务配置，您必须手动创建您的游戏，每个单独的版本的服务配置 XDP 中或在合作伙伴中心适当地。

<a name="get_ids"></a>

## <a name="get-your-ids"></a>获取你的 ID

若要启用 Xbox Live 服务，你需要获取多个 ID，以配置你的开发工具包和主题作品。 这些 ID 可通过执行 Xbox Live 服务配置获得。

如果你当前没有 XDP 或合作伙伴中心中的标题，请参阅上面部分[Xbox Live 服务配置门户](#xbox_live_portals)有关的指南。

### <a name="critical-ids"></a>重要 ID

对于开发适用于 Xbox One 的作品和应用程序，下面 3 个 ID 至关重要：沙盒 ID、作品 ID 和 SCID。

虽然需要拥有沙盒 ID 才能使用开发工具包，但是，初始开发时不需要作品 ID 和 SCID，只是在使用 Xbox Live 服务时才需要用到这两个 ID。 因此，我们建议你一次性获取所有三个 ID。

#### <a name="sandbox-ids"></a>沙盒 ID

沙盒在开发期间为你的开发工具包提供了内容隔离，可确保你拥有一个全新的环境，用于开发和测试你的作品。 沙盒 ID 用于标识你的沙盒。 主机一次只能访问一个沙盒，尽管一个沙盒可供多个主机访问。

沙盒 ID 区分大小写。

**合作伙伴中心**

如果要在合作伙伴中心中配置你的标题，将会出现"Xbox Live"根配置页上，如下所示的沙盒 ID:

![](images/getting_started/devcenter_sandbox_id.png)

**XDP**

如果你在 XDP 配置你的标题，获取您的沙盒 ID 概述页上您的产品，如下所示：

![](images/getting_started/xdp_sandbox_id.png)

#### <a name="service-configuration-id-scid"></a>服务配置 ID (SCID)

在开发过程中，你将创建事件、成就和其他一系列联机功能。 这些内容都属于你的服务配置，需要 SCID 才能访问。

SCID 区分大小写。

**合作伙伴中心**

若要检索你 SCID 在合作伙伴中心，导航到 Xbox Live 服务部分并转到*Xbox Live 安装*。 你的 SCID 将显示在下表中：

![](images/getting_started/devcenter_scid.png)

**XDP**

若要在 XDP 上检索你的 SCID，请导航到你的作品下方的“产品设置”部分，你将会看到“作品 ID”和“SCID”。

![](images/getting_started/xdp_scid.png)

#### <a name="title-id"></a>作品 ID

作品 ID 用于向 Xbox Live 服务唯一地标识你的作品。 该 ID 用于整个服务中，使你的用户可以访问你的主题作品的实时内容、其用户统计数据和成就等内容，并支持实时多人游戏功能。

主题作品 ID 可以区分大小写，具体视使用方式和位置而定。

**合作伙伴中心**

与在 SCID 相同的表中找到了合作伙伴中心中应用标题 ID *Xbox Live 安装*页。

**XDP**

XDP 上的作品 ID 和 SCID 可从同一个位置获得。

<a name="xbox_live_portals"></a>
