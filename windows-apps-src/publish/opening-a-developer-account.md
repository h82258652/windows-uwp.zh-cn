---
ms.assetid: 284EBA1F-BFB4-4CDA-9F05-4927CDACDAA7
title: 开设开发者帐户
description: 下面概述了如何在合作伙伴中心为 Microsoft Store 和其他 Microsoft 程序注册 Windows 开发人员帐户。
ms.date: 3/30/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0ab3dcc5f774de6bd7ce422ecdcec61b26cc9a4b
ms.sourcegitcommit: 9aef3bc26a56b8d266b3089d509f79b119234b6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2020
ms.locfileid: "80538201"
---
# <a name="opening-a-developer-account"></a>开设开发者帐户

本文介绍如何在[合作伙伴中心](https://partner.microsoft.com/dashboard)注册 Windows 开发人员帐户。

> [!NOTE]
> 当你注册开发人员帐户时，我们将使用你在联系人信息中提供的电子邮件地址来发送与你的帐户相关的消息。 有时，这些信息可能包含有关我们的程序的信息。 如果你选择不[opt out](https://account.microsoft.com/account/Account?ru=https%3A%2F%2Faccount.microsoft.com%2Fprofile%2Fcontact-info&destrt=profile-landing)使用这些信息性电子邮件，请注意，我们仍会向你发送事务性消息（例如，让你知道你的应用程序已通过认证或付款正在进行）。 这些事务性电子邮件是帐户的必需部分，除非你关闭帐户，否则你将继续接收这些电子邮件。

## <a name="the-account-signup-process"></a>帐户注册过程

> [!NOTE]
> 在某些情况下，注册开发人员帐户时看到的屏幕和字段可能与以下步骤中所述的内容略有不同。 但基本信息和过程将匹配这些步骤所描述的内容。

> [!NOTE]
> 有一个已知问题，某些区域设置中的用户可能无法完成其注册。 在我们确认它已解决之前，建议你在 partner.microsoft.com 上开始注册过程后，手动将浏览器的区域设置标记更改为**en-us** 。

1.  请在[注册页](https://developer.microsoft.com/store/register)**上**，选择 "注册"。
2.  如果尚未使用 Microsoft 帐户登录，请立即登录，或创建新的 Microsoft 帐户。 此处使用的 Microsoft 帐户用于登录开发人员帐户。
3.  选择您所在的[国家/地区](account-types-locations-and-fees.md#developer-account-and-app-submission-markets)或业务所在的位置。 你将无法在以后更改此设置。
4.  选择[开发者帐户类型](account-types-locations-and-fees.md)（个人或公司）。 你之后将无法更改该设置，因此请确保选择正确的帐户类型。
5.  输入要使用的**发布服务器显示名称**（50个字符或更少）。 客户在浏览应用时将看到此名称，并通过此名称记住你的应用，因此请谨慎选择该名称。 对于公司帐户，请确保使用组织的注册公司名称或商标。 如果输入其他人已选择的名称，或其他人有权使用该名称，则不允许使用该名称。

    > [!NOTE]
    > 确保有权使用在此输入的名称。 如果其他人拥有所选名称的商标或版权，帐户可能会关闭。 有关详细信息，请参阅[应用开发人员协议](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)。 如果其他人正在使用你对其持有商标或其他法定权利的发布者显示名称，请[联系 Microsoft](https://www.microsoft.com/info/cpyrtInfrg.html)。    

6.  为你的开发者帐户输入要使用的联系人信息。

    > [!NOTE]
    > 遇到帐户相关事宜时，我们将通过此信息与你联系。 例如，你将在完成注册后收到电子邮件确认。 完成此操作后，我们将在我们向你发送消息时发送消息，或者你需要通过你的帐户进行修复。 我们可能还会发送信息性电子邮件（如前文所述），除非你选择不接收非事务性电子邮件。

    如果要注册为公司，还需要输入将批准公司帐户的人员的姓名、电子邮件地址和电话号码。

7.  选择 "**下一步**" 转到 "**付款**" 部分。

8.  输入一次性注册费用的付款信息。 如果你有涵盖注册成本的促销充值码，可以在此处进行输入。 否则，请提供信用卡信息（或支持市场中的 PayPal 信息）。 请注意，预付款的信用卡无法用于此项购买。 完成后，选择 "**下一步**" 转到 "**查看**" 屏幕。

9.  审查你的帐户信息并确认所有内容都正确无误。 然后，阅读并接受[应用开发人员协议](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)的条款和条件。 选中相应的复选框，以指示已阅读并接受这些条款。

10.  选择 "**完成**" 以确认你的注册。 将处理你的付款，我们将向你的电子邮件地址发送一条确认消息。

注册后，你的帐户将经历验证。 对于个人帐户，我们将进行检查，确保其他用户尚未使用您的发布者显示名称。 对于公司帐户，过程所花时间会长一点，因为我们还需要确认用户得到授权，可设置公司帐户。 此验证可能需要几天到几周的时间，并且通常包含公司电话呼叫。 可在**帐户设置**页上查看验证状态。


## <a name="additional-guidelines-for-company-accounts"></a>有关公司帐户的其他指南

> [!IMPORTANT]
> 若要允许多个用户访问开发人员帐户，建议使用 Azure Active Directory （Azure AD）将角色分配给单个用户，而不是共享对 Microsoft 帐户的访问权限。 然后，每个用户都可以通过使用其个人 Azure AD 凭据登录到合作伙伴中心来访问开发人员帐户。 有关详细信息，请参阅[管理帐户用户](manage-account-users.md)。

如果希望让多个用户通过使用打开该帐户的 Microsoft 帐户（而不是添加到帐户中的单个用户）登录来访问公司帐户，请参阅以下指南：

-   使用不属于你或其他个人的电子邮件地址（如 MyCompany_PartnerCenter@outlook.com）创建 Microsoft 帐户。 请勿使用公司域中的电子邮件地址，尤其是在公司已使用 Azure AD 的情况下。 如前文所述，稍后可以从公司的 Azure AD 服务添加其他用户。
-   将此 Microsoft 帐户的访问权限限制为尽可能少的用户数量。
-   设置公司电子邮件通讯组列表，其中包括需要访问开发人员帐户的所有用户。 将此电子邮件地址添加到[与 Microsoft 帐户关联的安全信息](https://account.microsoft.com/security)。 此方法允许列表中的所有员工接收发送到此别名的安全代码。 如果设置通讯组列表不可行，可以将个人电子邮件地址添加到安全信息。 但是，该电子邮件地址的所有者是唯一可以在系统提示时访问和共享安全代码的人员（例如，将新的安全信息添加到该帐户时或从新设备访问该帐户时）。
-   将公司电话号码添加到 Microsoft 帐户的安全信息。 尝试使用不需要扩展且可供关键团队成员访问的数字。
-   鼓励开发人员使用[受信任的设备](https://support.microsoft.com/help/12369/microsoft-account-add-a-trusted-device)登录公司的开发人员帐户。 所有关键团队成员都应具有对这些受信任的设备的访问权限。 这种安排降低了团队成员访问帐户时要发送安全代码的需要。 每个帐户每周可以生成的代码数有限制。
-   如果需要允许从不受信任的电脑访问帐户，请将开发人员访问人数上限限制为五人。 理想情况下，这些开发人员应从共享相同地理和网络位置的计算机访问该帐户。
-   经常在 https://account.microsoft.com/security 查看公司的安全信息，以确保所有信息都是最新的。


## <a name="microsoft-account-security"></a>Microsoft 帐户安全性

我们通过将 Microsoft 帐户与多种形式的身份验证相结合，使用所提供的安全信息来提高 Microsoft 帐户的安全级别。 这样一来，未经授权访问 Microsoft 帐户（和开发者帐户）的难度将大大提高。 此外，如果你忘记了密码，或者其他人尝试访问你的帐户，我们将能够联系你确认所有权，并/或重新建立适当的帐户控制。

你的 Microsoft 帐户上必须至少有两个电子邮件地址或电话号码。 我们建议添加尽可能多的信息。 请记住，必须对某些安全信息进行确认，它才能生效。 此外，请确保经常查看安全信息并验证它是否是最新的。 你可以通过转到 https://account.microsoft.com/security 并使用 Microsoft 帐户登录来管理安全信息。 有关详细信息，请参阅[帐户安全信息 & 验证代码](https://support.microsoft.com/help/12428/microsoft-account-security-info-verification-codes)。

当你通过 Microsoft 帐户登录到合作伙伴中心时，系统可能会提示你通过发送安全代码来验证你的身份，你必须输入该安全代码才能完成登录过程。 建议将经常使用的电脑指定为*受信任的设备*。 当你从受信任的设备登录时，通常不会提示你输入代码，但在特定情况下可能偶尔会提示你，或者你在很长一段时间内未登录到该设备。 有关详细信息，请参阅[将受信任的设备添加到 Microsoft 帐户](https://support.microsoft.com/help/12369/microsoft-account-add-a-trusted-device)。


## <a name="closing-your-account"></a>关闭你的帐户

开发人员帐户不会过期，因此无需续订你的帐户即可将其保持打开状态。 如果你决定完全关闭你的帐户，你可以通过联系技术支持人员来执行此操作。

当关闭你的帐户时，务必了解你在 Microsoft Store 中发布的任何应用会发生什么情况：

-   应用的当前客户仍可使用该应用。 但是，它们不能进行应用内购买。
-   即使应用仍可供以前获取该应用的客户使用，你的应用列表也会从 Microsoft Store 中删除。 任何新客户都无法获得您的应用程序。
-   将释放你的应用的名称，以供其他开发人员使用。
-   如果由于之前的应用销售额而产生余额，则即使到期金额不满足标准支付阈值，也可以请求支付此余额。
