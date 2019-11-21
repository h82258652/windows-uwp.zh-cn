---
ms.assetid: 284EBA1F-BFB4-4CDA-9F05-4927CDACDAA7
title: 开设开发者帐户
description: Here's an overview of how to register for a Windows developer account for Microsoft Store and other Microsoft programs in Partner Center.
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d00fcee11b8cf813144a6f8ea021dc40829056d2
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259952"
---
# <a name="opening-a-developer-account"></a>开设开发者帐户

This article describes how to register for a Windows developer account in [Partner Center](https://partner.microsoft.com/dashboard).

> [!NOTE]
> When you sign up for a developer account, we'll use the email address you provide in your contact info to send messages related to your account. At times, these may include information about our programs. If you choose to [opt out](https://account.microsoft.com/account/Account?ru=https%3A%2F%2Faccount.microsoft.com%2Fprofile%2Fcontact-info&destrt=profile-landing) of these informational emails, be aware that we'll still send you transactional messages (for example, to let you know that your app has passed certification or that a payment is on the way). These transactional emails are a necessary part of your account, and unless you close your account, you'll continue to receive them.

## <a name="the-account-signup-process"></a>帐户注册过程

> [!NOTE]
> In some cases, the screens and fields you see when you register for a developer account may vary slightly from what's outlined in the following steps. But the basic information and process will match what these steps describe.

1.  Go to the [registration page](https://developer.microsoft.com/store/register) and select **Sign up**.
2.  如果尚未使用 Microsoft 帐户登录，请立即登录，或创建新的 Microsoft 帐户。 The Microsoft account you use here is what you'll use to sign in to your developer account.
3.  Select the [country/region](account-types-locations-and-fees.md#developer-account-and-app-submission-markets) where you live or where your business is located. 你将无法在以后更改此设置。
4.  选择[开发者帐户类型](account-types-locations-and-fees.md)（个人或公司）。 之后将无法更改该设置，因此请确保选择正确的帐户类型。
5.  Enter the **publisher display name** that you want to use (50 characters or fewer). 客户在浏览应用时将看到此名称，并通过此名称了解应用，因此请谨慎选择该名称。 对于公司帐户，请确保使用组织的注册公司名称或商标。 If you enter a name that someone else has already selected, or if someone else has the rights to use that name, we won't permit you to use it.

    > [!NOTE]
    > 确保有权使用在此输入的名称。 如果其他人拥有所选名称的商标或版权，帐户可能会关闭。 See [App Developer Agreement](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) for more info. 如果其他人正在使用你对其持有商标或其他法定权利的发布者显示名称，请[联系 Microsoft](https://www.microsoft.com/info/cpyrtInfrg.html)。    

6.  输入希望用于开发者帐户的联系人信息。

    > [!NOTE]
    > 遇到帐户相关事宜时，我们将通过此信息与你联系。 For example, you'll receive an email confirmation after you complete your registration. After that, we'll send messages when we pay you or if you need to fix something with your account. We may also send informational emails as described earlier, unless you opt out of receiving non-transactional emails.

    If you're registering as a company, you'll also need to enter the name, email address, and phone number of the person who will approve your company's account.

7.  Select **Next** to move on to the **Payment** section.

8.  输入一次性注册费用的付款信息。 如果你有涵盖注册成本的促销充值码，可以在此处进行输入。 Otherwise, provide your credit card info (or PayPal info in supported markets). 请注意，预付款的信用卡无法用于此项购买。 When you're finished, select **Next** to move on to the **Review** screen.

9.  审查你的帐户信息并确认所有内容都正确无误。 然后，阅读并接受[应用开发人员协议](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)的条款和条件。 Check the box to indicate you've read these terms and accept them.

10.  Select **Finish** to confirm your registration. Your payment will be processed and we'll send a confirmation message to your email address.

After you've signed up, your account will go through verification. For individual accounts, we check to make sure another user isn't already using your publisher display name. 对于公司帐户，过程所花时间会长一点，因为我们还需要确认用户得到授权，可设置公司帐户。 This verification can take from a few days to a couple of weeks, and it often includes a phone call to your company. 可在**帐户设置**页上查看验证状态。


## <a name="additional-guidelines-for-company-accounts"></a>有关公司帐户的其他指南

> [!IMPORTANT]
> To allow multiple users to access your developer account, we recommend using Azure Active Directory (Azure AD) to assign roles to individual users instead of sharing access to the Microsoft account. Each user can then access the developer account by signing in to Partner Center with their individual Azure AD credentials. 有关详细信息，请参阅[管理帐户用户](manage-account-users.md)。

If you want to let multiple people access the company account by signing in with the Microsoft account that opened it (instead of as individual users added to the account), see the following guidelines:

-   Create the Microsoft account by using an email address that doesn't already belong to you or another individual, such as MyCompany_PartnerCenter@outlook.com. Don't use an email address at your company's domain, particularly if your company already uses Azure AD. As noted earlier, you can add additional users from your company's Azure AD service later.
-   Limit access to this Microsoft account to the least number of users possible.
-   Set up a corporate email distribution list that includes everyone who needs to access the developer account. Add this email address to the [security info associated with the Microsoft account](https://account.microsoft.com/security). This approach allows all the employees on the list to receive security codes that are sent to this alias. If setting up a distribution list isn't feasible, you can add an individual's email address to your security info. But, the owner of that email address will be the only person who can access and share the security code when prompted (such as when new security info is added to the account or when the account is accessed from a new device).
-   Add a company phone number to the Microsoft account's security info. Try to use a number that doesn't require an extension and that's accessible to key team members.
-   Encourage developers to use [trusted devices](https://support.microsoft.com/help/12369/microsoft-account-add-a-trusted-device) to sign in to your company's developer account. 所有关键团队成员都应具有对这些受信任的设备的访问权限。 This arrangement reduces the need for security codes to be sent when team members access the account. There's a limit to the number of codes that can be generated per account per week.
-   如果需要允许从不受信任的电脑访问帐户，请将开发人员访问人数上限限制为五人。 理想情况下，这些开发人员应从共享相同地理和网络位置的计算机访问该帐户。
-   Frequently review your company’s security info at https://account.microsoft.com/security to make sure it's all current.


## <a name="microsoft-account-security"></a>Microsoft account security

我们通过将 Microsoft 帐户与多种形式的身份验证相结合，使用所提供的安全信息来提高 Microsoft 帐户的安全级别。 这样一来，未经授权访问 Microsoft 帐户（和开发者帐户）的难度将大大提高。 Also, if you ever forget your password or if someone else tries to access your account, we’ll be able to reach you to confirm ownership and/or re-establish appropriate control of your account.

You must have at least two email addresses or phone numbers on your Microsoft account. 我们建议添加尽可能多的信息。 请记住，必须对某些安全信息进行确认，它才能生效。 Also, make sure to review your security info frequently and verify that it's up to date. You can manage your security info by going to https://account.microsoft.com/security and signing in with your Microsoft account. For more info, see [Account security info & verification codes](https://support.microsoft.com/help/12428/microsoft-account-security-info-verification-codes).

When you sign in to Partner Center through your Microsoft account, the system may prompt you to verify your identity by sending a security code, which you must enter to complete the sign-in process. We recommend designating PCs that you use frequently as *trusted devices*. When you sign in from a trusted device, you typically aren't prompted for a code, although you may occasionally be prompted in specific situations or if you haven’t signed in on that device in a long time. For more info, see [Add a trusted device to your Microsoft account](https://support.microsoft.com/help/12369/microsoft-account-add-a-trusted-device).


## <a name="closing-your-account"></a>关闭你的帐户

Developer accounts don't expire, so there's no need to renew your account in order to keep it open. 如果你决定完全关闭你的帐户，你可以通过联系技术支持人员来执行此操作。

当关闭你的帐户时，务必了解你在 Microsoft Store 中发布的任何应用会发生什么情况：

-   Your app's current customers can still use the app. However, they can't make in-app purchases.
-   Even though the app is still available to customers who have previously acquired it, your app listing is removed from Microsoft Store. No new customers can acquire your app.
-   将释放你的应用的名称，以供其他开发人员使用。
-   If you have a balance due from previous app sales, you can request payment for that balance even if the amount due doesn't meet the standard payment threshold.
