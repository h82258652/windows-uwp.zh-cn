---
Description: The Windows Push Notification Services (WNS) enables third-party developers to send toast, tile, badge, and raw updates from their own cloud service. This provides a mechanism to deliver new updates to your users in a power-efficient and dependable way.
title: Windows 推送通知服务 (WNS) 概述
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 267e6e1cf9a004b6703e000b694274b802220f60
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2019
ms.locfileid: "9047522"
---
# <a name="windows-push-notification-services-wns-overview"></a>Windows 推送通知服务 (WNS) 概述
 

Windows 推送通知服务 (WNS) 使第三方开发人员可从自己的云服务发送 Toast、磁贴、锁屏提醒和原始更新。 这提供了一种高效而可靠地向用户提供新更新的机制。

## <a name="how-it-works"></a>工作原理


下图显示了用于发送推送通知的完整数据流。 它涉及到以下步骤：

1.  你的应用从通用 Windows 平台请求推送通知通道。
2.  Windows 要求 WNS 创建通知通道。 此通道以统一资源标识符 (URI) 的形式返回到调用设备。
3.  通知通道 URI 由 Windows 返回到应用。
4.  你的应用将 URI 发送到你自己的云服务。 然后你将 URI 存储在自己的云服务上，以便在发生通知时访问该 URI。 URI 是你自己的应用与自己的服务之间的接口；它负责通过安全可靠的 Web 标准来实现此接口。
5.  当你的云服务有要发送的更新时，它使用通道 URI 通知 WNS。 通过安全套接字层 (SSL) 发送 TTP POST 请求（包括通知负载）来执行此操作。 此步骤需要身份验证。
6.  WNS 接收请求，并将通知路由到相应的设备。

![推送通知的 WNS 数据流关系图](images/wns-diagram-01.png)

## <a name="registering-your-app-and-receiving-the-credentials-for-your-cloud-service"></a>注册你的应用，并为你的云服务接收凭据


