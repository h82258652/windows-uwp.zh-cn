---
title: 发现 Xbox 锦标赛
author: KevinAsgari
description: 了解如何为游戏创建用于发现锦标赛的 UI。
ms.author: kevinasg
ms.date: 10-10-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, arena, 锦标赛, ux
ms.localizationpriority: medium
ms.openlocfilehash: 3417304a1033084ef7543b602b80901a38a5b0b1
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "4464620"
---
# <a name="discovering-xbox-tournaments"></a>发现 Xbox 锦标赛

玩家可通过 Xbox 操作面板中的多个选项或通过电脑上的 Xbox 应用来了解有关参与或观看锦标赛的更多信息。 还有越来越多的机会可从你的作品中发现锦标赛。  

玩家可以找到支持 Arena 的锦标赛，方法是：

* 从 Xbox 主页中打开最近玩的游戏的“锦标赛中心”。
* 访问玩家的个人资料，以了解已参加、正在参加或已注册的锦标赛。
* 响应他们所属俱乐部的查找组 (LFG) 文章。
* 在 Mixer 应用、网站中观看锦标赛流视频或在游戏中心/Arena 中观看比赛数据。
* 访问 Xbox 操作面板中的游戏中心。

## <a name="promoting-tournaments-in-game"></a>推广游戏内锦标赛

人们由于对作品感兴趣而观看锦标赛。 Xbox 游戏有一个独特的机会可在关键时刻吸引玩家，即在玩家寻找扩展游戏体验的新途径时。

> [!TIP]
> **UX 建议：** 在制定决策时推广锦标赛。
>
> 在玩家需要决定继续玩还是退出时提高锦标赛的认知度（例如，在游戏启动时、主菜单、多人游戏菜单或比赛结束时）。

### <a name="1--use-a-dynamic-announcement-feature-example-message-of-the-day"></a>1. 使用动态公告功能（示例：当日消息）

在游戏 UI 中内置公告功能的游戏通常使用允许即时更新的内容管理系统。 这可用于向玩家推送新内容，而不依赖内容更新或可下载的内容版本。 例如，Halo 使用“当日消息”功能突出显示社区公告和提示。 锦标赛推广特别适合这种情况。

**用户影响**

* 在玩家启动游戏会话之前向其介绍新的竞争性游戏机会。
* 曝光是间歇性的（在启动时、与其他的公告组合在一起）。
* 若要了解详细信息，玩家将被指引退出游戏并转到 Arena 中心。
* 该功能不需要内容更新来填充。
* 推广的相关性和计时很灵活。

###### <a name="ui-example-message-of-the-day"></a>UI 示例：“当日消息”

![当日的锦标赛消息](../../images/arena/arena-ux-motd.png)

#### <a name="a-main-heading-ex-play-watch-learn"></a>A. 主标题示例：比赛、观看、了解  

向玩家通知所有锦标赛或特定锦标赛。

#### <a name="b-go-to-arena-hub"></a>B. 转到 Arena 中心  

到游戏中心/锦标赛的深层链接 - 建议在推广与游戏相关的所有锦标赛时使用。  
– 或者 –  
到游戏中心/锦标赛/详细信息的深层链接 - 建议在推广特定锦标赛时使用。

#### <a name="c-game-exit-confirmation"></a>C. 游戏退出确认

屏幕元素间 A 和 B 导致用户退出游戏转至 Xbox 操作面板。 在玩家做出决策前，在游戏的 UI 中设置这一预期。

###### <a name="ui-example-exit-game-confirmation"></a>UI 示例：退出游戏确认
![锦标赛退出游戏确认](../../images/arena/arena-ux-exit-confirm.png)

### <a name="2--promote-tournaments-on-the-main-menu"></a>2. 在主菜单上推广锦标赛

在此示例中，公告是关于近期活动的交互式广告。 它提供了足够的信息来吸引玩家了解详情。 通过**选择**，玩家将转到 Arena 中心的锦标赛详细信息页面，在这里他们可以管理注册。

###### <a name="ui-example-a-tournament-ad-displayed-alongside-the-main-menu"></a>UI 示例：主菜单旁边显示的锦标赛广告

![](../../images/arena/arena-ux-promo.png)

> [!TIP]
> **UX 建议**  
> 公告应只包含能够帮助玩家决定是否要了解详情的足够细节。 例如，***描述***、***热门程度***和***时间安排***。

## <a name="browsing-tournaments-in-game"></a>浏览游戏内锦标赛

