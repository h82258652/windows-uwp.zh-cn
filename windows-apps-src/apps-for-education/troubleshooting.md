---
author: TylerMSFT
Description: "使用事件查看器对 Microsoft 参加测验事件和错误进行疑难解答。"
title: "使用事件查看器对 Microsoft 参加测验进行疑难解答。"
translationtype: Human Translation
ms.sourcegitcommit: f2838d95da66eda32d9cea725a33fc4084d32359
ms.openlocfilehash: 1b99b959cfdde997f7995c1bdf40d51921b2f1d5

---

# 使用事件查看器对 Microsoft 参加测验进行疑难解答

可以使用事件查看器来查看参加测验事件和错误。 参加测验会在已收到锁定请求、设备注册已成功、已成功应用锁定策略等情况下记录事件。

若要在事件查看器中启用查看事件：
1. 打开 `Event Viewer`
2. 导航到 `Applications and Services Logs > Microsoft > Windows > Management-SecureAssessment`
3. 右键单击 `Operational`，然后选择 `Enable Log`

若要保存事件日志：
1. 右键单击 `Operational`
2. 单击 `Save All Events As…`



<!--HONumber=Aug16_HO3-->


