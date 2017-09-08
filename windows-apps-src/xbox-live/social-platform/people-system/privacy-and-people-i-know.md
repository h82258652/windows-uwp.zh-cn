---
title: "隐私和我认识的联系人"
author: KevinAsgari
description: "了解 Xbox Live 社交平台服务的隐私模型。"
ms.assetid: 9031ca37-bab7-44b1-ae40-fca7459f5f59
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 社交平台, 隐私, 匿名"
ms.openlocfilehash: 2b78adfc10822c6e05ffdf488f4e27c648f92784
ms.sourcegitcommit: 90fbdc0e25e0dff40c571d6687143dd7e16ab8a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2017
---
# <a name="privacy-and-people-i-know"></a>隐私和我认识的联系人

新人脉系统的核心功能是，它既面向匿名关系也面向非匿名关系提供；用户可以有真实姓名好友，也可以有在 Xbox Live 上遇到的加入联系人列表的随机玩家代号关系。 这意味着必须支持两个不同的权限集：一个用来允许受信任的现实世界的好友查看敏感信息，第二个可让用户添加感觉对方不会泄露任何敏感信息的随机玩家代号关系。

之前的 Xbox Live 隐私模型提供了基于“3 桶”系统限制访问帐户信息的功能；给定设置如：联机状态可以设置为“任何人”、“仅好友”或“已阻止”。 例如，试想一下一名使用 Xbox Live 的 14 岁女孩。 她添加了一些她完全信任的同学，然后又在与一些几乎不认识的随机用户玩了几轮 Halo 后将他们添加到了联系人列表中。 以前的模型意味着在她列表上的每个人都只能被给予相同级别的访问权限；无法让她的同学看到她的个人简历或游戏历史记录，她添加的那些几乎不认识的人也同样看不到；这最后让玩家在共享信息时成为“保守主义者”，或对于要添加的人犹豫不决。

新的隐私模型在结构上保持不变，但移到了“4 桶”系统：“任何人”、“我列表中的联系人”、“我认识的联系人”或“已阻止”。 “我认识的联系人”桶包含用户允许其访问真实姓名信息的联系人。 “我认识的联系人”的隐私权限是“我列表中的联系人”的严格子集；换言之，授予后者的权限同时会给予前者。
