---
title: Windows Hello
description: 本文介绍了作为 Windows 10 操作系统的一部分随附的新 Windows Hello 技术，并讨论了开发人员可如何实现此技术来保护他们的通用 Windows 平台 (UWP) 应用和后端服务。 它重点介绍了这些技术的特定功能，这些功能有助于缓解由使用传统凭据引发的威胁，并提供有关设计和部署这些技术作为 Windows 10 部署的一部分的指南。
ms.assetid: 0B907160-B344-4237-AF82-F9D47BCEE646
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp 安全
ms.localizationpriority: medium
ms.openlocfilehash: b81bf3c55b493d8e36f186ec56b68367c6245cc7
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2018
ms.locfileid: "4312706"
---
# <a name="windows-hello"></a>Windows Hello




本文介绍了作为 Windows 10 操作系统的一部分随附的新 Windows Hello 技术，并讨论了开发人员可如何实现此技术来保护他们的通用 Windows 平台 (UWP) 应用和后端服务。 它重点介绍了这些技术的特定功能，这些功能有助于缓解由使用传统凭据引发的威胁，并提供有关设计和部署这些技术作为 Windows 10 部署的一部分的指南。

请注意，本文侧重于应用开发。 有关 Windows Hello 的体系结构和实现细节的信息，请参阅 [TechNet 上的 Windows Hello 指南](https://technet.microsoft.com/library/mt589441.aspx)。

有关完整代码示例，请参阅 [GitHub 上的 Windows Hello 代码示例](http://go.microsoft.com/fwlink/?LinkID=717812)。

有关使用 Windows Hello 创建 UWP 应用和支持身份验证服务的分步演练，请参阅 [Windows Hello 登录应用](microsoft-passport-login.md)和 [Windows Hello 登录服务](microsoft-passport-login-auth-service.md)文章。

## <a name="1-introduction"></a>1 简介


有关信息安全的基本假设是系统可以标识谁在使用它。 标识用户使系统可以确定用户是否已相应地标识其自身（此过程称为身份验证），然后确定经过正确身份验证的用户应能够执行哪些操作（授权）。 在全世界部署的绝大多数计算机系统依靠用户凭据进行身份验证和授权决策，这意味着这些系统依靠可重复使用、用户创建的密码作为其安全性的基础。 经常被引用的格言：身份验证可能涉及到“已知信息、已有事物或你的身份”恰到好处地强调了这一问题：可重复使用的密码是单一的身份验证因素，因此任何知道密码的人都可以模拟拥有它的用户。

## <a name="11-problems-with-traditional-credentials"></a>1.1 传统凭据的问题


自从 1960 年代中期 Fernando Corbató 和他的麻省理工学院团队成功引入了密码开始，用户和管理员就必须使用密码来进行用户身份验证和授权。 随着时间的推移，密码存储和使用的技术发展水平已取得某种程度的进步（例如，通过安全哈希和盐析法），但我们仍然面临两个问题。 密码易于克隆，并且易于窃取。 此外，实现错误可能会使它们不安全，而且用户难以在便利和安全之间取得平衡。

## <a name="111-credential-theft"></a>1.1.1 凭据盗窃


密码的最大风险是简单：攻击者可以轻易地窃取它们。 每个输入、处理或存储密码的位置都易受攻击。 例如，攻击者可以通过窃听对应用程序服务器的网络通信、在应用程序或设备中植入恶意软件、日志记录用户在设备上的击键或观察用户键入的字符来从身份验证服务器窃取密码或哈希的集合。 这些只是最常见的攻击方法。

另一个相关风险是凭据重播的风险，即攻击者可以通过窃听不安全的网络来捕获有效凭据，然后在稍后重播它来模拟有效用户。 大多数身份验证协议（包括 Kerberos 和 OAuth）通过在凭据交换过程中包含时间戳来抵御重播攻击，但是该策略只能保护身份验证系统颁发的令牌，而无法保护用户一开始提供的用于获取票证的密码。

## <a name="112-credential-reuse"></a>1.1.2 凭据重复使用




将电子邮件地址用作用户名这一常见做法使本已糟糕的问题变得更糟。 攻击者从损坏的系统成功恢复用户名/密码对后，可以在其他系统上尝试同一个对。 这种手段在相当多的情况下使攻击者得以从损坏的系统转到其他系统中。 将电子邮件地址用作用户名会导致其他问题，我们稍后将在本指南中进行探索。

## <a name="12-solving-credential-problems"></a>1.2 解决凭据问题


解决密码带来的问题很棘手。 仅收紧密码策略不会起效：用户可能只会循环使用、共享或者写下密码。 尽管用户教育对于身份验证安全至关重要，但仅凭教育也无法消除该问题。

Windows Hello 使用强有力的双因素身份验证 (2FA) 来替换密码，方法是验证现有凭据并创建一个受用户手势（基于生物识别或 PIN）保护的特定于设备的凭据。 


## <a name="2-what-is-windows-hello"></a>2 什么是 Windows Hello？


Windows Hello 是 Microsoft 为内置于 Windows10 的新生物识别登录系统所取的名称。 因为它直接内置于操作系统，所以 Windows Hello 允许面部或指纹标识解锁用户的设备。 当用户提供其唯一生物识别标识符来访问特定于设备的凭据时会发生身份验证，这意味着窃取设备的攻击者无法登录它，除非该攻击者具有 PIN。 Windows 安全凭据存储可保护设备上的生物识别数据。 通过使用 Windows Hello 解锁设备，未经授权的用户可以获取其所有 Windows 体验、应用、数据、网站和服务的访问权限。

Windows Hello 验证器称为 Hello。 Hello 特定于单个设备和特定用户的组合。 它不会跨设备漫游、不与服务器或调用应用共享，并且无法轻易地从设备中提取。 如果多个用户共享一台设备，每个用户都需要设置他或她自己的帐户。 每个帐户都获取该设备的唯一 Hello。 你可以将 Hello 视为可用于解锁（或释放）已存储凭据的令牌。 Hello 本身不会针对应用或服务对你进行身份验证，但它会释放可以执行此操作的凭据。 换言之，Hello 不是用户凭据，而是身份验证流程的第二个因素。

## <a name="21-windows-hello-authentication"></a>2.1 Windows Hello 身份验证


Windows Hello 为设备识别个人用户提供了可靠的方法；这解决了用户和请求的服务或数据项之间的路径的第一部分。 在设备已识别该用户后，它仍然必须先对该用户进行身份验证，然后确定是否要授予所请求的资源的访问权限。 Windows Hello 提供完全集成到 Windows 的强 2FA，并将可重复使用的密码替换为特定设备和生物识别手势或 PIN 的组合。

不过，Windows Hello 不仅仅是传统 2FA 系统的替代品。 它在概念上类似于智能卡：通过使用加密基元而不是字符串比较来执行身份验证，并且用户的密钥材料在防篡改的硬件内很安全。 Windows Hello 不需要额外的基础结构组件，而智能卡部署需要这些组件。 尤其是，无需公钥基础结构 (PKI) 即可管理证书（如果你当前没有）。 Windows Hello 继承了智能卡的主要优点（虚拟智能卡的部署灵活性以及物理智能卡的强大安全性），而摒弃了其所有缺点。

## <a name="22-how-windows-hello-works"></a>2.2 Windows Hello 工作原理


当用户在其计算机上设置 Windows Hello 时，它将在该设备上生成一个新的公钥/私钥对。 [受信任的平台模块](https://technet.microsoft.com/itpro/windows/keep-secure/trusted-platform-module-overview) (TPM) 生成并保护此私钥。 如果设备没有 TPM 芯片，私钥将由软件进行加密和保护。 此外，支持 TPM 的设备生成一个数据块，可用于保证将密钥绑定到 TPM。 例如，此保证信息可在你的解决方案中使用，用于确定是否向用户授予了不同的授权级别。

若要在设备上启用 Windows Hello，用户必须在 Windows 设置中连接其 Azure Active Directory 帐户或 Microsoft 帐户。

## <a name="221-how-keys-are-protected"></a>2.2.1 如何保护密钥


每当生成密钥材料时，必须保护它免受攻击。 执行此操作的最可靠方法是通过专用硬件。 使用硬件安全模块 (HSM) 为安全关键应用程序生成、存储和处理密钥有很长的历史。 智能卡是一种特殊类型的 HSM，符合受信任的计算组 TPM 标准的设备也是如此。 在任何可能的情况下，Windows Hello 实现充分利用板载 TPM 硬件来生成、存储和处理密钥。 但是，Windows Hello 和 Windows Hello for Work 不需要板载 TPM。

在任何可行的情况下，Microsoft 建议使用 TPM 硬件。 TPM 可抵御各种已知和潜在的攻击，包括 PIN 暴力攻击。 TPM 在帐户锁定后也会提供一层额外的保护。 当 TPM 已锁定密钥材料时，用户必须重置 PIN。 重置 PIN 意味着使用旧密钥材料加密的所有密钥和证书都将删除。

## <a name="222-authentication"></a>2.2.2 身份验证


当用户希望访问受保护的密钥材料时，身份验证过程将从用户输入 PIN 或生物识别手势以解锁设备开始，此过程有时称为“释放密钥”。

应用程序永远无法使用其他应用程序的密钥，并且任何人都永远无法使用其他用户的密钥。 这些密钥用对发送给标识提供程序或 IDP 的请求进行签名，从而寻找对特定资源的访问权限。 应用程序可以使用特定 API 请求需要将密钥材料用于特定操作的操作。 通过这些 API 进行访问确实需要通过用户手势的显式验证，并且密钥材料不会向请求的应用程序公开。 相反，应用程序会要求特定的操作（如对一部分数据进行签名），并且 Windows Hello 层将处理实际工作并返回结果。

## <a name="23-getting-ready-to-implement-windows-hello"></a>2.3 准备好实现 Windows Hello


现在我们已基本了解了 Windows Hello 的工作原理，让我们查看一下如何在自己的应用程序中实现它们。

我们可以使用 Windows Hello 实现各种不同的方案。 例如，仅仅在设备上登录应用。 其他常见方案是针对服务进行身份验证。 你将使用 Windows Hello，而不是使用登录名和密码。 在以下章节中，我们将讨论几个不同的方案，包括如何使用 Windows Hello 针对你的服务进行身份验证，以及如何从现有用户名/密码系统转换为 Windows Hello 系统。

最后，请注意 Windows Hello API 需要使用与将使用该应用的操作系统匹配的 Windows 10 SDK。 换句话说，10.0.10240 Windows SDK 必须用于将部署到 Windows 10 的应用，并且 10.0.10586 必须用于将部署到 Windows 10 版本 1511 的应用。

## <a name="3-implementing-windows-hello"></a>3 实现 Windows Hello


在本章节中，我们将从没有现有身份验证系统的全新方案开始，并且将说明如何实现 Windows Hello。

下一部分介绍如何从现有用户名/密码系统迁移。 但是，即使该章节更吸引你，你可能希望浏览本章节以大致了解所需的过程和代码。

## <a name="31-enrolling-new-users"></a>3.1 注册新用户


我们从将使用 Windows Hello 的全新服务和一个准备在新设备上注册的假想新用户开始。

第一步是验证用户是否可以使用 Windows Hello。 应用验证用户设置和计算机功能以确保它可以创建用户 ID 密钥。 如果应用确定该用户尚未启用 Windows Hello，它会提示用户在使用应用前进行此设置。

若要启用 Windows Hello，用户只需在 Windows 设置中设置 PIN，除非用户已在全新体验 (OOBE) 期间进行此设置。

以下代码行显示用于检查用户是否已为 Windows Hello 做好准备的简单方法。

```cs
var keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
if (!keyCredentialAvailable)
{
   // User didn't set up PIN yet
   return;
}
```

下一步是要求用户输入信息来注册你的服务。 你可以选择要求用户输入名字、姓氏、电子邮件地址和唯一用户名。 你可以使用电子邮件地址作为唯一标识符；这由你来决定。

在此方案中，我们使用电子邮件地址作为用户的唯一标识符。 用户注册后，你应考虑发送验证电子邮件以确保该地址有效。 这为你提供了用于重置帐户（如果有必要）的机制。

如果用户已设置他或她的 PIN，应用将创建用户的 [**KeyCredential**](https://msdn.microsoft.com/library/windows/apps/dn973029)。 应用还获取可选的密钥证明信息，以获取密钥已在 TPM 上生成的加密证明。 生成的公钥和证明（可选）将发送到后端服务器以注册要使用的设备。 每台设备上生成的每个密钥对都将是唯一的。

用于创建 [**KeyCredential**](https://msdn.microsoft.com/library/windows/apps/dn973029) 的代码如下所示：

```cs
var keyCreationResult = await KeyCredentialManager
    .RequestCreateAsync(AccountId, KeyCredentialCreationOption.ReplaceExisting);
```

[**RequestCreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn973048) 是创建公钥和私钥的部分。 如果设备具有正确的 TPM 芯片，API 将请求 TPM 芯片创建私钥和公钥并存储结果；如果不存在可用的 TPM 芯片，操作系统将在代码中创建密钥对。 应用无法直接访问所创建的私钥。 部分创建的密钥对也是生成的证明信息。 （有关证明的详细信息，请参阅下一部分。）

在设备上创建密钥对和证明信息后，需要将公钥、可选证明信息和唯一标识符（如电子邮件地址）发送到后端注册服务并存储在后端中。

若要允许用户在多台设备上访问该应用，后端服务需要能够为同一个用户存储多个密钥。 由于每个密钥对于每台设备是唯一的，因此我们将存储所有这些与同一用户相关的密钥。 设备标识符用于在对用户进行身份验证时帮助优化服务器部分。 我们将在下一章节对此进行更详细的讨论。

用于在后端存储此信息的示例数据库架构可能如下所示：

![Windows Hello 示例数据库架构](images/passport-db.png)

注册逻辑可能如下所示：

![Windows Hello 注册逻辑](images/passport-registration.png)

你收集的注册信息当然可能比我们在此简单方案中要包括更多的标识信息。 例如，如果你的应用访问受保护的服务（如银行服务），你可能需要在注册过程中请求标识证明和其他内容。 满足所有条件后，此用户的公钥将存储在后端，并用于在用户下次使用该服务时进行验证。

```cs
using System;
using System.Runtime;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Security.Credentials;

static async void RegisterUser(string AccountId)
{
    var keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
    if (!keyCredentialAvailable)
    {
        // The user didn't set up a PIN yet
        return;
    }

    var keyCreationResult = await KeyCredentialManager.RequestCreateAsync(AccountId, KeyCredentialCreationOption.ReplaceExisting);
    if (keyCreationResult.Status == KeyCredentialStatus.Success)
    {
        var userKey = keyCreationResult.Credential;
        var publicKey = userKey.RetrievePublicKey();
        var keyAttestationResult = await userKey.GetAttestationAsync();
        IBuffer keyAttestation = null;
        IBuffer certificateChain = null;
        bool keyAttestationIncluded = false;
        bool keyAttestationCanBeRetrievedLater = false;

        keyAttestationResult = await userKey.GetAttestationAsync();
        KeyCredentialAttestationStatus keyAttestationRetryType = 0;

        if (keyAttestationResult.Status == KeyCredentialAttestationStatus.Success)
        {
            keyAttestationIncluded = true;
            keyAttestation = keyAttestationResult.AttestationBuffer;
            certificateChain = keyAttestationResult.CertificateChainBuffer;
        }
        else if (keyAttestationResult.Status == KeyCredentialAttestationStatus.TemporaryFailure)
        {
            keyAttestationRetryType = KeyCredentialAttestationStatus.TemporaryFailure;
            keyAttestationCanBeRetrievedLater = true;
        }
        else if (keyAttestationResult.Status == KeyCredentialAttestationStatus.NotSupported)
        {
            keyAttestationRetryType = KeyCredentialAttestationStatus.NotSupported;
            keyAttestationCanBeRetrievedLater = true;
        }
    }
    else if (keyCreationResult.Status == KeyCredentialStatus.UserCanceled ||
        keyCreationResult.Status == KeyCredentialStatus.UserPrefersPassword)
    {
        // Show error message to the user to get confirmation that user
        // does not want to enroll.
    }
}
```

## <a name="311-attestation"></a>3.1.1 证明


在创建密钥对时，还可以选择请求证明信息，该信息由 TMP 芯片生成。 此可选信息可发送到服务器作为注册过程的一部分。 TPM 密钥证明是以加密方式证明密钥是 TPM 绑定密钥的协议。 此类型的证明可用于保证特定加密操作在特定计算机的 TPM 中发生。

当服务器收到生成的 RSA 密钥、证明语句和 AIK 证书时，它将验证以下条件：

-   AIK 证书签名有效。
-   AIK 证书链接到受信任的根。
-   AIK 证书及其链针对 EKU OID“2.23.133.8.3”（友好名称为“证明身份密钥证书”）启用。
-   AIK 证书具有有效期。
-   链中所有颁发的 CA 证书都具有有效期，并且不会撤销。
-   证明语句格式正确。
-   [**KeyAttestation**](https://msdn.microsoft.com/library/windows/apps/dn298288) blob 上的签名使用 AIK 公钥。
-   包含在 [**KeyAttestation**](https://msdn.microsoft.com/library/windows/apps/dn298288) blob 中的公钥与和证明语句一起发送的公共 RSA 密钥匹配。

你的应用可能为用户分配不同的授权级别，具体取决于这些条件。 例如，如果其中一项检查失败，它可能不会注册用户或可能限制用户可以执行的操作。

## <a name="32-logging-on-with-windows-hello"></a>3.2 使用 Windows Hello 登录


用户在系统中注册后，他或她可以使用该应用。 根据方案，你可以要求用户先进行身份验证，然后才允许他们开始使用该应用，或者只在他们开始使用后端服务后要求他们进行身份验证。

## <a name="33-force-the-user-to-sign-in-again"></a>3.3 强制用户再次登录


对于某些方案，你可能希望用户在访问应用前或有时在应用内执行特定操作前证明他或她是当前登录的人员。 例如，在银行应用向服务器发送转帐命令前，你希望确保是用户而不是某个捡到已登录设备的人正在尝试执行该事务。 你可以通过使用 [**UserConsentVerifier**](https://msdn.microsoft.com/library/windows/apps/dn279134) 类强制用户再次登录。 以下代码行将强制用户输入他们的凭据。

以下代码行将强制用户输入他们的凭据。

```cs
UserConsentVerificationResult consentResult = await UserConsentVerifier.RequestVerificationAsync("userMessage");
if (consentResult.Equals(UserConsentVerificationResult.Verified))
{
   // continue
}
```

当然，你还可以从服务器使用质询响应机制，这要求用户输入他或她的 PIN 码或生物识别凭据。 它取决于你作为开发人员需要实现的方案。 以下部分中将介绍此机制。

## <a name="34-authentication-at-the-backend"></a>3.4 后端的身份验证


当应用尝试访问受保护的后端服务时，服务将向应用发送质询。 该应用使用来自用户的私钥来对该质询进行签名，并将其发送回服务器。 由于服务器已存储该用户的公钥，因此它使用标准加密 API 来确保消息确实已使用正确的私钥进行签名。 在客户端上，签名通过 Windows Hello API 来完成，开发人员将永远无法访问任何用户的私钥。

除了检查密钥，该服务还可以检查密钥证明并辨别是否对密钥存储在设备上的方式调用了任何限制。 例如，当设备使用 TPM 保护密钥时，它比不使用 TPM 存储密钥的设备更安全。 例如，后端逻辑可以决定在未使用 TPM 降低风险时仅允许用户转移特定的金额。

证明仅适用于具有版本 2.0 或更高版本的 TPM 芯片的设备。 因此，你需要考虑到此信息可能不适用于每台设备。

客户端工作流可能如下图所示：

![Windows Hello 客户端工作流](images/passport-client-workflow.png)

当应用在后端调用服务时，服务器会发送质询。 该质询使用以下代码进行签名：

```cs
var openKeyResult = await KeyCredentialManager.OpenAsync(AccountId);

if (openKeyResult.Status == KeyCredentialStatus.Success)
{
    var userKey = openKeyResult.Credential;
    var publicKey = userKey.RetrievePublicKey();
    var signResult = await userKey.RequestSignAsync(message);
    
    if (signResult.Status == KeyCredentialStatus.Success)
    {
        return signResult.Result;
    }
    else if (signResult.Status == KeyCredentialStatus.UserPrefersPassword)
    {
        
    }
}
```

第一行 [**KeyCredentialManager.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn973046) 将要求操作系统打开密钥句柄。 如果成功完成该操作，你可以对质询消息进行签名，其中 [**KeyCredential.RequestSignAsync**](https://msdn.microsoft.com/library/windows/apps/dn973058) 方法将触发操作系统通过 Windows Hello 请求用户的 PIN 或生物识别。 开发人员在任何时候都无法访问用户的私钥。 这全部通过 API 保持安全。

这些 API 请求操作系统使用私钥对质询进行签名。 然后系统要求用户输入 PIN 码或进行已配置的生物识别登录。 输入正确的信息后，系统可以要求 TPM 芯片执行加密功能并对质询进行签名。 （或者在 TPM 不可用时，使用回退软件解决方案）。 客户端必须将已签名的质询发送回服务器。

此序列图中显示了一个基本的质询响应流：

![Windows Hello 质询响应](images/passport-challenge-response.png)

接下来，服务器必须验证签名。 当你请求公钥并将其发送到服务器以供将来验证使用时，它是 ASN.1 编码的 publicKeyInfo blob。如果你查看 [GitHub 上的 Windows Hello 代码示例](http://go.microsoft.com/fwlink/?LinkID=717812)，你将看到可用于封装 Crypt32 函数以将 ASN.1 编码的 blob 转换到 CNG blob 的帮助程序类，这会更加常用。 该 blob 包含公钥算法（即 RSA ）和 RSA 公钥。

具有 CNG blob 后，你需要针对用户的公钥验证已签名的质询。 由于每个人都使用他或她自己的系统或后端技术，因此没有实现此逻辑的通用方法。 我们正在使用 SHA256 作为哈希算法并针对 SignaturePadding 使用 Pkcs1，因此请确保你在从客户端验证已签名的响应时使用它们。 同样，指向该示例作为在服务器上使用 .NET 4.6 执行此操作的方法，但它大致上如下所示：

```cs
using (RSACng pubKey = new RSACng(publicKey))
{
   retval = pubKey.VerifyData(originalChallenge, responseSignature,  HashAlgorithmName.SHA256, RSASignaturePadding.Pkcs1); 
}
```

我们读取已存储的公钥，即 RSA 密钥。 我们使用该公钥验证已签名的质询消息，如果检查无误，我们将授权该用户。 如果对用户进行身份验证，应用可以像往常一样调用后端服务。

完整代码可能如下所示：

```cs
using System;
using System.Runtime;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.Core;
using Windows.Security.Credentials;

static async Task<IBuffer> GetAuthenticationMessageAsync(IBuffer message, String AccountId)
{
    var openKeyResult = await KeyCredentialManager.OpenAsync(AccountId);

    if (openKeyResult.Status == KeyCredentialStatus.Success)
    {
        var userKey = openKeyResult.Credential;
        var publicKey = userKey.RetrievePublicKey();
        var signResult = await userKey.RequestSignAsync(message);
        if (signResult.Status == KeyCredentialStatus.Success)
        {
            return signResult.Result;
        }
        else if (signResult.Status == KeyCredentialStatus.UserCanceled)
        {
            // Launch app-specific flow to handle the scenario 
            return null;
        }
    }
    else if (openKeyResult.Status == KeyCredentialStatus.NotFound)
    {
        // PIN reset has occurred somewhere else and key is lost.
        // Repeat key registration
        return null;
    }
    else
    {
        // Show custom UI because unknown error has happened.
        return null;
    }
}
```

实现正确的质询响应机制不在本文档的范围内，但是为了成功创建安全机制来防止重播攻击或中间人攻击等威胁，本主题是需要注意的内容。

## <a name="35-enrolling-another-device"></a>3.5 注册其他设备


如今，用户通常拥有安装了相同应用的多台设备。 Windows Hello 在多台设备上使用时的工作原理是什么？

在使用 Windows Hello 时，每台设备都将创建唯一的私钥和公钥集。 这意味着，如果你希望用户能够使用多台设备，你的后端必须能够存储来自此用户的多个公钥。 有关表结构的示例，请参考 2.1 部分中的数据库关系图。

注册其他设备与第一次注册用户大致相同。 你仍然需要确保注册此新设备的用户确实是他或她所自称的用户。 你可以使用如今使用的任何双因素身份验证机制执行此操作。 有多种以安全方式完成此操作的方法。 它完全取决于你的方案。

例如，如果你仍然使用登录名和密码，你可以使用它们来对用户进行身份验证，并要求他们使用其验证方法之一，如短信或电子邮件。 如果你没有登录名和密码，你还可以使用已经注册的设备之一，并向该设备上的应用发送通知。 MSA 验证器应用是它的一个示例。 简而言之，你应使用常用的 2FA 机制来为该用户注册额外设备。

用于为新设备注册的代码与第一此注册用户（从应用内）的代码完全相同。

```cs
var keyCreationResult = await KeyCredentialManager.RequestCreateAsync(
    AccountId, KeyCredentialCreationOption.ReplaceExisting);
```

若要使用户更容易识别注册了哪些设备，你可以选择发送设备名称或另一个标识符作为注册的一部分。 例如，如果你希望在后端上实现一项服务，以便用户可以在丢失设备时注销设备，这也非常有用。

## <a name="36-using-multiple-accounts-in-your-app"></a>3.6 在应用中使用多个帐户


除了为单个帐户支持多台设备，在单个应用中支持多个帐户也很常见。 例如，也许你正从应用内连接到多个 Twitter 帐户。 借助 Windows Hello，你可以创建多个密钥对并在应用内支持多个帐户。

执行此操作的一个方法是将在上一章节中标识的用户名或唯一标识符存储在隔离存储中。 因此，每次你创建新帐户时，都将帐户 ID 存储在隔离存储中。

在应用 UI 中，你允许用户选择之前创建的帐户之一或注册新帐户。 创建新帐户的流程与之前所述的相同。 选择帐户只需在屏幕上列出已存储的帐户。 在用户选择某个帐户后，使用该帐户 ID 在应用中登录该用户：

```cs
var openKeyResult = await KeyCredentialManager.OpenAsync(AccountId);
```

其余流程与之前所述的相同。 需要明确的是，所有这些帐户都受相同的 PIN 或生物识别手势保护，因为在此方案中，在使用了同一 Windows 帐户的单台设备上使用这些帐户。

## <a name="4-migrating-an-existing-system-to-windows-hello"></a>4 将现有系统迁移到 Windows Hello


在此简短部分中，我们将处理现有通用 Windows 平台应用和使用可存储用户名和哈希密码的数据库的后端系统。 这些应用会在应用启动时收集用户的凭据，并在后端系统返回身份验证质询时使用它们。

我们将在此处讨论需要更改或替换哪些部分以便使 Windows Hello 起效。

我们已在之前的章节中介绍大部分技术。 将 Windows Hello 添加到现有系统涉及到添加一些注册中不同的流和代码的身份验证部分。

一个方法是允许用户选择升级时间。 在用户登录应用并且你检测到该应用和操作系统能够支持 Windows Hello 后，你可以询问用户他或她是否希望升级凭据以使用此更现代且更安全的系统。 你可以使用以下代码来检查用户是否能够使用 Windows Hello。

```cs
var keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
```

UI 外观可能如下所示：

![Windows Hello UI](images/passport-ui.png)

如果用户选择开始使用 Windows Hello，你可以创建之前所述的 [**KeyCredential**](https://msdn.microsoft.com/library/windows/apps/dn973029)。 后端注册服务器将公钥和可选证明语句添加到数据库。 由于已经使用用户名和密码对用户进行身份验证，因此服务器可以将新的凭据链接到数据库中的当前用户信息。 数据库模型可能与之前所述的示例相同。

如果应用能够创建用户 [**KeyCredential**](https://msdn.microsoft.com/library/windows/apps/dn973029)，它会将用户 ID 存储在隔离存储中，以便用户可以在应用再次启动后从列表中选取此帐户。 从此时起，流程完全遵循之前的章节所述的示例。

迁移到完整 Windows Hello 方案的最后步骤是在应用中禁用登录名和密码选项，并从数据库中删除已存储的哈希密码。

## <a name="5-summary"></a>5 摘要


Windows 10 引入了更高级别且易于实施的安全性。 Windows Hello 提供了新的生物识别登录系统，可识别用户并主动挫败绕开正常标识的尝试。 然后，它可以提供永远无法泄露或在受信任的平台模块外使用的多层密钥和证书。 此外，可以选择使用证明标识密钥和证书来增加一层安全性。

作为开发人员，你可以使用此有关这些技术的设计和部署的指南，以便向 Windows 10 推出轻松添加安全身份验证来保护应用和后端服务。 所需代码很少且易于理解。 Windows 10 负责繁重的工作。

灵活的实现选项使 Windows Hello 可以替换你的现有身份验证系统或与其协同工作。 部署体验轻松且实惠。 部署 Windows 10 安全无需额外基础结构。 借助内置于操作系统中的 Microsoft Hello，Windows 10 针对现代开发人员所面临的身份验证问题提供了最安全的解决方案。

任务完成！ 你刚刚使 Internet 变得更安全了！

## <a name="6-resources"></a>6. 资源


### <a name="61-articles-and-sample-code"></a>6.1 文章和示例代码

-   [Windows Hello 概述](http://windows.microsoft.com/windows-10/getstarted-what-is-hello)
-   [Windows Hello 的实现细节](https://msdn.microsoft.com/library/mt589441)
-   [GitHub 上的 Windows Hello 代码示例](http://go.microsoft.com/fwlink/?LinkID=717812)

### <a name="62-terminology"></a>6.2 术语

|                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AIK                 | 证明标识密钥用于提供此类加密证明（TPM 密钥证明），方法是对不可迁移密钥的属性进行签名，并向信赖方提供属性和签名以供验证。 生成的签名称为“证明语句”。 由于该签名使用 AIK 私钥（只能在创建它的 TPM 中使用）创建，信赖方可以信任经证明的密钥确实不可迁移，并且无法在该 TPM 之外使用。 |
| AIK 证书     | AIK 证书用于证明 TPM 内存在 AIK。 它还用于证明经源自该特定 TPM 的 AIK 认证的其他密钥。                                                                                                                                                                                                                                                                                                                                              |
| IDP                 | IDP 是标识提供程序。 一个示例是由 Microsoft 为 Microsoft 帐户生成的 IDP。 每次应用程序需要使用 MSA 进行身份验证时，它可以调用 MSA IDP。                                                                                                                                                                                                                                                                                                                                        |
| PKI                 | 公钥基础结构通常用于指向由组织本身托管的环境，并且负责创建密钥、撤销密钥等。                                                                                                                                                                                                                                                                                                                                                           |
| TPM                 | 受信任的平台模块可用于创建加密公钥/私钥对，其创建方式使私钥永远无法泄露或在 TPM 之外使用（即，该密钥不可迁移）。                                                                                                                                                                                                                                                                                                               |
| TPM 密钥证明 | 一种以加密方式证明密钥是 TPM 绑定密钥的证明。 此类型的证明可用于保证特定加密操作在特定计算机的 TPM 中发生                                                                                                                                                                                                                                                                                                                       |

 

## <a name="related-topics"></a>相关主题

* [Windows Hello 登录应用](microsoft-passport-login.md)
* [Windows Hello 登录服务](microsoft-passport-login-auth-service.md)