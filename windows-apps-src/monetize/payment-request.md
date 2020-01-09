---
description: 支付请求 API 为 UWP 应用提供集成的解决方案，以绕过要求用户输入付款信息并选择发货方法的过程。
title: 通过付款请求 API 简化付款
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10，uwp，付款请求
ms.openlocfilehash: 3e54767496d9c8d25ce42ea389f43ca2075d128d
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685042"
---
# <a name="simplify-payments-with-the-payment-request-api"></a>通过付款请求 API 简化付款
UWP 应用的付款请求 API 基于[W3C 支付请求 api 规范](https://w3c.github.io/browser-payment-api/)。它使你能够在 UWP 应用中简化结帐过程。 用户可以通过使用支付选项和已使用其 Microsoft 帐户保存的寄送地址来加快结帐速度。 可以增加转换速率并降低数据泄露风险，因为支付信息已标记化。 从 Windows 10 创意者更新开始，用户可以使用其保存的付款选项，轻松地在 UWP 应用中体验。

## <a name="prerequisites"></a>先决条件
在开始使用付款请求 API 之前，必须执行一些操作或了解一些事项。

### <a name="getting-a-merchant-id"></a>获取商家 ID
作为付款请求过程的一部分，Microsoft 将代表你从你的服务提供商处请求支付令牌。 因此，在开始使用 API 之前，我们需要你的授权才能请求这些令牌。  必须执行几个步骤来注册卖方帐户并提供必要的授权。 为此，请前往[Microsoft 卖方 Center](https://partner.microsoft.com/dashboard/registration/seller?accountprogram=uwp)。 完成此操作后，在构造付款请求时，将生成的商家 ID 从合作伙伴中心复制到应用中。 然后，当您的应用程序提交付款请求时，您将收到您的处理器发出的付款标记，需要提交您的付款。

### <a name="geographic-restrictions-and-language-support"></a>地理限制和语言支持
支付请求 API 只能由美国企业用于处理美国中的事务。

## <a name="using-the-payment-request-api-in-your-app-step-by-step"></a>在应用中使用付款请求 API：逐步执行
本部分演示如何在应用中使用[UWP 支付请求 API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments) 。 为了清楚起见，我们在此处使用 API 的最简单形式。 有关这些 Api 的更高级使用的示例，请参阅[GitHub 上的 UWP 购物应用示例](https://github.com/Microsoft/Windows-appsample-shopping)。

### <a name="1-create-a-set-of-all-the-payment-options-that-you-accept"></a>1. 创建一组您接受的所有付款选项。
> [!Note]
> 将**商家 id-从卖方门户**文本替换为从卖方中心收到的商家 id。

[!code-csharp[SnippetEnumerate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetEnumerate)]

### <a name="2-pull-the-payment-details-together"></a>2. 将支付详细信息请求一起。 

这些详细信息将在付款应用程序中向用户显示。 

[!code-csharp[SnippetDisplayItems](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDisplayItems)]

### <a name="3-include-the-sales-tax"></a>3. 包含销售税。 

> [!Important]
> API 不会为你添加项或计算销售税。 请记住，税率因区域而异。 为清楚起见，我们使用假设的9.5% 税率。

[!code-csharp[SnippetTaxes](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetTaxes)]

### <a name="4-optional--add-discounts-or-other-modifiers-to-the-total"></a>4. （可选）向合计添加折扣或其他修饰符。 

下面是一个示例，说明如何使用特定的 Contoso 信用卡向显示项添加折扣。 （*Contoso*是一个虚构的名称。）

[!code-csharp[SnippetDiscountRate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDiscountRate)]

### <a name="5-assemble-all-the-payment-details"></a>5. 收集所有支付详情。

[!code-csharp[SnippetAggregate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetAggregate)]
[!code-csharp[SnippetPaymentOptions](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetPaymentOptions)]

### <a name="6-submit-the-payment-request"></a>6. 提交付款请求。 

调用**SubmitPaymentRequestAsync**方法提交付款请求。 此时会显示付款应用，其中显示了可用的付款选项。

[!code-csharp[SnippetSubmit](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetSubmit)]

系统将提示用户登录其 Microsoft 帐户。

用户登录后，他们可以选择以前保存的付款选项和发货地址。

![付款请求用户界面](./images/33.png "付款请求用户界面")

应用会等待用户点击 "**付款**"，然后完成订单。

[!code-csharp[SnippetComplete](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetComplete)]

付款完成后，用户会看到 "已**确认订单**" 屏幕。

![已确认订单](./images/44.png "已确认订单")

## <a name="see-also"></a>另请参阅
- [Windows.applicationmodel.resources.core 参考文档](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments)
- [GitHub 上的 UWP 购物应用示例](https://github.com/Microsoft/Windows-appsample-shopping)
- [W3C 支付请求 API 规范](https://www.w3.org/TR/payment-request/)
- [支付请求 API](https://docs.microsoft.com/microsoft-edge/dev-guide/windows-integration/payment-request-api)

