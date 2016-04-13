---
Description: 了解用户如何在执行语音命令期间通过 Cortana 语音和画布与后台应用交互。
title: 与后台应用交互
ms.assetid: 6C60F03C-A242-435D-96BB-736892CC1CA6
label: Interact with a background app
template: detail.hbs
---

# 使用 Cortana 与后台应用交互


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**语音命令定义 (VCD) 元素和属性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

在执行语音命令的同时，支持用户通过 **Cortana** 画布中的语音和文本输入来与后台应用交互。

Cortana 支持应用完整的逐向工作流。 此工作流由应用定义，并且可以支持如下功能： 

-   成功完成
-   交付
-   进度
-   确认
-   消除歧义
-   错误

**先决条件：**

本主题基于[在 Cortana 中使用语音命令启动后台应用](launch-a-background-app-with-voice-commands-in-cortana.md)展开。 在这里，我们将继续通过名为 **Adventure Works** 的旅行规划和管理应用演示相关功能。

如果你还不熟悉通用 Windows 平台 (UWP) 应用开发，请仔细阅读这些主题来熟悉此处讨论的技术。

-   [创建你的第一个应用](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   借助[事件和路由事件概述](https://msdn.microsoft.com/library/windows/apps/mt185584)了解事件

**用户体验指南：**

有关如何将你的应用与 **Cortana** 集成的信息，请参阅 [Cortana 设计指南](https://msdn.microsoft.com/library/windows/apps/dn974233)；有关设计出既实用又有吸引力且支持语音的应用的有用提示，请参阅[语音设计指南](https://msdn.microsoft.com/library/windows/apps/dn596121)。

## <span id="Feedback_strings"> </span> <span id="feedback_strings"> </span> <span id="FEEDBACK_STRINGS"> </span>反馈字符串

撰写由 **Cortana** 显示并说出的反馈字符串。

[Cortana 设计指南](https://msdn.microsoft.com/library/windows/apps/dn974233)提供有关撰写 **Cortana** 字符串的建议。

## <span id="Feedback_strings"> </span> <span id="feedback_strings"> </span> <span id="FEEDBACK_STRINGS"> </span>反馈字符串

内容卡可以为用户提供额外的上下文，并且有助于使反馈字符串保持简洁。

**Cortana** 支持以下内容卡模板（完成屏幕上只能使用一个模板）：

    -   Title only
    -   Title with up to three lines of text
    -   Title with image
    -   Title with image and up to three lines of text

图像可以：

    -   68w x 68h
    -   68w x 92h
    -   280w x 140h

你还可以让用户通过点击卡或应用的文本链接，在前台启动你的应用。

## <span id="Completion_screen"> </span> <span id="completion_screen"> </span> <span id="COMPLETION_SCREEN"> </span>完成屏幕

完成屏幕将向用户提供有关已完成的语音命令任务的信息。

使应用响应耗时不超过 500 毫秒且不要求用户提供额外信息的任务可以在不与 **Cortana** 进一步交互的情况下完成。 Cortana 仅显示完成屏幕。

我们在此处使用 **Adventure Works** 应用来展示要显示即将到来的伦敦之旅的语音命令请求的完成屏幕。 

![Cortana 后台应用完成屏幕](images/cortana-completion-screen-upcomingtrip-small.png)

语音命令在 AdventureWorksCommands.xml 中定义：
```
<Command Name="whenIsTripToDestination">
  <Example> When is my trip to Las Vegas?</Example>
  <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
  <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
  <Feedback> Looking for trip to {destination}</Feedback>
  <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
</Command>
```

AdventureWorksVoiceCommandService.cs 包含完成消息方法：

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

## <span id="Hand-off_screen"> </span> <span id="hand-off_screen"> </span> <span id="HAND-OFF_SCREEN"> </span>交付屏幕

识别语音命令后，**Cortana** 必须在大约 500 毫秒内调用 ReportSuccessAsync 和显示反馈。 如果应用服务无法在 500 毫秒内完成语音命令指定的操作，则 **Cortana** 将呈现一直显示到应用调用 ReportSuccessAsync 或最长 5 秒的交付屏幕。

如果应用服务未调用 ReportSuccessAsync 或任何其他 VoiceCommandServiceConnection 方法，则用户会收到一条错误消息并且该应用服务调用将会取消。

以下是 **Adventure Works** 应用的交付屏幕的一个示例。 在本示例中，用户已通过 **Cortana** 查询即将到来的旅行。 交付屏幕包含应用服务名称、图标和 VCD 文件中声明的 **Feedback** 字符串所自定义的消息。

![Cortana 后台应用交付屏幕](images/cortana-backgroundapp-progress-result.png)


## <span id="Progress_screen"> </span> <span id="progress_screen"> </span> <span id="PROGRESS_SCREEN"> </span>进度屏幕


如果应用服务调用 ReportSuccessAsync 的时间不止 500 毫秒，**Cortana** 将向用户提供进度屏幕。 将显示应用图标，并且你必须同时提供 GUI 和 TTS 进度字符串，以指示正在主动处理该任务。

**Cortana** 将显示进度屏幕最多 5 秒。 如果显示时间超过 5 秒，**Cortana** 将向用户显示一条错误消息，该应用服务已关闭。 如果应用服务完成操作所需的时间超过 5 秒，则它可以继续根据进度屏幕更新 **Cortana**。

以下是 **Adventure Works** 应用的进度屏幕的一个示例。 在此示例中，用户已取消了拉斯维加斯之旅。 进度屏幕包含针对操作、图标和带有关于正在取消旅行的信息的内容磁贴所自定义的消息。

![Cortana 后台应用进度屏幕 ](images/cortana-progress-screen.png)

AdventureWorksVoiceCommandService.cs 包含以下调用 [**ReportProgressAsync**](https://msdn.microsoft.com/library/windows/apps/dn706579) 以在 **Cortana** 中显示进度屏幕的进度消息方法。


```    CSharp
/// <summary>
/// Show a progress screen. These should be posted at least every 5 seconds for a 
/// long-running operation.
/// </summary>
/// <param name="message">The message to display, relating to the task being performed.</param>
/// <returns></returns>
private async Task ShowProgressScreen(string message)
{
    var userProgressMessage = new VoiceCommandUserMessage();
    userProgressMessage.DisplayMessage = userProgressMessage.SpokenMessage = message;

    VoiceCommandResponse response = VoiceCommandResponse.CreateResponse(userProgressMessage);
    await voiceServiceConnection.ReportProgressAsync(response);
}
```

## <span id="Confirmation_screen"> </span> <span id="confirmation_screen"> </span> <span id="CONFIRMATION_SCREEN"> </span>确认屏幕


应用服务可以在以下情况下请求确认：语音命令指定的操作不可更改，该操作具有重大的影响力，或者识别置信度不太高。

下面是一个 **Adventure Works** 应用的确认屏幕示例。 在此示例中，用户已通过 **Cortana** 指示应用服务取消拉斯维加斯之旅。 应用服务已提供带有确认屏幕的 **Cortana**，以在取消该旅途之前提示用户说出“是”或“否”。

如果用户说出“是”或“否”之外的答案，**Cortana** 将无法确定该问题的答案。 在此情况下，**Cortana** 会提示用户说出应用服务所提供的类似问题的答案。

在第二次提示下，如果用户仍没有说出“是”或“否”，**Cortana** 将再一次提示用户说出相同问题的答案，该提示以一则道歉为前缀。 如果用户仍没有说出“是”或“否”，**Cortana** 将停止侦听语音输入，改为要求用户点击其中一个按钮。

确认屏幕包含针对操作、图标和带有关于正在取消旅行的信息的内容磁贴所自定义的消息。

![Cortana 后台应用确认屏幕](images/cortana-confirmation-screen.png)

AdventureWorksVoiceCommandService.cs 包含以下调用 [**RequestConfirmationAsync**](https://msdn.microsoft.com/library/windows/apps/dn706582) 来在 **Cortana** 中显示确认屏幕的行程取消方法。

```    CSharp
/// <summary>
/// Handle the Trip Cancellation task. This task demonstrates how to prompt a user
/// for confirmation of an operation, show users a progress screen while performing
/// a long-running task, and show a completion screen.
/// </summary>
/// <param name="destination">The name of a destination.</param>
/// <returns></returns>
private async Task SendCompletionMessageForCancellation(string destination)
{
    // Begin loading data to search for the target store. 
    // Consider inserting a progress screen here, in order to prevent Cortana from timing out. 
    string progressScreenString = string.Format(
        cortanaResourceMap.GetValue("ProgressLookingForTripToDest", cortanaContext).ValueAsString,
        destination);
    await ShowProgressScreen(progressScreenString);

    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);
    Model.Trip trip = null;
    if (trips.Count() > 1)
    {
        // If there is more than one trip, provide a disambiguation screen.
        // However, if a significant number of items are returned, you might want to 
        // just display a link to your app and provide a deeper search experience.
        string disambiguationDestinationString = string.Format(
            cortanaResourceMap.GetValue("DisambiguationWhichTripToDest", cortanaContext).ValueAsString,
            destination);
        string disambiguationRepeatString = cortanaResourceMap.GetValue("DisambiguationRepeat", cortanaContext).ValueAsString;
        trip = await DisambiguateTrips(trips, disambiguationDestinationString, disambiguationRepeatString);
    }
    else
    {
        trip = trips.FirstOrDefault();
    }

    var userPrompt = new VoiceCommandUserMessage();
    
    VoiceCommandResponse response;
    if (trip == null)
    {
        var userMessage = new VoiceCommandUserMessage();
        string noSuchTripToDestination = string.Format(
            cortanaResourceMap.GetValue("NoSuchTripToDestination", cortanaContext).ValueAsString,
            destination);
        userMessage.DisplayMessage = userMessage.SpokenMessage = noSuchTripToDestination;

        response = VoiceCommandResponse.CreateResponse(userMessage);
        await voiceServiceConnection.ReportSuccessAsync(response);
    }
    else
    {
        // Prompt the user for confirmation that this is the correct trip to cancel.
        string cancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("CancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userPrompt.DisplayMessage = userPrompt.SpokenMessage = cancelTripToDestination;
        var userReprompt = new VoiceCommandUserMessage();
        string confirmCancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("ConfirmCancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userReprompt.DisplayMessage = userReprompt.SpokenMessage = confirmCancelTripToDestination;
        
        response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt);

        var voiceCommandConfirmation = await voiceServiceConnection.RequestConfirmationAsync(response);

        // If RequestConfirmationAsync returns null, Cortana has likely been dismissed.
        if (voiceCommandConfirmation != null)
        {
            if (voiceCommandConfirmation.Confirmed == true)
            {
                string cancellingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("CancellingTripToDestination", cortanaContext).ValueAsString,
               destination);
                await ShowProgressScreen(cancellingTripToDestination);

                // Perform the operation to remove the trip from app data. 
                // As the background task runs within the app package of the installed app,
                // we can access local files belonging to the app without issue.
                await store.DeleteTrip(trip);

                // Provide a completion message to the user.
                var userMessage = new VoiceCommandUserMessage();
                string cancelledTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("CancelledTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = cancelledTripToDestination;
                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
            else
            {
                // Confirm no action for the user.
                var userMessage = new VoiceCommandUserMessage();
                string keepingTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("KeepingTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = keepingTripToDestination;

                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
        }
    }
}
```

## <span id="Disambiguation_screen"> </span> <span id="disambiguation_screen"> </span> <span id="DISAMBIGUATION_SCREEN"> </span>消除歧义屏幕


当语音命令指定的操作具有多个可能的结果时，应用服务可以向用户请求更多信息。

下面是一个 **Adventure Works** 应用的消除歧义屏幕示例。 在此示例中，用户已通过 **Cortana** 指示应用服务取消拉斯维加斯之旅。 但是，用户的拉斯维加斯之旅有两个日期不同的行程，并且应用服务必须在用户选定预定行程的情况下才能完成操作。

应用服务提供带有消除歧义屏幕的 **Cortana**，以提示用户从匹配的行程列表中进行选择，然后才能进行取消操作。

在此情况下，**Cortana** 会提示用户说出应用服务所提供的类似问题的答案。

在第二次提示下，如果用户仍没有说出可用于确定选项的内容，**Cortana** 将先道歉，然后第三次向用户提出相同的问题。 如果用户仍没有说出可用于确定选项的内容，**Cortana** 将停止侦听语音输入，改为要求用户点击其中一个按钮。

消除歧义屏幕包含针对操作、图标和带有关于正在取消旅行的信息的内容磁贴所自定义的消息。

![Cortana 后台应用消除歧义屏幕 ](images/cortana-disambiguation-screen.png)

AdventureWorksVoiceCommandService.cs 包含以下调用 [**RequestDisambiguationAsync**](https://msdn.microsoft.com/library/windows/apps/dn706583) 以在 **Cortana** 中显示消除歧义屏幕的行程取消方法。

```csharp
/// <summary>
/// Provide the user with a way to identify which trip to cancel. 
/// </summary>
/// <param name="trips">The set of trips</param>
/// <param name="disambiguationMessage">The initial disambiguation message</param>
/// <param name="secondDisambiguationMessage">Repeat prompt retry message</param>
private async Task<Model.Trip> DisambiguateTrips(IEnumerable<Model.Trip> trips, string disambiguationMessage, string secondDisambiguationMessage)
{
    // Create the first prompt message.
    var userPrompt = new VoiceCommandUserMessage();
    userPrompt.DisplayMessage =
        userPrompt.SpokenMessage = disambiguationMessage;

    // Create a re-prompt message if the user responds with an out-of-grammar response.
    var userReprompt = new VoiceCommandUserMessage();
    userReprompt.DisplayMessage =
        userReprompt.SpokenMessage = secondDisambiguationMessage;

    // Create card for each item. 
    var destinationContentTiles = new List<VoiceCommandContentTile>();
    int i = 1;
    foreach (Model.Trip trip in trips)
    {
        var destinationTile = new VoiceCommandContentTile();

        destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
        destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));
        
        // The AppContext can be any arbitrary object.
        destinationTile.AppContext = trip;
        string dateFormat = "";
        if (trip.StartDate != null)
        {
            dateFormat = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
        }
        else
        {
            // The app allows a trip to have no date.
            // However, the choices must be unique so they can be distinguished.
            // Here, we add a number to identify them.
            dateFormat = string.Format("{0}", i);
        } 

        destinationTile.Title = trip.Destination + " " + dateFormat;
        destinationTile.TextLine1 = trip.Description;

        destinationContentTiles.Add(destinationTile);
        i++;
    }

    // Cortana handles re-prompting if no valid response.
    var response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt, destinationContentTiles);

    // If cortana is dismissed in this operation, null is returned.
    var voiceCommandDisambiguationResult = await
        voiceServiceConnection.RequestDisambiguationAsync(response);
    if (voiceCommandDisambiguationResult != null)
    {
        return (Model.Trip)voiceCommandDisambiguationResult.SelectedItem.AppContext;
    }

    return null;
}
```

## <span id="Error_screen"> </span> <span id="error_screen"> </span> <span id="ERROR_SCREEN"> </span>错误屏幕


当语音命令指定的操作无法完成时，应用服务可能会提供一个错误屏幕。

下面是一个 **Adventure Works** 应用的错误屏幕示例。 在此示例中，用户已通过 **Cortana** 指示应用服务取消拉斯维加斯之旅。 但是，用户并没有安排到拉斯维加斯的任何行程。

应用服务提供带有错误屏幕的 **Cortana**，该屏幕中包含针对操作、图标和特定错误消息所自定义的消息。

调用 [**ReportFailureAsync**](https://msdn.microsoft.com/library/windows/apps/dn706578) 以在 **Cortana** 中显示错误屏幕。

```csharp
var userMessage = new VoiceCommandUserMessage();
    userMessage.DisplayMessage = userMessage.SpokenMessage = 
      "Sorry, you don't have any trips to Las Vegas";
                
    var response = VoiceCommandResponse.CreateResponse(userMessage);

    response.AppLaunchArgument = "showUpcomingTrips";
    await voiceServiceConnection.ReportFailureAsync(response);
```

## <span id="related_topics"> </span>相关文章


**开发人员**
* [Cortana 交互](cortana-interactions.md)
* [**VCD 元素和属性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**设计人员**
* [Cortana 设计指南](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [语音设计指南](https://msdn.microsoft.com/library/windows/apps/dn596121)

**示例**
* [Cortana 语音命令示例](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=Mar16_HO4-->


