---
title: 安全 Windows 应用开发简介
description: 本入门文章可以帮助应用架构师和开发人员更好地了解可加速创建安全通用 Windows 平台 (UWP) 应用的各种 windows 10 平台功能。
ms.assetid: 6AFF9D09-77C2-4811-BB1A-BBF4A6FF511E
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp 安全
ms.localizationpriority: medium
ms.openlocfilehash: adc74410a5012dbc59cc995e80ee5fa729f0176f
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6464454"
---
# <a name="intro-to-secure-windows-app-development"></a>安全 Windows 应用开发简介




本入门文章可以帮助应用架构师和开发人员更好地了解可加速创建安全通用 Windows 平台 (UWP) 应用的各种 windows 10 平台功能。 它详细介绍了如何在以下各个阶段使用可用的 Windows 安全功能：身份验证、未送达数据和静态数据。 可以通过查看包括在每章中的其他资源来查找有关每个主题的更详细信息。

## <a name="1-introduction"></a>1 简介


开发安全应用非常具有挑战性。 在当今充斥着移动、社交、云和复杂企业应用的快节奏世界中，客户希望比以前更快地使用和更新应用。 他们还使用许多类型的设备，这会进一步增加创建应用体验的复杂性。 如果为 Windows 10 通用 Windows 平台 (UWP) 生成应用，除了日益增长的一列跨越物联网、Xbox One、Microsoft Surface Hub 和 HoloLens 的新设备外，还包括一列传统的台式机、笔记本电脑、平板电脑和移动设备。 作为开发人员，必须确保应用在涉及到的所有平台和设备上能够安全通信和存储数据。

以下是一些使用 Windows 10 安全功能的优势。

-   通过将一致的 API 用于安全组件和技术，你可以在所有支持 Windows 10 的设备上拥有标准化安全性。
-   如果你实现了自定义代码来覆盖这些安全方案，则所编写、测试和维护的代码要比实际的少。
-   由于你使用操作系统控制应用访问其资源和本地或远程系统资源的方式，因此应用变得更加稳定和安全。

在身份验证期间，将验证请求访问特定服务的用户的身份。 Windows Hello 是 Windows 10 中的组件，可帮助在 Windows 应用中创建更安全的身份验证机制。 通过它，你可以使用个人标识号 (PIN) 或生物识别（例如用户指纹、面部或虹膜）来为应用实现多重身份验证。

未送达数据是指连接和通过该连接传输的消息。 它的一个示例是使用 Web 服务在远程服务器中检索数据。 使用安全套接字层 (SSL) 和安全超文本传输协议 (HTTPS) 可确保连接的安全性。 阻止中间方访问这些消息，或阻止未经授权的应用与 Web 服务进行通信对保护未送达数据而言很关键。

最后，静态数据与驻留在内存或存储媒体上的数据相关。 Windows 10 具有在应用间阻止未经授权的数据访问和提供进一步保护设备数据的加密 API 的应用模型。 称为“凭据保险箱”的功能可以用于在设备上安全存储用户凭据，该设备上配有阻止其他应用访问这些凭据的操作系统。

## <a name="2-authentication-factors"></a>2 身份验证因素


若要保护数据，必须标识请求访问它的人员并向其授予所请求的数据资源的访问权限。 标识用户的过程称为身份验证，而确定对某项资源的访问权限称为授权。 这些操作紧密相关，用户可能无法辨别。 它们可以是相对简单或复杂的操作，具体取决于许多因素：例如数据驻留在一台服务器上还是分配在许多系统上。 提供身份验证和授权服务的服务器被称为标识提供程序。

为了针对特定服务和/或应用对自己进行身份验证，用户要利用包含他们所知信息的凭据、他们拥有的内容和/或他们的身份。 其中的每项内容均称为身份验证因素。

-   **Something the user knows** 通常是密码，但它也可以是个人标识号 (PIN) 或“密码”问答对。
-   **Something the user has** 通常是包含用户独有的身份验证数据的硬件内存设备（例如 U 盘）。
-   **Something the user is** 通常包括其指纹，但诸如用户语音、面部、眼部（眼睛）特征或行为模式等因素越来越流行。 在存储为数据时，这些度量值称为生物识别。

用户创建的密码本身是一个身份验证因素，但它通常不同分；任何知道密码的人都可以冒充拥有该密码的用户。 智能卡可以提供较高级别的安全性，但它可能被盗、丢失或遗忘。 通过指纹或眼部扫描对用户进行身份验证的系统可提供最高和最便利的安全级别，但它需要昂贵且专业的硬件（如用于面部识别的 Intel RealSense 相机），这些硬件并非可供所有用户使用。

设计系统使用的身份验证方法是数据安全的一个复杂且重要的方面。 通常，你在身份验证中使用的因素数量越多，系统就越安全。 同时，身份验证必须可以便于使用。 用户通常在一天内多次登录，因此该过程必须快速。 选择身份验证类型时需权衡安全性和易用性；单因素身份验证最不安全但最易于使用，而多重身份验证随着因素的增加而变得越来越安全但也越来越复杂。

