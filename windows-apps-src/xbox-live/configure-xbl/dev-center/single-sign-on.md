---
title: 在开发人员中心配置单一登录
author: KevinAsgari
description: 介绍如何在开发人员中心配置单一登录，以允许游戏通过用户的 Xbox Live ID 让用户登录你的服务。
ms.assetid: ''
ms.author: kevinasg
ms.date: 02/21/2018
ms.topic: article
ms.localizationpriority: medium
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, udc, 通用开发人员中心, 单一登录
ms.openlocfilehash: 42f3e77e8f1f622d8c275769b6f77fdd85a1bff0
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2018
ms.locfileid: "5989699"
---
# <a name="configure-single-sign-on-in-dev-center"></a>在开发人员中心配置单一登录

单一登录允许使用你游戏的玩家使用他们的 Xbox Live 登录来登录你的服务。 这样，登录 Xbox Live 的玩家就可以运行你的提供服务的应用或游戏，而无需使用特定于你的服务的其他帐户凭据再次登录。

> [!NOTE]
> 本主题不适用于 Xbox Live 创意者计划中的主题作品。

例如，你的主题作品可能是应用，只要用户拥有有效的服务帐户，通过该应用，你的服务可将内容（视频，音乐等）流式传输到用户设备上。 如果用户登录了 Xbox Live 帐户，他们就应该能够流式传输内容，而无需每次登录服务。

此外，如果应用将 Kinect 数据发送到外部服务，则可在此处配置。

如果服务要求用户创建与 Xbox Live 不同的帐户，则应为用户提供一种方法，让用户能一次性将其 Xbox Live 帐户与服务上的帐户相链接。

配置单一登录时，可指定 URL 及其信赖方。 只要应用调用指定的任何 URL，Xbox Live 就会自动附加 Xbox 安全令牌服务 (XSTS) 令牌。 接收密钥的服务称为*信赖方*，必须先配置[信赖方](https://developer.microsoft.com/en-US/xboxconfig/relyingparties/index)方可配置单一登录。 每个信赖方配置指定 XSTS 令牌中包含的信息，以及信赖方可用于解码 XSTS 令牌的唯一加密密钥。

通过执行以下操作添加配置：

1. 在[开发人员中心](https://developer.microsoft.com/dashboard/windows/overview)选择主题作品后，导航到**服务** > **Xbox Live**。

2. 点击以下 **Xbox Live 单一登录**链接。

3. 点击**添加 URL** 按钮，创建新的单一登录条目。 此后会在配置列表的底部添加新行。

4. 在 URL 框中，使用完全限定的域名输入服务的 URL。 可使用通配符 ('\ *') 替换最低级别子域。 替换后，会匹配具有相同高等级域的任何 URL。 例如，“* .example.com”将匹配“bar.example.com”或“foo.bar.example.com”。

5. 在信赖方框中，选择指定 XSTS 令牌编码方式的信赖方配置。

6. 点击**保存**按钮保存更改。

![单一登录配置页面的屏幕截图](../../images/dev-center/single-signon.png)
