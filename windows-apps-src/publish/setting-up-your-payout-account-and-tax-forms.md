---
Description: In order to receive money from app sales in the Microsoft Store, you need to set up your payout account and fill out the necessary tax forms.
title: 设置你的付款帐户和税单
ms.assetid: 690A2EBC-11B1-4547-B422-54F15A6C26A7
ms.date: 12/14/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5a56630a0a2f0acdc71241ac0234cad463e45ace
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259903"
---
# <a name="set-up-your-payout-account-and-tax-forms"></a>设置你的付款帐户和税单

In order to receive money from app sales in the Microsoft Store, you need to set up your payout account and fill out the necessary tax forms in [Partner Center](https://partner.microsoft.com/dashboard).

如果你仅计划列出免费应用（未计划提供应用内购买或使用 Microsoft Advertising），则无需设置付款帐户或填写任何税单。 If you change your mind later and decide you do want to sell apps (or add-ons), you can set up your payout account and fill out tax forms at that time. 在付款帐户和纳税配置文件设置完成前，将无法提交任何付费应用或加载项。

> [!NOTE]
> 在[某些市场](account-types-locations-and-fees.md#developer-account-and-app-submission-markets)中，开发人员只可以提交免费应用。 如果在其中一个市场中注册帐户，则无法设置付款帐户。

After you have [set up your developer account](opening-a-developer-account.md), there are two things you need to do before you can sell apps (or add-ons) in the Microsoft Store:

- [Fill out your tax forms](#tax-forms)
- [Set up your payout account](#payout-account)

> [!NOTE]
> 有关获得应用销售所得款项的方式和时间，请参阅[获得收入](getting-paid-apps.md)。

## <a name="tax-forms"></a>税单

### <a name="filling-out-your-tax-forms"></a>Filling out your tax forms

First, you'll need to create a tax profile and assign it to the programs you participate in. You can create your *tax profile* for the Microsoft Store by completing the following steps:

- 指定你的居住地和公民身份所属的国家/地区。
- 填写相应的税单。

You can complete and submit your tax forms electronically in Partner Center; in most cases, you don't need to print and mail any forms.

> [!IMPORTANT]
> 不同的国家和地区有不同的纳税要求。 你必须支付的税款的精确金额取决于你在哪些国家和地区销售你的应用。 请参阅[应用开发人员协议](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)，以了解 Microsoft 为哪些国家和地区代缴销售和使用税。 在其他国家或地区中，根据你的注册地点，你可能需要直接向当地的税务机构为你的应用缴纳销售和使用税 另外，你所获得的应用销售收益可能需要缴纳所得税。 We strongly encourage you to contact the relevant authority for your country or region that can best help you determine the right tax info for your Microsoft Store developer activities.

1. In [Partner Center](https://partner.microsoft.com/dashboard), select the **Account settings** icon in the top right corner, then select **Developer settings**.
2. In the left navigation menu, select **Payout and tax**, then select **Payout and tax assignments**.

    ![Payout and tax profile assignment](images/payout-tax-profile-assignment.png)

3. Select the program and seller id combination for which you want to configure tax information.

    ![Payout select seller id](images/payout-select-seller-id.png)

4. If you would like to use an existing tax profile, select it from the dropdown. Otherwise, select **Create new profile** and press **Submit**. You will be taken to the tax profiles page.
5. Click the **Edit** button to edit your tax information.
6. Select the appropriate radio button, and select your country if prompted. This step determines the Microsoft business entity that will be used to make payouts on your account.

    ![Payout select tax country](images/payout-select-tax-country.png)

7. Depending on your selections in step 6, you will be prompted to provide tax information required for your country.

> [!NOTE]
> Regardless of your country of residence or citizenship, you must fill out United States tax forms to sell any apps or add-ons through the Microsoft Store. 符合某些美国居住地要求的开发人员必须填写 IRS W-9 表格。 美国境外的其他开发人员必须填写 IRS W-8 表格。 可以在完成纳税配置文件时在线填写这些表单。

### <a name="withholding-rates"></a>预缴比率

你在税单中提交的信息将确定相应的预缴比率。 预缴比率仅适用于你在美国的销售；非美国地区的销售不受预缴约束。 不同情况下的预缴比率有所不同，但对于在美国之外国家或地区注册的大多数开发人员，黙认税率是 30%。 如果你的国家和地区与美国签有所得税条约，你可以选择降低此税率。

### <a name="tax-treaty-benefits"></a>免税待遇

如果你在美国境外，你也许能够利用免税待遇。 These benefits vary from country to country, and may allow you to reduce the amount of taxes that the Microsoft Store withholds. 你可以通过填写 W-8BEN 表格的第 II 部分来申请免税待遇。 我们建议你咨询所在国家或地区的相关人员，以确定是否可以享受这些优惠。

> [!NOTE]
> 不需要提供美国个人纳税识别号码（或 ITIN）即可从 Microsoft 接收付款或申请免税待遇。

## <a name="payout-account"></a>付款帐户

付款帐户是我们用于为你存入销售收益的银行帐户。 You can view all payment accounts that you have entered on the Profile page.

> [!NOTE]
> 在某些市场中，PayPal 可用于付款帐户。 See [Payment thresholds, methods, and timeframes](payment-thresholds-methods-and-timeframes.md) to find out if PayPal is supported for a specific market, and read the [PayPal info](#paypal-info) below for more details.

### <a name="create-a-payment-profile"></a>Create a payment profile

1. In [Partner Center](https://partner.microsoft.com/dashboard), select the **Settings** gear icon in the top right corner, then select **Developer settings**.
2. Underneath the *Payout and tax* heading, select **Payout and tax profile assignment**.

    > [!NOTE]
    > 由于这是敏感信息，系统可能提示你再次登录。

3. Select the payment method you would like to configure.

    ![Payout account type selection](images/payout-account-type-selection.png)

4. Select an existing payment profile, or click **Create a new payment profile** to create a new profile for the chosen payment method.

> [!NOTE]
> If, for some reason, your account is not ready to receive funds from Microsoft, you may check the **Hold my payment** checkbox. You will continue to earn proceeds from your sales, but payments will not be distributed until you disable **Hold my payment.**

### <a name="create-a-bank-based-payment-profile"></a>Create a bank-based payment profile

If you elected to use a bank account to receive payouts, you'll complete the following process to configure your bank account.

1. On the *Bank Profile* page, provide the required information about your bank.
2. Provide your bank account details.

    > [!NOTE]
    > 用于提供帐户信息的字段仅接受字母数字字符。

    ![Payout bank info](images/payout-bank-info.png)

3. Provide beneficiary details.
4. Back on the *Profile assignment* page, select the currency you would like us to use when we issue your payouts.

    > [!WARNING]
    > Make sure your bank accepts the payout currency you select.

5. You will need to select a payment profile for each program you participate in, though you can use the same profile for multiple programs.

    ![Payout use bank profile](images/payout-use-bank-profile.png)

6. Click submit to save your changes.

> [!NOTE]
> Microsoft may take up to 48 hours to validate the information in your profile. When this process is complete *verification status* will show **Complete**

为了确保付款成功，请谨记以下事项：

- The **Account holder name** entered for your payout account in Partner Center must be the exact same name associated with your bank account. 例如，如果银行帐户名称包含中间名，则向“帐户持有者名称”添加中间名。
- 付款会直接从 Microsoft 转移到你的银行帐户，货币单位为美元。
- Bank information entered in Partner Center in Latin characters is translated to Cyrillic characters.

### <a name="editing-existing-payment-profiles"></a>Editing existing payment profiles

You can edit existing payment profiles if you need to make changes or correct any incorrect information.

1. In [Partner Center](https://partner.microsoft.com/dashboard), select the **Settings** gear icon in the top right corner, then select **Developer settings**.
2. Underneath the *Payout and tax* heading, select **Payout and tax profiles**.
3. Your payment profiles will be listed along with their status. Find the profile you wish to edit and click **Edit** at the far right

> [!IMPORTANT]
> 更改你的付款帐户最多会使你的付款延迟一个付款周期。 出现此延迟是因为我们需要验证帐户更改，就像你第一次设置付款帐户时我们所进行的验证一样。 在帐户通过验证后，你仍能获得全额付款；当前付款周期的所有应付款项均被计入下一付款周期。 有关详细信息，请参阅[获得收入](getting-paid-apps.md)。

### <a name="paypal-info"></a>PayPal 信息

在选定的国家和地区中，您可以通过输入 PayPal 信息来创建付款帐户。 但是，在选择 PayPal 作为付款帐户选项之前：

- Check [Payment thresholds, methods, and timeframes](payment-thresholds-methods-and-timeframes.md) to confirm whether PayPal is a supported payment method in your country or region.
- 查看以下常见问题解答。 根据你的情况，PayPal 可能不是最适合你的付款帐户选项，银行帐户也许是首选。

关于使用 PayPal 作为付款方式的常见问题：

- **What PayPal settings do I need to have in order to receive payments?** 必须确保你的 PayPal 帐户不会阻止 eCheck 付款。 此设置在 PayPal 的“付款接收首选项”页面中管理。 有关详细信息，请参阅 [PayPal 的帐户设置页面](https://developer.paypal.com/webapps/developer/docs/classic/admin/setup-account/)。
- **Is my country/region supported?** See [Payment thresholds, methods, and timeframes](payment-thresholds-methods-and-timeframes.md) to find out where PayPal is a supported payment method.
- **Does my PayPal account have to be registered in the same country/region as my Partner Center account?** 不相同。 设置 PayPal 帐户时，可以接受默认配置。 您不应该有任何其他国家/地区和货币的问题，除非您阻止了某些货币的付款。 此设置在 PayPal 的“付款接收首选项”页面中管理。
- **Do I have to accept PayPal payments manually?** 不相同。 PayPal 帐户默认设置为需要用户手动接受付款，这意味着如果你不在 30 天内接受付款，将返回付款。 你可以更改此设置，方法是在 PayPal 的“更多设置”页面中关闭“询问我”。
- **What currencies does PayPal support?** Please see [PayPal's support page](https://developer.paypal.com/docs/classic/api/currency-codes/#paypal) for the current list

### <a name="specific-requirements-for-certain-countriesregions"></a>某些国家/地区的特定要求

在一些国家和地区中，必须遵循付款帐户的其他要求。 如果你居住在巴基斯坦、俄罗斯或乌克兰，请注意以下要求。

#### <a name="pakistan"></a>巴基斯坦

表单 R 是巴基斯坦银行业法规要求。 它用于指示接收国外资金的目的和原因。 因此，每当你有资格获得来自 Microsoft 的每月付款时，你将需要向银行提交表单 R，然后付款才能发放到你的帐户。 有关如何获取表单 R 的副本的说明，请联系你的当地银行分支机构。

你将需要在你有资格获得付款的每个月都向银行提交表单 R。 例如，如果希望在一年中的每个月都接收付款，你将需要提交表单 R 12 次（每个月一次）。

付款提交到你的银行后，你有 30 天的时间来提交表单 R。 如果未在 30 天内提交，资金将返回到 Microsoft。

#### <a name="russia"></a>俄罗斯

如果你是居住在俄罗斯的开发人员，可能需要在银行将资金存入你的帐户前向银行提供文档。 当你有资格获得付款时，我们将在电子邮件中为你提供以下文档：

1. 验收证书 (AC) - 包含转移到你的帐户的付款金额。
2. 应用开发人员协议 (ADA) - 需要副签的开发人员协议的已签署副本。

为了确保付款成功，请谨记以下事项：

- The **Account holder name** entered for your payout account in Partner Center must be the exact same name associated with your bank account. 例如，如果银行帐户名称包含中间名，则向“帐户持有者名称”添加中间名。
- 付款会直接从 Microsoft 转移到您的银行帐户，货币单位为卢比 (RUB)。
- Bank information entered in Partner Center in Latin characters is translated to Cyrillic characters.
- 付款必须支付到银行帐户而不是银行卡。

#### <a name="ukraine"></a>乌克兰

如果你是居住在乌克兰的开发人员，可能需要在银行将资金存入你的帐户前向银行提供文档。 当你有资格获得付款时，我们将在电子邮件中为你提供以下文档：

1. 验收证书 (AC) - 包含转移到你的帐户的付款金额。
2. 应用开发人员协议 (ADA) - 需要副签的开发人员协议的已签署副本。
3. 修订协议 (AA) - 你的银行可使用此文档帮助识别你的付款资金。

当尝试首次付款时，Microsoft 将提供所有三个文档。 对于任何后续付款，你将仅收到 AC 文档。 请保留 ADA 和 AA 文档，以防你从银行接收将来付款时需要它们。

### <a name="create-a-paypal-payment-profile"></a>Create a PayPal payment profile

If you elected to use a bank account to receive payouts, you'll complete the following process to configure your bank account.

1. On the *PayPal* page, provide the required information about your PayPal account.
2. Provide your paypal account details.

    > [!NOTE]
    > 用于提供帐户信息的字段仅接受字母数字字符。

    ![Payout paypal info](images/payout-paypal-info.png)

3. Provide beneficiary details.
4. Back on the *Profile assignment* page, select the currency you would like us to use when we issue your payouts.
5. You will need to select a payment profile for each program you participate in, though you can use the same profile for multiple programs.
6. Click submit to save your changes.