## <a name="21-single-factor-authentication"></a>2.1 单因素身份验证


这种形式的身份验证基于单个用户凭据。 这通常是一个密码，但它也可以是个人标识号 (PIN)。

下面是单因素身份验证的过程。

-   用户向标识提供程序提供其用户名和密码。 标识提供程序是验证用户身份的服务器进程。
-   标识提供程序检查用户名和密码是否与系统中存储的用户名和密码相同。 在大多数情况下，将加密密码来提供额外的安全性，以便其他人无法读取密码。
-   标识提供程序返回一个身份验证状态，用于指示身份验证是否已成功。
-   如果成功，开始数据交换。 如果不成功，用户必须重新进行身份验证。

![单因素身份验证](images/secure-sfa.png)

如今此身份验证方法是服务中最常用的方法。 当用作身份验证的唯一方法时，它也是最不安全的身份验证形式。 密码复杂性要求、“密码问题”和定期密码更改可使密码更安全，但它们对用户造成了更大的负担，并且无法有效抵御黑客攻击。

密码质询比具有多个因素的系统更易于成功猜测密码。 如果他们从一家小型 Web 商店窃取了具有用户帐户和已进行哈希操作的密码的数据库，则他们可以使用在其他网站上使用过的密码。 用户通常会一直重复使用帐户，因为复杂的密码难以记住。 对于 IT 部门，管理密码也会带来一定的复杂性，他们必须提供重置机制、需要频繁更新密码和以安全方式存储密码。

相对于它的所有缺点，单因素身份验证使用户可以控制凭据。 他们创建和修改它，并且在身份验证过程中只需要一个键盘。 这是将单因素和多重身份验证区分开来的主要方面。

## <a name="211-web-authentication-broker"></a>2.1.1 Web 身份验证代理


如前所述，IT 部门所面临的密码身份验证的挑战之一是管理用户名/密码基础、重置机制等的开销增加。越来越受欢迎的选择是依靠通过 OAuth（身份验证的开放标准）提供身份验证的第三方标识提供商。

通过使用 OAuth，IT 部门可以有效地将维护带有用户名和密码的数据库、重置密码功能等复杂事务“外包”给第三方标识提供商（例如 Facebook、Twitter 或 Microsoft）。

用户在这些平台上可以完全控制其标识，但是在用户经过身份验证并征得他们的同意后，应用可以向提供商请求可用于授权经过身份验证的用户的令牌。

