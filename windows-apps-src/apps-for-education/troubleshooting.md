---
Description: Troubleshoot Microsoft Take a Test events and errors with the event viewer.
title: 使用事件查看器对 Microsoft 参加测验进行疑难解答。
author: PatrickFarley
ms.author: pafarley
ms.assetid: 9218e542-f520-4616-98fc-b113d5a08e0f
ms.date: 10/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp 教育
ms.localizationpriority: medium
ms.openlocfilehash: 3193525316d085e56244d6f03da99e3e07c6539f
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "2856153"
---
# <a name="troubleshoot-microsoft-take-a-test-with-the-event-viewer"></a>使用事件查看器对 Microsoft 参加测验进行疑难解答

可以使用事件查看器来查看参加测验事件和错误。 参加测验会在已收到锁定请求、设备注册已成功、已成功应用锁定策略等情况下记录事件。

若要在事件查看器中启用查看事件：
1. 打开 `Event Viewer`
2. 导航到 `Applications and Services Logs > Microsoft > Windows > Management-SecureAssessment`
3. 右键单击 `Operational`，然后选择 `Enable Log`

若要保存事件日志：
1. 右键单击 `Operational`
2. 单击 `Save All Events As…`
