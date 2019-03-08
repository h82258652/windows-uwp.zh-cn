---
title: 在 UWP 中使用 Webpush 和 VAPID 备用推送通道
description: 有关如何使用备用推送通道使用的 UWP 应用中的 VAPID 协议说明
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10，uwp，WinRT API WNS
localizationpriority: medium
ms.openlocfilehash: ba8630a2e877345adeac7eb443dd3e418d3ed277
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639522"
---
# <a name="alternate-push-channels-using-webpush-and-vapid-in-uwp"></a>在 UWP 中使用 Webpush 和 VAPID 备用推送通道 
从 Fall Creators Update，UWP 应用可以使用 web 推送 VAPID 身份验证以发送推送通知。  

## <a name="introduction"></a>简介
Web 推送标准的引入允许网站可以更像应用程序，即使用户不是网站上发送通知。

创建 VAPID 身份验证协议是为了允许网站使用供应商产品推送服务器进行身份验证无关的方式。 与所有供应商使用 VAPID 协议，网站可以发送推送通知，而不必知道它正在其运行的浏览器。 这是一项显著改进对实现每个平台一个不同的推送协议。 

UWP 应用可以使用 webpush 和 VAPID 发送推送通知与这些优势。 这些协议可以保存为新应用程序的开发时间并简化对现有应用的跨平台支持。 此外，企业应用或旁加载应用现在可以发送通知而无需在 Microsoft Store 中注册。 希望这会打开新的方法来跨所有平台的用户参与进来。  

## <a name="alternate-channels"></a>备用通道 
在 UWP 中，这些 VAPID 通道被称为备用通道，并提供与 web 推送通道类似的功能。 它们可以触发应用的后台任务运行，请启用消息加密，并允许通过单个应用的多个通道。 有关不同的通道类型之间的差异的详细信息，请参阅[选择了正确的信道](channel-types.md)。

使用备用的通道是访问推送通知，如果您的应用程序不能使用主通道或如果想要你的网站和应用之间共享代码的好办法。 设置通道既简单又熟悉的任何人已使用 web 推送标准版或 Windows 推送通知之前的工作。

## <a name="code-example"></a>代码示例

设置 UWP 应用的备用通道的基本过程是类似于设置主密钥或辅助通道。 首先，注册了通道[WNS server](windows-push-notification-services--wns--overview.md)。 然后，注册以作为后台任务运行。 发送通知并触发后台任务后，处理的事件。  

### <a name="get-a-channel"></a>获取通道 
若要创建备用通道，该应用程序必须提供两条信息： 其服务器和创建的通道的名称的公共密钥。 有关服务器密钥的详细信息均位于 web 推送规范，但我们建议在服务器上使用标准 web 推送库来生成密钥。  

通道 ID 是特别重要，因为应用程序可以创建多个备用的通道。 每个通道必须由将使用该通道上发送任何通知有效负载包含的唯一 ID 标识。  

```csharp
private async void AppCreateVAPIDChannelAsync(string appChannelId, IBuffer applicationServerKey) 
{ 
    // From the spec: applicationServer Key (p256dh):  
    //               An Elliptic curve Diffie–Hellman public key on the P-256 curve 
    //               (that is, the NIST secp256r1 elliptic curve).   
    //               The resulting key is an uncompressed point in ANSI X9.62 format             
    // ChannelId is an app provided value for it to identify the channel later.  
    // case of this app it is from the set { "Football", "News", "Baseball" } 
    PushNotificationChannel webChannel = await PushNotificationChannelManager.Current.CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId); 
 
    //Save the channel  
    AppUpdateChannelMapping(appChannelId, webChannel); 
             
    //Tell our web service that we have a new channel to push notifications to 
    AppPassChannelToSite(webChannel.Uri); 
} 
```
应用程序发送备份到其服务器通道，并将其保存在本地。 正在保存本地通道 ID 允许此应用来区分通道和通道续订才能防止过期。

每个其他类型的推送通知通道，如 web 通道可能会过期。 若要防止您感觉不到应用程序过期通道，创建新的通道，每次启动您的应用程序。    

### <a name="register-for-a-background-task"></a>注册以通过后台任务 

一旦您的应用程序创建后的备用通道，它应注册为接收通知的前景色或背景。 下面的示例注册为使用一个进程模型为在后台接收通知。  

```csharp
var builder = new BackgroundTaskBuilder(); 
builder.Name = "Push Trigger"; 
builder.SetTrigger(new PushNotificationTrigger()); 
BackgroundTaskRegistration task = builder.Register(); 
```
### <a name="receive-the-notifications"></a>接收通知 

一旦应用程序已注册为接收通知，它必须是能够处理传入通知。 由于单个应用程序可以注册多个通道，请务必处理通知之前检查通道 ID。  

```csharp
protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args) 
{ 
    base.OnBackgroundActivated(args); 
    var raw = args.TaskInstance.TriggerDetails as RawNotification; 
    switch (raw.ChannelId) 
    { 
        case "Football": 
            AppPostFootballScore(raw.Content); 
            break; 
        case "News": 
            AppProcessNewsItem(raw.Content); 
            break; 
        case "Baseball": 
            AppProcessBaseball(raw.Content); 
            break; 
 
        default: 
            //We don't have the channelID registered, should only happen in the case of a 
            //caching issue on the application server 
            break; 
    }                           
} 
```

请注意，是否从主通道传入通知，然后通道 ID 将不会设置。  

## <a name="note-on-encryption"></a>请注意加密 

可以使用任何加密方案更有用的应用程序。 在某些情况下，它就足以依赖于服务器和任何 Windows 设备之间的 TLS 标准。 在其他情况下，它可能会更有意义使用 web 推送加密方案或另一种方案设计。  

如果你想要使用其他形式，关键是加密的使用原始。标头属性。 它包含所有对推送服务器的 POST 请求中包含的加密标头。 在这里，您的应用程序可以使用密钥对消息进行解密。  

## <a name="related-topics"></a>相关主题
- [通知的通道类型](channel-types.md)
- [Windows 推送通知服务 (WNS)](windows-push-notification-services--wns--overview.md)
- [PushNotificationChannel 类](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannel)
- [PushNotificationChannelManager 类](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager)