Windows 10 中的 Web 身份验证代理为应用提供了一组 API 和基础结构，以便使用身份验证和授权协议（如 OAuth 和 OpenID）。 应用可以通过 [**WebAuthenticationBroker**](https://msdn.microsoft.com/library/windows/apps/br227025) API 启动身份验证操作，从而可以返回 [**WebAuthenticationResult**](https://msdn.microsoft.com/library/windows/apps/br227038)。 下图阐释了通信流概述。

![WAB 工作流](images/secure-wab.png)

应用充当代理，通过应用中的 [**WebView**](https://msdn.microsoft.com/library/windows/apps/br227702) 使用标识提供程序启动身份验证。 当标识提供程序已对用户进行身份验证时，它将向应用返回一个令牌，可用于从标识提供程序请求有关该用户的信息。 作为一项安全措施，应用必须先向标识提供程序注册才能通过标识提供程序代理身份验证过程。 对于每个提供程序，注册步骤都不同。

下面是在调用 [**WebAuthenticationBroker**](https://msdn.microsoft.com/library/windows/apps/br227025) API 来与提供程序通信时所使用的常规工作流。

-   构建要发送到标识提供程序的请求字符串。 对于每项 Web 服务，字符串的数量以及每个字符串中的信息都不相同，但它通常包括两个各自均包含一个 URL 的 URI 字符串：一个是身份验证请求的发送对象，一个是用户在授权完成后重定向到的对象。
-   调用 [**WebAuthenticationBroker.AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066)、传入请求字符串，然后等待来自标识提供程序的响应。
-   调用 [**WebAuthenticationResult.ResponseStatus**](https://msdn.microsoft.com/library/windows/apps/br227041) 以在收到响应时获取状态。
-   如果通信成功，则处理标识提供程序返回的响应字符串。 如果不成功，则处理错误。

如果通信成功，则处理标识提供程序返回的响应字符串。 如果不成功，则处理错误。

此过程的示例 C# 代码如下所示。 有关信息和详细演练，请参阅 [WebAuthenticationBroker](web-authentication-broker.md)。 有关完整代码示例，请查看 [GitHub 上的 WebAuthenticationBroker 示例](http://go.microsoft.com/fwlink/p/?LinkId=620622)。

```cs
string startURL = "https://<providerendpoint>?client_id=<clientid>";
string endURL = "http://<AppEndPoint>";

var startURI = new System.Uri(startURL);
var endURI = new System.Uri(endURL);

try
{
    WebAuthenticationResult webAuthenticationResult = 
        await WebAuthenticationBroker.AuthenticateAsync( 
            WebAuthenticationOptions.None, startURI, endURI);

    switch (webAuthenticationResult.ResponseStatus)
    {
        case WebAuthenticationStatus.Success:
            // Successful authentication. 
            break;
        case WebAuthenticationStatus.ErrorHttp:
            // HTTP error. 
            break;
        default:
            // Other error.
        break;
    }
}
catch (Exception ex)
{
    // Authentication failed. Handle parameter, SSL/TLS, and
    // Network Unavailable errors here. 
}
```

## <a name="22-multi-factor-authentication"></a>2.2 多重身份验证


多重身份验证利用多个身份验证因素。 通常情况下，将“已知信息”（例如密码）与“已有事物”（可以是手机或智能卡）组合起来。 即使攻击者发现了用户的密码，他仍然无法在没有设备或卡的情况下访问帐户。 并且如果设备或卡受到损害，在没有密码的情况下它对攻击者无用。 因此多重身份验证比单因素身份验证更安全但也更复杂。

使用多重身份验证的服务经常让用户选择如何接收第二个凭据。 此类型身份验证的一个示例是使用短信向用户的移动电话发送验证码的常用过程。

-   用户向标识提供程序提供其用户名和密码。
-   标识提供程序像在单因素身份验证中一样验证用户名和密码，然后查找存储在系统中的该用户的移动电话号码。
-   服务器将一条包含生成的验证码的短信发送到用户的移动电话。
-   用户向标识提供程序提供验证码；通过某种形式向用户呈现。
-   标识提供程序返回一个身份验证状态，用于指示这两个凭据的身份验证是否已成功。
-   如果成功，开始数据交换。 否则，用户必须重新进行身份验证。

![双重身份验证](images/secure-tfa.png)

如你所见，此过程也不同于单因素身份验证，因为第二个用户凭据发送给用户，而不是由用户创建或提供。 因此用户并没有对必要凭据的完全控制。 这还适用于将智能卡用作第二个凭据的情况：组织负责创建它和将其提供给用户。

## <a name="221-azure-active-directory"></a>2.2.1 Azure Active Directory


Azure Active Directory (Azure AD) 是一种基于云的标识和访问权限管理服务，可用作单因素或多重身份验证中的标识提供程序。 Azure AD 身份验证可以与验证码一起使用，也可以单独使用。

虽然 Azure AD 也可以实现单因素身份验证，但企业通常需要安全性更高的多重身份验证。 在多重身份验证配置中，使用 Azure AD 帐户的用户身份验证可以选择以短信形式将验证码发送到他们的移动电话或 Azure Authenticator 移动应用上。

另外，Azure AD 可用作 OAuth 提供程序，以便向标准用户提供各种平台上应用的身份验证和授权机制。 若要了解详细信息，请参阅 [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) 和 [Azure 上的多重身份验证](https://azure.microsoft.com/services/multi-factor-authentication/)。

## <a name="24-windows-hello"></a>2.4 Windows Hello


在 Windows 10 中，操作系统内置了方便的多重身份验证机制。 Windows Hello 是内置于 Windows 10 的新生物识别登录系统。 因为它直接内置于操作系统，所以 Windows Hello 允许面部或指纹标识解锁用户的设备。 Windows 安全凭据存储可保护设备上的生物识别数据。

Windows Hello 为设备识别个人用户提供了可靠的方法；这解决了用户和请求的服务或数据项之间的路径的第一部分。 在设备已识别该用户后，它仍然必须先对该用户进行身份验证，然后确定是否要授予所请求的资源的访问权限。 Windows Hello 还提供完全集成到 Windows 的强双因素身份验证 (2FA)，并将可重复使用的密码替换为特定设备和生物识别手势或 PIN 的组合。 该 PIN 在用户的 Microsoft 帐户注册过程中由用户指定。

不过，Windows Hello 不仅仅是传统 2FA 系统的替代品。 它在概念上类似于智能卡：通过使用加密基元而不是字符串比较来执行身份验证，并且用户的密钥材料在防篡改的硬件内很安全。 Microsoft Hello 也不需要智能卡部署所需的额外基础结构组件。 尤其是，无需公钥基础结构 (PKI) 即可管理证书（如果你当前没有）。 Windows Hello 继承了智能卡的主要优点（虚拟智能卡的部署灵活性以及物理智能卡的强大安全性），而摒弃了其所有缺点。

设备必须先向 Windows Hello 注册，然后用户才能使用它进行身份验证。 Windows Hello 使用非对称（公钥/私钥）加密，其中一方使用公钥对另一方可以使用私钥解密的数据进行加密。 Windows Hello 创建一组公钥/私钥对，并且将私钥写入设备的受信任的平台模块 (TPM) 芯片。 设备注册后，UWP 应用可以调用系统 API 检索可用于在服务器上注册用户的用户公钥。

应用的注册工作流可能如下所示：

![Windows Hello 注册](images/secure-passport.png)

你收集的注册信息可能比在此简单方案中要包括更多的标识信息。 例如，如果你的应用访问受保护的服务（如银行服务），你需要在注册过程中请求标识证明和其他内容。 满足所有条件后，此用户的公钥将存储在后端，并用于在用户下次使用该服务时进行验证。

有关 Windows Hello 的详细信息，请参阅 [Windows Hello 指南](https://msdn.microsoft.com/library/mt589441)和 [Windows Hello 开发人员指南](microsoft-passport.md)。

## <a name="3-data-in-flight-security-methods"></a>3 未送达数据安全方法


未送达数据安全方法适用于在连接到网络的设备之间传输的数据。 该数据可能在高度安全的专用企业 Intranet 环境的系统之间传输，也可能在不安全的 Web 环境中的客户端和 Web 服务之间传输。 Windows 10 应用通过其网络 API 支持 SSL 等标准，并且使用 Azure API 管理等技术（开发人员可以通过这些技术确保其应用的相应安全级别）。

## <a name="31-remote-system-authentication"></a>3.1 远程系统身份验证


与远程计算机系统发生通信的常规方案有两种。

-   本地服务器通过直接连接对用户进行身份验证。 例如，当服务器和客户端位于公司 Intranet 上时。
-   通过 Internet 与 Web 服务通信。

Web 服务通信的安全要求比直接连接方案中的安全要求要高，因为数据不再仅是安全网络的一部分，并且恶意攻击者伺机截获数据的可能性也更高。 因为各类设备都会访问服务，例如与 WCF 相比，它们可能会生成为 RESTful 服务，这意味着服务的身份验证和授权还会引入新挑战。 我们将谈论安全的远程系统通信的两个要求。

第一个要求是消息保密性：在客户端和 Web 服务器之间传递的信息（例如用户的身份和其他个人信息）在传输时不得被第三方读取。 满足此要求的方式通常是加密发送消息所使用的连接和加密消息本身。 在私钥/公钥加密中，公钥可供所有人使用，并用于加密要发送到特定接收方的消息。 私钥仅由接收方保留，并且用于解密消息。

第二个要求是消息完整性：客户端和 Web 服务必须能够验证他们接收的消息是其他方试图发送的消息，并且该消息未在传输过程中发生更改。 通过使用数字签名对消息进行签名并使用证书身份验证，可完成此操作。

## <a name="32-ssl-connections"></a>3.2 SSL 连接


为了建立和维护到客户端的安全连接，Web 服务可使用安全套接字层 (SSL)，后者受安全超文本传输协议 (HTTPS) 支持。 SSL 通过支持公钥加密与服务器证书来为消息提供保密性和完整性。 SSL 被传输层安全 (TLS) 所取代，但 TLS 通常却被随意称为 SSL。

当客户端请求访问服务器上的资源时，SSL 会使用服务器启动协商过程。 这称为 SSL 握手。 同意加密级别、一组公钥和私钥加密密钥以及客户端和服务器证书中的标识信息作为 SSL 连接的持续时间内所有通信的基础。 服务器可能还要求在此时对客户端进行身份验证。 建立连接后，使用经协商的公钥加密所有消息，直到连接关闭。

## <a name="321-ssl-pinning"></a>3.2.1 SSL 固定


虽然 SSL 可以使用加密和证书提供消息保密性，但它对验证与客户端通信的服务器是否是正确的服务器却不执行任何操作。 未经授权的第三方可以模拟服务器的行为，从而截获客户端传输的敏感数据。 若要避免此问题，可以使用一种称为 SSL 固定的技术来验证服务器上的证书是否是客户端预期和信任的证书。

可使用几种不同方法在应用中实现 SSL 固定，每个方法都各有利弊。 最简单的方法是通过应用的程序包清单中的证书声明。 此声明支持应用包安装数字证书并向它们指定独占信任。 这导致仅在其证书链中拥有相应证书的应用和服务器之间允许 SSL 连接。 此机制还支持安全使用自签名证书，因为受信任的公共证书颁发机构无需第三方依赖关系。

![ssl 清单](images/secure-ssl-manifest.png)

有关对验证逻辑的更多控制，API 可用于验证服务器返回的证书，以响应 HTTPS 请求。 请注意，此方法需要发送请求和检查响应，因此请确保在请求中实际发送敏感信息前将其添加为验证。

以下 C# 代码演示此 SSL 固定方法。 **ValidateSSLRoot** 方法使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 类执行 HTTP 请求。 客户端发送响应后，它使用 [**RequestMessage.TransportInformation.ServerIntermediateCertificates**](https://msdn.microsoft.com/library/windows/apps/dn279681) 集合检查服务器返回的证书。 然后客户端可以使用它所包括的指纹来验证整个证书链。 在服务器证书过期并续订后，此方法确实需要在应用中更新证书指纹。

```cs
private async Task ValidateSSLRoot()
{
    // Send a get request to Bing
    var httpClient = new HttpClient();
    var bingUri = new Uri("https://www.bing.com");
    HttpResponseMessage response = 
        await httpClient.GetAsync(bingUri);

    // Get the list of certificates that were used to
    // validate the server's identity
    IReadOnlyList<Certificate> serverCertificates = response.RequestMessage.TransportInformation.ServerIntermediateCertificates;
  
    // Perform validation
    if (!ValidateCertificates(serverCertificates))
    {
        // Close connection as chain is not valid
        return;
    }
    // Validation passed, continue with connection to service
}

private bool ValidateCertificates(IReadOnlyList<Certificate> certs)
{
    // In this example, we iterate through the certificates
    // and check that the chain contains
    // one specific certificate we are expecting
    foreach (var cert in certs)
    {
        byte[] thumbprint = cert.GetHashValue();

        // Check if the thumbprint matches whatever you 
        // are expecting
        var expected = new byte[] { 212, 222, 32, 208, 94, 102, 
            252, 83, 254, 26, 80, 136, 44, 120, 219, 40, 82, 202, 
            228, 116 };

        // ThumbprintMatches does the byte[] comparison 
        if (ThumbprintMatches(thumbprint, expected))
        {
            return true;
        }
    }
    return false;
}
```

## <a name="33-publishing-and-securing-access-to-rest-apis"></a>3.3 发布和保护对 REST API 的访问


若要确保对 Web 服务的访问经过授权，它们必须在每次调用 API 时要求身份验证。 能够控制性能和可扩展性也是在 Web 上公开 Web 服务时要考虑的事情。 Azure API 管理是一项在提供三个级别功能的同时可以帮助在 Web 上公开 API 的服务。

API 的 **Publishers/Administrators** 可以通过 Azure API 管理的发布者门户轻松配置 API。 可以在此处创建 API 集，并且可以通过管理对它们的访问权限来控制谁有权访问哪些 API。

要访问这些 API 的 **Developers** 可以通过开发人员门户提出请求，这可以立即提供访问权限，或需要获取发布者/管理员的批准。 开发人员也可以在开发人员门户中查看 API 文档和示例代码，以快速采用 Web 服务提供的 API。

开发人员先创建 **apps**，然后通过 Azure API 管理提供的代理访问 API。 代理可以提供隐匿层以在发布者/管理员服务器上隐藏 API 的实际终结点，还可以包括其他逻辑（例如 API 转换）以确保在一个 API 的调用重定向到另一个 API 时，公开的 API 保持一致。 它还可以使用 IP 筛选来阻止来源于特定 IP 域或域组的 API 调用。 Azure API 管理还通过使用一组称为 API 密钥的公钥来对每个 API 调用进行身份验证和授权，从而使其 Web 服务保持安全。 当授权失败时，将阻止对 API 的访问和它所支持的功能。

Azure API 管理还可以减少对某个服务的 API 调用数（称为限制的过程），以优化 Web 服务的性能。 若要了解详细信息，请参阅 [Azure API 管理](https://azure.microsoft.com/services/api-management/)和 [AzureCon 2015 上的 Azure API 管理](https://channel9.msdn.com/events/Microsoft-Azure/AzureCon-2015/ACON313)。

## <a name="4-data-at-rest-security-methods"></a>4 静态数据安全方法


当数据到达某个设备上时，我们将其称为“静态数据”。 此数据需要以安全方式存储在设备上，以便未经授权的用户或应用无法访问它。 Windows 10 中的应用模型执行许多操作来确保任何应用存储的数据仅可由该应用访问，同时提供用于在必要时共享该数据的 API。 还提供其他 API 来确保数据可以加密并且凭据可以安全存储。

## <a name="41-windows-app-model"></a>4.1 Windows 应用模型


传统上来讲，Windows 从未对应用下过定义。 它通常是指可执行文件 (.exe)，但从来不包括安装、状态存储、执行长度、版本控制、操作系统集成和应用到应用通信。 通用 Windows 平台模型定义涵盖安装、运行时环境、资源管理、更新、数据模型和卸载的应用模型。

Windows 10 应用中的容器，这意味着，它们的权限有限 （可以请求并由用户授予额外权限），默认情况下运行。 例如，如果某个应用想要在系统上访问文件，必须使用 [**Windows.Storage.Pickers**](https://msdn.microsoft.com/library/windows/apps/br207928) 命名空间的文件选取器才可以让用户选取某个文件（不支持任何对文件的直接访问权限）。 另一个示例是，如果某个应用想要访问用户的位置数据，它需要启用要声明的位置设备功能，从而在下载时提示用户此应用会请求访问用户的位置。 除此之外，应用首次想要访问用户位置时，会向用户显示请求访问数据的权限的额外许可提示。

请注意，此应用模型充当应用的“监狱”，这意味着无法访问它们，但它不是无法从外部访问的“城堡”（具有管理员权限的应用程序当然仍可以访问里面的内容）。 Windows 10 中的 Device Guard 因为支持组织/IT 指定允许执行哪些 (Win32) 应用，所以可以进一步帮助限制此访问权限。

该应用模型还管理应用生命周期。 例如，它默认限制在后台执行应用；只要应用进入后台，过程就将暂停（会给予应用一点时间在代码中处理应用暂停），并且内存也会被冻结。 操作系统会向应用提供要求特定后台任务执行的机制（这会发生在由 Internet/蓝牙连接、电源更换等各种事件触发的计划上和类似音乐播放和 GPS 跟踪等特定方案中）。

当设备上的内存资源不足时，Windows 将通过终止应用来释放内存空间。 此生命周期模型强制应用在每当它们暂停时保留数据，因为暂停和终止之间没有额外时间。

有关详细信息，请参阅[通用：了解 Windows 10 应用程序的生命周期](https://visualstudiomagazine.com/articles/2015/09/01/its-universal.aspx)。

## <a name="42-stored-credential-protection"></a>4.2 存储凭据保护


访问经身份验证的服务的 Windows 应用通常向用户提供将他们的凭据存储在本地设备上的选项。 这对于用户来说是一项便利；当他们提供用户名和密码时，应用将在应用的后续启动中自动使用它们。 由于如果攻击者获取此存储数据的访问权限，这可能成为安全问题，因此 Windows 10 为 Windows 应用提供了将用户凭据存储在安全凭据保险箱中的功能。 应用调用凭据保险箱 API 来存储凭据并从保险箱进行检索，而不是将它们存储在应用的存储容器中。 凭据保险箱由操作系统管理，但访问权限仅限于存储它们的应用，从而为凭据存储提供了安全托管的解决方案。

当用户提供要存储的凭据时，应用使用 [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089) 命名空间中的 [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) 对象来获取对凭据保险箱的引用。 然后，它创建一个 [**PasswordCredential**](https://msdn.microsoft.com/library/windows/apps/br227061) 对象，其中包含 Windows 应用以及用户名和密码的标识符。 这会传递到 [**PasswordVault.Add**](https://msdn.microsoft.com/library/windows/apps/hh701231) 方法以将凭据存储在保险箱中。 以下 C# 代码示例演示如何执行此操作。

```cs
var vault = new PasswordVault();
vault.Add(new PasswordCredential("My App", username, password));
```

在以下 C# 代码示例中，应用通过调用 [**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) 对象的 [**FindAllByResource**](https://msdn.microsoft.com/library/windows/apps/br227083) 方法来请求对应于该应用的所有凭据。 如果返回多个凭据，它将提示用户输入其用户名。 如果凭据不在保险箱中，应用将提示用户输入它们。 然后用户将使用这些凭据登录服务器。

```cs
private string resourceName = "My App";
private string defaultUserName;

private void Login()
{
    PasswordCredential loginCredential = GetCredentialFromLocker();

    if (loginCredential != null)
    {
        // There is a credential stored in the locker.
        // Populate the Password property of the credential
        // for automatic login.
        loginCredential.RetrievePassword();
    }
    else
    {
        // There is no credential stored in the locker.
        // Display UI to get user credentials.
        loginCredential = GetLoginCredentialUI();
    }
    // Log the user in.
    ServerLogin(loginCredential.UserName, loginCredential.Password);
}

private PasswordCredential GetCredentialFromLocker()
{
    PasswordCredential credential = null;

    var vault = new PasswordVault();
    var credentialList = vault.FindAllByResource(resourceName);

    if (credentialList.Count == 1)
    {
        credential = credentialList[0];
    }
    else if (credentialList.Count > 0)
    {
        // When there are multiple usernames,
        // retrieve the default username. If one doesn't
        // exist, then display UI to have the user select
        // a default username.
        defaultUserName = GetDefaultUserNameUI();

        credential = vault.Retrieve(resourceName, defaultUserName);
    }
    return credential;
}
```

有关详细信息，请参阅[凭据保险箱](credential-locker.md)。

## <a name="43-stored-data-protection"></a>4.3 存储数据保护


当处理存储的数据（通常称为静态数据）时，进行加密可以防止未经授权的用户访问存储的数据。 加密数据的两个常见机制是使用对称密钥或使用非对称密钥。 但是，数据加密无法确保数据在发送它和存储它的时间之间不发生更改。 换言之，无法确保数据完整性。 使用消息验证码、哈希和数字签名是解决此问题的常见技术。

## <a name="431-data-encryption"></a>4.3.1 数据加密


使用对称加密，发送方和接收方均拥有相同的密钥，并使用它加密和解密数据。 此方法的困难之处是安全共享密钥，以便双方都知道它。

此问题的一个解答是非对称加密，即使用公钥/私钥对。 公钥可免费共享给任何要加密消息的人。 私钥会始终保密，以便只有你可以使用它解密数据。 允许发现公钥的常用技术是使用数字证书，也简称为证书。 除有关用户的信息或服务器的信息（例如名称、颁发者、电子邮件地址和国家/地区）外，证书还托管有关公钥的信息。

Windows 应用开发人员可以使用 [**SymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241537) 和 [**AsymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241478) 类在他们的 UWP 应用中实现对称和非对称加密。 此外，[**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) 类可用于加密和解密数据、对内容进行签名和验证数字签名。 应用还可以使用 [**Windows.Security.Cryptography.DataProtection**](https://msdn.microsoft.com/library/windows/apps/br241585) 命名空间中的 [**DataProtectionProvider**](https://msdn.microsoft.com/library/windows/apps/br241559) 类来加密和解密存储的本地数据。

## <a name="432-detecting-message-tampering-macs-hashes-and-signatures"></a>4.3.2 检测消息篡改（MAC、哈希和签名）


MAC 是通过使用对称密钥（称为密钥）产生的代码（或标记）或作为对 MAC 加密算法的输入的消息。 发送方和接收方在消息传输前商定密钥和算法。

MAC 以如下方式验证消息。

-   发送方通过使用密钥作为对 MAC 算法的输入来派生 MAC 标记。
-   发送方将 MAC 标记和消息发送给接收方。
-   发送方通过使用密钥和消息作为对 MAC 算法的输入来派生 MAC 标记。
-   接收方将它们的 MAC 标记与发送方的 MAC 标记进行比较。 如果它们是相同的，那么我们知道消息未被篡改。

![mac 验证](images/secure-macs.png)

Windows 应用可以实现 MAC 消息验证，方法是调用 [**MacAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241530) 类来生成密钥和调用 [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) 类来执行 MAC 加密算法。

## <a name="433-using-hashes"></a>4.3.3 使用哈希


哈希函数是一种加密算法，用于获取任意长度的数据块并返回固定大小的位字符串（称为哈希值）。 整个系列的哈希函数都可以执行此操作。

在上述消息传输方案中可以使用哈希值代替 MAC。 发送方发送哈希值和消息，接收方从发送方的哈希值和消息派生其自己的哈希值并比较这两个哈希值。 在 Windows 10 上运行的应用可以调用 [**HashAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241511) 类来枚举可用的哈希算法并运行其中一个算法。 [**CryptographicHash**](https://msdn.microsoft.com/library/windows/apps/br241498) 类表示哈希值。 [**CryptographicHash.GetValueAndReset**](https://msdn.microsoft.com/library/windows/apps/hh701376) 方法可用于重复对不同的数据进行哈希操作，而无需在每次使用时重新创建对象。 **CryptographicHash** 类的 Append 方法将新数据添加到缓冲区以进行哈希操作。 以下 C# 代码示例中显示了这一完整过程。

```cs
public void SampleReusableHash()
{
    // Create a string that contains the name of the
    // hashing algorithm to use.
    string strAlgName = HashAlgorithmNames.Sha512;

    // Create a HashAlgorithmProvider object.
    HashAlgorithmProvider objAlgProv = HashAlgorithmProvider.OpenAlgorithm(strAlgName);

    // Create a CryptographicHash object. This object can be reused to continually
    // hash new messages.
    CryptographicHash objHash = objAlgProv.CreateHash();

    // Hash message 1.
    string strMsg1 = "This is message 1";
    IBuffer buffMsg1 = CryptographicBuffer.ConvertStringToBinary(strMsg1, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg1);
    IBuffer buffHash1 = objHash.GetValueAndReset();

    // Hash message 2.
    string strMsg2 = "This is message 2";
    IBuffer buffMsg2 = CryptographicBuffer.ConvertStringToBinary(strMsg2, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg2);
    IBuffer buffHash2 = objHash.GetValueAndReset();

    // Convert the hashes to string values (for display);
    string strHash1 = CryptographicBuffer.EncodeToBase64String(buffHash1);
    string strHash2 = CryptographicBuffer.EncodeToBase64String(buffHash2);
}
```

## <a name="434-digital-signatures"></a>4.3.4 数字签名


数字签名存储消息的数据完整性使用与 MAC 身份验证类似的方法进行验证。 下面是数字签名工作流的运行方式。

-   发送方通过将消息用作对哈希算法的输入来派生一个哈希值（也称为摘要）。
-   接收方使用其私钥加密摘要。
-   发送方发送消息、加密摘要以及曾使用的哈希算法的名称。
-   接收方使用公钥来解密它所接收的加密摘要。 然后，它使用哈希算法来对消息进行哈希操作，从而创建它自己的摘要。 最后，接收方比较这两个摘要（它接收并解密的摘要和它创建的摘要）。 仅当两者匹配时，接收方才可以确保消息由私钥的持有人发送，从而确定他们是他们所声称的身份以及消息在传输过程中未经过更改。

![数字签名](images/secure-digital-signatures.png)

哈希算法非常快，因此甚至从较大的消息中都可以快速派生哈希值。 生成的哈希值的长度是任意的，并且可能比完整消息更短，因此使用公钥和私钥仅加密和解密摘要，而不是优化完整消息。

有关详细信息，请查看以下主题中的文章：[数字签名](https://msdn.microsoft.com/library/windows/desktop/aa381977)、[MAC、哈希以及签名](macs-hashes-and-signatures.md)和[加密](cryptography.md)。

## <a name="5-summary"></a>5 摘要


Windows 10 中的通用 Windows 平台提供许多种利用操作系统功能创建更多安全应用的方法。 在不同的身份验证方案（例如单因素、多重或使用 OAuth 标识提供程序的代理身份验证）中，存在 API 以减少最常见的身份验证挑战。 Windows Hello 提供了新的生物识别登录系统，可识别用户并主动挫败绕开正常标识的尝试。 它还提供永远无法泄露或在受信任的平台模块外使用的多层密钥和证书。 此外，可以选择使用证明标识密钥和证书来增加一层安全性。

为保护未送达数据，在可能使用 SSL 固定验证服务器的真实性的同时，存在 API 以通过 SSL 安全地与远程系统通信。 Azure API 管理可帮助以可控的安全方式发布 API，方法是使用提供 API 终结点的额外混淆的代理提供在 Web 上公开 API 的强大配置选项。 使用 API 密钥可以保护对这些 API 的访问，并且 API 调用可以限制为控制性能。

当数据到达设备时，在防止 Windows 应用模型以未经授权的方式访问其他应用的数据时，它会更好地控制如何安装、更新应用以及如何访问其数据。 凭据保险箱可以安全存储操作系统管理的用户凭据，而其他数据可以在设备上使用通用 Windows 平台提供的加密和哈希 API 进行保护。

## <a name="6-resources"></a>6. 资源


### <a name="61-how-to-articles"></a>6.1 操作方法文章

-   [身份验证和用户身份](authentication-and-user-identity.md)
-   [Windows Hello](microsoft-passport.md)
-   [凭据保险箱](credential-locker.md)
-   [Web 身份验证代理](web-authentication-broker.md)
-   [指纹生物识别](fingerprint-biometrics.md)
-   [智能卡](smart-cards.md)
-   [共享的证书](share-certificates.md)
-   [加密](cryptography.md)
-   [证书](certificates.md)
-   [加密密钥](cryptographic-keys.md)
-   [数据保护](data-protection.md)
-   [MAC、哈希以及签名](macs-hashes-and-signatures.md)
-   [有关加密的出口限制](export-restrictions-on-cryptography.md)
-   [常见的加密任务](common-cryptography-tasks.md)

### <a name="62-code-samples"></a>6.2 代码示例

-   [凭据保险箱](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/PasswordVault)
-   [凭据选取器](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/CredentialPicker)
-   [使用 Azure 登录锁定设备](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/DeviceLockdownAzureLogin)
-   [企业数据保护](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/EnterpriseDataProtection)
-   [KeyCredentialManager](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/KeyCredentialManager)
-   [智能卡](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/SmartCard)
-   [Web 帐户管理](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/WebAccountManagement)
-   [WebAuthenticationBroker](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/WebAuthenticationBroker)

### <a name="63-api-reference"></a>6.3 API 参考

-   [**Windows.Security.Authentication.OnlineId**](https://msdn.microsoft.com/library/windows/apps/hh701371)
-   [**Windows.Security.Authentication.Web**](https://msdn.microsoft.com/library/windows/apps/br227044)
-   [**Windows.Security.Authentication.Web.Core**](https://msdn.microsoft.com/library/windows/apps/dn921967)
-   [**Windows.Security.Authentication.Web.Provider**](https://msdn.microsoft.com/library/windows/apps/dn921965)
-   [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089)
-   [**Windows.Security.Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089)
-   [**Windows.Security.Credentials.UI**](https://msdn.microsoft.com/library/windows/apps/hh701356)
-   [**Windows.Security.Cryptography**](https://msdn.microsoft.com/library/windows/apps/br241404)
-   [**Windows.Security.Cryptography.Certificates**](https://msdn.microsoft.com/library/windows/apps/br241476)
-   [**Windows.Security.Cryptography.Core**](https://msdn.microsoft.com/library/windows/apps/br241547)
-   [**Windows.Security.Cryptography.DataProtection**](https://msdn.microsoft.com/library/windows/apps/br241585)
-   [**Windows.Security.ExchangeActiveSyncProvisioning**](https://msdn.microsoft.com/library/windows/apps/hh701506)
-   [**Windows.Security.EnterpriseData**](https://msdn.microsoft.com/library/windows/apps/dn279153)
