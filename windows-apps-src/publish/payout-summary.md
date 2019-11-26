---
Description: “付款摘要”会向你显示有关通过应用和加载项获得的收益的详细信息。 它还使你可以知道你将何时收到付款和收到多少付款。
title: 付款摘要
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 08/02/2019
ms.topic: article
keywords: windows 10, uwp, 付款摘要, 声明, 付款, 收益, 支出, 付款, 收入
ms.localizationpriority: medium
ms.openlocfilehash: d609af268cfe304b34797cea4bf91e36d1475c29
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259008"
---
# <a name="payout-summary"></a>付款摘要

费用**摘要**显示了你在 Microsoft 获得的资金的详细信息。 它还使你可以知道你将何时收到付款和收到多少付款。

如果在 Azure Marketplace 中销售产品，还将在**付款摘要**中看到成功付款信息。 有关 Azure Marketplace 付款的更多详细信息，请参阅 [Microsoft Azure 应用商店参与策略](https://docs.microsoft.com/legal/marketplace/participation-policy)和 [Microsoft Azure 应用商店发布者协议](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt)。

> [!NOTE]
> 为了满足付出的条件，你的继续必须达到[支付阈值](payment-thresholds-methods-and-timeframes.md)$50。 有关付款阈值的详细信息，请参阅此页并查看应用开发人员协议。

## <a name="access-the-payout-summary-pages"></a>访问支出摘要页面

若要打开一个付出的摘要页面：

1. 选择右上角的 "货币" 图标。
2. 选择 "付款"、"事务历史记录" 或 "导出数据"。

## <a name="payments-page"></a>付款页

此页上的总计表示你参与的所有程序。 您可以按参与者 ID、节目、付款 ID 和收入类型进行筛选。 金额为美元提供。 支付的值还会以货币支付。

| 区域                   | 说明                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------|
| 本年度支付的总金额   | 你的所有计划在今年（美元）支付的总金额。       |
| 下一估计付款 | 接下来的一项付款（即使有其他人即将发布），以美元为单位。 |
| 上次付款           | 最近支付的金额（美元）、节目名称和计划。           |
| 按源付款     | 过去12个月内按计划表示的付款金额，单位为美元。           |
| 付款               | 选择 "付费" 或 "挂起"，然后根据需要进行排序。 有关特定付款的其他详细信息，请选择 "查看"。 若要下载付款汇款报表的副本，请选择 "下载"。 请注意，事务历史记录数据可能需要长达24小时才能显示，因此你可能看不到相关收入。 |

若要导出此页上的任何数据，请选择 "导出"，然后按照 "导出数据" 页上的说明进行操作。

## <a name="transaction-history-page"></a>事务历史记录页

此页面显示您的个人收入，包括每个收入的日期、类型和收益。 你可以选择要查看的时间段，还可以按注册 ID、计划、付款 ID、收益类型、杠杆和状态进行筛选。 数据可用于当前会计年度（7月1日–6月30日）和前两个会计年度。

若要查看有关收益的详细信息，请选择页面右侧的向下箭头。 这将显示杠杆、收入金额和产品。 如果出于某种原因，此数据无法使用，但你需要访问它，请与[支持](https://developer.microsoft.com/en-us/windows/support)人员联系。 如果收益是调整的结果，而不是交易的，则不会显示 "产品" 字段。

若要导出此页上的任何事务数据，请选择 "导出"，然后按照 "导出数据" 页上的说明操作。 从 "事务历史记录" 页导出的文件以交易货币显示数据，按交易币种和美元计算收入，并按货币支付付薪值。

## <a name="payment-status"></a>付款状态

| 收益状态           | 原因                                                                                                                                      | 需要合作伙伴操作？                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| 尚未              | 收益有资格支付。 它在激励计划的收视指南中定义的冷却周期处于此状态。 | 否                                                         |
| 到来                 | 支付订单在处理付款之前生成待定内部评审。                                                               | 否                                                         |
| 待定销售税发票      | 您的纳税发票不完整或无效。                                                                                                  | 您需要更新您的纳税发票才能支付费用 |
| 审核时被拒绝   | 付款在评审期间被拒绝。                                                                                                     | 有关详细信息，请联系[Microsoft 支持](https://developer.microsoft.com/en-us/windows/support)部门                      |
| Failed                   | 由于 Microsoft 系统错误，付款失败。                                                                                         | 有关详细信息，请联系[Microsoft 支持](https://developer.microsoft.com/en-us/windows/support)部门                      |
| 正在进行              | 付款正在进行。                                                                                                                 | 否                                                         |
| 付款错误        | 付款 recouping 正在进行。                                                                                                       | 否                                                         |
| Sent                     | 已将付款发送到银行。                                                                                                     | 否                                                         |
| 处理             | 付款遇到 Microsoft 系统错误，正在重新处理。                                                                  | 否                                                         |
| 反向                 | 付款已被银行冲销，并将在下一个付款周期再次发送。                                                     | 否                                                         |
| 销售税发票已拒绝     | 您的纳税发票在审查期间被拒绝。 在纳税发票评审完成之前，所有待定付款都将处于暂停状态。                 | 有关详细信息，请联系[Microsoft 支持](https://developer.microsoft.com/en-us/windows/support)部门                      |
| 审查后的纳税发票 | 正在查看您的纳税发票。 发票获得批准后，你的付款就会发布。                                   | 否                                                         |
| 否决                 | 银行拒绝付款。                                                                                                      | 联系银行获取详细信息。                             |

## <a name="export-data-page"></a>"导出数据" 页

按照此页上的说明导出所需的数据。

注意：

- "导出数据" 页不会自行刷新。 你可能需要手动刷新页面以查看最新数据。
- 筛选器可能会导致 "无数据" 错误。 这可能意味着，你已将默认时间段设置为三个月的时间，然后从超出该时间段的收入中选择付款 ID。 展开你的时间段，然后重试。

## <a name="payment-download-export"></a>支付下载导出

此选项可用于下载你在银行中收到的给定节目、相关税款和总收入金额的付款。 此报告用于许多合作伙伴中心计划，因此某些列可能会不适用您的报表。 这些列标记如下。

| 列名称              | 说明                                                                                                                               |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------  |
| participantID            | 计划下合作伙伴收益的主要标识                                                                             |
| participantIDType        | 应用商店计划的激励计划和卖方 ID 的程序 id                                                                |
| participantName          | 收益伙伴的名称                                                                                                               |
| programName              | 激励/存储程序名称                                                                                                              |
| 所得                   | 该计划/participantID 的货币支付金额                                                                       |
| earnedUSD                | 节目/参与者 ID 的获得额（美元）                                                                                      |
| withheldTax              | 计划/participantID 的货币的预扣税金                                                               |
| salesTax                 | 计划/participantID （仅适用于激励计划）的支付的总销售税金额                   |
| serviceFeeTax            | 计划/participantID 的 serviceFeeTax 总金额（仅适用于商店计划和 Azure Marketplace） |
| totalPayment             | 按本币支付的总付款金额，包括预缴税金，并包括该计划/participantID 的销售税（如果适用）   |
| currencyCode             | 支付给货币代码                                                                                                                      |
| paymentMethod            | 用于支付合作伙伴的方法例如，电子银行转帐、贷方说明                                                             |
| paymentID                | 付款的唯一标识符。 此数字通常在银行报表中可见。 （仅适用于 SAP 付款）              |
| paymentStatus            | 付款状态                                                                                                                            |
| paymentStatusDescription | 付款状态的友好说明                                                                                                    |
| paymentDate              | 从 Microsoft 发送了日期支付                                                                                                      |

## <a name="transaction-history-download-export"></a>事务历史记录下载导出

此选项可用于下载您在 "交易历史记录" 页中看到的每个收益行项、收入类型、日期、关联的交易金额、客户、产品和适用于您的计划的其他事务详细信息。

| 列名称                    | 说明                                                                                                                              | 激励/存储/Azure Marketplace 的适用性           |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| earningId                      | 每个收益的唯一标识符                                                                                                       | 全部                                                            |
| participantId                  | 计划下合作伙伴收益的主要标识                                                                            | 全部                                                            |
| participantIdType              | 应用商店计划和 Azure Marketplace 的激励计划和卖方的主要计划 ID                                          | 全部                                                            |
| participantName                | 收益伙伴的名称                                                                                                              | 全部                                                            |
| partnerCountryCode             | 收益合作伙伴所在国家/地区                                                                                                  | 全部                                                            |
| programName                    | 激励/存储程序名称                                                                                                             | 全部                                                            |
| transactionId                  | 事务的唯一标识符                                                                                                    | 全部                                                            |
| transactionCurrency            | 原始客户交易发生的币种（这不是合作伙伴位置货币）                                     | 全部                                                            |
| transactionDate                | 交易日期。 适用于多个事务贡献给一个收益的程序                                           | 全部                                                            |
| transactionExchangeRate        | 用于显示相应事务美元的汇率日期                                                                 | 全部                                                            |
| transactionAmount              | 基于所生成收益的原始交易币种的交易金额                                              | 全部                                                            |
| transactionAmountUSD           | 交易金额（美元）                                                                                                                | 全部                                                            |
| 断裂                          | 指示收入的业务规则                                                                                                  | 全部                                                            |
| earningRate                    | 应用于交易金额以生成收入的激励率                                                                      | 全部                                                            |
| quantity                       | 根据程序而异。 指示交易计划的计费数量                                                            | 全部                                                            |
| quantityType                   | 指示数量的类型，例如，计费数量、MAU                                                                                     | 全部                                                            |
| earningType                    | 指示是费用、回扣、市场活动、销售等。                                                                                          | 全部                                                            |
| earningAmount                  | 按原始交易币种的收入金额                                                                                      | 全部                                                            |
| earningAmountUSD               | 收入金额（美元）                                                                                                                    | 全部                                                            |
| earningDate                    | 收益日期                                                                                                                      | 全部                                                            |
| calculationDate                | 在系统中计算收入的日期                                                                                            | 全部                                                            |
| earningExchangeRate            | 用于显示相应 USD 金额的汇率                                                                                  | 全部                                                            |
| exchangeRateDate               | 用于计算 EarningAmount USD 的汇率日期                                                                                   | 全部                                                            |
| paymentAmountWOTax             | 仅为 "已发送" 付款支付的金额（不含税）                                                                 | 全部                                                            |
| paymentCurrency                | 支付给伙伴在付款配置文件中选择的货币。 仅显示已发送的付款                                                   | 全部                                                            |
| paymentExchangeRate            | 用于使用 ExchangeRateDate 计算 paymentAmountWOTax 的汇率                                            | 全部                                                            |
| claimId                        | 声明的唯一标识符                                                                                                              | 激励-仅限一些程序                                |
| planId                         | 计划的唯一标识符                                                                                                               | 激励-仅限一些程序                                |
| paymentId                      | 付款的唯一标识符。 此数字通常显示在银行对帐单中                                                 | 仅限 SAP 支付                                              |
| paymentStatus                  | 付款状态                                                                                                                           | 全部                                                            |
| paymentStatusDescription       | 付款状态的友好说明                                                                                                   | 全部                                                            |
| 顾客                     | 始终为空                                                                                                                     | 仅激励计划（例外： OEM）和 Azure Marketplace |
| customerName                   | 始终为空                                                                                                                     | 仅激励计划（例外： OEM）和 Azure Marketplace |
| partNumber                     | 始终为空                                                                                                                     | 某些激励和商店计划和 Azure Marketplace        |
| productName                    | 链接到事务的产品名称                                                                                                       | 全部                                                            |
| productId                      | 唯一产品标识符                                                                                                                | 存储和 Azure Marketplace                                    |
| ParentProductID                | 唯一的父产品标识符。 请注意：如果没有用来交易的父产品，则父产品 ID = 产品 ID。 | 存储和 Azure Marketplace                                    |
| parentProductName              | 父产品的名称。 请注意：如果没有用来交易的父产品，则父产品名称 = 产品名称。   | 存储和 Azure Marketplace                                    |
| productType                    | 产品的类型（如应用、加载项、游戏等）                                                                                        | 存储和 Azure Marketplace                                    |
| invoiceNumber                  | 发票编号（仅适用于 EA）                                                                                                  | 激励和 Azure Marketplace-仅限一些程序           |
| subscriptionId                 | 与客户关联的订阅标识符                                                                                         | 激励-仅限一些程序                                 |
| And subscription.subscriptionstartdate          | 订阅开始日期                                                                                                                  | 激励-仅限一些程序                                 |
| Subscription.subscriptionenddate            | 订阅结束日期                                                                                                                    | 激励-仅限一些程序                                 |
| resellerId                     | 经销商标识符                                                                                                                      | 激励-仅限一些程序                                 |
| resellerName                   | 经销商名称                                                                                                                            | 激励-仅限一些程序                                 |
| distributorId                  | 分发服务器标识符                                                                                                                   | 激励-仅限一些程序                                 |
| distributorName                | 分发服务器名称                                                                                                                         | 激励-仅限一些程序                                 |
| agreementNumber                | 协议编号                                                                                                                         | 激励-仅限一些程序                                 |
| agreementStartDate             | 协议开始日期                                                                                                                     | 激励-仅限一些程序                                 |
| agreementEndDate               | 协议结束日期                                                                                                                       | 激励-仅限一些程序                                 |
| 均衡                       | 工作负荷                                                                                                                                 | 激励-仅限一些程序                                 |
| transactionType                | 交易的类型（如购买、退款、冲销、拒付等）                                                               | 存储和 Azure Marketplace                                    |
| localProviderSeller            | 本地提供商/记录的卖方                                                                                                          | 仅限应用商店                                                     |
| taxRemitted                    | 免除的税收额（销售税、使用税或 VAT/GST 税）。                                                                                   | 存储和 Azure Marketplace                                    |
| taxRemitModel                  | 负责代缴税款（销售税、使用税或 VAT/GST 税）的一方。                                                                    | 仅限应用商店                                                     |
| storeFee                       | Microsoft 为使应用或外接程序在应用商店中可用而保留的金额。                                           | 仅限应用商店                                                     |
| transactionPaymentMethod       | 客户用于交易的付款方式（如信用卡、移动运营商结算、PayPal 等）                                | 存储和 Azure Marketplace                                    |
| tpan                           | 指示第三方 ad 网络                                                                                                     | 存储-仅广告                                               |
| customerCountry                | 客户所在国家/地区                                                                                                                         | 存储和 Azure Marketplace                                    |
| customerCity                   | 客户城市                                                                                                                            | 存储和 Azure Marketplace                                    |
| customerState                  | 客户状态                                                                                                                           | 存储和 Azure Marketplace                                    |
| customerZip                    | 客户邮政编码                                                                                                                 | 存储和 Azure Marketplace                                    |
| purchaseTypeCode               | 始终为空                                                                                                                     | 激励计划-CRI                                        |
| purchaseOrderType              | 始终为空                                                                                                                     | 激励计划-CRI                                        |
| purchaseOrderCoverageStartDate | 始终为空                                                                                                                     | 激励计划-CRI                                        |
| purchaseOrderCoverageEndDate   | 始终为空                                                                                                                     | 激励计划-CRI                                        |
| programOfferingLevel           |                                                                                                                                          | 激励计划-CRI                                        |
| TenantID                       |                                                                                                                                          | 激励计划                                             |
| externalReferenceId            | 程序的唯一标识符                                                                                                        | 直接付费计划（激励和商店）                      |
| externalReferenceIdLabel       | 唯一标识符标签                                                                                                                  | 直接付费计划（激励和商店）                      |
