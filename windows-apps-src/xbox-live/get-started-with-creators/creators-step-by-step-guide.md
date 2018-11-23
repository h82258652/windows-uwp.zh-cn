---
title: Xbox Live 创意者分步指南
author: KevinAsgari
description: 提供为创意者计划集成 Xbox Live 的步骤指南。
ms.assetid: 7f9d093e-479a-4ad4-9965-a4ea6f0b2ac3
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 创意者
ms.localizationpriority: medium
ms.openlocfilehash: 3c4d7d03bc218d7507c8c2971bf76946a9fca6b4
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2018
ms.locfileid: "7554361"
---
# <a name="step-by-step-guide-to-integrate-xbox-live-creators-program"></a>Xbox Live 创意者计划集成分步指南

此部分将帮助你准备主题作品并让其在 Xbox Live 上运行：

## <a name="1-ensure-you-have-a-title-created-in-partner-center"></a>1.确保你在合作伙伴中心中创建主题作品
每个 Xbox Live 主题作品必须先将能够在登录并进行 Xbox Live 服务调用定义在[合作伙伴中心](https://partner.microsoft.com/dashboard)。  [创建新的创意者主题作品](create-and-test-a-new-creators-title.md)将为你介绍如何执行此操作。

## <a name="2-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>2. 遵循相应指南设置 IDE 或游戏引擎
你可以遵循适用于自己平台和引擎的相应入门指南，并在过程中了解 Xbox Live 的基础知识。

* [针对开发创意者主题作品使用 Visual Studio](develop-creators-title-with-visual-studio.md)将向你介绍如何关联 Visual Studio 项目与合作伙伴中心中的 Xbox Live 配置。

* [使用 Unity 开发创意者主题作品](develop-creators-title-with-unity.md)将为你介绍如何创建支持 Xbox Live 的 Unity 主题作品、如何为主题作品添加排行榜等功能，以及如何生成本机 Visual Studio 项目。

## <a name="3-xbox-live-concepts"></a>3. Xbox Live 概念
创建主题作品后，你应会了解一些将影响开发主题作品体验的 Xbox Live 概念。

- [Xbox Live 测试环境](../xbox-live-sandboxes.md)
- [授权 Xbox Live 帐户](authorize-xbox-live-accounts.md)

## <a name="4-add-xbox-live-features"></a>4. 添加 Xbox Live 功能

为游戏添加 Xbox Live 功能：

- [Xbox Live 社交平台 - 个人资料、好友、状态](../social-platform/social-platform.md)
- [Xbox Live 数据平台 - 统计数据、排行榜、成就](../data-platform/data-platform.md)
- [Xbox Live 存储平台 - 连接存储、主题作品存储](../storage-platform/storage-platform.md)

## <a name="5-release-your-title"></a>5. 发布主题作品

如果你使用的是 Xbox Live 创意者计划，则该过程与发布任何其他 UWP 的过程相同。  有关该过程的详细信息，可以参阅[发布 Windows 应用](https://developer.microsoft.com/en-us/store/publish-apps)。
