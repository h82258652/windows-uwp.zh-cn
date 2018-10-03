---
title: Xbox Live People System
author: aablackm
description: 了解 Xbox Live People System。
ms.assetid: f1881a52-8e65-4364-9937-d2b8b8476cbf
ms.author: aablackm
ms.date: 03/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 社交, people system, 好友
ms.localizationpriority: medium
ms.openlocfilehash: 6ab0add0f379654be1285faac85690794bf48f9e
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2018
ms.locfileid: "4257797"
---
# <a name="xbox-live-people-system"></a>Xbox Live People System

在开始将 Xbox Live 社交服务集成到作品之前，了解你所使用的系统可能会很有用。 为此，我们将简要说明 Xbox Live People System 的工作方式。 People System 规定并维护 Xbox Live 生态系统中玩家之间的关系。 People System 允许玩家在 Xbox Live 上轻松识别和组织其联系人，无论他们是亲密的好友还是在线熟人。 你可以向信任的人显示真实姓名，并向不信任的人隐藏真实姓名。 你可以与好友一起玩最喜爱的游戏，这样，当你在线时，便始终能知道他们在做什么。 Xbox Live People System 允许你添加好友，并且向你提供好游戏的人都在一个安全、易于组织的包中。

## <a name="one-and-two-way-relationships"></a>单向和双向关系

People System 通过*关注*概念，允许你建立单向和双向关系。 以前，你只能发送邀请并等待他们接受加为好友的请求，以这种方式将其他玩家*加为好友*，从而建立一种可以彼此关注的双向关系。 这个过程可能需要几天时间，具体取决于玩家查看其好友请求的频率。 通过添加单向关系，你可以*关注*某个用户，并立即查看其在线活动，而不是等待他们响应。 在这个阶段，你是*关注者*。 你*关注*的人将会收到一条通知，并有机会反过来关注你，这是一种可以让你们通过双向关系*加为好友*的操作。 单向关系可以减少互动等待时间，并有额外的好处，它允许你与受观众欢迎或因其竞争实力而受到关注的著名玩家保持联系。

## <a name="gametags-and-real-names"></a>玩家代号和真实姓名

当你可能具有无限多的好友和关注关系时，创建和维护“现实世界的用户是哪个玩家代号”的心理地图可能会非常困难。 正是由于这个原因，我们才为你提供了向你认识的玩家显示真实姓名的选项，以便他们可以更容易识别你。 显示真实姓名是可选的，必须由用户进行选择。 不会向 Xbox Live 上的任何人显示真实姓名，只会向用户认识的人显示，这些人通过用户手动指示的连接或通过与用户帐户相关联的社交网络进行表示。 在匹配或其他活动过程中与随机陌生人交互时，用户始终显示为玩家代号；此外，没有向陌生人显示你的真实姓名的选项。

用户可以控制他们在游戏中的显示方式，并可以决定其他人在游戏中始终只能看到他们的玩家代号，即使这些人中有一些可能有权访问他们的真实姓名。 在用户选择加入允许显示（仅限于他们认识的人）真实姓名的功能时，他们会看到在游戏中始终显示为玩家代号的选项。 选择此选项意味着在游戏屏幕上显示在其头部上方的名称始终是他们的玩家代号。

## <a name="privacy-and-people-i-know"></a>隐私和我认识的人

People System 的核心功能是，它既提供匿名关系，也提供非匿名关系；用户可以有真实姓名好友，也可以与在 Xbox Live 上遇到的用户建立随机玩家代号关系。 这意味着必须支持两个不同的权限集：一个用来允许受信任的现实世界的好友查看敏感信息，第二个可让用户添加感觉对方不会泄露任何敏感信息的随机玩家代号关系。
除此之外，用户还可以对有关自己的单独几条信息设置限制，例如他们的配置文件、在线状态和好友列表。 此信息的访问权限可以分为三类。

- 任何人：向遇到该用户的任何人公开信息。
- 好友：仅向用户视为好友的人提供信息。
- 阻止：阻止访问信息。

请参阅 [xbox.com](https://account.xbox.com/Settings) 上的隐私设置完整列表。

## <a name="favorite-people"></a>喜欢的人

Xbox Live People System 为最终用户提供了一个针对其自己的人物列表的简单类别系统。 用户可以将列表中的任何人标记为最喜爱的人物，这些人会在各个功能中首先显示并拥有更高的优先级。 每当显示人物列表时，作品中将首先显示最喜爱的人物。 用户可以拥有的最喜爱的人物的数量没有限制。 用户可以在其列表上将任何人切换为最喜爱的人物，无论那个人是真实姓名还是玩家代号关系，不会有任何隐私隐患。 “喜欢的人”是用户人物列表的子集，不同于单独的人物列表。