Arena API 为你的作品提供了足够数据，以用于在游戏内创建浏览体验。 如果要生成浏览功能，请在“多人游戏”部分添加**锦标赛**入口点。

> [!TIP]
> **UX 建议**  
> 将锦标赛呈现为多人游戏模式。  
> 由于 Xbox Arena 是一项新功能，请确保其在第 1 层或第 2 层级别保持可见。

###### <a name="ui-example-tournaments-listed-as-an-additional-mode-in-the-multiplayer-section"></a>UI 示例：锦标赛在“多人游戏”部分列为附加模式

![锦标赛模式](../../images/arena/arena-ux-tournament-mode.png)

### <a name="provide-a-comprehensive-list-of-tournaments"></a>提供锦标赛的完整列表

使玩家能够快速检查它们当前已注册的锦标赛的状态，并能够在游戏会话期间浏览锦标赛。

#### <a name="user-impact"></a>用户影响

* 尽量减少玩家离开游戏以在仪表板中检查锦标赛状态的需求。
* 如果玩家错过 Xbox Arena Toast 通知，则作为出色的备份解决方案。
* 涉及供游戏开发人员管理的额外成本。
* 将玩家指引至用于加入、注册、检查、播种和团队形成的 Arena UI。

> [!NOTE]  
> Arena API 不针对用户生成的锦标赛（俱乐部生成的）提供查询。


> [!TIP]
> **UX 建议**  
> 创建一个可以缩放的 UI，并提供可供玩家管理大型锦标赛列表的方法。

### <a name="filters"></a>筛选器

通过**所有**、**活动**（其中涉及到锦标赛结束之前的所有内容）、**已取消**和**已结束**状态筛选锦标赛，以及玩家已参加或即将参加的锦标赛（例如，My Tournaments）。

###### <a name="ui-example"></a>UI 示例：

![锦标赛筛选器屏幕](../../images/arena/arena-ux-filters.png)

> [!NOTE]  
> 添加自定义筛选器，筛选出可能提供、*但不推荐* API 支持的情况。

你的作品可在发出请求后通过其他属性进行筛选，但不能在查询参数中指定这些属性。 这会使得通过其他属性筛选对于此类型的 UI 而言不可靠。 Arena API 一次可在作品中检索指定数量的结果，例如，作品可将 **maxItems** 值指定为 10 次锦标赛。 这样有助于改善你的作品管理性能，并尽量降低大型列表让用户难以应对的风险。 但是，你的游戏提供的任何自定义筛选器都只会应用于此次请求的 10 个项目。 这可能导致每个 API 调用后的筛选结果数不一致。 例如，返回的第一组 10 个锦标赛中可能有 2 个与自定义筛选器（例如“比赛”）相关。 但是下 10 个锦标赛可能显示 9 个此类项目，等等。 作品完全填充每个自定义筛选器的唯一可靠的方法是，请求每个活动的锦标赛然后对其全部进行筛选，我们并不推荐这种做法。

#### <a name="filter-recommendations"></a>筛选建议：

##### <a name="all"></a>所有

查看所有**活动**、**已取消**和**已结束**的锦标赛，从最新的开始排序。

结果：

* 锦标赛状态
* 锦标赛开始日期
* 锦标赛状态 - 例如，注册正在进行
* 到仪表板中锦标赛详细信息页面的深层链接

##### <a name="my-tournaments"></a>MY TOURNAMENTS

查看参与者当前已注册的锦标赛。

结果：

* 当前已登录用户所参与的锦标赛
* 锦标赛状态
* 进入锦标赛比赛的方法（对于“比赛就绪”状态）
* 到仪表板中锦标赛详细信息页面的深层链接

##### <a name="active"></a>活动

查看已创建的所有锦标赛：即将推出、正在注册、报到和正在比赛。

结果：

* 正在进行注册并且当前用户尚未加入的锦标赛
* 注册关闭之前的剩余时间

##### <a name="completedcanceled"></a>已结束/已取消

查看最新结果。 随着时间的推移，刚刚结束的锦标赛会逐渐停止，而不是完成后便消失。

结果：

* 到仪表板中锦标赛详细信息页面的深层链接
* 锦标赛状态
* 结束日期/时间

##### <a name="canceled"></a>已取消

查看已取消的锦标赛。

结果：

* 到仪表板中锦标赛详细信息页面的深层链接
* 取消日期/时间

> [!div class="nextstepaction"]
> [使用 Arena UI 加入锦标赛](arena-ux-join-tournament.md)
