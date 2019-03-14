---
Description: 在合作伙伴中心中定义实验，您的应用程序中的代码的实验后，你就试验准备好处于活动状态，并使用合作伙伴中心以查看试验的结果。
title: 在合作伙伴中心中管理实验
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, Microsoft Store Services SDK, A/B 测试, 实验
ms.localizationpriority: medium
ms.openlocfilehash: 6e5c0d0ca1b1d771df2b224cc41ec5a37e267bc9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594922"
---
# <a name="manage-your-experiment-in-partner-center"></a>在合作伙伴中心中管理实验

后[在合作伙伴中心定义试验](define-your-experiment-in-the-dev-center-dashboard.md)并[编码都会针对试验应用](code-your-experiment-in-your-app.md)，已准备好激活你的试验和使用合作伙伴中心以查看试验的结果。 在获取所需的全部数据后，可以结束你的实验，然后选择是继续使用你的所有应用的控件变体中的变量值还是切换到使用其他变体之一中的变量值。

> [!NOTE]
> 激活实验后，合作伙伴中心立即开始收集数据从任何应用程序进行性能分析记录数据的实验。 但是，可能需要几个小时的试验数据出现在合作伙伴中心。

有关演示如何创建并运行实验的端到端过程的演练，请参阅[通过 A/B 测试来创建并运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)。

## <a name="activate-your-experiment"></a>激活实验

当你感到满意的在合作伙伴中心试验参数和已更新应用程序代码时，你现可激活你的试验，以便开始从您的应用程序中收集实验数据。 当实验处于活动状态时，您的应用程序可以检索变体值和到合作伙伴中心报告视图和转换事件。

1. 登录到[合作伙伴中心](https://partner.microsoft.com/dashboard)。
2. 在 **“你的应用”** 下，通过要激活的实验选择该应用。
3. 在导航窗格中，选择**服务**，然后选择**实验**。
4. 在**项目**部分的项目表中，展开包含你的实验的项目，然后执行以下操作之一：
  * 为你的实验单击**激活**链接。 你的实验已添加到页面顶部附近的**活动实验**部分。
  * 单击实验名称、滚动到实验页面底部，然后单击**激活**。

> [!IMPORTANT]
> 激活实验后，不可再对实验参数进行修改，除非在创建实验时，选中了**可编辑实验**复选框。 在激活实验之前，我们建议你在应用中为实验编码。

## <a name="review-the-results-of-your-experiment"></a>查看实验结果

1. 在合作伙伴中心，返回到**试验**应用页。
2. 在**活动实验**部分中，单击活动实验的名称以转到实验页面。
3. 有关活动实验或已完成的实验，此页中的前两个部分提供了实验的结果：
  * **“结果摘要”** 部分列出了实验目标和每个变体的转换率百分比。
  * **“结果详细信息”** 部分为实验中的所有目标的每个变体提供了更多详细信息，包括视图、转换、独特用户、转换率、增量百分比、置信度和重要性。 *置信度*是用来计算错误边距的估计的可靠性统计度量。 *重要性*是基于示例大小的统计度量，用来确定结果不是偶然得出，而是由于特定原因。

> [!NOTE]
> 合作伙伴中心报告在 24 小时时间段内仅第一个转换事件的每个用户。 如果用户在 24 小时时段内在应用中触发多个转换事件，仅报告第一个转换事件。 这是为了帮助防止触发很多转换事件的单个用户扭曲一组示例用户得出的实验结果。


## <a name="complete-your-experiment"></a>完成实验

1. 在合作伙伴中心，返回到实验页。 有关说明，请参阅上一节。
2. 在**结果摘要**部分中，执行以下操作之一：
  * 如果想要结束实验并继续使用应用的控件变体中的变量值，请单击**继续**。
  * 如果想要结束实验但切换到使用应用的不同变体中的变量值，请在要对其进行切换的变体下单击**切换**。
3. 单击 **“确定”** 以确认你想要结束该实验。


## <a name="related-topics"></a>相关主题

* [创建项目并在合作伙伴中心定义远程变量](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [代码应用程序进行试验](code-your-experiment-in-your-app.md)
* [在合作伙伴中心定义试验](define-your-experiment-in-the-dev-center-dashboard.md)
* [创建并运行你的第一个试验使用 A / B 测试](create-and-run-your-first-experiment-with-a-b-testing.md)
* [使用一个运行应用试验 / B 测试](run-app-experiments-with-a-b-testing.md)
