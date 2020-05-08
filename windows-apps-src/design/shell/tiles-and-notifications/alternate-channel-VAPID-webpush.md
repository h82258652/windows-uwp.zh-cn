---
title: 使用 UWP 中的 VAPID 的备用推送通道
description: 在 Windows 应用中使用 VAPID 协议的备用推送通道的说明
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10，uwp，WinRT API，WNS
localizationpriority: medium
ms.openlocfilehash: 382dca376e2393d83c2803043b61db76226b3995
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970862"
---
# <a name="alternate-push-channels-using-vapid-in-windows"></a>在 Windows 中使用 VAPID 的备用推送通道 
从秋季创意者更新开始，Windows 应用可以使用 VAPID authentication 来发送推送通知。  

> [!NOTE]
> 这些 Api 适用于托管其他网站并代表用户创建频道的 web 浏览器。  如果你想要将 webpush 通知添加到你的 web 应用，我们建议遵循用于创建服务工作人员和发送通知的 W3C 和 WhatWG 标准。

## <a name="introduction"></a>介绍
引入 web 推送标准后，即使用户不在网站上时，网站也能像应用一样操作，也可以发送通知。

已创建 VAPID authentication 协议，以允许网站以与供应商无关的方式在推送服务器上进行身份验证。 使用 VAPID 协议的所有供应商，网站无需知道运行它的浏览器就能发送推送通知。 这是针对每个平台实现不同推送协议的一项重大改进。 

Windows 应用程序还可以使用 VAPID 发送具有这些优势的推送通知。 这些协议可节省新应用的开发时间，并简化对现有应用的跨平台支持。 此外，企业应用或旁加载应用现在可以发送通知，而无需在 Microsoft Store 中注册。 但愿，这将打开新的方法，以便与所有平台上的用户进行沟通。  

## <a name="alternate-channels"></a>备用通道 
在 UWP 中，这些 VAPID 通道称为备用通道，并为 web 推送通道提供类似功能。 它们可以触发要运行的应用程序后台任务，启用消息加密，并允许来自单个应用的多个通道。 有关不同通道类型之间差异的详细信息，请参阅[选择正确的通道](channel-types.md)。

如果你的应用程序不能使用主通道，或者如果你想要在你的网站和应用程序之间共享代码，则可以使用备用通道来访问推送通知。 使用 web 推送标准或使用 Windows 推送通知的任何人都可以轻松地设置频道。

## <a name="code-example"></a>代码示例

为 Windows 应用程序设置备用通道的基本过程类似于设置主通道或辅助通道。 首先，使用[WNS 服务器](windows-push-notification-services--wns--overview.md)注册通道。 然后，注册以作为后台任务运行。 发送通知并触发后台任务后，处理事件。  

### <a name="get-a-channel"></a>获取通道 
若要创建备用通道，应用程序必须提供两条信息：其服务器的公钥以及它正在创建的通道的名称。 有关服务器密钥的详细信息可在 web 推送规范中找到，但建议在服务器上使用标准的 web 推送库来生成密钥。  

通道 ID 尤其重要，因为应用可创建多个备用通道。 每个通道必须由一个唯一的 ID 标识，该 ID 将随该通道发送的任何通知负载一起提供。  

```csharp
private async void AppCreateVAPIDChannelAsync(string appChannelId, IBuffer applicationServerKey) 
{ 
    // From the spec: applicationServer Key (p256dh):  
    //               An Elliptic curve Diffie–Hellman public key on the P-256 curve 
    //               (that is, the NIST secp256r1 elliptic curve).   
    //               The resulting key is an uncompressed point in ANSI X9.62 format             
    // ChannelId is an app provided value for it to identify the channel later.  
    // case of this app it is from the set { "Football", "News", "Baseball" } 
    PushNotificationChannel webChannel = await PushNotificationChannelManager.GetDefault().CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId); 
 
    //Save the channel  
    AppUpdateChannelMapping(appChannelId, webChannel); 
             
    //Tell our web service that we have a new channel to push notifications to 
    AppPassChannelToSite(webChannel.Uri); 
} 
```
应用会将通道回发到其服务器，并将其保存在本地。 若要在本地保存通道 ID，应用程序可以区分通道和续订通道，以防止其过期。

与所有其他类型的推送通知通道一样，web 频道也会过期。 若要防止在你的应用知道的情况下通道过期，请在应用每次启动时创建新的通道。    

### <a name="register-for-a-background-task"></a>注册后台任务 

应用创建备用通道后，应注册以便在前台或后台接收通知。 下面的示例注册使用单进程模型在后台接收通知。  

```csharp
var builder = new BackgroundTaskBuilder(); 
builder.Name = "Push Trigger"; 
builder.SetTrigger(new PushNotificationTrigger()); 
BackgroundTaskRegistration task = builder.Register(); 
```
### <a name="receive-the-notifications"></a>接收通知 

应用注册接收通知后，需要能够处理传入的通知。 由于单个应用可以注册多个通道，因此请确保在处理通知前检查通道 ID。  

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

请注意，如果通知来自主通道，则不会设置通道 ID。  

## <a name="note-on-encryption"></a>加密时的注意事项 

你可以使用你找到的任何加密方案来帮助你的应用程序。 在某些情况下，足以依赖于服务器和任何 Windows 设备之间的 TLS 标准。 在其他情况下，使用 web 推送加密方案或设计的其他方案可能更有意义。  

如果希望使用另一种形式的加密，则密钥是使用原始。标头属性。 它包含发送到推送服务器的 POST 请求中包含的所有加密标头。 在该处，应用可以使用密钥对消息进行解密。  

## <a name="related-topics"></a>相关主题
- [通知通道类型](channel-types.md)
- [Windows 推送通知服务 (WNS)](windows-push-notification-services--wns--overview.md)
- [PushNotificationChannel 类](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannel)
- [PushNotificationChannelManager 类](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager)


