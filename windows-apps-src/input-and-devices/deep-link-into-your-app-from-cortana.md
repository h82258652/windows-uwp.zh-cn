---
author: Karl-Bridge-Microsoft
Description: 从 Cortana 的后台应用服务提供深层链接，以便在特定状态或上下文中将应用启动到前台。
title: Cortana 到后台应用的深层链接
ms.assetid: BE811A87-8821-476A-90E4-2E20D37E4043
label: Deep link to a background app
template: detail.hbs
---

# Cortana 到后台应用的深层链接


**重要的 API**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**语音命令定义 (VCD) 元素和属性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

从 **Cortana** 中的后台应用提供深层链接，以便在特定状态或上下文中将应用启动到前台。

> **注意**  
在启动前台应用时，会终止 **Cortana** 和后台应用服务。

默认情况下，在 **Cortana** 完成屏幕上显示在此（“转到 AdventureWorks”）所示的深层链接，但你可以在各种不同的屏幕上显示深层链接。 

![Cortana 后台应用完成屏幕](images/cortana-completion-screen-upcomingtrip-small.png)

**先决条件：  **

本主题基于[在 Cortana 中与后台应用交互](interact-with-a-background-app-in-cortana.md)生成。 我们将继续使用名为 **Adventure Works** 的旅行规划和管理应用演示各种 **Cortana** 功能。

如果你还不熟悉通用 Windows 平台 (UWP) 应用开发，请仔细阅读这些主题来熟悉此处讨论的技术。

