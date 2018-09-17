---
title: Xbox Arena
author: KevinAsgari
description: 了解如何使用 Xbox Arena 为你的游戏运行锦标赛。
ms.author: kevinasg
ms.date: 09-20-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, arena, 锦标赛, ux
ms.localizationpriority: medium
ms.openlocfilehash: 7e4df0894cc2c8ab214e129193a37b68f408194f
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2018
ms.locfileid: "3990283"
---
# <a name="xbox-arena"></a>Xbox Arena

Xbox Arena 是通过 Xbox Live 跨 Xbox One 和 Windows 10 创建和运行在线锦标赛的平台，目标是为广大受众带来竞争性游戏的乐趣。
当涉及竞争性游戏时，每个发布者会有不同的需求，使用 Arena，我们希望能够尽可能地灵活。 有三种不同方法可在 Arena 上为你的游戏运行锦标赛：

* 通过特许经营锦标赛，Xbox Live 可为你提供用于设置和运行自己的锦标赛的工具和服务。

* Xbox 已与行业领先的比赛组织者 (TO) 合作，你可以通过 Arena 让 TO 代表你的游戏运行锦标赛。 （有关受支持的锦标赛组织者的列表，请联系你的 Microsoft 客户经理）。

* 玩家可以通过用户生成的锦标赛或 UGT 创建并运行自己的 Arena 锦标赛。

最重要的是，通过向对你的游戏中添加对 Arena 的支持，你可以利用上述三种方法。 我们提供的强大 API 使游戏能够推广游戏内锦标赛，并便于参与者在比赛之间来回切换。 Xbox Arena 中心支持比赛管理任务，例如注册、通知、归类和播种以及报告结果。

> [!IMPORTANT]  
> Xbox Arena 仅对托管的合作伙伴和 ID@Xbox 开发人员可用。 不可用于 Xbox Live 创意者计划。

## <a name="a-titles-baseline-tournament-experience"></a>游戏的基准锦标赛体验

如果你的游戏与一个或多个锦标赛组织者集成，它必须提供基准锦标赛体验。 如果你愿意，可以自由地与特定的锦标赛组织者进行更深入的集成，以便为竞争提供更丰富的体验。 但你还必须为其他锦标赛组织者、UGT 和特许经营锦标赛（如果选择运行这些比赛）提供基准体验。

### <a name="baseline-requirements-for-a-title"></a>游戏的基准要求

* 有关的完整列表的要求，请联系你的 Microsoft 帐户管理器。

### <a name="ui-recommendations"></a>UI 建议

* 在 UI 中识别比赛是锦标赛。

* 在大厅 UI 中，包括一个 UI 元素，它可将用户链接至 Xbox Arena shell UI 或锦标赛组织者应用中的锦标赛中心和/或锦标赛详细信息页面。



游戏提供的基准用户体验必须简单且能充分灵活地使用多种可能的竞争格式。 你可以自由地调整用户体验指导和要求来适应游戏的 UI 流，以确保顺畅的用户体验。

例如：

* 所需的信息不一定显示在主屏幕上，前提是它在某处可用，例如在详细信息页面或弹出窗口中。

* 每个屏幕可能有许多版本，或者这些屏幕会彼此组合或与现有的游戏屏幕进行组合。 例如，许多游戏都包括赛后“作战报告”屏幕，可对其进行调整以满足“等待结果”和“就绪”屏幕要求。

* 阶段更改时，屏幕不需要立即更改。 例如，如果团队阶段从“就绪”切换到“比赛”，而用户在“就绪”屏幕上，不要求你的游戏立即跳转到玩游戏屏幕。 它可以（可能应该）将“等待比赛…” 指示器切换至例如“准备比赛”按钮，这样用户可控制并更好地理解流程。 “比赛”阶段的要求可以一直延迟到用户确认转换。


## <a name="arena-vs-title-roles"></a>Arena 与游戏“角色”

当用户在锦标赛阶段中进阶时，移入和移出游戏会使用户感到复杂。 当针对用户玩的每个游戏该过程有所不同时，甚至很少有机会记住具体位置和预期操作。

> [!TIP]
> **UX 建议**  
>
> 通过使游戏和 Xbox Arena UI 可以清晰地区分，简化两者之间的功能性角色。 例如，与管理相关的所有任务都在 Arena 中完成，与玩游戏相关的所有任务都在游戏内完成。

Xbox Arena 角色（设置锦标赛）   | 游戏的角色（游戏）
--- | ---
<ul><li>注册和报到</li><li>通知</li><li>种子设定和分组</li><li>团队组建</li></ul> |     <ul><li>参与者进出 Arena UI 的过渡</li><li>在多人游戏大厅 UI 中标识特定于锦标赛的比赛</li><li>推广和/或浏览游戏内锦标赛</li></ul>

## <a name="engineering-guidance"></a>工程指南

文章 | 描述
--- | ---
[Arena 游戏集成](arena-title-integration.md) | 了解如何在游戏中集成对 Xbox Arena 的支持。

## <a name="operations-guidance"></a>操作指南

文章 | 描述
--- | ---
[Xbox Arena 操作门户](operations-portal.md) | 介绍了可用于创建和管理游戏与 Xbox Arena 集成的官方锦标赛操作门户。

## <a name="user-experience-guidance"></a>用户体验指南

文章 | 描述
--- | ---
[发现 Xbox 锦标赛](discovering-xbox-tournaments.md) | 提供用于打造发现现有锦标赛的卓越用户体验的技巧和建议。
[加入锦标赛](arena-ux-join-tournament.md)  |  提供了用于打造注册和加入锦标赛的卓越用户体验的技巧和建议。
[比赛参与](arena-ux-match-engagement.md) | 描述玩家在锦标赛经历中不断进阶的用户体验阶段。
[Arena API UI 元数据](arena-apis-metadata.md)  | 描述 Arena API 返回的元数据，可以使用这些数据显示有关锦标赛当前状态的游戏内信息。
[Arena 通知](arena-notifications.md)  | 描述 Xbox Arena 向锦标赛参与者发送通知的条件。
[Arena 用户方案](arena-user-scenarios.md)  | 根据玩家的最常见动机描述面向玩家的 Arena 方案。
