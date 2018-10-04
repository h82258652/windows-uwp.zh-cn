---
title: 对 Rich Presence 进行编程
author: KevinAsgari
description: 提供用于设置 Xbox Live 成员的联机状态的代码示例。
ms.assetid: 7e6e7b69-d7c3-42fa-bcc4-6d68947f6fdb
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 完整状态
ms.localizationpriority: medium
ms.openlocfilehash: bd39074e67c58e20154083be66211dbefc9f782d
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4354281"
---
# <a name="programming-rich-presence"></a>对 Rich Presence 进行编程

Rich Presence 提供将玩家的当前活动播发给其他玩家的功能。 有关详细信息，请参阅 [Rich presence 字符串：概述](rich-presence-strings-overview.md)。

```cpp

XboxLiveContext^ xboxLiveContext = NULL;

// Set the XboxLiveContext for a user *once* otherwise you may encounter unpredictable behavior.
void OneTimeInit()
{
  // Get the XboxLiveContext.  This should only be done once per user after signing in.
  xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));
}

void Example_PresenceService_SetPresenceAsync()
{
    // The config ID below is an example.
    // The real ID to use is defined when you set up the game on Xbox Development Portal.
    String^ aServiceConfigurationId = "C1BA92A9-0000-0000-0000-000000000000";

    PresenceData^ presenceData = ref new PresenceData(
        aServiceConfigurationId,
        "mainMenuPresence"
        );

    IAsyncAction^ setPresenceAction = xboxLiveContext->PresenceService->SetPresenceAsync(
        xboxLiveContext->User->XboxLiveId,
        true, // isUserActive
        presenceData    // richPresenceData is optional
        );

    create_task( setPresenceAction )
    .then([] ()
    {
        LogComment( L"SetPresenceAsync Done" );
    })
    .wait();
}
```
