---
title: XDP 上的 Achievements 2017
author: KevinAsgari
description: " Achievements 2017"
ms.assetid: d424db04-328d-470c-81d3-5d4b82cb792f
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: low
ms.openlocfilehash: 26155f83d567bda4c5472b7e262e888757bee87f
ms.sourcegitcommit: 01760b73fa8cdb423a9aa1f63e72e70647d8f6ab
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/24/2018
---
# <a name="configure-achievements-2017-on-xdp"></a>在 XDP 上配置 Achievements 2017

在 Achievements 2017 系统中，唯一的成就配置需求为其名称、锁定/解锁描述、显示图像和奖励信息。 （注意：有效的成就奖励仍包括：玩家分数、艺术作品奖励和游戏内奖励。）

<span id="_Enable_Simplified_Achievements" class="anchor"></span>

## <a name="enable-achievements-2017"></a>启用 Achievements 2017

游戏使用的成就系统在产品级别进行管理。  

在发布至 RETAIL 之前，开发人员随时可以在简单与由云支持的成就系统之间切换其产品。 切换成就系统时，在任一方向，所有已配置和发布的游戏成就（和挑战，如适用）将从每个沙盒中删除。 

游戏的服务配置发布到 RETAIL 后，其成就系统已永久设置，不能更改。 **不允许任何例外。 这是出于技术和策略原因。**

1.  从 XDP 中的产品页中，导航至**产品设置**。
![XDP 中的产品页的屏幕截图](../../images/omega/simplified-achievements-1.png)

2.  选择**产品详细信息**。
![XDP 中的产品设置页的屏幕截图](../../images/omega/simplified-achievements-2.png)

1.  将**成就配置系统**切换按钮切换为 *Achievements 2017*。
![显示 Achievements 2017 与 Achievements 2013 之间的切换的产品详细信息页的屏幕截图](../../images/omega/simplified-achievements-3.png)

1.  你将会收到一条提示将从所有沙盒中删除所有游戏成就的警告消息。 如果你同意删除所有沙盒中的现有成就，则单击**保存**。
![显示警告的屏幕截图](../../images/omega/simplified-achievements-4.png)

## <a name="configure-an-achievement"></a>配置成就

1.  为你的游戏启用 Achievements 2017。

2.  导航至**服务配置**并选择**成就**。
![XDP 中的服务配置任务页的屏幕截图](../../images/omega/simplified-achievements-5.png)

1.  输入成就显示详细信息。

    *注意：这些字符串用于在 XDP UI 中显示。 必须在“本地化字符串”服务配置选项（步骤 5）中配置向用户显示的最终字符串。*<br>
![XDP 中的“添加新成就”对话框的屏幕截图](../../images/omega/simplified-achievements-6.png)

1.  要为成就添加游戏分数、插图或应用内奖励，请单击**奖励**部分下面的**新建**。
![XDP 中的“Edit Reward”对话框的屏幕截图](../../images/omega/simplified-achievements-7.png)

1.  如果要为成就名称和描述提供本地化的字符串，请导航至**本地化字符串**。

    *注意：请不要忘记定义英语本地化字符串。 否则，喜欢英语文本、位于美国以外的国家/地区的用户可能无法获得预期结果。*<br>
![XDP 中的“本地化字符串”页的屏幕截图](../../images/omega/simplified-achievements-8.png)

1.  要将当前更改与近期发布的服务配置数据对比，请导航至**比较数据**并选择比较所需的沙盒。
![XDP 上的“比较数据”页的屏幕截图](../../images/omega/simplified-achievements-9.png)

1.  准备好在开发人员沙盒中进行发布和测试时，请返回**服务配置**并单击**发布**按钮。
![XDP 上显示“发布”按钮的“服务配置任务”页的屏幕截图](../../images/omega/simplified-achievements-10.png)

1.  选择要测试的目标沙盒（可能是你在其中绘制成就的同一沙盒）。

    选中*事件、状态规则、成就…* 复选框（位于服务配置下面）。

    单击**提交**。

![XDP 上的“正在发布审批”页的屏幕截图](../../images/omega/simplified-achievements-11.png)
