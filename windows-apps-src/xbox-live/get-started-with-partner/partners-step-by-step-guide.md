---
title: Xbox Live 合作伙伴分步指南
author: KevinAsgari
description: 为托管的合作伙伴提供集成 Xbox Live 的步骤指南。
ms.assetid: f0c9db8f-f492-4955-8622-e1736f0a4509
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6c1b1741beaf1ffa2b034726a41c56339136e6f1
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3960320"
---
# <a name="step-by-step-guide-to-integrate-xbox-live-for-managed-partners-and-idxbox-members"></a>为托管的合作伙伴和 ID@Xbox 成员集成 Xbox Live 的分步指南

此部分将帮助你启动并运行 Xbox Live：

## <a name="1-choose-a-platform"></a>1. 选择平台
在制作 Xbox 开发工具包 (XDK)、通用 Windows 平台 (UWP) 或跨平台游戏之间作出选择。

- 基于 XDK 的游戏只能在 Xbox One 主机上运行
- UWP 游戏可面向所有 Windows 平台，如 Windows 电脑、Windows Phone 或 Xbox One
  - 对于 Xbox One，请参阅 [Xbox One 上的 UWP](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index)，尤其是 [Xbox One 上 UWP 应用和游戏的系统资源](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation)
- 跨平台游戏通常是使用 XDK 和 UWP 路径的面向 Xbox One 和 Windows 电脑的游戏。

## <a name="2-ensure-you-have-a-title-created-on-dev-center-or-xdp"></a>2. 确保你已在开发人员中心或 XDP 上创建了一个主题作品
必须先在开发人员中心或 Xbox 开发人员门户 (XDP) 上定义每个 Xbox Live 主题作品，然后才能登录并进行 Xbox Live 调用。  [创建新的主题作品](create-a-new-title.md)将为你介绍如何执行此操作。

## <a name="3-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>3. 遵循相应指南设置 IDE 或游戏引擎
你可以遵循适用于自己平台和引擎的相应入门指南，并在过程中了解 Xbox Live 的基础知识。

* [使用适用于 UWP 游戏的 Visual Studio 入门](get-started-with-visual-studio-and-uwp.md)将为你介绍如何关联 Visual Studio 项目与开发人员中心上的 Xbox Live 配置。

* [使用适用于 UWP 游戏的 Unity 入门](partner-add-xbox-live-to-unity-uwp.md)将为你介绍如何新建支持 Xbox Live 的 Unity 主题作品，如何为主题作品添加排行榜等功能，以及如何生成本机 Visual Studio 项目。

* [使用适用于基于 XDK 的游戏的 Visual Studio 入门](xdk-developers.md)将向你介绍如何在使用 XDK 制作 Xbox One 主题作品时获取 Visual Studio 项目设置。

* [制作跨平台游戏入门](get-started-with-cross-play-games.md)介绍如何制作适用于 Xbox One 的基于 XDK 的游戏，以及适用于 Windows 10 电脑的基于 UWP 的游戏。

## <a name="4-xbox-live-concepts"></a>4. Xbox Live 概念
创建主题作品后，你应会了解一些将影响开发主题作品体验的 Xbox Live 概念。

- [Xbox Live 沙盒](../xbox-live-sandboxes.md)
- [Xbox Live 测试帐户](../xbox-live-test-accounts.md)
- [Xbox Live API 简介](../introduction-to-xbox-live-apis.md)

## <a name="5-add-xbox-live-features"></a>5. 添加 Xbox Live 功能

为游戏添加 Xbox Live 功能：

- [Xbox Live 社交平台 - 个人资料、好友、状态](../social-platform/social-platform.md)
- [Xbox Live 数据平台 - 统计数据、排行榜、成就](../data-platform/data-platform.md)
- [Xbox Live 多人游戏平台 - 邀请、匹配、锦标赛](../multiplayer/multiplayer-intro.md)
- [Xbox Live 存储平台 - 连接存储、主题作品存储](../storage-platform/storage-platform.md)
- [上下文搜索](../contextual-search/introduction-to-contextual-search.md)

## <a name="6-release-your-title"></a>6. 发布主题作品

游戏发布者（即 Microsoft 合作伙伴）在发布 ID@Xbox 主题作品和主题作品之前必须通过认证过程。  这是因为这些主题作品有权限访问多人游戏、成就和富状态等其他功能。  在准备好发布你的主题作品后，请联系 Microsoft 代表以了解有关提交和发布过程的更多详细信息。
