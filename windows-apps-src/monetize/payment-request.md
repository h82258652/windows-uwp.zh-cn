---
description: 付款请求 API 提供了适用于 UWP 应用，以绕过需要用户输入付款信息并选择传送方法的过程集成的解决方案。
title: 通过付款请求 API 简化付款
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10，uwp，付款请求
ms.openlocfilehash: e5fb5cead7833b8cc213c6633cae6cee0da3466b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607862"
---
# <a name="simplify-payments-with-the-payment-request-api"></a>通过付款请求 API 简化付款
适用于 UWP 应用付款请求 API 为基础[W3C 付款请求 API 规范](https://w3c.github.io/browser-payment-api/)。这样可以简化签出过程在 UWP 应用中的功能。 使用付款选项和发货地址与 Microsoft 帐户已保存的情况下，用户可以快速通过签出。 您可以提高转换率并减少数据泄露的风险，因为标记化的付款信息。 从 Windows 10 创意者更新开始，用户可以使用其已保存的付款方式付款轻松地跨 UWP 应用中的体验。

## <a name="prerequisites"></a>必备条件
在开始使用付款请求 API 之前，有必须执行或注意的事项。

### <a name="getting-a-merchant-id"></a>获取商户 ID
为付款请求过程的一部分，Microsoft 服务提供商请求代表你的支付令牌。 因此在可以开始使用 API 之前，我们需要你的授权请求这些令牌。  必须执行几个步骤以注册为卖家帐户并提供必要的授权。 若要执行此操作，请转到[Microsoft 卖家 Center](https://seller.microsoft.com/en-us/dashboard/registration/seller/?accountprogram=uwp)。 完成连接后，复制生成商户 ID 从合作伙伴中心到你的应用构造付款请求时。 然后，当你的应用程序提交付款请求时，你将从您将需要提交您的付款的处理器接收付款令牌。

### <a name="geographic-restrictions-and-language-support"></a>地理限制和语言支持
付款请求 API 仅在美国处理交易的基于美国的企业使用。

## <a name="using-the-payment-request-api-in-your-app-step-by-step"></a>在您的应用程序中使用付款请求 API： 分步
本部分演示如何使用[UWP 付款请求 API](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.payments)应用程序中。 为清楚起见最简单的形式将使用此处的 API。 有关更多高级使用这些 Api 的示例，请参阅[GitHub 上的 UWP 购物应用示例](https://github.com/Microsoft/Windows-appsample-shopping)。

### <a name="1-create-a-set-of-all-the-payment-options-that-you-accept"></a>1.创建一组接受的所有付款选项。
> [!Note]
> 替换**商户 id 从卖方门户**文本到零售商 ID 您接收到从卖方中心。

[!code-cs[SnippetEnumerate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetEnumerate)]

### <a name="2-pull-the-payment-details-together"></a>2.汇集付款详细信息。 

将付款应用程序中向用户显示这些详细信息。 

[!code-cs[SnippetDisplayItems](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDisplayItems)]

### <a name="3-include-the-sales-tax"></a>3.包含销售税。 

> [!Important]
> 该 API 不添加项，也不为您计算增值税。 请记住税率因管辖权。 为清楚起见，我们使用假设 9.5%税率。

[!code-cs[SnippetTaxes](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetTaxes)]

### <a name="4-optional--add-discounts-or-other-modifiers-to-the-total"></a>4.（可选） 添加与总折扣或其他修饰符。 

下面是添加针对使用特定的 Contoso 信用卡号给显示项的折扣的示例。 (*Contoso*是虚构的名称。)

[!code-cs[SnippetDiscountRate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDiscountRate)]

### <a name="5-assemble-all-the-payment-details"></a>5.组合所有付款详细信息。

[!code-cs[SnippetAggregate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetAggregate)]
[!code-cs[SnippetPaymentOptions](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetPaymentOptions)]

### <a name="6-submit-the-payment-request"></a>6.提交付款请求。 

调用**SubmitPaymentRequestAsync**方法以提交你支付的请求。 此时会打开显示可用的付款方式付款应用。

[!code-cs[SnippetSubmit](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetSubmit)]

提示用户使用其 Microsoft 帐户登录。

在用户登录后，他们可以选择支付选项和以前保存的寄送地址。

![付款请求 UI](./images/33.png "付款请求 UI")

您的应用程序等待用户点击**支付**，然后完成顺序。

[!code-cs[SnippetComplete](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetComplete)]

完成付款后，用户会看到**订单已确认**屏幕。

![订单已确认](./images/44.png "订单已确认 ")

## <a name="see-also"></a>另请参阅
- [Windows.ApplicationModel.Payments 参考文档](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.payments)
- [GitHub 上 UWP 购物应用示例](https://github.com/Microsoft/Windows-appsample-shopping)
- [W3C 付款请求 API 规范](https://www.w3.org/TR/payment-request/)
- [付款请求 API ](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide/device/payment-request-api)

