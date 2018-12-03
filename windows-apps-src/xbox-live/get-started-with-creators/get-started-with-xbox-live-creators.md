---
title: Xbox Live 创意者计划入门
description: 提供了帮你开始使用 Xbox Live 创意者计划的链接。
ms.assetid: 2a744405-7ee4-42b4-8f36-9916e8c3a530
ms.date: 12/13/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cce9d34679884d48475b7a7ae0fa8286204a6289
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8457625"
---
# <a name="get-started-with-the-xbox-live-creators-program"></a>Xbox Live 创意者计划入门
 
Xbox Live 创意者计划让你可以通过简化的认证流程将你的游戏快速地直接发布到 Xbox One 和 Windows10，并且无需任何概念审批。 如果你的游戏集成了 Xbox Live 且遵循我们的[标准应用商店策略](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx)，你已经可以发布了。 本文将概括介绍通过 Xbox Live 集成发布和运行游戏所需要的操作。 

Xbox Live 创意者计划游戏必须属于通用 Windows 平台 (UWP) 应用程序。 对于 Xbox One，请参阅 [Xbox One 上的 UWP](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index)，尤其是 [Xbox One 上 UWP 应用和游戏的系统资源](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation)。 通过 Xbox Live 创意者计划发布的游戏没有访问成就或多人游戏联机服务的权限。 有关支持的服务的完整列表，请参阅[开发人员计划概述功能表](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/developer-program-overview#feature-table)。

## <a name="1-ensure-you-have-a-title-created-in-partner-center"></a>1.确保你在合作伙伴中心中创建主题作品
每个 Xbox Live 的主题作品必须在[合作伙伴中心](https://partner.microsoft.com/dashboard)中定义之前你将能够在登录并进行 Xbox Live 服务调用。  [创建新的创意者主题作品](create-and-test-a-new-creators-title.md)将为你介绍如何执行此操作。

## <a name="2-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>2. 遵循相应指南设置 IDE 或游戏引擎
你可以遵循适用于自己平台和引擎的相应“入门指南”，并在过程中了解 Xbox Live 的基础知识：

* [针对开发创意者主题作品使用 Visual Studio](develop-creators-title-with-visual-studio.md)将向你介绍如何关联 Visual Studio 项目与合作伙伴中心中的 Xbox Live 配置。
* [使用 Unity 开发创意者主题作品](develop-creators-title-with-unity.md)将为你介绍如何创建支持 Xbox Live 的新 Unity 游戏、处理单用户和多用户登录、添加排行榜和统计信息等功能，以及如何生成本机 Visual Studio 项目。

虽然 Unity 仅我们为其提供文档的第三方游戏引擎，游戏引擎[构建 （2 和 3）](https://www.scirra.com/construct2)和[Game Maker Studio](https://www.yoyogames.com/gamemaker)还有文档可帮助你将 Xbox Live 集成到你的 Construct 或 Game Maker Studio 游戏分别。

* [游戏 Maker Studio 2 UWP 现在支持 Xbox Live 创意者计划](https://www.yoyogames.com/gamemaker/xblc)将向你介绍如何导出你的 Game Maker Studio 项目，以在 Xbox One 和 Windows 10 电脑上播放。
* [使用 Xbox Live 在 UWP 应用的构造](https://www.scirra.com/tutorials/9540/using-xbox-live-in-uwp-apps)将向你介绍了如何使用 Xbox Live 在你的 Construct 2 和 3 的游戏中。

你仍可以为没有记录 Xbox Live 集成的其他游戏开发引擎，使用 Xbox Live Api 将 Xbox Live 添加到你的游戏。 要从你的项目使用 Xbox Live API，可以通过 NuGet 程序包添加对二进制文件的引用或者添加 API 源。 添加 NuGet 程序包可加快编译速度，而添加源可简化调试。

为支持 Xbox Live 服务使用第三方不可 Unity 的游戏引擎工作用相应的游戏引擎的员工要回答问题。

## <a name="3-xbox-live-concepts--testing"></a>3. Xbox Live 概念和测试
创建主题作品后，你应会了解一些将影响开发主题作品体验的 Xbox Live 概念。 此外，你还需要在游戏计划支持的所有平台上测试游戏，以确保游戏能够按预期运行。

- [创意者计划的 Xbox Live 服务配置](xbox-live-service-configuration-creators.md)
- [Xbox Live 测试环境](../xbox-live-sandboxes.md)
- [授权 Xbox Live 帐户](authorize-xbox-live-accounts.md)

## <a name="4-enable-xbox-live-sign-in"></a>4. 启用 Xbox Live 登录
所有 Xbox Live 创意者计划游戏都必须集成 Xbox Live 登录并显示用户身份（玩家代号、玩家头像等）。 你可以选择自动登录用户或允许用户利用按钮来启动登录进程。 登录后必须显示玩家代号，以便玩家验证使用的配置文件正确无误。

- [Xbox Live 社交平台 - 个人资料、好友、状态](../social-platform/social-platform.md)

## <a name="5-add-optional-xbox-live-features"></a>5. 添加可选的 Xbox Live 功能

Xbox Live 创意者计划提供了一系列功能，旨在帮助你推广游戏和吸引玩家：

- [Xbox Live 数据平台 - 统计信息、排行榜](../data-platform/data-platform.md)可通过让玩家竞相击败其好友并提高排名来提升你的游戏参与度。
- [Xbox Live 存储平台 - 连接存储、主题作品存储](../storage-platform/storage-platform.md)提供跨设备的免费游戏进度漫游，因此，玩家可以轻松地在 Xbox One 和 Windows 电脑之间延续游戏进度。
- [Xbox Live 社交平台 - 个人资料、好友、状态](../social-platform/social-platform.md)可让玩家与好友联系，还能讨论你的游戏。

请务必注意，Xbox Live 创意者计划不支持联机多人游戏、成就或玩家分数。

## <a name="6-release-your-game"></a>6. 发布游戏

如果你使用的是 Xbox Live 创意者计划，则该过程与发布任何其他 UWP 应用程序的过程相同：

- [Microsoft Store 策略](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx)包括[游戏和 Xbox 策略](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx#pol_10_13)
- [发布 Windows 应用](https://developer.microsoft.com/en-us/store/publish-apps)