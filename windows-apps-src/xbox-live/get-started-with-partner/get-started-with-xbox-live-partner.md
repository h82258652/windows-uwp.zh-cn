---
title: 以合作伙伴的身份开始使用 Xbox Live
description: 提供了帮助托管的合作伙伴或 ID@Xbox 成员开始使用 Xbox Live 开发的链接。
ms.assetid: 69ab75d1-c0e7-4bad-bf8f-5df5cfce13d5
ms.date: 06/07/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 合作伙伴, ID@Xbox
ms.localizationpriority: medium
ms.openlocfilehash: 1a219ee6f4e1dc80ead893c4e230d70e8caa2400
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659822"
---
# <a name="get-started-with-xbox-live-as-a-managed-partner-or-an-idxbox-developer"></a>以托管的合作伙伴或 ID@Xbox 开发人员的身份开始使用 Xbox Live

本部分介绍如何以托管合作伙伴或 ID@Xbox 开发人员的身份开始使用 Xbox Live。 合作伙伴和 ID 开发人员可以访问 Xbox Live 的所有功能，包括成就、多人游戏等。

托管的合作伙伴和 ID@Xbox 开发人员可以针对通用 Windows 平台 (UWP) 或 Xbox 开发人员工具包 (XDK) 平台开发 Xbox Live 主题作品。

除了提供的内容外，还有适用于使用经过授权的合作伙伴中心帐户合作伙伴的其他文档。 您可以访问此处的内容：[Xbox Live 的合作伙伴内容](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/partner-content)。

## <a name="why-should-you-use-xbox-live"></a>为什么要使用 Xbox Live？

Xbox Live 提供了一系列功能，旨在帮助你推广游戏和吸引玩家：

- 通过 Xbox Live 社交平台，可让玩家与好友联系，还能讨论你的游戏。
- 使用 Xbox Live 成就功能，可在玩家解锁成就时通过将游戏免费推广到 Xbox Live 社交图谱上来让你的游戏更受欢迎。
- 使用 Xbox Live 排行榜功能，可通过让玩家竞相击败其好友并提高排名来提升你的游戏参与度。
- 使用 Xbox Live 多人游戏功能，可让玩家与好友一起玩，或者与其他玩家配对，以便在你的游戏中进行竞争或合作。
- Xbox Live 连接存储提供跨设备的免费游戏进度漫游，因此，玩家可以轻松地在 Xbox One 和 Windows 电脑之间延续游戏进度。

## <a name="1-choose-a-platform"></a>1.选择平台
在制作 Xbox 开发工具包 (XDK)、通用 Windows 平台 (UWP) 或跨平台游戏之间作出选择：

- 基于 XDK 的游戏只能在 Xbox One 主机上运行
- UWP 游戏可面向所有 Windows 平台，如 Windows 电脑、Windows Phone 或 Xbox One
  - 对于 Xbox One，请参阅 [Xbox One 上的 UWP](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index)，尤其是 [Xbox One 上 UWP 应用和游戏的系统资源](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation)
- 跨平台游戏通常是使用 XDK 和 UWP 路径的面向 Xbox One 和 Windows 电脑的游戏。

## <a name="2-ensure-that-you-have-a-title-created-in-partner-center-or-xdp"></a>2.请确保已在合作伙伴中心或 XDP 中创建的标题
将能够登录，并进行 Xbox Live 服务调用之前，必须在合作伙伴中心或 Xbox 开发人员门户 (XDP) 中定义 Xbox Live 的每个标题。  [创建新的主题作品](create-a-new-title.md)将为你介绍如何执行此操作。

## <a name="3-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>3.遵照相应的指南来设置您的 IDE 或游戏引擎
你可以遵循适用于自己平台和引擎的相应入门指南，并在过程中了解 Xbox Live 的基础知识。

* [开始使用 Visual Studio 的 UWP 游戏](get-started-with-visual-studio-and-uwp.md)将向您演示如何使用合作伙伴中心中的 Xbox Live 配置将 Visual Studio 项目链接。
* [使用适用于 UWP 游戏的 Unity 入门](partner-add-xbox-live-to-unity-uwp.md)将为你介绍如何新建支持 Xbox Live 的 Unity 主题作品，如何为主题作品添加排行榜等功能，以及如何生成本机 Visual Studio 项目。
* [使用适用于基于 XDK 的游戏的 Visual Studio 入门](xdk-developers.md)将向你介绍如何在使用 XDK 制作 Xbox One 主题作品时获取 Visual Studio 项目设置。
* [制作跨平台游戏入门](get-started-with-cross-play-games.md)介绍如何制作适用于 Xbox One 的基于 XDK 的游戏，以及适用于 Windows 10 电脑的基于 UWP 的游戏。

## <a name="4-xbox-live-concepts"></a>4.Xbox Live 概念
创建主题作品后，你应会了解一些将影响开发主题作品体验的 Xbox Live 概念：

- [Xbox Live 沙盒](../xbox-live-sandboxes.md)
- [Xbox Live 的测试帐户](../xbox-live-test-accounts.md)
- [Xbox Live Api 简介](../introduction-to-xbox-live-apis.md)

## <a name="5-add-xbox-live-features"></a>5.添加 Xbox Live 功能

为游戏添加 Xbox Live 功能：

- [Xbox Live 社交平台的配置文件中，友元，存在](../social-platform/social-platform.md)
- [Xbox Live 数据平台的统计信息、 排行榜、 成就](../data-platform/data-platform.md)
- [Xbox Live 多玩家平台-邀请，比赛比赛](../multiplayer/multiplayer-intro.md)
- [Xbox Live 存储平台-已连接的存储，标题存储](../storage-platform/storage-platform.md)
- [上下文搜索](../contextual-search/introduction-to-contextual-search.md)

## <a name="6-release-your-title"></a>6.发布你的标题

ID@Xbox 和托管合作伙伴在发布其主题作品前必须先通过认证过程。  这是因为这些主题作品有权限访问联机多人游戏、成就和 Rich Presence 等其他功能。  在准备好发布你的主题作品后，请联系 Microsoft 代表以了解有关提交和发布过程的更多详细信息。
