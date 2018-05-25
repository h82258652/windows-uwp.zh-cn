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
ms.localizationpriority: low
ms.openlocfilehash: afdc8bb7bcd7dff3664bd1cc7de1ca1b9628b74e
ms.sourcegitcommit: 01760b73fa8cdb423a9aa1f63e72e70647d8f6ab
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/24/2018
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
