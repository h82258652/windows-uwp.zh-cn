---
Description: 支出报表显示你在应用和外接程序中获得的资金的详细信息。 它还使你可以知道你将何时收到付款和收到多少付款。
title: 支出报表
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 08/02/2019
ms.topic: article
keywords: windows 10, uwp, 付款摘要, 声明, 付款, 收益, 支出, 付款, 收入
ms.localizationpriority: medium
ms.openlocfilehash: f4d8727a48cd68b304d515fe34082b4c4f632b4b
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853454"
---
# <a name="payout-reports"></a>支出报表

费用**摘要**显示了你在 Microsoft 获得的资金的详细信息。 它还使你可以知道你将何时收到付款和收到多少付款。

如果在 Azure Marketplace 中销售产品，还将在**付款摘要**中看到成功付款信息。 有关 Azure Marketplace 付款的更多详细信息，请参阅 [Microsoft Azure 应用商店参与策略](https://docs.microsoft.com/legal/marketplace/participation-policy)和 [Microsoft Azure 应用商店发布者协议](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt)。

> [!NOTE]
> 为了满足付出的条件，你的继续必须达到[支付阈值](payment-thresholds-methods-and-timeframes.md)$50。 有关付款阈值的详细信息，请参阅此页并查看应用开发人员协议。

> [!NOTE]
> 如果你正在寻找有关付款的支持，包括配置帐户、缺失付款、将付款置于保持状态或其他任何内容，请在[此处](https://developer.microsoft.com/windows/support)联系支持人员。

## <a name="access-the-payout-summary-pages"></a>访问支出摘要页面

若要打开一个付出的摘要页面：

1. 选择右上角的 "支出" 图标。
2. 选择 "事务历史记录"、"付款" 或 "导出数据"。

## <a name="transaction-history-page"></a>事务历史记录页

此页面显示您的个人收入，包括每个收入的日期、类型和收益。 你可以选择要查看的时间段，还可以按注册 ID、计划、付款 ID、收益类型、杠杆和状态进行筛选。 数据可用于当前会计年度（7月1日–6月30日）和前两个会计年度。

若要查看有关收益的详细信息，请选择页面右侧的向下箭头。 这将显示杠杆、收入金额和产品。 如果出于某种原因，此数据无法使用，但你需要访问它，请与[支持](https://developer.microsoft.com/windows/support)人员联系。 如果收益是调整的结果，而不是交易的，则不会显示 "产品" 字段。

若要导出此页上的任何事务数据，请使用 "**导出数据**" 页。

## <a name="payments-page"></a>付款页

此页上的总计表示你参与的所有程序。 您可以按参与者 ID、节目、付款 ID 和收入类型进行筛选。 金额为美元提供。 支付的值还会以货币支付。

| 区域                   | 说明                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------|
| 本年度支付的总金额   | 你的所有计划在今年（美元）支付的总金额。       |
| 下一估计付款 | 接下来的一项付款（即使有其他人即将发布），以美元为单位。 |
| 上次付款           | 最近支付的金额（美元）、节目名称和计划。           |
| 按源付款     | 过去12个月内按计划表示的付款金额，单位为美元。           |
| 支付               | 选择 "付费" 或 "挂起"，然后根据需要进行排序。 有关特定付款的其他详细信息，请选择 "查看"。 若要下载付款汇款报表的副本，请选择 "下载"。 请注意，事务历史记录数据可能需要长达24小时才能显示，因此你可能看不到相关收入。 |

若要导出此页上的任何数据，请选择 "导出"，然后按照 "导出数据" 页上的说明进行操作。

## <a name="transaction-history-page"></a>事务历史记录页

此页面显示您的个人收入，包括每个收入的日期、类型和收益。 你可以选择要查看的时间段，还可以按注册 ID、计划、付款 ID、收益类型、杠杆和状态进行筛选。 数据可用于当前会计年度（7月1日–6月30日）和前两个会计年度。

若要查看有关收益的详细信息，请选择页面右侧的向下箭头。 这将显示杠杆、收入金额和产品。 如果出于某种原因，此数据无法使用，但你需要访问它，请与[支持](https://developer.microsoft.com/windows/support)人员联系。 如果收益是调整的结果，而不是交易的，则不会显示 "产品" 字段。

若要导出此页上的任何事务数据，请选择 "导出"，然后按照 "导出数据" 页上的说明操作。 从 "事务历史记录" 页导出的文件以交易货币显示数据，按交易币种和美元计算收入，并按货币支付付薪值。

## <a name="payment-status"></a>付款状态

| 收益状态           | Reason                                                                                                                                      | 需要合作伙伴操作？                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| 尚未              | 收益有资格支付。 它在激励计划的收视指南中定义的冷却周期处于此状态。 | 是                                                         |
| 到来                 | 支付订单在处理付款之前生成待定内部评审。                                                               | 是                                                         |
| 待定销售税发票      | 您的纳税发票不完整或无效。                                                                                                  | 您需要更新您的纳税发票才能支付费用 |
| 审核时被拒绝   | 付款在评审期间被拒绝。                                                                                                     | 有关详细信息，请联系[Microsoft 支持](https://developer.microsoft.com/windows/support)部门                      |
| 已失败                   | 由于 Microsoft 系统错误，付款失败。                                                                                         | 有关详细信息，请联系[Microsoft 支持](https://developer.microsoft.com/windows/support)部门                      |
| 正在进行中              | 付款正在进行。                                                                                                                 | 是                                                         |
| 付款错误        | 付款 recouping 正在进行。                                                                                                       | 是                                                         |
| 已发送                     | 已将付款发送到银行。                                                                                                     | 是                                                         |
| 处理             | 付款遇到 Microsoft 系统错误，正在重新处理。                                                                  | 是                                                         |
| 反转                 | 付款已被银行冲销，并将在下一个付款周期再次发送。                                                     | 是                                                         |
| 销售税发票已拒绝     | 您的纳税发票在审查期间被拒绝。 在纳税发票评审完成之前，所有待定付款都将处于暂停状态。                 | 有关详细信息，请联系[Microsoft 支持](https://developer.microsoft.com/windows/support)部门                      |
| 审查后的纳税发票 | 正在查看您的纳税发票。 发票获得批准后，你的付款就会发布。                                   | 是                                                         |
| 已拒绝                 | 银行拒绝付款。                                                                                                      | 联系银行获取详细信息。                             |

## <a name="export-data-page"></a>"导出数据" 页

按照此页上的说明导出所需的数据。

注：

- "导出数据" 页不会自行刷新。 你可能需要手动刷新页面以查看最新数据。
- 筛选器可能会导致 "无数据" 错误。 这可能意味着，你已将默认时间段设置为三个月的时间，然后从超出该时间段的收入中选择付款 ID。 展开你的时间段，然后重试。

## <a name="payments"></a>支付

![导出付款](images/pc-export-payments.png)

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
| paymentMethod            | 用于支付合作伙伴的方法例如，电子银行转帐、贷方说明                                                     |
| paymentID                | 付款的唯一标识符。 此数字通常在银行报表中可见。 （仅适用于 SAP 付款）              |
| paymentStatus            | 付款状态                                                                                                                            |
| paymentStatusDescription | 付款状态的友好说明                                                                                                    |
| paymentDate              | 从 Microsoft 发送了日期支付                                                                                                      |

## <a name="transaction-history"></a>事务历史记录

![导出事务历史记录](images/pc-export-transaction.png)

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
| 数量                       | 根据程序而异。 指示交易计划的计费数量                                                            | 全部                                                            |
| quantityType                   | 指示数量的类型，例如，计费数量、MAU                                                                             | 全部                                                            |
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
| AgreementNumber                | 协议编号                                                                                                                         | 激励-仅限一些程序                                 |
| agreementStartDate             | 协议开始日期                                                                                                                     | 激励-仅限一些程序                                 |
| agreementEndDate               | 协议结束日期                                                                                                                       | 激励-仅限一些程序                                 |
| 均衡                       | 工作负载                                                                                                                                 | 激励-仅限一些程序                                 |
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

## <a name="historical-statements"></a>历史语句

![导出历史记录语句](images/pc-export-statements.png)

早于 1 2019 年7月之前的事务历史记录单独处理。 语句将使用以下字段，而不是当前的字段。

> [!NOTE]
> 旧的事务历史记录中有一个名为 "Reserved" 的列，该列对应于新式历史记录中的 "收入" 列，只不过它不包括状态为 "已发送付款" 的所有收入。

> [!NOTE]
> 筛选器（3M、6分钟、12M 等）将不适用于**历史语句**部分。

| 字段名称              | 说明                                                                                                                                                             |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 收入来源          | 收入的来源，基于交易发生的位置（例如 Microsoft Store、Windows Phone 应用商店、Windows 应用商店 8、广告等）                  |
| Order ID                | 唯一订单标识符。 此 ID 允许你通过各自的非购买交易（如退款、拒付等）标识购买交易。 两者将具有相同订单编码。 此外，在拆分付费（已针对单个购买使用了多种付款方式）时，它将允许你链接购买交易。 |
| Transaction ID          | 唯一交易标识符。                                                                                                                                          |
| 交易日期时间   | 进行交易的日期和时间 (UTC)。                                                                                                                       |
| 父产品 ID       | 唯一的父产品标识符。 请注意：如果没有用来交易的父产品，则父产品 ID = 产品 ID。                                |
| Product ID              | 唯一产品标识符。                                                                                                                                              |
| 父产品名称     | 父产品的名称。 请注意：如果没有用来交易的父产品，则父产品名称 = 产品名称。                                  |
| 产品名称            | 产品的名称。                                                                                                                                                    |
| 产品类型            | 产品的类型（如应用、加载项、游戏等）                                                                                                                       |
| 数量                | 当收入来源为适用于企业的 Microsoft Store 时，数量表示已购买的许可证数量。 对于所有其他收入来源，数量将始终为 1。 注意：即使当单笔交易因使用了两种不同的付款方式而拆分为两个明细项目时，每个明细项目都将显示数量为 1。 |
| 事务类型        | 交易的类型（如购买、退款、冲销、拒付等）                                                                                              |
| 付款方式          | 客户用于交易的付款方式（如信用卡、移动运营商结算、PayPal 等）                                                               |
| 国家/地区        | 进行交易的国家/地区。                                                                                                                          |
| 本地提供商/卖家 | 记录的本地提供商/卖家。                                                                                                                                        |
| 交易币种    | 交易的币种。                                                                                                                                            |
| 交易金额      | 交易的金额。                                                                                                                                              |
| 免除的税收            | 免除的税收额（销售税、使用税或 VAT/GST 税）。                                                                                                                  |
| 净收入            | 交易金额减去免除的税收。                                                                                                                                   |
| 应用商店费用               | 由 Microsoft 扣除的净收入百分比将用作支付在应用商店中提供应用或加载项的费用。                                                      |
| 应用收益            | 净收入减去应用商店费用。                                                                                                                                       |
| 扣缴的税款          | 所得税扣缴金额。 （未包括在“已保留”的 .csv 文件中。）                                                                                                |
| 付款                 | 应用收益减去任何相应的所得税预缴（交易币种中所示的金额）。 （未包括在“已保留”的 .csv 文件中。）                               |
| 外汇汇率                 | 用于将交易币种兑换为付款货币的外汇汇率。                                                                                         |
| 付款货币        | 付款时所使用的货币。                                                                                                                                       |
| 已兑换的付款       | 使用外汇汇率，将付款金额兑换为付款货币。                                                                                                         |
| 税款代缴模型         | 负责代缴税款（销售税、使用税或 VAT/GST 税）的一方。                                                                                                   |
| 资格日期时间   | 交易收益可用于付款的日期和时间 (UTC)。 在创建付款时，这包括资格日期时间早于付款创建日期的交易收益。 （仅包括在“已保留”的 .csv 文件中。） |
| Charges                 | 显示“交易金额”列汇总的所有费用明细的细目。 （仅包括在 Azure Marketplace 中；不包括在“已保留”的 .csv 文件中。） |