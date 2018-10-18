---
title: Web 帐户管理器
description: 本文介绍如何通过 Windows 10 Web 帐户管理器 API，使用 AccountsSettingsPane 将通用 Windows 平台 (UWP) 应用连接到外部标识提供者，如 Microsoft 或 Facebook。
author: PatrickFarley
ms.author: pafarley
ms.date: 12/6/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp 安全
ms.assetid: ec9293a1-237d-47b4-bcde-18112586241a
ms.localizationpriority: medium
ms.openlocfilehash: 2de5c969610aa6b4fa1a3af01af565d35854b5f2
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2018
ms.locfileid: "4752070"
---
# <a name="web-account-manager"></a>Web 帐户管理器

本文介绍如何通过 Windows 10 Web 帐户管理器 API，使用 **[AccountsSettingsPane](https://docs.microsoft.com/uwp/api/Windows.UI.ApplicationSettings.AccountsSettingsPane)** 将通用 Windows 平台 (UWP) 应用连接到外部标识提供者，如 Microsoft 或 Facebook。 你将了解如何请求用户的权限以使用其 Microsoft 帐户、获取访问令牌，并使用它来执行基本操作（如获取配置文件数据或将文件上传到他们的 OneDrive 帐户）。 相关步骤类似于通过支持 Web 帐户管理器的任何标识提供者来获取用户权限和访问权限。

> [!NOTE]
> 有关完整代码示例，请参阅 [GitHub 上的 WebAccountManagement 示例](http://go.microsoft.com/fwlink/p/?LinkId=620621)。

## <a name="get-set-up"></a>准备工作

首先，在 Visual Studio 中创建一个新的空白应用。 

第二，你需要将你的应用与应用商店关联，以便连接到标识提供者。 若要执行此操作，请右键单击你的项目、依次选择**应用商店** > **将应用与应用商店关联**，然后按照向导的说明进行操作。 

第三，创建一个非常基本的 UI，由一个简单的 XAML 按钮和两个文本框组成。

```XML
<StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
    <Button x:Name="LoginButton" Content="Log in" Click="LoginButton_Click" />
    <TextBlock x:Name="UserIdTextBlock"/>
    <TextBlock x:Name="UserNameTextBlock"/>
</StackPanel>
```

并在代码隐藏中创建一个附加到按钮的事件处理程序：

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{   
}
```

最后，添加以下命名空间，以便你以后无需担心任何引用问题： 

```csharp
using System;
using Windows.Security.Authentication.Web.Core;
using Windows.System;
using Windows.UI.ApplicationSettings;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.Data.Json;
using Windows.UI.Xaml.Navigation;
using Windows.Web.Http;
```

## <a name="show-the-accounts-settings-pane"></a>显示帐户设置窗格

系统提供了一个名为 **AccountsSettingsPane** 的内置用户界面，用于管理标识提供者和 Web 帐户。 你可以按如下方式显示它：

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    AccountsSettingsPane.Show(); 
}
```

如果你运行应用并单击“登录”按钮，应该会显示一个空窗口。 

![帐户设置窗格](images/tb-1.png)

窗格为空的原因是系统只提供了一个 UI shell，这取决于开发人员是否使用标识提供者以编程方式填充窗格。 

## <a name="register-for-accountcommandsrequested"></a>注册 AccountCommandsRequested

若要向窗格添加命令，请先注册 AccountCommandsRequested 事件处理程序。 当用户要求查看窗格时（例如，单击 XAML 按钮），此事件处理程序将告知系统来运行生成逻辑。 

在隐藏代码中，替代 OnNavigatedTo 和 OnNavigatedFrom 事件，并向它们添加以下代码： 

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested += BuildPaneAsync; 
}
```

```csharp
protected override void OnNavigatedFrom(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested -= BuildPaneAsync; 
}
```

用户不会与帐户非常频繁地交互，因而以此种方式注册和取消注册事件处理程序有助于防止内存泄漏。 这样，当用户需要自定义窗格的可能性比较大时（例如，当他们在“设置”或“登录”页面时），自定义窗格只会在内存中。 

## <a name="build-the-account-settings-pane"></a>生成帐户设置窗格

只要显示 **AccountsSettingsPane**，就会调用 BuildPaneAsync 方法。 这是我们放置代码来自定义窗格中显示的命令的位置。 

首先获取延迟。 这将告知系统延迟显示 **AccountsSettingsPane**，直到我们完成它的生成为止。

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    deferral.Complete(); 
}
```

接下来，使用 WebAuthenticationCoreManager.FindAccountProviderAsync 方法获取提供程序。 提供程序的 URL 因提供程序而异，并且可以在提供程序的文档中找到。 对于 Microsoft 帐户和 Azure Active Directory，应为“https://login.microsoft.com”。 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers"); 
        
    deferral.Complete(); 
}
```

请注意，我们还向可选 *authority* 参数传递字符串“consumers”。 这是因为 Microsoft 提供了两种不同类型的身份验证，即适用于“消费者”的 Microsoft 帐户 (MSA) 和适用于“组织”的 Azure Active Directory (AAD)。 “consumers”颁发机构指示我们需要 MSA 选项。 如果在开发一款企业应用，请使用“organizations”字符串。

最后，通过新建 **[WebAccountProviderCommand](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings.webaccountprovidercommand)** 将提供商添加到 **AccountsSettingsPane**，如下所示： 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();

    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers");

    var command = new WebAccountProviderCommand(msaProvider, GetMsaTokenAsync);  

    e.WebAccountProviderCommands.Add(command);

    deferral.Complete(); 
}
```

