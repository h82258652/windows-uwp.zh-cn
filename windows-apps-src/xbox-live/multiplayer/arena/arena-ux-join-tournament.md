---
title: 加入锦标赛
author: KevinAsgari
description: 了解如何创建 UI，以供玩家加入你的游戏的锦标赛。
ms.author: kevinasg
ms.date: 10-10-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, arena, 锦标赛, ux
ms.localizationpriority: medium
ms.openlocfilehash: 6d5f1e4c658d2638f53baaa4176ba741990c667f
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/05/2018
ms.locfileid: "4391666"
---
# <a name="join-a-tournament-by-using-the-arena-ui"></a>使用 Arena UI 加入锦标赛

Arena API 可向游戏提供有关注册、报到和团队信息的数据，但它们*不*提供*执行*相关任务的功能。 你的游戏将使用 Arena UI 或第三方锦标赛组织者 (TO)，或构建自己的锦标赛管理支持。

## <a name="xbox-arena-ui-team-formation"></a>Xbox Arena UI：团队组建

Arena UI 向玩家提供组建或加入团队的方法。 对游戏没有任何要求。

###### <a name="ui-example-create-a-team"></a>UI 示例：创建团队

![构建团队屏幕](../../images/arena/arena-ux-create-team.png)

#### <a name="when-forming-a-team-a-gamer-can"></a>在组建团队时，玩家可以：

* 向一个或多个好友或俱乐部成员发送加入邀请。
* 通过发布 LFG 广告查找团队成员。
* 注册或注销团队。
* 删除团队成员。

## <a name="xbox-arena-ui-registration"></a>Xbox Arena UI：注册

Arena UI 向玩家提供注册团队的方法。 对游戏没有任何要求。

###### <a name="ui-example-register-a-team"></a>UI 示例：注册团队

![注册团队屏幕](../../images/arena/arena-ux-register-team.png)

#### <a name="when-registering-for-a-tournament-a-user-can"></a>注册锦标赛时，用户可以：

* 在注册关闭*之前*注销锦标赛。
* 在报到时或在锦标赛进行期间注销团队。
* 注销整个团队。 *请注意，单个用户离开团队会注销整个团队。*
* 注册成为队长。
* 在注册之前了解锦标赛要求和参与规则。
* 收到注册成功的通知。
* 实时看到锦标赛状态更改为“已注册”。
* 在锦标赛开始时预览比赛对手。
* 查看已注册锦标赛的玩家。
* 如果不符合锦标赛资格，则阻止其注册。
* 如果玩家已被禁赛，则阻止其注册。

> [!div class="nextstepaction"]
> [比赛参与](arena-ux-match-engagement.md)
