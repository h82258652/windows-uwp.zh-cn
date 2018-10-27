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
ms.openlocfilehash: 5590476c6188a9fb3402da53ecb051abd09433aa
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5701117"
---
# <a name="step-by-step-guide-to-integrate-xbox-live-creators-program"></a>Xbox Live 创意者计划集成分步指南

此部分将帮助你准备主题作品并让其在 Xbox Live 上运行：

## <a name="1-ensure-you-have-a-title-created-on-dev-center"></a>1. 确保你已在开发人员中心上创建了一个主题作品
必须先在开发人员中心上定义每个 Xbox Live 主题作品，然后才能登录并进行 Xbox Live 调用。  [创建新的创意者主题作品](create-and-test-a-new-creators-title.md)将为你介绍如何执行此操作。

## <a name="2-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>2. 遵循相应指南设置 IDE 或游戏引擎
你可以遵循适用于自己平台和引擎的相应入门指南，并在过程中了解 Xbox Live 的基础知识。

* [使用 Visual Studio 开发创意者主题作品](develop-creators-title-with-visual-studio.md)将为你介绍如何关联 Visual Studio 项目与开发人员中心上的 Xbox Live 配置。

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