我们传递给新 **WebAccountProviderCommand** 的 GetMsaToken 方法尚不存在（我们将在下一步骤中生成该方法），因此当前可以将其作为空方法添加。

运行上述代码，你的窗格应当如下所示： 

![帐户设置窗格](images/tb-2.png)

### <a name="request-a-token"></a>请求令牌

“Microsoft 帐户”选项显示在 **AccountsSettingsPane** 中后，我们需要处理用户选择它时发生的情况。 我们注册了 GetMsaToken 方法，以便在用户选择使用其 Microsoft 帐户登录时触发，这样我们会在此时获得令牌。 

若要获取令牌，请使用 RequestTokenAsync 方法，如下所示： 

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

在此示例中，我们向_作用域_参数传递字符串“wl.basic”。 作用域表示你正在从提供给特定用户的服务请求的信息类型。 某些作用域仅提供对用户基本信息的访问权限，例如姓名和电子邮件地址，而其他作用域可能允许访问敏感信息，如用户的照片或电子邮件收件箱。 通常，应用应该至少使用实现其功能所必需的最小许可作用域。 有关需要哪些作用域才能获得用以与它们的服务结合使用的令牌，服务提供商将提供相关文档。 

* 有关 Office 365 和 Outlook.com 的作用域，请参阅 [使用 v2.0 身份验证终结点对 Office 365 和 Outlook.com API 进行身份验证](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)。 
* 有关 OneDrive 的作用域，请参阅 [OneDrive 身份验证和登录](https://dev.onedrive.com/auth/msa_oauth.htm#authentication-scopes)。 

> [!TIP]
> （可选） 如果你的应用使用的登录提示 （若要填充默认电子邮件地址的用户字段） 或其他与登录体验相关的特殊属性，则列出**[WebTokenRequest.AppProperties](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webtokenrequest.appproperties#Windows_Security_Authentication_Web_Core_WebTokenRequest_AppProperties)** 属性中。 这将导致系统缓存的 web 帐户，可以防止在缓存中的帐户不匹配时忽略此属性。

如果在开发企业应用，可能需要连接到 Azure Active Directory (AAD) 实例，并使用 Microsoft Graph API，而非常规的 MSA 服务。 在此方案中，使用以下代码： 

```csharp
private async void GetAadTokenAsync(WebAccountProviderCommand command)
{
    string clientId = "your_guid_here"; // Obtain your clientId from the Azure Portal
    WebTokenRequest request = new WebTokenRequest(provider, "User.Read", clientId);
    request.Properties.Add("resource", "https://graph.microsoft.com");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

本文的剩余部分将继续介绍 MSA 方案，但 AAD 代码非常相似。 有关 AAD/Graph 的详细信息，包括 GitHub 上的完整示例，请参阅 [Microsoft Graph 文档](https://graph.microsoft.io/docs/platform/get-started)。

## <a name="use-the-token"></a>使用令牌

RequestTokenAsync 方法会返回一个 WebTokenRequestResult 对象，它包含你请求的结果。 如果请求成功，它将包含一个令牌。  

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        string token = result.ResponseData[0].Token; 
    }
}
```

> [!NOTE]
> 如果在请求令牌时收到错误，请确保已将应用与 Microsoft Store 关联，如步骤一中所述。 如果你跳过此步骤，你的应用将无法获取令牌。 

获得令牌后，可以使用它来调用提供商的 API。 在以下代码中，我们将调用[用户信息 Microsoft Live API](https://msdn.microsoft.com/library/hh826533.aspx)，以获取有关用户的基本信息并将其显示在我们的 UI 中。 但请注意，在大多数情况下，建议获取令牌后立即存储，然后以单独的方法使用该令牌。

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        string token = result.ResponseData[0].Token; 
        
        var restApi = new Uri(@"https://apis.live.net/v5.0/me?access_token=" + token);

        using (var client = new HttpClient())
        {
            var infoResult = await client.GetAsync(restApi);
            string content = await infoResult.Content.ReadAsStringAsync();

            var jsonObject = JsonObject.Parse(content);
            string id = jsonObject["id"].GetString();
            string name = jsonObject["name"].GetString();

            UserIdTextBlock.Text = "Id: " + id; 
            UserNameTextBlock.Text = "Name: " + name;
        }
    }
}
```

在提供程序之间，调用不同 REST API 的方式均不相同；有关如何使用令牌的信息，请参阅提供程序的 API 文档。 

## <a name="store-the-account-for-future-use"></a>存储帐户以供将来使用

令牌可用于立即获取有关用户的信息，但它们通常具有不同的生命期，例如，MSA 令牌的有效期只有几个小时。 幸运的是，每当令牌过期时，都不需要重新显示 **AccountsSettingsPane**。 在用户对你的应用进行一次授权后，你可以存储用户的帐户信息，以供将来使用。 

若要执行此操作，请使用 **[WebAccount](https://docs.microsoft.com/uwp/api/windows.security.credentials.webaccount)** 类。 将以用于请求令牌的相同方法返回 **WebAccount**：

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        WebAccount account = result.ResponseData[0].WebAccount; 
    }
}
```

获得 **WebAccount** 实例后，即可轻松存储它。 在下面的示例中，我们使用 LocalSettings。 有关使用 LocalSettings 和其他方法来存储用户数据的详细信息，请参阅[存储和检索应用设置和数据](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)。

```csharp
private async void StoreWebAccount(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"] = account.WebAccountProvider.Id;
    ApplicationData.Current.LocalSettings.Values["CurrentUserId"] = account.Id; 
}
```

然后，我们可以使用如下所示的异步方法，通过存储的 **WebAccount** 尝试在后台获取令牌。

```csharp
private async Task<string> GetTokenSilentlyAsync()
{
    string providerId = ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"]?.ToString();
    string accountId = ApplicationData.Current.LocalSettings.Values["CurrentUserId"]?.ToString();

    if (null == providerId || null == accountId)
    {
        return null; 
    }

    WebAccountProvider provider = await WebAuthenticationCoreManager.FindAccountProviderAsync(providerId);
    WebAccount account = await WebAuthenticationCoreManager.FindAccountAsync(provider, accountId);

    WebTokenRequest request = new WebTokenRequest(provider, "wl.basic");

    WebTokenRequestResult result = await WebAuthenticationCoreManager.GetTokenSilentlyAsync(request, account);
    if (result.ResponseStatus == WebTokenRequestStatus.UserInteractionRequired)
    {
        // Unable to get a token silently - you'll need to show the UI
        return null; 
    }
    else if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        // Success
        return result.ResponseData[0].Token;
    }
    else
    {
        // Other error 
        return null; 
    }
}
```

将上述方法置于构建 **AccountsSettingsPane** 的代码之前。 如果已在后台获取令牌，则不需要显示该窗格。 

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    string silentToken = await GetMsaTokenSilentlyAsync();

    if (silentToken != null)
    {
        // the token was obtained. store a reference to it or do something with it here.
    }
    else
    {
        // the token could not be obtained silently. Show the AccountsSettingsPane
        AccountsSettingsPane.Show();
    }
}
```

由于采用静默方式获取令牌非常简单，所以你应该使用此过程来刷新会话之间的令牌，而不是缓存现有的令牌（因为令牌可能会随时过期）。

> [!NOTE]
> 上述示例中仅介绍了基本的成功和失败情况。 你的应用还应当考虑特殊情况（例如，用户吊销你的应用的权限或者从 Windows 中删除他们的帐户），并进行合理处理。  

## <a name="remove-a-stored-account"></a>删除已存储帐户

如果保留 web 帐户，你可能希望用户能够解除他们的帐户与你的应用之间的关联。 这样，他们可以有效地“注销”应用：他们的帐户信息将不再在启动时自动加载。 若要执行此操作，首先从存储中删除任何保存的帐户和提供商信息。 然后，调用 **[SignOutAsync](https://docs.microsoft.com/uwp/api/windows.security.credentials.webaccount.SignOutAsync)** 清除缓存，并使应用可能拥有的任何现有令牌失效。 

```csharp
private async Task SignOutAccountAsync(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserProviderId");
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserId"); 
    account.SignOutAsync(); 
}
```

## <a name="add-providers-that-dont-support-webaccountmanager"></a>添加不支持 WebAccountManager 的提供程序

如果想要将身份验证从某个服务集成到应用中，但该服务不支持 WebAccountManager（例如 Google+ 或 Twitter），仍可以将该提供商手动添加到 **AccountsSettingsPane**。 若要执行此操作，请新建 WebAccountProvider 对象，并提供自己的名称和 .png 图标，然后将其添加到 WebAccountProviderCommands 列表。 下面是一些存根代码： 

 ```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 

    var twitterProvider = new WebAccountProvider("twitter", "Twitter", new Uri(@"ms-appx:///Assets/twitter-auth-icon.png")); 
    var twitterCmd = new WebAccountProviderCommand(twitterProvider, GetTwitterTokenAsync);
    e.WebAccountProviderCommands.Add(twitterCmd);   
    
    // other code here
}

