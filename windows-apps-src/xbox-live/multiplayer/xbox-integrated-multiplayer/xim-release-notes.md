---
title: Xbox 集成多人游戏发行说明
author: KevinAsgari
description: 包含关于 Xbox 集成多人游戏的发行说明。
ms.assetid: 38df7a49-71b9-4d86-9c49-683ffa7308d6
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, xbox 集成多人游戏
ms.localizationpriority: medium
ms.openlocfilehash: 07c3094b5308bb0062d046440f83b45b842eb820
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2018
ms.locfileid: "3983733"
---
# <a name="xbox-integrated-multiplayer-release-notes"></a>Xbox 集成多人游戏发行说明

更新于 2016 年 12 月 14 日

在此版本的 Xbox 集成多人游戏 (XIM) 中，以下方法不可用：

-   `xim_authority::set_authority_reconciled_data()`
-   `xim_authority::set_authority_reconciliation_data()`
-   `xim_authority::send_data_to_players()`
-   `xim_authority::network_path_information()`
-   `xim_player::xim_local::send_data_to_authority()`

在此版本的 XIM 中，未提供以下状态更改：

-   `xim_state_change_type::player_to_authority_data_received`
-   `xim_state_change_type::authority_to_player_data_received`
-   `xim_state_change_type::authority_reconciling`
-   `xim_state_change_type::authority_reconciled_local`
-   `xim_state_change_type::authority_reconciled_remote`
-   `xim_state_change_type::send_queue_alert_triggered`
