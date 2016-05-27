---
title: 通过 Web 帐户管理器连接到标识提供者
description: 本文将介绍如何通过新的 Windows 10 Web 帐户管理器 API，使用 AccountsSettingsPane 将你的通用 Windows 平台 (UWP) 应用连接到外部标识提供者，如 Microsoft 或 Facebook。 
author: awkoren
---
# 通过 Web 帐户管理器连接到标识提供者

本文将介绍如何通过新的 Windows 10 Web 帐户管理器 API，显示 AccountsSettingsPane 并将你的通用 Windows 平台 (UWP) 应用连接到外部标识提供者，如 Microsoft 或 Facebook。 你将了解如何请求用户的权限以使用其 Microsoft 帐户、获取访问令牌，并使用它来执行基本操作（如获取配置文件数据或将文件上传到他们的 OneDrive）。 相关步骤类似于通过支持 Web 帐户管理器的任何标识提供者来获取用户权限和访问权限。

> 注意：有关完整代码示例，请参阅 [GitHub 上的 WebAccountManagement 示例](http://go.microsoft.com/fwlink/p/?LinkId=620621)。

## 准备工作

首先，在 Visual Studio 中创建一个新的空白应用。 

第二，你需要将你的应用与应用商店关联，以便连接到标识提供者。 若要执行此操作，请右键单击你的项目、依次选择“应用商店”**** > “将应用与应用商店关联”****，然后按照向导的说明进行操作。 

第三，创建一个非常基本的 UI，由一个简单的 XAML 按钮和两个文本框组成。

```XML
<StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
    <Button x:Name="LoginButton" Content="Log in" Click="LoginButton_Click" />
    <TextBlock x:Name="UserIdTextBlock"/>
    <TextBlock x:Name="UserNameTextBlock"/>
</StackPanel>
```

并在代码隐藏中创建一个附加到按钮的事件处理程序：

```C#
private void LoginButton_Click(object sender, RoutedEventArgs e)
{   
}
```

最后，添加以下命名空间，以便你以后无需担心任何引用问题： 

```C#
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

## 显示 AccountSettingsPane

系统提供了一个名为 AccountSettingsPane 的内置用户界面，用于管理标识提供者和 Web 帐户。 你可以按如下方式显示它：

```C#
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    AccountsSettingsPane.Show(); 
}
```

如果你运行应用并单击“登录”按钮，应该会显示一个空窗口。 

![帐户设置窗格](images/tb-1.png)

窗格为空的原因是系统只提供了一个 UI shell，这取决于开发人员是否使用标识提供者以编程方式填充窗格。 

## 注册 AccountCommandsRequested

若要向窗格添加命令，请先注册 AccountCommandsRequested 事件处理程序。 当用户要求查看窗格时（例如，单击 XAML 按钮），此事件处理程序将告知系统来运行生成逻辑。 

在隐藏代码中，替代 OnNavigatedTo 和 OnNavigatedFrom 事件，并向它们添加以下代码： 

```C#
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested += BuildPaneAsync; 
}
```

```C#
protected override void OnNavigatedFrom(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested -= BuildPaneAsync; 
}
```

用户不会与帐户非常频繁地交互，因而以此种方式注册和取消注册事件处理程序有助于防止内存泄漏。 这样，当用户需要自定义窗格的可能性比较大时（例如，当他们在“设置”或“登录”页面时），自定义窗格只会在内存中。 

## 生成帐户设置窗格

只要显示 AccountSettingsPane，就会调用 BuildPaneAsync 方法。 这是我们放置代码来自定义窗格中显示的命令的位置。 

首先获取延迟。 这将告知系统延迟显示 AccountsSettingsPane，直到我们完成它的生成为止。

```C#
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    deferral.Complete(); 
}
```

接下来，使用 WebAuthenticationCoreManager.FindAccountProviderAsync 方法获取提供程序。 提供程序的 URL 因提供程序而异，并且可以在提供程序的文档中找到。 对于 Microsoft 帐户和 Azure Active Directory，网址为“https://login.microsoft.com”。 

```C#
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers"); 
        
    deferral.Complete(); 
}
```

请注意，我们还向可选 *authority* 参数传递字符串“consumers”。 这是因为 Microsoft 提供了两种不同类型的身份验证，即适用于“消费者”的 Microsoft 帐户 (MSA) 和适用于“组织”的 Azure Active Directory (AAD)。 “消费者”颁发机构允许提供程序知道我们对先前的选项感兴趣。

如果你正在开发一款企业应用，你可能需要改用 AAD 图形终结点。 有关如何执行此操作的详细信息，请参阅完整的 [GitHub 上的 WebAccountManagement 示例](http://go.microsoft.com/fwlink/p/?LinkId=620621)和 Azure 文档。 

最后，通过创建新的 WebAccountProviderCommand 将提供程序添加到 AccountsSettingsPane，如下所示： 

```C#
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

请注意，我们传递给新 WebAccountProviderCommand 的 GetMsaToken 方法尚不存在（我们将在下一步骤中生成该方法），因此当前可以将其作为空方法添加。

运行上述代码，你的窗格应当如下所示： 

![帐户设置窗格](images/tb-2.png)

### 请求令牌

在使“Microsoft 帐户”选项一直显示在 AccountsSettingsPane 中后，我们需要处理在用户选择它时发生的情况。 我们注册了 GetMsaToken 方法，以便在用户选择使用其 Microsoft 帐户登录时触发，这样我们会在此时获得令牌。 