在使用 WNS 发送通知之前，应用必须先向应用商店仪表板进行注册。 这将为应用提供凭据，云服务在向 WNS 进行验证的过程中将使用该凭据。 这些凭据由程序包安全标识符 (SID) 和密钥组成。 若要执行此注册，登录到[合作伙伴中心](https://partner.microsoft.com/dashboard)。 创建应用后，可以按照**应用管理 - WNS/MPNS** 页面上的说明检索凭证。 如果想使用 Live 服务解决方案，请访问此页面上的 **Live 服务网站**链接。

每个应用都有其各自的一组云服务凭据。 这些凭据无法用于向其他任何应用发送通知。

若要详细了解如何注册你的应用，请参阅[如何向 Windows 通知服务 (WNS) 进行验证](https://msdn.microsoft.com/library/windows/apps/hh465407)。

## <a name="requesting-a-notification-channel"></a>请求通知通道


当能够接收推送通知的应用运行时，它必须首先通过 [**CreatePushNotificationChannelForApplicationAsync**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager#Windows_Networking_PushNotifications_PushNotificationChannelManager_CreatePushNotificationChannelForApplicationAsync_System_String_) 请求通知通道。 若要查看全面介绍和示例代码，请参阅[如何请求、创建和保存通知通道](https://msdn.microsoft.com/library/windows/apps/hh465412)。 此 API 会返回一个唯一链接到进行调用的应用程序及其磁贴的通道 URI，所有通知类型均可通过此 URI 发送。

应用成功创建了通道 URI 之后，会将其与任何应该与该 URI 关联的特定于应用的元数据一起发送到它的云服务。

### <a name="important-notes"></a>重要说明

-   我们不保证应用的通知通道 URI 将始终保持相同。 我们建议应用在每次运行时均请求一个新的通道，并在 URI 更改时更新其服务。 开发人员绝不能修改该通道 URI，而应将其视作一段黑盒字符串。 此时，通道 URI 于 30 天后过期。 如果 windows 10 应用会定期更新其通道在后台，你可以下载适用于 windows 8.1[推送和定期通知示例](https://go.microsoft.com/fwlink/p/?linkid=231476)并重新使用其源代码和/或它演示了模式。
-   云服务和客户端应用之间的接口由你这个开发人员来实现。 我们建议应用使用其自身的服务完成身份验证过程，并通过安全的协议（如 HTTPS）来传输数据。
-   云服务必须始终确保通道 URI 使用域“notify.windows.com”。 该服务永远不应向任何其他域中的通道推送通知。 如果应用的回调发生了泄露，恶意攻击者可能会将该通道 URI 提交给假冒 WNS。 如果不对域进行检查，你的云服务可能会在你不知情的情况下向此攻击者泄露信息。
-   如果你的云服务尝试将通知传递到过期通道，WNS 将返回[响应代码 410](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#WNSResponseCodes)。 为响应此代码，你的服务不应再尝试将通知发送到该 URI。

## <a name="authenticating-your-cloud-service"></a>验证你的云服务


若要发送通知，云服务必须通过 WNS 进行验证。 此过程的第一步出现在使用 Microsoft Store 仪表板注册应用之时。 在注册过程中，应用会获得一个程序包安全标识符 (SID) 和一个密钥。 该信息由你的云服务用于向 WNS 进行验证。

WNS 身份验证方案通过来自 [OAuth 2.0](https://go.microsoft.com/fwlink/p/?linkid=226787) 协议的客户端凭据配置文件来实现。 云服务通过提供其凭据（程序包 SID 和密钥）来向 WNS 进行验证。 反过来，云服务会获得一个访问令牌。 该访问令牌允许云服务发送通知。 每次向 WNS 发送通知请求时都必须使用该令牌。

该信息链简述如下：

1.  云服务遵循 OAuth 2.0 协议通过 HTTPS 将其凭据发送给 WNS。 这会向 WNS 验证该服务。
2.  如果身份验证成功，则 WNS 会返回一个访问令牌。 此访问令牌在所有后续通知请求中使用，直到令牌过期。

![云服务身份验证的 WNS 关系图](images/wns-diagram-02.png)

在对 WNS 进行身份验证的过程中，云服务会通过安全套接字层 (SSL) 提交一个 HTTP 请求。 参数以“application/x-www-for-urlencoded”格式提供。 在“client_id”字段中提供你的程序包 SID，并在“client_secret”字段中提供你的密钥。 有关语法的详细信息，请参阅[访问令牌请求](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#access_token_request)参考。

**注意**这是只是一个示例中，你可以在自己的代码中成功使用的不剪切和粘贴代码。

 

``` http
 POST /accesstoken.srf HTTP/1.1
 Content-Type: application/x-www-form-urlencoded
 Host: https://login.live.com
 Content-Length: 211
 
 grant_type=client_credentials&client_id=ms-app%3a%2f%2fS-1-15-2-2972962901-2322836549-3722629029-1345238579-3987825745-2155616079-650196962&client_secret=Vex8L9WOFZuj95euaLrvSH7XyoDhLJc7&scope=notify.windows.com
```

WNS 对云服务进行身份验证，如果成功，则发送“200 OK”响应。 访问令牌使用“application/json”媒体类型在该 HTTP 响应正文所含的参数中返回。 在你的服务收到该访问令牌后，可随时开始发送通知。

以下示例将显示一个包含访问令牌身份的成功验证响应。 有关语法的详细信息，请参阅[推送通知服务请求和响应头](https://msdn.microsoft.com/library/windows/apps/hh465435)。

``` http
 HTTP/1.1 200 OK   
 Cache-Control: no-store
 Content-Length: 422
 Content-Type: application/json
 
 {
     "access_token":"EgAcAQMAAAAALYAAY/c+Huwi3Fv4Ck10UrKNmtxRO6Njk2MgA=", 
     "token_type":"bearer"
 }
```

### <a name="important-notes"></a>重要说明

-   以上过程支持的 OAuth 2.0 协议遵循草案版本 V16。
-   OAuth 征求意见文档 (RFC) 使用术语“客户端”指示云服务。
-   当该 OAuth 草案定稿时，以上过程可能有所更改。
-   访问令牌可用于多个通知请求。 这样，云服务只需验证一次即可发送多个通知。 然而，当访问令牌过期时，云服务必须再次验证，以获得一个新的访问令牌。

## <a name="sending-a-notification"></a>发送通知


通过使用通道 URI，无论何时有适用于客户的更新，云服务都可以发送通知。

上述访问令牌可用于多个通知请求；云服务不必为每个通知都请求一个新的访问令牌。 如果访问令牌过期，通知请求将返回一个错误。 如果访问令牌被拒绝，我们建议你不要尝试多次重新发送通知。 如果遇到该错误，你将需要请求一个新的访问令牌并重新发送通知。 有关准确的错误代码，请参阅[推送通知响应代码](https://msdn.microsoft.com/library/windows/apps/hh465435)。

1.  云服务会向通道 URI 发出一个 HTTP POST。 该请求必须通过 SSL 发出并包含必要的标头和通知负载。 授权头必须包含授权所必需的访问令牌。

    示例请求如下。 有关语法的详细信息，请参阅[推送通知响应代码](https://msdn.microsoft.com/library/windows/apps/hh465435)。

    有关编写通知负载的详细信息，请参阅[快速入门：发送推送通知](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)。 磁贴、Toast 或锁屏提醒通知的负载作为 XML 内容提供，并依附于其分别定义的[适应磁贴架构](adaptive-tiles-schema.md)或[传统磁贴架构](https://msdn.microsoft.com/library/windows/apps/br212853)。 原始通知的负载没有指定的结构。 严格来讲它是由应用定义的。

    ``` http
     POST https://cloud.notify.windows.com/?token=AQE%bU%2fSjZOCvRjjpILow%3d%3d HTTP/1.1
     Content-Type: text/xml
     X-WNS-Type: wns/tile
     Authorization: Bearer EgAcAQMAAAAALYAAY/c+Huwi3Fv4Ck10UrKNmtxRO6Njk2MgA=
     Host: cloud.notify.windows.com
     Content-Length: 24

     <body>
     ....
    ```

2.  WNS 发出响应，表明通知已收到，并将在下次可能时传递。 但是，WNS 不会提供设备或应用程序已收到通知的端到端确认。

此关系图演示数据流：

![发送通知的 WNS 关系图](images/wns-diagram-03.png)

### <a name="important-notes"></a>重要说明

-   WNS 不保证通知的可靠性或延迟。
-   通知绝不应包含机密或敏感数据。
-   若要发送通知，云服务必须先对 WNS 进行身份验证并获得访问令牌。
-   访问令牌仅允许云服务将通知发送到为其创建令牌的单个应用。 单个访问令牌无法用于在多个应用中发送通知。 因此，如果你的云服务支持多个应用，则在向每个通道 URI 推送通知时都必须提供相应应用的正确访问令牌。
-   当设备脱机时，WNS 将默认为每个通道 URI 存储至多 5 个磁贴通知（如果启用了队列；否则只能存储 1 个磁贴通知）和 1 个锁屏提醒通知，不存储原始通知。 可以通过 [X-WNS-Cache-Policy 标头](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_cache)更改这种默认缓存行为。 请注意，当设备离线时，永远不会存储 Toast 通知。
-   在对用户个性化通知内容的方案中，WNS 建议云服务在收到这些更新时立即发送这些更新。 此方案的示例包括社交媒体源更新、即时通信邀请、新消息通知或警报。 作为备用方法，你可以使用向大部分用户频繁提供相同的通用更新的方案；例如，天气、股票和新闻更新。 WNS 指南中指定这些更新的频率最高为每 30 分钟一个。 最终用户或 WNS 可以将超过该频率的例常更新确定为滥发更新。

## <a name="expiration-of-tile-and-badge-notifications"></a>磁贴和锁屏提醒通知过期时间


默认情况下，磁贴和徽标通知在下载完成时的三天后过期。 通知过期时，此内容将从磁贴或队列中删除，且不再向用户显示。 最佳做法是在所有磁贴和锁屏提醒通知上设置过期时间（使用对你的应用有意义的时间），以便使磁贴的内容不会在它不相关时继续保留。 对于具有已定义的使用寿命的内容来说，显式过期时间是必需的。 这还确保在你的云服务停止发送通知或用户在长时间内与网络断开连接时删除过时的内容。

你的云服务可以为每个通知设置一个过期时间，方法是设置 X-WNS-TTL HTTP 标头以指定通知在发送后保持有效的时间（以秒为单位）。 有关详细信息，请参阅[推送通知服务请求和响应头](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_ttl)。

例如，股票市场活跃交易日期间，你可将股票价格更新到期时间设置为发送间隔的两倍（例如，如果是每半小时发送一次通知，则将股票价格更新到期时间设置为一小时）。 另一个示例是，新闻应用可确定每日新闻磁贴更新的适当到期时间为一天。

## <a name="push-notifications-and-battery-saver"></a>推送通知和节电模式


节电模式可通过限制设备上的后台活动，延长电池使用时间。 Windows 10 允许用户设置使用节电模式电池电量低于指定的阈值时自动打开。 在节电模式处于打开状态时，将禁用推送消息接收，以节省电量。 但是也有几种例外情况。 以下 windows 10 节电模式设置 （在**设置**应用中找到） 允许应用接收推送通知，即使节电模式打开。

-   **允许在节电模式下接收任何应用的推送通知**：此设置允许所有应用在节电模式处于打开状态时接收推送通知。 请注意，此设置仅适用于 windows 10 桌面版 （家庭版、 专业版、 企业版和教育版）。
-   **始终允许**：此设置允许在节电模式处于打开状态时在后台运行特定应用，包括接收推送通知。 此列表由用户手动维护。

此两种设置的状态无法检查，但可以检查节电模式的状态。 在 windows 10 中，使用[**EnergySaverStatus**](https://docs.microsoft.com/uwp/api/Windows.System.Power.PowerManager.EnergySaverStatus)属性检查节电模式状态。 应用也可以使用 [**EnergySaverStatusChanged**](https://docs.microsoft.com/uwp/api/Windows.System.Power.PowerManager.EnergySaverStatusChanged) 事件侦听对节电模式的更改。

如果应用严重依赖推送通知，我们建议通知用户，在节电模式打开时，他们可能无法接收通知，并让他们可以轻松地调整**节电模式设置**。 在 windows 10 中使用节电模式设置 URI 架构`ms-settings:batterysaver-settings`，你可以提供指向设置应用的简便链接。

**提示**时通知用户有关节电模式设置，我们建议提供在未来阻止消息的方法。 例如，以下示例中的 `dontAskMeAgainBox` 复选框保留用户在 [**LocalSettings**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData.LocalSettings) 中的首选项。

 

下面是如何检查节电模式在 windows 10 中打开的示例。 此示例将通知用户，并将“设置”应用启动到**节电模式设置**。 `dontAskAgainSetting` 允许用户在不希望再次收到通知时阻止消息。

```cs
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Navigation;
using Windows.System;
using Windows.System.Power;
...
...
async public void CheckForEnergySaving()
{
   //Get reminder preference from LocalSettings
   bool dontAskAgain;
   var localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
   object dontAskSetting = localSettings.Values["dontAskAgainSetting"];
   if (dontAskSetting == null)
   {  // Setting does not exist
      dontAskAgain = false;
   }
   else
   {  // Retrieve setting value
      dontAskAgain = Convert.ToBoolean(dontAskSetting);
   }
   
   // Check if battery saver is on and that it's okay to raise dialog
   if ((PowerManager.EnergySaverStatus == EnergySaverStatus.On)
         && (dontAskAgain == false))
   {
      // Check dialog results
      ContentDialogResult dialogResult = await saveEnergyDialog.ShowAsync();
      if (dialogResult == ContentDialogResult.Primary)
      {
         // Launch battery saver settings (settings are available only when a battery is present)
         await Launcher.LaunchUriAsync(new Uri("ms-settings:batterysaver-settings"));
      }

      // Save reminder preference
      if (dontAskAgainBox.IsChecked == true)
      {  // Don't raise dialog again
         localSettings.Values["dontAskAgainSetting"] = "true";
      }
   }
}
```

这是适用于此示例中重点介绍的 [**ContentDialog**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) 的 XAML。

```xaml
<ContentDialog x:Name="saveEnergyDialog"
               PrimaryButtonText="Open battery saver settings"
               SecondaryButtonText="Ignore"
               Title="Battery saver is on."> 
   <StackPanel>
      <TextBlock TextWrapping="WrapWholeWords">
         <LineBreak/><Run>Battery saver is on and you may 
          not receive push notifications.</Run><LineBreak/>
         <LineBreak/><Run>You can choose to allow this app to work normally
         while in battery saver, including receiving push notifications.</Run>
         <LineBreak/>
      </TextBlock>
      <CheckBox x:Name="dontAskAgainBox" Content="OK, got it."/>
   </StackPanel>
</ContentDialog>
```

## <a name="related-topics"></a>相关主题


* [发送本地磁贴通知](sending-a-local-tile-notification.md)
* [快速入门：发送推送通知](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)
* [如何通过推送通知更新锁屏提醒](https://msdn.microsoft.com/library/windows/apps/hh465450)
* [如何请求、创建和保存通知通道](https://msdn.microsoft.com/library/windows/apps/hh465412)
* [如何为正在运行的应用程序截获通知](https://msdn.microsoft.com/library/windows/apps/xaml/jj709907.aspx)
* [如何使用 Windows 推送通知服务 (WNS) 进行验证](https://msdn.microsoft.com/library/windows/apps/hh465407)
* [推送通知服务请求和响应头](https://msdn.microsoft.com/library/windows/apps/hh465435)
* [推送通知指南和清单](https://msdn.microsoft.com/library/windows/apps/hh761462)
* [原始通知](https://msdn.microsoft.com/library/windows/apps/hh761488)
 

 




