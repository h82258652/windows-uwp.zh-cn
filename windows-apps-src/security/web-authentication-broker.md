---
title: Web 身份验证代理
description: 本文介绍了如何将通用 Windows 平台 (UWP) 应用连接到使用身份验证协议（如 OpenID 或 OAuth）的联机标识提供商（如 Facebook、Twitter、Flickr、Instagram 等）。
ms.assetid: 05F06961-1768-44A7-B185-BCDB74488F85
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp 安全
ms.localizationpriority: medium
ms.openlocfilehash: d354f0babec3ec2346c6e76fcae8666f40f3f6be
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2018
ms.locfileid: "3665902"
---
# <a name="web-authentication-broker"></a>Web 身份验证代理




本文介绍了如何将通用 Windows 平台 (UWP) 应用连接到使用身份验证协议（如 OpenID 或 OAuth）的联机标识提供商（如 Facebook、Twitter、Flickr、Instagram 等）。 [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066) 方法将请求发送给联机标识提供者，并取回描述应用有权访问的提供者资源的访问令牌。

>[!NOTE]
>有关完整的有效代码示例，请克隆 [GitHub 上的 WebAuthenticationBroker 存储库](http://go.microsoft.com/fwlink/p/?LinkId=620622)。

 

## <a name="register-your-app-with-your-online-provider"></a>向联机提供商注册应用


你必须将你的应用注册到要连接到的联机标识提供商。 可从标识提供商处找到如何注册应用的信息。 注册后，联机提供商通常会向你提供应用的 ID 或私钥。

## <a name="build-the-authentication-request-uri"></a>生成身份验证请求 URI


请求 URI 由以下内容组成：你将身份验证请求发送到联机提供商的地址，其中附有其他所需信息，例如应用 ID 或机密信息、用户在完成身份验证后发送的重定向 URI 以及预期响应类型。 你可以从你的提供商处找到需要的参数。

请求 URI 作为 [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066) 方法的 *requestUri* 参数进行发送。 必须是安全地址（即，必须以 `https://` 开头）

下例介绍如何建立请求 URI。

```cs
string startURL = "https://<providerendpoint>?client_id=<clientid>&scope=<scopes>&response_type=token";
string endURL = "http://<appendpoint>";

System.Uri startURI = new System.Uri(startURL);
System.Uri endURI = new System.Uri(endURL);
```

## <a name="connect-to-the-online-provider"></a>连接到联机提供商


调用 [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066) 方法以连接到联机标识提供者并获取访问令牌。 该方法将上一步骤中构建的 URI 用作 *requestUri* 参数，并将你希望用户被重定向到的 URI 用作 *callbackUri* 参数。

```cs
string result;

try
{
    var webAuthenticationResult = 
        await Windows.Security.Authentication.Web.WebAuthenticationBroker.AuthenticateAsync( 
        Windows.Security.Authentication.Web.WebAuthenticationOptions.None, 
        startURI, 
        endURI);

    switch (webAuthenticationResult.ResponseStatus)
    {
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.Success:
            // Successful authentication. 
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.ErrorHttp:
            // HTTP error. 
            result = webAuthenticationResult.ResponseErrorDetail.ToString(); 
            break;
        default:
            // Other error.
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
    } 
}
catch (Exception ex)
{
    // Authentication failed. Handle parameter, SSL/TLS, and Network Unavailable errors here. 
    result = ex.Message;
}
```

>[!WARNING]
>除 [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066) 之外，[**Windows.Security.Authentication.Web**](https://msdn.microsoft.com/library/windows/apps/br227044) 命名空间还包括 [**AuthenticateAndContinue**](https://msdn.microsoft.com/library/windows/apps/dn632425) 方法。 请勿调用此方法。 它仅为面向 Windows Phone 8.1 的应用设计，并从 Windows 10 开始已启用。

## <a name="connecting-with-single-sign-on-sso"></a>使用单一登录 (SSO) 连接。


默认情况下，Web 身份验证代理不允许保留 cookie。 正因为如此，所以即使应用用户指示他们希望保留登录状态（例如，通过选择提供商的登录对话框中的复选框），他们也必须在每次希望访问该提供商的资源时登录。 若要使用 SSO 登录，则联机标识提供者必须为 Web 身份验证代理启用 SSO，而你的应用必须调用未获取 *callbackUri* 参数的 [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212068) 的重载。 借此，可通过 Web 身份验证代理存储永久 Cookie，同一应用将来进行身份验证调用时将无需要求用户重复登录（用户的有效“登录”将可持续至访问令牌过期）。

若要支持 SSO，联机提供商必须允许你采用 `ms-app://<appSID>` 形式注册重定向 URI，其中 `<appSID>` 为应用的 SID。 可从应用开发人员页面中找到应用的 SID，或通过调用 [**GetCurrentApplicationCallbackUri**](https://msdn.microsoft.com/library/windows/apps/br212069) 方法达到此目的。

```cs
string result;

try
{
    var webAuthenticationResult = 
        await Windows.Security.Authentication.Web.WebAuthenticationBroker.AuthenticateAsync( 
        Windows.Security.Authentication.Web.WebAuthenticationOptions.None, 
        startURI);

    switch (webAuthenticationResult.ResponseStatus)
    {
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.Success:
            // Successful authentication. 
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.ErrorHttp:
            // HTTP error. 
            result = webAuthenticationResult.ResponseErrorDetail.ToString(); 
            break;
        default:
            // Other error.
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
    } 
}
catch (Exception ex)
{
    // Authentication failed. Handle parameter, SSL/TLS, and Network Unavailable errors here. 
    result = ex.Message;
}
```

## <a name="debugging"></a>调试


有多种方法对 Web 身份验证代理 API 进行疑难解答，包括查看操作日志和使用 Fiddler 查看 Web 请求和响应。

### <a name="operational-logs"></a>操作日志

操作日志通常用来确定哪些内容不工作。 有一个专门的事件日志通道 Microsoft-Windows-WebAuth\\Operational，该通道允许网站开发人员了解 Web 身份验证代理正在如何处理其网页。 为了启用操作日志，请启动 eventvwr.exe 并在“Application and Services\\Microsoft\\Windows\\WebAuth“下启用 Operational 日志。 此外，Web 身份验证代理还在用户代理字符串后面附加一个用来在 Web 服务器上标识其本身的唯一字符串。 该字符串为“MSAuthHost/1.0”。 请注意，版本号可能会在将来更改，因此，不应当在代码中依赖该版本号。 如下所示是带有全部调试步骤的完整用户代理字符串示例。

`User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; Win64; x64; Trident/6.0; MSAuthHost/1.0)`

1.  启用操作日志。
2.  运行 Contoso 社交应用程序。 ![显示 WebAuth 操作日志的事件查看器](images/wab-event-viewer-1.png)
3.  所生成的日志条目可用来更详细地了解 Web 身份验证代理的行为。 在本例中，这些行为包括：
    -   导航开始：记录 AuthHost 何时启动，包含有关起始和终止 URL 的信息。
    -   ![阐释“导航开始”的详细信息](images/wab-event-viewer-2.png)
    -   导航完成：记录网页已完成加载。
    -   Meta 标记：记录何时遇到了 meta 标记（包括详细信息）。
    -   导航终止：导航由用户终止。
    -   导航错误：AuthHost 在某个 URL 处遇到一个导航错误（包括 HttpStatusCode）。
    -   导航结束：遇到了终止 URL。

### <a name="fiddler"></a>Fiddler

Fiddler Web 调试程序可与应用一起使用。

1.  由于 AuthHost 在自己的应用容器中运行，如果要为其指定的私有网络功能必须先设置注册表项： Windows 注册表编辑器版本 5.00

    **HKEY\_LOCAL\_MACHINE**\\**SOFTWARE**\\**Microsoft**\\**Windows NT**\\**CurrentVersion**\\**Image File Execution Options**\\**authhost.exe**\\**EnablePrivateNetwork** = 00000001

    如果此注册表项，你可以在具有管理员权限的命令提示符中创建。

    ```cmd 
    REG ADD "HKLM\Software\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\authhost.exe" /v EnablePrivateNetwork /t REG_DWORD /d 1 /f
    ```

2.  为 AuthHost 添加规则，因为这是出站流量的来源。
    ```syntax
    CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.windows.authhost.a.p_8wekyb3d8bbwe
    CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.windows.authhost.sso.p_8wekyb3d8bbwe
    CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.windows.authhost.sso.c_8wekyb3d8bbwe
    D:\Windows\System32>CheckNetIsolation.exe LoopbackExempt -s
    List Loopback Exempted AppContainers
    [1] -----------------------------------------------------------------
        Name: microsoft.windows.authhost.sso.c_8wekyb3d8bbwe
        SID:  S-1-15-2-1973105767-3975693666-32999980-3747492175-1074076486-3102532000-500629349
    [2] -----------------------------------------------------------------
        Name: microsoft.windows.authhost.sso.p_8wekyb3d8bbwe
        SID:  S-1-15-2-166260-4150837609-3669066492-3071230600-3743290616-3683681078-2492089544
    [3] -----------------------------------------------------------------
        Name: microsoft.windows.authhost.a.p_8wekyb3d8bbwe
        SID:  S-1-15-2-3506084497-1208594716-3384433646-2514033508-1838198150-1980605558-3480344935
    ```

3.  针对传入 Fiddler 的流量添加防火墙规则。