-   [创建你的第一个应用](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   借助[事件和路由事件概述](https://msdn.microsoft.com/library/windows/apps/mt185584)了解事件

**用户体验指南：  **

有关如何将你的应用与 **Cortana** 集成的信息，请参阅 [Cortana 设计指南](https://msdn.microsoft.com/library/windows/apps/dn974233)；有关设计出既实用又有吸引力且支持语音的应用的有用提示，请参阅[语音设计指南](https://msdn.microsoft.com/library/windows/apps/dn596121)。

## <span id="Overview"></span><span id="overview"></span><span id="OVERVIEW"></span>概述


用户可以通过 **Cortana** 访问你的应用，方法如下：

-   将应用激活为前台应用（请参阅[通过 Cortana 使用语音命令激活前台应用](launch-a-foreground-app-with-voice-commands-in-cortana.md)）。
-   将特定功能公开为后台应用服务（请参阅[通过 Cortana 使用语音命令激活后台应用](launch-a-background-app-with-voice-commands-in-cortana.md)）。
-   深层链接到特定页面、内容以及状态或上下文。

我们将在此讨论深层链接。

当 Cortana 和你的应用服务是完整功能应用（而不是要求用户通过“开始”菜单启动应用）的网关时，或是对于提供对你的应用内更丰富的详细信息和功能（无法通过 Cortana 实现）的访问权限，深层链接非常有用。 深层链接是提高可用性和推广应用的另一种方法。

可以使用三种方法提供深层链接：

-   各种 **Cortana** 屏幕上的“转到&lt;应用&gt;”链接。
-   各种 **Cortana** 屏幕上嵌入内容磁贴的链接。
-   以编程方式从后台应用服务启动前台应用。

## <span id="Go_to__app__deep_link"></span><span id="go_to__app__deep_link"></span><span id="GO_TO__APP__DEEP_LINK"></span>“转到&lt;应用&gt;”深层链接


**Cortana** 在大部分屏幕上会在内容卡下显示“转到&lt;应用&gt;”深层链接。

![Cortana 后台应用完成屏幕](images/cortana-completion-screen.png)

可以为此链接提供在与应用服务类似的上下文中打开应用的启动参数。 如果未提供启动参数，则应用将启动到主屏幕。

在 **AdventureWorks** 样例的 AdventureWorksVoiceCommandService.cs 示例中，我们将指定目标传递到 SendCompletionMessageForDestination 方法，该方法可检索所有匹配的行程并提供应用的深层链接。

首先，我们创建由 **Cortana** 说出并在 **Cortana** 画布上显示的 [**VoiceCommandUserMessage**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandusermessage.aspx) (```userMessage```)。 然后创建 [**VoiceCommandContentTile**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandcontenttile.aspx) 列表对象以在该画布上显示结果卡集合。 

稍后这两个对象对传递到 [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) 对象 (```response```) 的 [CreateResponse](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandresponse.createresponse.aspx) 方法。 然后我们将设置语音命令目标值的 [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) 属性值。

最后，我们调用 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 的 [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) 方法。

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
...
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
...
    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```


## <span id="Content_tile_deep_link"></span><span id="content_tile_deep_link"></span><span id="CONTENT_TILE_DEEP_LINK"></span>内容磁贴深层链接


你可以在各种 **Cortana** 屏幕上将深层链接添加到内容卡。

![Cortana 后台应用交付屏幕 ](images/cortana-backgroundapp-progress-result.png)

与“转到&lt;应用&gt;”链接一样，你可以提供启动参数以打开你的应用，其上下文与应用服务类似。 如果未提供启动参数，则内容磁贴将不会链接到你的应用。

在 **AdventureWorks** 样例的 AdventureWorksVoiceCommandService.cs 示例中，我们将指定目标传递到 SendCompletionMessageForDestination 方法，该方法可检索所有匹配的行程并提供应用的深层链接的内容卡。

首先，我们创建由 **Cortana** 说出并在 **Cortana** 画布上显示的 [**VoiceCommandUserMessage**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandusermessage.aspx) (```userMessage```)。 然后创建 [**VoiceCommandContentTile**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandcontenttile.aspx) 列表对象以在该画布上显示结果卡集合。 

稍后这两个对象对传递到 [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) 对象 (```response```) 的 [CreateResponse](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandresponse.createresponse.aspx) 方法。 然后我们将设置语音命令目标值的 [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) 属性值。

最后，我们调用 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 的 [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) 方法。
我们在此处将具有不同 [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) 参数值的两个内容磁贴添加到在 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 对象的 [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) 调用中使用的 [**VoiceCommandContentTile**](https://msdn.microsoft.com/library/windows/apps/dn974168) 列表。

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
    // If this operation is expected to take longer than 0.5 seconds, the task must
    // supply a progress response to Cortana before starting the operation, and
    // updates must be provided at least every 5 seconds.
    string loadingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("LoadingTripToDestination", cortanaContext).ValueAsString,
               destination);
    await ShowProgressScreen(loadingTripToDestination);
    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    // Query for the specified trip. 
    // The destination should be in the phrase list. However, there might be  
    // multiple trips to the destination. We pick the first.
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
    if (trips.Count() == 0)
    {
        string foundNoTripToDestination = string.Format(
               cortanaResourceMap.GetValue("FoundNoTripToDestination", cortanaContext).ValueAsString,
               destination);
        userMessage.DisplayMessage = foundNoTripToDestination;
        userMessage.SpokenMessage = foundNoTripToDestination;
    }
    else
    {
        // Set plural or singular title.
        string message = "";
        if (trips.Count() > 1)
        {
            message = cortanaResourceMap.GetValue("PluralUpcomingTrips", cortanaContext).ValueAsString;
        }
        else
        {
            message = cortanaResourceMap.GetValue("SingularUpcomingTrip", cortanaContext).ValueAsString;
        }
        userMessage.DisplayMessage = message;
        userMessage.SpokenMessage = message;

        // Define a tile for each destination.
        foreach (Model.Trip trip in trips)
        {
            int i = 1;
            
            var destinationTile = new VoiceCommandContentTile();

            destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
            destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));

            destinationTile.AppLaunchArgument = trip.Destination;
            destinationTile.Title = trip.Destination;
            if (trip.StartDate != null)
            {
                destinationTile.TextLine1 = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
            }
            else
            {
                destinationTile.TextLine1 = trip.Destination + " " + i;
            }

            destinationsContentTiles.Add(destinationTile);
            i++;
        }
    }

    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```
## <span id="Programmatic_deep_link"></span><span id="programmatic_deep_link"></span><span id="PROGRAMMATIC_DEEP_LINK"></span>编程访问深层链接


还可以使用启动参数以编程方式启动你的应用以打开应用，其上下文与应用服务类似。 如果未提供启动参数，则应用将启动到主屏幕。

我们在此处将值为“Las Vegas”的 [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) 参数添加到在 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 对象的 [**RequestAppLaunchAsync**](https://msdn.microsoft.com/library/windows/apps/dn706581) 调用中使用的 [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) 对象。

```CSharp
var userMessage = new VoiceCommandUserMessage();
userMessage.DisplayMessage = "Here are your trips.";
userMessage.SpokenMessage = 
  "You have one trip to Vegas coming up.";

response = VoiceCommandResponse.CreateResponse(userMessage);
response.AppLaunchArgument = “Las Vegas”;
await  VoiceCommandServiceConnection.RequestAppLaunchAsync(response);
```

## <span id="App_manifest"></span><span id="app_manifest"></span><span id="APP_MANIFEST"></span>应用清单


为了支持深层链接到你的应用，必须在你的应用项目的 Package.appxmanifest 文件中声明 `windows.personalAssistantLaunch` 扩展。

我们在此处为 **Adventure Works** 应用声明 `windows.personalAssistantLaunch` 扩展。

```XML
<Extensions>
  <uap:Extension Category="windows.appService" 
    EntryPoint="AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService">
    <uap:AppService Name="AdventureWorksVoiceCommandService"/>
  </uap:Extension>
  <uap:Extension Category="windows.personalAssistantLaunch"/> 
</Extensions>
```

## <span id="Protocol_contract"></span><span id="protocol_contract"></span><span id="PROTOCOL_CONTRACT"></span>协议合约


你的应用将使用 [**Protocol**](https://msdn.microsoft.com/library/windows/apps/br224693) 合约通过统一资源标识符 (URI) 激活启动到前台。 你的应用必须替代你的应用的 [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 事件，并检查 **Protocol** 的 **ActivationKind**。 有关详细信息，请参阅[处理 URI 激活](https://msdn.microsoft.com/library/windows/apps/mt228339)。

我们在此处对 [**ProtocolActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224742) 提供的 URI 进行解码以访问启动参数。 针对此示例，将 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br224746) 设置为“windows.personalassistantlaunch:?LaunchContext=Las Vegas”。

```CSharp
if (args.Kind == ActivationKind.Protocol)
  {
    var commandArgs = args as ProtocolActivatedEventArgs;
    Windows.Foundation.WwwFormUrlDecoder decoder = 
      new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
    var destination = decoder.GetFirstValueByName("LaunchContext");

    navigationCommand = new ViewModel.TripVoiceCommand(
      "protocolLaunch",
      "text",
      "destination",
      destination);

    navigationToPageType = typeof(View.TripDetails);

    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active.
    Window.Current.Activate();
  }
```

## <span id="related_topics"></span>相关文章


**开发人员**
* [Cortana 交互](cortana-interactions.md)
* [**VCD 元素和属性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**设计人员**
* [Cortana 设计指南](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [语音设计指南](https://msdn.microsoft.com/library/windows/apps/dn596121)

**示例**
* [Cortana 语音命令示例](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=May16_HO2-->