若要获取令牌，请使用 RequestTokenAsync 方法，如下所示： 

```C#
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

在此示例中，我们向作用域参数传递字符串“wl.basic”。 作用域表示你正在从提供给特定用户的服务请求的信息类型。 某些作用域仅提供对用户基本信息（如姓名和电子邮件地址）的访问权限。 其他作用域可能会授予对敏感信息（如用户的照片或电子邮件收件箱）的访问权限。 通常情况下，应用应使用最少的许可作用域，除非我们的应用明确需要其他权限，例如，如果你的应用并不是一定需要敏感信息的访问权限，就不要请求该权限。 

有关需要指定哪些作用域才能获得用以与它们的服务结合使用的令牌，服务提供程序将提供相关文档。 

有关 Office 365 和 Outlook.com 作用域，请参阅（使用 v2.0 身份验证终结点对 Office 365 和 Outlook.com API 进行身份验证）[https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2]。 

有关 OneDrive，请参阅（OneDrive 身份验证和登录）[https://dev.onedrive.com/auth/msa_oauth.htm#authentication-scopes]。 

## 使用令牌

RequestTokenAsync 方法会返回一个 WebTokenRequestResult 对象，它包含你请求的结果。 如果你的请求成功，它将包含一个令牌。  

```C#
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

在获得令牌之后，可以使用它来调用提供程序的 API。 在下面的代码中，我们将调用 Microsoft Live API，以获取有关用户的基本信息并将其显示在我们的 UI 中。 

```C#
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

## 保存帐户状态

令牌可用于立即获取有关用户的信息，但它们通常具有不同的生命期，例如，MSA 令牌的有效期只有几个小时。 幸运的是，每当令牌过期时，你不需要重新显示 AccountsSettingsPane。 在用户对你的应用进行一次授权后，你可以存储用户的帐户信息，以供将来使用。 

若要执行此操作，请使用 WebAccount 类。 请求令牌时会返回一个 WebAccount：

```C#
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

在获得 WebAccount 后，你可以轻松地存储它。 在下面的示例中，我们使用 LocalSettings： 

```C#
private async void StoreWebAccount(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"] = account.WebAccountProvider.Id;
    ApplicationData.Current.LocalSettings.Values["CurrentUserId"] = account.Id; 
}
```

当用户下次启动你的应用时，你可以尝试以静默方式获取令牌（在后台），如下所示： 

```C#
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

由于采用静默方式获取令牌非常简单，所以你应该使用此过程来刷新会话之间的令牌，而不是缓存现有的令牌（因为令牌可能会随时过期）。

请注意，上述示例中仅介绍了基本的成功和失败情况。 你的应用还应当考虑特殊情况（例如，用户吊销你的应用的权限或者从 Windows 中删除他们的帐户），并进行合理处理。  

## 注销帐户 

如果你坚持使用 WebAccount，你可能需要向用户提供“注销”功能，以便他们可以切换帐户或只是取消其帐户与你的应用之间的关联。 若要执行此操作，首先要删除任何保存的帐户和提供程序信息。 然后，调用 WebAccount.SignOutAsync() 清除缓存，并使你的应用可能拥有的任何现有令牌失效。 

```C#
private async Task SignOutAccountAsync(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserProviderId");
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserId"); 
    account.SignOutAsync(); 
}
```

## 添加不支持 WebAccountManager 的提供程序

如果你想要将身份验证从某个服务集成到你的应用中，但该服务不支持 WebAccountManager（例如 Google+ 或 Twitter），你仍可以将该提供程序手动添加到 AccountsSettingsPane。 若要执行此操作，请创建一个新的 WebAccountProvider 对象，并提供自己的名称和 .png 图标，然后将其添加到 WebAccountProviderCommands。 下面是一些存根代码： 

 ```C#
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

请注意，这只会将图标添加到 AccountsSettingsPane 并在单击该图标时运行你指定的方法（在本例中为 GetTwitterTokenAsync）。 必须提供用于处理实际身份验证的代码。 有关详细信息，请参阅（Web 身份验证代理）[web-authentication-broker]，它提供了使用 REST 服务进行身份验证的帮助程序方法。 

## 添加自定义标题

你可以使用 HeaderText 属性自定义帐户设置窗格，如下所示： 

```C#
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    args.HeaderText = "MyAwesomeApp works best if you're signed in.";   
    
    // other code here
}
```

![帐户设置窗格](images/tb-3.png)

标题文本不要过于冗长；保持简洁明了。 如果你的登录过程很复杂并且需要显示详细信息，请使用自定义链接将用户链接到单独的页面。 

## 添加自定义链接

你可以向 AccountsSettingsPane 添加自定义命令，它们会在受支持的 WebAccountProviders 下方显示为链接。 自定义命令非常适合与用户帐户相关的简单任务，如显示隐私策略或为遇到问题的用户启动支持页面。 

下面是一个示例： 

```C#
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

## 另请参阅

[Windows.Security.Authentication.Web.Core 命名空间](https://msdn.microsoft.com/library/windows/apps/windows.security.authentication.web.core.aspx)

[Windows.Security.Credentials 命名空间](https://msdn.microsoft.com/library/windows/apps/windows.security.credentials.aspx)

[AccountsSettingsPane](https://msdn.microsoft.com/library/windows/apps/windows.ui.applicationsettings.accountssettingspane)

[Web 身份验证代理](web-authentication-broker.md)

[WebAccountManagement 示例](http://go.microsoft.com/fwlink/p/?LinkId=620621)


<!--HONumber=May16_HO2-->


