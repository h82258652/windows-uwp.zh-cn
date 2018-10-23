---
title: 单点入网 (SPOP)
author: KevinAsgari
description: 了解如何使用 Xbox Live 单点入网 (SPOP) 确保只可在一个设备上玩游戏一次。
ms.assetid: 40f19319-9ccc-4d35-85eb-4749c2cf5ef8
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, spop, 单点入网
ms.localizationpriority: medium
ms.openlocfilehash: 1ad187ea8645138d3076892e893cb0b770236ae8
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2018
ms.locfileid: "5396518"
---
# <a name="single-point-of-presence-spop"></a>单点入网 (SPOP)

## <a name="overview"></a>概述
单点入网 (SPOP) 是一种 Xbox Live 强制执行条件，在此条件下，只可在一个设备上玩游戏一次。 这将对任何设备上的 Xbox One XDK 和 UWP 游戏强制执行。
Xbox Live 游戏（无论设备是否已打开）将拒绝已登录至其他 Xbox One 或 Windows 10 设备上的游戏的用户。

## <a name="how-to-handle-spop"></a>如何处理 SPOP
游戏对 SPOP 的处理方式与任何其他类型的注销事件相同。 当用户执行用于启动 SPOP 以验证他们是否想要断开其他设备上的游戏时，将始终通过 UI 通知用户。

* 对于 XDK 游戏，发生此情况时将会触发 `User::SignOutCompleted` 事件。
* 对于 UWP 游戏，将通过 `xbox_live_user` 类中的 `sign_out_complete` 处理程序通知它们注销。 请参阅 [UWP 项目的身份验证](authentication-for-UWP-projects.md)，以了解更多详细信息
