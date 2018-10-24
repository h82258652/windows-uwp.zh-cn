---
author: jnHs
Description: Here’s some important info you’ll need to ensure that you receive payment for your apps, in-app products (IAPs), and advertising earnings.
title: 获取付款
ms.assetid: 37D1EF45-C4A8-4849-8819-3D4A4898215C
ms.author: wdg-dev-content
ms.date: 02/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 付款, 应用销售, 应用收款, 付款, 应用商店费用, 付款暂停, 百分比
ms.localizationpriority: medium
ms.openlocfilehash: 0c128bedd1c889f4c2dcf0565c7c10575eb75013
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5478698"
---
# <a name="getting-paid"></a>获取付款
以下是一些你需要的信息，可确保你收到你的应用、加载项和广告收益的付款。

> [!IMPORTANT]
> 在从 Microsoft Store 的应用销售中获得收益之前，需要[设置付款帐户并填写必要的税单](setting-up-your-payout-account-and-tax-forms.md)。

## <a name="store-fee"></a>应用商店费用

当你[注册开发者帐户](http://go.microsoft.com/fwlink/p/?LinkID=615100)时，你将接受[应用开发人员协议](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)。 此协议针对你在 Microsoft Store 中出售应用的相关事宜，包括 Microsoft 对每笔销售收取的应用商店费用，阐述了你与 Microsoft 之间的关系。

在大多数情况下，应用商店费用是 30%。 费用已在[应用开发人员协议](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)中正式规定。 如果你有任何问题，请随时查看该文档。

Microsoft Store 费用适用于通过 Microsoft Store 收到的所有应用销售，包括加载项。


## <a name="price-tiers"></a>价格段

所选价格段用来设置选择分配应用的所有国家/地区中的[销售价格](set-and-schedule-app-pricing.md#base-price)。 还可以使用其他定价功能，例如[为不同的市场选择不同的价格](set-and-schedule-app-pricing.md#override-base-price-for-specific-markets)或[促销应用](put-apps-and-add-ons-on-sale.md)。

可以免费提供应用，也可以设定客户获取应用所需支付的价格金额。 价格段从 0.99 美元开始，并具有附加增量（1.09 美元、1.19 美元等）。 价格越高，价格段之间的增量就越大。

> [!NOTE] 
> 这些价格段也适用于从应用中提供的任何加载项。

应用商店为每个价格段都提供了多种不同货币对应值。 我们使用这些值来帮助你在世界范围内以适当价格销售应用。 但由于外汇汇率的变化，从一种货币转换到另一种货币的确切销售金额可能会略有不同。

你还可以选择使用所选的特定市场的当地货币输入自由格式价格。 输入时，除非你提交新价格更新，否则不会调整该价格（即使汇率发生变化）。 

请记住，你选择的价格可能包含你的客户必须支付的销售税或增值税。 有关详细信息，请参阅[付费应用的详细税收信息](tax-details-for-paid-apps.md)。


## <a name="payout-reporting"></a>付款报告

你可以在 Windows 开发人员中心仪表板的 **“付款摘要”** 中访问有关付款信息的详细内容并下载报告。 有关此处显示的信息和我们如何对你的收入进行归类的详细信息，请参阅[付款摘要](payout-summary.md)。


## <a name="payout-timeframe"></a>付款时限

每月进行付款（前提是已达到适用的付款阈值，并且你未暂停你的付款，如下所述）。 我们通常在指定月的第 15 日发送该月的所有到期付款。 请注意，付款通常需要 3 到 10 个工作日才会到达你的付款帐户。 有关详细信息，请参阅[付款阈值、方法和时间范围](payment-thresholds-methods-and-timeframes.md)。


##  <a name="payout-hold-status"></a>付款暂停状态

默认情况下，我们将每月发送付款，如上所述。 但是，你可以选择暂停付款，这将阻止我们向你的帐户发送付款。 如果你选择暂停付款，我们将继续记录你所赚取的所有收入，并在你的**付快摘要**中提供详细信息。 但是，在你删除暂停前，我们不会向你的帐户发送任何付款。 

若要暂停付款，请转到**帐户设置**。 在 **“财务详细信息”** 下的 **“付款暂停状态”** 部分中，将滑块切换为 **“开”**。 你可以随时更改你的付款暂停状态，但请注意，你的决定将影响下一次每月付款。 例如，如果你希望暂停四月的付款，请确保在三月结束前将付款暂停状态设置为 **“开”**。

将付款暂停状态设置为 **“开”** 后，所有付款都将处于暂停状态，直到你将滑块切换回 **“关”**。 当执行此操作时，将在下一个每月付款周期期间包含你（前提是已达到任何适用的付款阈值）。 例如，如果你已暂停付款，但希望在 6 月生成一个付款，则请确保在 5 月结束前将付款暂停状态切换为 **“关”**。

> [!NOTE]
> **付款暂停状态**选择适用于**所有**通过 Windows 开发人员中心支付的收入来源（Microsoft Store、广告和 Azure Marketplace 等）。 无法为每个收入来源选择不同的暂停状态。


 

 




