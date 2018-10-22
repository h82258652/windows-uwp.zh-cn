---
title: XDK 项目的身份验证
author: KevinAsgari
description: 了解如何在 Xbox 开发工具包 (XDK) 游戏中登录 Xbox Live 用户。
ms.assetid: 713bb2e3-80c5-4ac9-8697-257525f243d3
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 594c966ab3d1e70d5a423da0e4681ecfca22f4b0
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2018
ms.locfileid: "5395566"
---
# <a name="authentication-for-xdk-projects"></a>XDK 项目的身份验证

为了在游戏中充分利用 Xbox Live 功能，用户需要创建一个 Xbox Live 个人资料，以在 Xbox Live 社区标识自己的身份。  Xbox Live 服务将会跟踪使用该 Xbox Live 个人资料的游戏相关活动，例如用户的玩家代号和玩家图片、用户的游戏好友、用户所玩的游戏、用户已解锁的成就、用户在特定游戏的排行榜中的排名等。

在高级别，你可以通过以下步骤使用 Xbox Live API：
1. 标识交互用户
2. 创建一个基于用户的 Xbox Live 上下文
3. 使用 Xbox Live 上下文访问 Xbox Live 服务
4. 在退出游戏或用户注销时，发布 XboxLiveContext 对象，方法是将其设为 null

### <a name="creating-an-xboxliveuser-object"></a>创建 XboxLiveUser 对象
大部分 Xbox Live 活动与 Xbox Live 用户相关。  作为游戏开发人员，你需要先创建一个代表本地用户的 XboxLiveUser 对象。

C++：
```
Windows::Xbox::System::User^ user; // the interacting user.  From User::Users, etc
std::shared_ptr<xbox::services::xbox_live_context> xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>( user );
```

WinRT：
```
Windows::Xbox::System::User^ user; // the interacting user.  From User::Users, etc
Microsoft::Xbox::Services::XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext( user );
```
