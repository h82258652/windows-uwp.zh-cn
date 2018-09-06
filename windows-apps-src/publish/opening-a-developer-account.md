---
author: jnHs
ms.assetid: 284EBA1F-BFB4-4CDA-9F05-4927CDACDAA7
title: 开设开发者帐户
description: 有关如何为 Microsoft Store 和其他 Microsoft 计划注册开发者帐户的此概述将有助于你了解设置帐户的过程。
ms.author: wdg-dev-content
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b707479d11cc7aef62385b476720bff8477ed401
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2018
ms.locfileid: "3370556"
---
# <a name="opening-a-developer-account"></a>开设开发者帐户

有关如何为 Microsoft Store 和其他 Microsoft 计划注册开发者帐户的此概述将有助于你了解设置帐户的过程。

> [!NOTE]
> 当你注册开发者帐户时，我们将使用你在联系人信息中提供的电子邮件地址，以发送与你的帐户相关的电子邮件通信。 有时，其中可能包括有关我们的计划的信息性电子邮件。 如果你通过单击[退出](http://go.microsoft.com/fwlink/p/?LinkId=533280)选择不接收这些信息电子邮件，则请注意，我们仍然会向你发送交易电子邮件（例如，为了让你知道你的应用已经通过认证，或付款已在路上）。 这些电子邮件是你的帐户的必要组成部分，除非关闭你的帐户，否则你将继续收到这些交易电子邮件。

## <a name="the-account-signup-process"></a>帐户注册过程

> [!NOTE]
> 在某些情况下，在注册开发者帐户时所看到的屏幕和字段可能与下方所述内容略有不同。 基本信息和过程都相同。

1.  转到[注册页面](http://go.microsoft.com/fwlink/p/?LinkId=615100)，然后单击**注册**。
2.  如果尚未使用 Microsoft 帐户登录，请立即登录，或创建新的 Microsoft 帐户。 你在此处所使用的 Microsoft 帐户将会是你用来登录到开发者帐户的帐户。
3.  选择你所居住的或你的企业所在的[国家/地区](account-types-locations-and-fees.md#developer-account-and-app-submission-markets)。 你将无法在以后更改此设置。
4.  选择[开发者帐户类型](account-types-locations-and-fees.md)（个人或公司）。 之后将无法更改该设置，因此请确保选择正确的帐户类型。
5.  输入希望使用的**发布者显示名称**（不超过 50 个字符）。 客户在浏览应用时将看到此名称，并通过此名称了解应用，因此请谨慎选择该名称。 对于公司帐户，请确保使用组织的注册公司名称或商标。 请注意，如果输入其他人已选择的名称，或者系统显示其他人有权使用该名称，我们将不允许你使用该名称。 

   > [!NOTE]
   > 确保有权使用在此输入的名称。 如果其他人拥有所选名称的商标或版权，帐户可能会关闭。 有关详细信息，请参阅[应用开发人员协议](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)。 如果其他人正在使用你对其持有商标或其他法定权利的发布者显示名称，请[联系 Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=233777)。    

6.  输入希望用于开发者帐户的联系人信息。

   > [!NOTE]
   > 遇到帐户相关事宜时，我们将通过此信息与你联系。 例如，完成注册后，你将会收到一封确认电子邮件。 在这之后，我们将在向你支付时或你需要使用你的帐户修复某些内容时发送电子邮件。 我们还可能发送上述信息之类的电子邮件，除非你选择了接收非交易类电子邮件。

   如果要注册公司帐户，还需要输入负责审批贵公司帐户的人员的姓名、电子邮件地址和电话号码。

7.  单击**下一步**以继续转到**付款**部分。

8.  输入一次性注册费用的付款信息。 如果你有涵盖注册成本的促销充值码，可以在此处进行输入。 否则，请提供你的信用卡信息（或在受支持的市场中提供 PayPal）。 请注意，预付款的信用卡无法用于此项购买。 完成后，请单击**下一步**以继续转到**审查**屏幕。

9.  审查你的帐户信息并确认所有内容都正确无误。 然后，阅读并接受[应用开发人员协议](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)的条款和条件。 选中该框以指示你已阅读并接受这些条款。

10.  单击**完成**以确认注册。 你的付款将被处理，我们将向你的电子邮件地址发送一封确认邮件。

登录后，帐户将进行验证过程。 对于个人帐户，我们将进行检查，确保其他公司没有在使用该发布者显示名称。 对于公司帐户，过程所花时间会长一点，因为我们还需要确认用户得到授权，可设置公司帐户。 此验证需要几天到几周时间，通常还会致电你的公司。 可在**帐户设置**页上查看验证状态。


## <a name="additional-guidelines-for-company-accounts"></a>有关公司帐户的其他指南

> [!IMPORTANT]
> 若要允许多个用户访问你的开发人员中心帐户，我们建议使用 Azure Active Directory 将角色分配给单个用户 （而不是对 Microsoft 帐户的共享访问）。 之后每位用户可以使用自己的单独 Azure AD 凭据进行登录，从而访问开发人员中心帐户。 有关详细信息，请参阅[管理帐户用户](manage-account-users.md)。

在创建公司帐户时，这些指南可帮助，如果有多人需要通过使用打开该帐户的 Microsoft 帐户 （而不是作为单个用户添加到开发人员中心帐户） 中访问该帐户。

-   使用尚未属于你或其他个人的电子邮件地址创建 Microsoft 帐户，如 MyCompany_DevCenter@outlook.com。 不要使用在贵公司的域的电子邮件地址，尤其是当你的公司已经使用 Azure AD。 (如上面所述，你可以添加其他用户从你的公司的 Azure AD 更高版本。)
-   将此 Microsoft 帐户的访问权限限制为尽可能少的用户。
-   设置企业电子邮件通讯组列表，其中包括每位需要访问开发人员帐户，并将添加到此电子邮件地址 [与 Microsoft 帐户相关联的安全信息 [(https://account.microsoft.com/security)。 这样的所有员工列表，以接收安全代码发送到此别名。 如果设置分发列表不可行，你可以将个人的电子邮件地址添加到你的安全信息，但该电子邮件地址的所有者将是唯一的人可以访问和共享安全代码收到提示时 （例如当新的安全信息添加到 t他帐户，或者从新设备的访问时）。
-   添加 Microsoft 帐户的安全信息的公司电话号码。 尝试使用无需分机并可供关键团队成员的数字。
-   一般情况下，让开发人员使用[受信任的设备](https://support.microsoft.com/help/12369/microsoft-account-add-a-trusted-device)登录到贵公司的开发人员帐户。 所有关键团队成员都应具有对这些受信任的设备的访问权限。 这将减少访问帐户时对发送安全代码的需求。 每个帐户每周可生成的代码数量有限。
-   如果需要允许从不受信任的电脑访问帐户，请将开发人员访问人数上限限制为五人。 理想情况下，这些开发人员应从共享相同地理和网络位置的计算机访问该帐户。
-   请经常在 https://account.microsoft.com/security 上查看公司的安全信息以确保它都是当前信息。


## <a name="microsoft-account-security"></a>Microsoft 帐户安全

我们通过将 Microsoft 帐户与多种形式的身份验证相结合，使用所提供的安全信息来提高 Microsoft 帐户的安全级别。 这样一来，未经授权访问 Microsoft 帐户（和开发者帐户）的难度将大大提高。 此外，如果你忘记密码或有人试图访问你的帐户，我们可联系你以确认所有权和/或恢复帐户相应控制权限。

Microsoft 帐户上必须具有至少两个电子邮件地址和/或电话号码。 我们建议添加尽可能多的信息。 请记住，必须对某些安全信息进行确认，它才能生效。 同样，确保经常查看你的安全信息并确保它处于最新状态。 你可以通过转到 https://account.microsoft.com/security 并使用 Microsoft 帐户登录来管理你的安全信息。 有关详细信息，请参阅[安全信息和安全代码](https://support.microsoft.com/help/12428/microsoft-account-security-info-and-security-codes)。

在登录到 Windows 开发人员中心仪表板使用你的 Microsoft 帐户时，系统可能会要求你验证身份通过发送安全代码，你必须将其提供来完成登录流程。 我们建议指定你经常使用作为*受信任的设备*的电脑。 当你从受信任的设备登录时，通常不会提示你代码，但有时可能会在特定情况下提示你或你内未登录该设备上长时间。 有关详细信息，请参阅[添加到你的 Microsoft 帐户受信任的设备](https://support.microsoft.com/help/12369/microsoft-account-add-a-trusted-device)。


## <a name="closing-your-account"></a>关闭你的帐户

开发人员帐户不会过期，因此无需续订你的帐户即可使其保持开设状态。 如果你决定完全关闭你的帐户，你可以通过联系技术支持人员来执行此操作。

当关闭你的帐户时，务必了解你在 Microsoft Store 中发布的任何应用会发生什么情况：

-   应用的当前客户仍然可以使用该应用。 不过，他们将无法执行应用内购买。
-   尽管应用仍可供以前购买的客户使用，但你的应用一览将从应用商店中删除。 新用户将无法购买你的应用。
-   将释放你的应用的名称，以供其他开发人员使用。
-   如果以前的应用销售中还有欠款，即使所欠金额不符合通常的支付阈值要求，也可以请求支付该欠款。
