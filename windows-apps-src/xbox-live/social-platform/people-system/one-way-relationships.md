---
title: "单向关系"
author: KevinAsgari
description: "介绍 Xbox Live 社交平台如何实现单向关系模型。"
ms.assetid: d5a1d311-6de3-4ccc-ab72-15b243e8c2ef
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 社交平台, 邀请, 添加好友"
ms.openlocfilehash: 88b04e1ad45e3e597699104fdd932b48e6165c1d
ms.sourcegitcommit: a7a1b41c7dce6d56250ce3113137391d65d9e401
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2017
---
# <a name="one-way-relationships"></a>单向关系

Xbox Live 已发展为单向关系模型。 用户可以关注任何人，从而更加轻松地查看这些人在 Xbox Live 上的近况、与之交流，并邀请他们加入。

此模型可让用户更快、更轻松地发展他们的社交图谱，而不是需要“另一方”来确认好友请求才能够建立连接。

-   [人脉和人脉列表](#people-and-people-list)
-   [使用开发人员帐户建立你的 Xbox 人脉图](#build-your-xbox-people-graph-with-dev-accounts)
-   [关注其他用户](#following-other-users)
-   [隐私](#privacy)


## <a name="people-and-people-list"></a>人脉和人脉列表

在 Xbox One 上，Xbox Live 现在有“人脉”和“人脉列表”。 人脉系统在 Xbox Live 上取消了以前的 100 限制；用户可以随意添加更多用户，没有实际绑定（虽然确实存在一些相对较大的技术绑定）。 在人脉服务内，对用户可以添加到列表中的人数设定了限制；具体人数尚未确定，但有可能是几千。 请注意，此数字可能随时更改，不像 Xbox 360 中的“固定”100 个好友限制。 不应有外部系统对最大限制假设进行硬编码，并且由于这个原因，目前的限制可能永远不会明确定义。

人脉系统还引入了单向关系模型；用户可以将任何人添加到其人脉列表，而且被添加的人会被立即添加，不会发送必须接受的邀请。 移至单向模型的核心原因是实现整体简单；好友请求让用户又多了一层需要管理的事务，而且经常导致多日延迟，直到其他人看到好友请求并接受它，这时发送方才意识到发生的情况。


## <a name="build-your-xbox-people-graph-with-dev-accounts"></a>使用开发人员帐户建立你的 Xbox 人脉图

你可以使用 xbox.com 利用开发帐户来构建用于测试的 Xbox 人脉图。

首先，从 DevAccountA 发送好友请求：

1.  转到 http://www.xbox.com
2.  单击登录。 输入 DevAccountA 的凭据。
3.  将加载 DevAccountA 的个人资料页面。 单击绿色的“好友”磁贴。
4.  在“按玩家代号查找”框中，键入 DevAccountB 的玩家代号。 单击“查找”。
5.  单击左侧列中的“添加好友(+)”按钮。
6.  在出现的确认消息中单击“关闭”。

然后，接受来自 DevAccountB 的请求：

1.  注销 xbox.com，然后再次单击“登录”。 输入 DevAccountB 的凭据。
2.  将加载 DevAccountB 的个人资料页面。 单击绿色的“消息”磁贴。
3.  在“好友请求”部分，单击“接受”按钮。

现在，在你使用 DevAccountA 登录并调用 ShowSendInvitesAsync() 时，DevAccountB 将出现在列表中（反之亦然）。

或者，你可以为任意一个 XUID 创建调用 **showProfileCardAsync()** 的 Xbox One 示例应用程序。 在调用 **showProfileCardAsync()** 时单击“个人资料卡”上的“关注”按钮，将使调用用户关注提供其 XUID 的用户。


## <a name="following-other-users"></a>关注其他用户

移至单向关系模型的附带好处是，它在 Xbox Live 上开辟了新的“名人”周边场景，任何人都可以将其添加到相应的人脉列表中。 例如，很多用户可能会将著名的赛车游戏玩家添加到他们的列表中，以便可以更轻松地在跟踪时间与他们对战，查看该玩家攀登排行榜的动态，或在他们创建新赛道时获得通知。 单向模型还让支持用户订阅内容的“关注”类型关系变得轻松，如订阅用户在不认识的人（可能有非常一致的音乐爱好）那里发现的音乐播放列表。


## <a name="privacy"></a>隐私

隐私问题往往是单向关系模型环境中最先引发关注的方面，如果有人将我添加到他们的列表中，然后在我不知情的情况下“追踪”我，他们会看到我的哪些信息？ 这里的模型实际与 Xbox Live 上之前的模型并没有不同；通过最新的玩家列表或只需在 Xbox.com 中随便键入一个玩家代号，任何人都可以看到用户在 Xbox Live 隐私设置中允许“每个人”看到的信息。 在单向模型中，通过隐私设置向“每个人”显示的信息，就是将某人添加到他们的列表中，但未被对方添加的用户能够看到的信息。
