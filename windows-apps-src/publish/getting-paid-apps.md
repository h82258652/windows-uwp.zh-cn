---
Description: 了解如何为应用、外接程序（应用内产品）和广告收益接收付款。
title: 收取付款
ms.assetid: 37D1EF45-C4A8-4849-8819-3D4A4898215C
ms.date: 05/29/2020
ms.topic: article
keywords: Windows 10, uwp, 付款, 应用销售, 应用收款, 付款, 应用商店费用, 付款暂停, 百分比
ms.localizationpriority: medium
ms.openlocfilehash: 0d42677aeda694e2fc8924cee1832b62d98b15e5
ms.sourcegitcommit: a937963ce63a14c254420926661b9b68be28a8ee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2020
ms.locfileid: "84746767"
---
# <a name="getting-paid"></a>收取付款
下面是有关接收应用、外接程序和广告收益的付款的重要信息。

> [!IMPORTANT]
> 在从 Microsoft Store 中的应用销售额获得资金之前，你需要[设置你的帐户，并填写所需的税务形式](setting-up-your-payout-account-and-tax-forms.md)。

> [!NOTE]
> 如果你正在寻找有关付款的支持，包括配置帐户、缺失付款、将付款置于保持状态或其他任何内容，请在[此处](https://developer.microsoft.com/windows/support)联系支持人员。

## <a name="store-fee"></a>应用商店费用

当你[注册开发者帐户](https://developer.microsoft.com/store/register)时，你将接受[应用开发人员协议](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)。 此协议针对你在 Microsoft Store 中出售应用的相关事宜，包括 Microsoft 对每笔销售收取的应用商店费用，阐述了你与 Microsoft 之间的关系。

费用已在[应用开发人员协议](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)中正式规定。 如果有任何问题，可始终查看该文档。

Microsoft Store 费用适用于通过 Microsoft Store 收到的所有应用销售，包括加载项。


## <a name="price-tiers"></a>价格段

所选价格段用来设置选择分配应用的所有国家/地区中的[销售价格](set-and-schedule-app-pricing.md#base-price)。 还可以使用其他定价功能，例如[为不同的市场选择不同的价格](set-and-schedule-app-pricing.md#override-base-price-for-specific-markets)或[促销应用](put-apps-and-add-ons-on-sale.md)。

可以免费提供应用，也可以设定客户获取应用所需支付的价格金额。 价格段从 0.99 美元开始，随后进行递增（1.09 美元、1.19 美元等）。 价格段之间的递增会随着价格变高而增加。

> [!NOTE] 
> 这些价格段也适用于从应用中提供的任何加载项。

每个价格段都在应用商店提供的每种货币中具有对应值。 我们使用这些值来帮助你在世界范围内以适当价格销售应用。 但是，由于外汇汇率会发生变化，因此确切的销售额可能会在货币之间稍有不同。 汇率按月计算。 根据事务发生的时间，将应用相应的汇率。 它所采用的汇率和日期范围分别在 exchangeRate 和 exchangeRateDate 列中的支出报表上指定。

你还可以选择使用所选的特定市场的当地货币输入自由格式价格。 执行此操作时，价格不会进行调整（即使转换率发生变化），除非你提交包含新价格的更新。 

请记住，所选的价格可以包括客户必须支付的销售税或增值税。 有关详细信息，请参阅[付费应用的详细税收信息](tax-details-for-paid-apps.md)。


## <a name="payout-reporting"></a>付款报告

可以在[合作伙伴中心](https://partner.microsoft.com/dashboard)的“付款摘要”中访问有关付款信息的详细信息以及下载报表。 有关此处显示的信息和我们如何对你的收入进行归类的详细信息，请参阅[付款摘要](payout-summary.md)。


## <a name="payout-timeframe"></a>付款时限

每月进行付款（前提是已达到适用的付款阈值，并且你未暂停你的付款，如下所述）。 我们通常会在给定月份的 15 日前发送该月应付的任何付款。 请注意，付款通常需要 3 到 10 个额外工作日才能到达你的付款帐户。 有关详细信息，请参阅[付款阈值、方法和时间范围](payment-thresholds-methods-and-timeframes.md)。


##  <a name="payout-hold-status"></a>付款暂停状态

默认情况下，我们将每月发送付款，如上所述。 但是，可以选择暂停每个计划的付款，这将阻止我们向你的帐户发送付款。 如果选择将付款置于保持状态，我们将继续记录你所获得的任何收入，并提供你的付出的详细信息**摘要**。 但是，在你删除暂停前，我们不会向你的帐户发送任何付款。

若要暂停付款，请转到“开发人员设置”。 在“付款和税务”下的“付款和税务配置文件分配”部分中，找到要对其暂停付款的计划。 单击“暂停我的付款”复选框以暂停此计划的付款。 可以随时更改付款暂停状态，但请注意，你的决策将影响下一次每月付款。 例如，如果您想要保留四月的付出，请确保在三月份结束之前将您的支出保持状态设置为 **"开**"。

将付款暂停状态设置为“打开”后，此计划的所有付款都将处于暂停状态，直到将滑块切换回“关闭” 。 当执行此操作时，将在下一个每月付款周期期间包含你（前提是已达到任何适用的付款阈值）。 例如，如果你已有付款，但想要在六月生成一项支出，则请确保在 "可能" 结束之前，将 "付出保留" 状态切换为 "**关**"。

> [!NOTE]
> “付款暂停状态”会分别应用于每个计划（Microsoft Store、广告、Azure 市场等）。 如果希望对所有计划暂停付款，则必须分别对每个计划暂停付款。


 

 