private async void GetTwitterTokenAsync(WebAccountProviderCommand command)
{
    // Manually handle Twitter login here
}

```

> [!NOTE] 
> 这只会将图标添加到 **AccountsSettingsPane** 并在单击该图标时运行指定的方法（在本例中为 GetTwitterTokenAsync）。 必须提供用于处理实际身份验证的代码。 有关详细信息，请参阅（Web 身份验证代理）[web-authentication-broker]，它提供了使用 REST 服务进行身份验证的帮助程序方法。 

## <a name="add-a-custom-header"></a>添加自定义标题

你可以使用 HeaderText 属性自定义帐户设置窗格，如下所示： 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    args.HeaderText = "MyAwesomeApp works best if you're signed in.";   
    
    // other code here
}
```

![帐户设置窗格](images/tb-3.png)

标题文本不要过于冗长；保持简洁明了。 如果你的登录过程很复杂并且需要显示详细信息，请使用自定义链接将用户链接到单独的页面。 

## <a name="add-custom-links"></a>添加自定义链接

你可以向 AccountsSettingsPane 添加自定义命令，它们会在受支持的 WebAccountProviders 下方显示为链接。 自定义命令非常适合与用户帐户相关的简单任务，如显示隐私策略或为遇到问题的用户启动支持页面。 

下面是一个示例： 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    var settingsCmd = new SettingsCommand(
        "settings_privacy", 
        "Privacy policy", 
        async (x) => await Launcher.LaunchUriAsync(new Uri(@"https://privacy.microsoft.com/en-US/"))); 

    e.Commands.Add(settingsCmd); 
    
    // other code here
}
```

![帐户设置窗格](images/tb-4.png)

理论上，你可以对任何内容使用设置命令。 但是，我们建议将它们的使用范围限制在直观的与帐户相关的情况，如上文所述。 

## <a name="see-also"></a>另请参阅

[Windows.Security.Authentication.Web.Core 命名空间](https://msdn.microsoft.com/library/windows/apps/windows.security.authentication.web.core.aspx)

[Windows.Security.Credentials 命名空间](https://msdn.microsoft.com/library/windows/apps/windows.security.credentials.aspx)

[AccountsSettingsPane 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.applicationsettings.accountssettingspane)

[Web 身份验证代理](web-authentication-broker.md)

[Web 帐户管理示例](http://go.microsoft.com/fwlink/p/?LinkId=620621)

[Lunch Scheduler 应用](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
