---
author: andrewleader
Description: Learn how to send a local toast notification and handle the user clicking the toast.
title: 发送本地 toast 通知
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 发送 toast 通知, 通知, 发送通知, toast 通知, 操作方法, 快速入门, 开始使用, 代码示例, 演练
ms.localizationpriority: medium
ms.openlocfilehash: 3004b7041838656890b3a967e858dddc64c29ee5
ms.sourcegitcommit: 5dda01da4702cbc49c799c750efe0e430b699502
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "4115679"
---
# <a name="send-a-local-toast-notification"></a>发送本地 toast 通知


Toast 通知是用户当前未在应用内部时应用可构造并发送给用户的消息。 此快速入门指南将指导你借助新自适应模板和交互式操作完成创建、交付并显示 Windows 10 toast 通知的步骤。 通过本地通知对这些操作进行说明，本地通知是实现起来最简单的通知。

> [!IMPORTANT]
> 桌面应用程序（包括桌面桥和经典 Win32）发送通知和处理激活的步骤不同。 请参阅[桌面 app](toast-desktop-apps.md) 文档，了解如何实现 toast。

我们会演示以下内容：

### <a name="sending-a-toast"></a>发送 toast

* 构造通知的可视部分（文本和图像）
* 将操作添加到通知
* 设置的 toast 的过期时间
* 设置标记/组，以便你在以后替换/删除 toast
* 使用本地 API 发送你的 toast

### <a name="handling-activation"></a>处理激活

* 正文或按钮被单击时处理激活
* 处理前台激活
* 处理后台激活

> **重要 API**：[ToastNotification 类](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)、[ToastNotificationActivatedEventArgs 类](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)


## <a name="prerequisites"></a>系统必备

若要完全理解此主题，事先掌握以下内容会很有用...

* Toast 通知术语和概念的应用知识。 有关详细信息，请参阅 [Toast 和操作中心概述](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/toast-notification-and-action-center-overview-for-windows-10/)。
* 熟悉 Windows 10 toast 通知内容。 有关详细信息，请参阅 [toast 内容文档](adaptive-interactive-toasts.md)。
* Windows 10 UWP 应用项目

> [!NOTE]
> 与 Windows 8/8.1 不同，无需在应用清单中声明应用能够显示 toast 通知。 所有应用都能发送和显示 toast 通知。

> [!NOTE]
> **Windows 8/8.1 应用**：请使用[存档的文档](https://msdn.microsoft.com/library/windows/apps/xaml/hh868254.aspx)。


## <a name="install-nuget-packages"></a>安装 NuGet 程序包

我们建议你对你的项目安装以下两个 NuGet 程序包。 代码示例将使用这些程序包。 文章结尾部分将提供不使用任何 NuGet 程序包的“Vanilla”代码片段。

* [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)：通过对象而不是原始 XML 生成 toast 有效负载。
* [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/)：使用 C# 生成和分析查询字符串


## <a name="add-namespace-declarations"></a>添加命名空间声明

`Windows.UI.Notifications` 包含 toast API。

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="send-a-toast"></a>发送 toast

在 Windows 10 中，你的 toast 通知内容是使用对于你的通知外观给予了最大程度灵活性的自适应语言描述的。 有关详细信息，请参阅 [toast 内容文档](adaptive-interactive-toasts.md)。

### <a name="constructing-the-visual-part-of-the-content"></a>构造内容的可视部分

现在先来构造内容的可视部分，其中包括希望用户看到的文本和图像。

通知库使得 XML 内容的生成变得十分简单。 如果不从 NuGet 安装通知库，则需要手动构造 XML，这样就可能出错。

> [!NOTE]
> 图像可来自于应用包、应用的本地存储或来自 Web。 自 Fall Creators Update 起，正常连接上的 Web 图像的大小限制提升至 3 MB，按流量计费的连接上的限制提升至 1 MB。 在尚未运行 Fall Creators Update 的设备上，Web 图像的大小不得超过 200 KB。

```csharp
// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "https://picsum.photos/360/202?image=883";
string logo = "ms-appdata:///local/Andrew.jpg";
 
// Construct the visuals of the toast
ToastVisual visual = new ToastVisual()
{
    BindingGeneric = new ToastBindingGeneric()
    {
        Children =
        {
            new AdaptiveText()
            {
                Text = title
            },
 
            new AdaptiveText()
            {
                Text = content
            },
 
            new AdaptiveImage()
            {
                Source = image
            }
        },
 
        AppLogoOverride = new ToastGenericAppLogo()
        {
            Source = logo,
            HintCrop = ToastGenericAppLogoCrop.Circle
        }
    }
};
```


### <a name="constructing-actions-part-of-the-content"></a>构造内容的操作部分

现在向内容中添加操作。

在以下示例中，包含了允许用户输入文本的输入元素，当用户单击其中一个按钮或 toast 本身时，文本会返回到应用。

然后，添加了两个按钮，每个按钮具有自己的激活类型、内容和参数。
* **ActivationType** 用于指定用户执行激活操作时要如何激活应用。 可选择在前台启动应用、启动后台任务或通过协议启动另一个应用。 无论应用选择前台或后台，你始终会收到用户输入和由你指定的参数，以便应用可以执行正确的操作，如发送消息或打开对话。

```csharp
// In a real app, these would be initialized with actual data
int conversationId = 384928;
 
// Construct the actions for the toast (inputs and buttons)
ToastActionsCustom actions = new ToastActionsCustom()
{
    Inputs =
    {
        new ToastTextBox("tbReply")
        {
            PlaceholderContent = "Type a response"
        }
    },
 
    Buttons =
    {
        new ToastButton("Reply", new QueryString()
        {
            { "action", "reply" },
            { "conversationId", conversationId.ToString() }
 
        }.ToString())
        {
            ActivationType = ToastActivationType.Background,
            ImageUri = "Assets/Reply.png",
 
            // Reference the text box's ID in order to
            // place this button next to the text box
            TextBoxId = "tbReply"
        },
 
        new ToastButton("Like", new QueryString()
        {
            { "action", "like" },
            { "conversationId", conversationId.ToString() }
 
        }.ToString())
        {
            ActivationType = ToastActivationType.Background
        },
 
        new ToastButton("View", new QueryString()
        {
            { "action", "viewImage" },
            { "imageUrl", image }
 
        }.ToString())
    }
};
```


### <a name="combining-the-above-to-construct-the-full-content"></a>结合上述内容构造完整内容

内容构造现已完成，可以使用它来实例化 [**ToastNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification) 对象。

**注意**：你还可以在根元素内提供激活类型，指定用户点击 toast 通知的正文时需要进行哪种类型的激活。 通常情况下，点击 toast 的正文应该会在前台启动你的应用，以提供一致的用户体验，但你也可以使用适合你的特定应用场景的其他激活类型，以便对用户更有意义。

应始终设置 **Launch** 属性，以便用户点击 toast 正文且应用启动时，应用能够知道应显示的内容。

```csharp
// Now we can construct the final toast content
ToastContent toastContent = new ToastContent()
{
    Visual = visual,
    Actions = actions,
 
    // Arguments when the user taps body of toast
    Launch = new QueryString()
    {
        { "action", "viewConversation" },
        { "conversationId", conversationId.ToString() }
 
    }.ToString()
};
 
// And create the toast notification
var toast = new ToastNotification(toastContent.GetXml());
```


## <a name="set-an-expiration-time"></a>设置过期时间

在 Windows 10 中，所有 toast 通知被用户消除或忽略后将转到操作中心，以便在弹出窗口消失后，用户仍可查看通知。

但是，如果你的通知中的消息仅在一段时间内相关，则应对 toast 通知设置过期时间，让用户不至于看到来自应用的过时信息。 例如，如果升级的有效时间仅为 12 个小时，则将过期时间设置为 12 个小时。 下面的代码中将过期时间设置为 2 天。

> [!NOTE]
> 本地 toast 通知的默认和最长过期时间为 3 天。

```csharp
toast.ExpirationTime = DateTime.Now.AddDays(2);
```


## <a name="provide-a-primary-key-for-your-toast"></a>为 toast 提供主键

如要以编程方式删除或替换发送的通知，需使用 Tag 属性（还可选择使用 Group 属性）来为通知提供主键。 然后，你可以在以后使用此主键来删除或替换该通知。

要查看有关替换/删除已发送的 toast 通知的更多详细信息，请参阅[快速入门：在操作中心 (XAML) 中管理 toast 通知](https://msdn.microsoft.com/library/windows/apps/xaml/dn631260.aspx)。

Tag 和 Group 组合充当复合主键。 Group 是两者中较为通用的标识符，你可以用它来分配如“wallPosts”、“messages”、“friendRequests”等组。而 Tag 应该唯一标识组中的通知本身。 使用通用组时，可以使用 [RemoveGroup API](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) 删除该组中的所有通知。

```csharp
toast.Tag = "18365";
toast.Group = "wallPosts";
```


## <a name="send-the-notification"></a>发送通知

初始化 toast 之后，只需创建 [ToastNotifier](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotifier) 并调用 Show()，传入 toast 通知。

```csharp
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


## <a name="clear-your-notifications"></a>清除你的通知

UWP 应用负责删除和清除它们自己的通知。 当你的应用启动时，我们不会自动清除你的通知。

仅当用户显式单击通知时，Windows 才会自动删除该通知。

下面是消息传递应用应执行的操作的示例...

1. 用户收到关于对话中新消息的多个 toast
2. 用户点击其中一个 toast 以打开该对话
3. 应用打开该对话，然后清除该对话的所有 toast（方法是对该对话的应用提供的组使用 [RemoveGroup](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_)）
4. 用户的操作中心现在能正确反映通知状态，因为操作中心未留有该对话的过期通知。

若要了解有关清除所有通知或删除特定通知的信息，请参阅[快速入门：在操作中心 (XAML) 中管理 toast 通知](https://msdn.microsoft.com/library/windows/apps/xaml/dn631260.aspx)。


## <a name="handling-activation"></a>处理激活

在 Windows 10 中，当用户单击你的 toast 时，你可以用以下两种不同方式激活你的应用...

* 前台激活
* 后台激活

> [!NOTE]
> 如果使用 Windows 8.1 提供的旧版 toast 模板，则将调用 **OnLaunched** 而不是 **OnActivated**。 以下文档仅适用于使用通知库的新式 Windows 10 通知（或者，如果使用原始 XML，则使用 ToastGeneric 模板）。


### <a name="handling-foreground-activation"></a>处理前台激活

在 Windows 10 中，当用户单击新式 toast（或 toast 上的按钮）时，将使用新激活类型 **ToastNotification** 调用 **OnActivated** 而不是 **OnLaunched**。 因此，开发人员能够轻松区分 toast 激活并相应地执行任务。

在下面的示例中，你可以检索最初在 toast 内容中提供的参数字符串。 还可以检索用户在文本框和选择框中输入的内容。

> [!IMPORTANT]
> 必须按 **OnLaunched** 代码那样初始化框架和激活窗口。 **如果用户单击你的 toast ，则不会调用 OnLaunched**，即使你的应用已关闭并是首次启动也是如此。 通常建议将 **OnLaunched** 和 **OnActivated** 合并到你自己的 `OnLaunchedOrActivated` 方法中，因为二者中均需执行相同的初始化。

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Get the root frame
    Frame rootFrame = Window.Current.Content as Frame;
 
    // TODO: Initialize root frame just like in OnLaunched
 
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs)
    {
        var toastActivationArgs = e as ToastNotificationActivatedEventArgs;
                 
        // Parse the query string (using QueryString.NET)
        QueryString args = QueryString.Parse(toastActivationArgs.Argument);
 
        // See what action is being requested 
        switch (args["action"])
        {
            // Open the image
            case "viewImage":
 
                // The URL retrieved from the toast args
                string imageUrl = args["imageUrl"];
 
                // If we're already viewing that image, do nothing
                if (rootFrame.Content is ImagePage && (rootFrame.Content as ImagePage).ImageUrl.Equals(imageUrl))
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ImagePage), imageUrl);
                break;
                             
 
            // Open the conversation
            case "viewConversation":
 
                // The conversation ID retrieved from the toast args
                int conversationId = int.Parse(args["conversationId"]);
 
                // If we're already viewing that conversation, do nothing
                if (rootFrame.Content is ConversationPage && (rootFrame.Content as ConversationPage).ConversationId == conversationId)
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ConversationPage), conversationId);
                break;
        }
 
        // If we're loading the app for the first time, place the main page on
        // the back stack so that user can go back after they've been
        // navigated to the specific page
        if (rootFrame.BackStack.Count == 0)
            rootFrame.BackStack.Add(new PageStackEntry(typeof(MainPage), null, null));
    }
 
    // TODO: Handle other types of activation
 
    // Ensure the current window is active
    Window.Current.Activate();
}
```


## <a name="handling-background-activation"></a>处理后台激活

当你对你的 toast（或 toast 内的按钮）指定后台激活时，将执行后台任务而不是激活前台应用。

有关后台任务的详细信息，请参阅[使用后台任务支持应用](/launch-resume/support-your-app-with-background-tasks.md)。

如果你的目标版本是 14393 或更高版本，则可以使用进程内后台任务，这样可大大简化操作。 请注意，无法在较旧版本的 Windows 上运行进程内后台任务。 在此代码示例中，将使用进程内后台任务。

```csharp
const string taskName = "ToastBackgroundTask";

// If background task is already registered, do nothing
if (BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name.Equals(taskName)))
    return;

// Otherwise request access
BackgroundAccessStatus status = await BackgroundExecutionManager.RequestAccessAsync();

// Create the background task
BackgroundTaskBuilder builder = new BackgroundTaskBuilder()
{
    Name = taskName
};

// Assign the toast action trigger
builder.SetTrigger(new ToastNotificationActionTrigger());

// And register the task
BackgroundTaskRegistration registration = builder.Register();
```


然后，与前台激活一样，在你的 App.xaml.cs 中覆盖你可检索预定义参数和用户输入的 OnBackgroundActivated。

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "ToastBackgroundTask":
            var details = args.TaskInstance.TriggerDetails as ToastNotificationActionTriggerDetail;
            if (details != null)
            {
                string arguments = details.Argument;
                var userInput = details.UserInput;

                // Perform tasks
            }
            break;
    }
 
    deferral.Complete();
}
```



## <a name="plain-vanilla-code-snippets"></a>纯“Vanilla”代码段

如果不从 NuGet 中使用通知库，可按如下所示的方法手动构造 XML，以创建 [ToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)。

```csharp
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;

// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "http://blogs.msdn.com/cfs-filesystemfile.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-01-71-81-permanent/2727.happycanyon1_5B00_1_5D00_.jpg";
string logo = "ms-appdata:///local/Andrew.jpg";
 
// TODO: all values need to be XML escaped
 
// Construct the visuals of the toast
string toastVisual =
$@"<visual>
  <binding template='ToastGeneric'>
    <text>{title}</text>
    <text>{content}</text>
    <image src='{image}'/>
    <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
  </binding>
</visual>";

// In a real app, these would be initialized with actual data
int conversationId = 384928;
 
// Generate the arguments we'll be passing in the toast
string argsReply = $"action=reply&conversationId={conversationId}";
string argsLike = $"action=like&conversationId={conversationId}";
string argsView = $"action=viewImage&imageUrl={Uri.EscapeDataString(image)}";
 
// TODO: all args need to be XML escaped
 
string toastActions =
$@"<actions>
 
  <input
      type='text'
      id='tbReply'
      placeHolderContent='Type a response'/>
 
  <action
      content='Reply'
      arguments='{argsReply}'
      activationType='background'
      imageUri='Assets/Reply.png'
      hint-inputId='tbReply'/>
 
  <action
      content='Like'
      arguments='{argsLike}'
      activationType='background'/>
 
  <action
      content='View'
      arguments='{argsView}'/>
 
</actions>";

// Now we can construct the final toast content
string argsLaunch = $"action=viewConversation&conversationId={conversationId}";
 
// TODO: all args need to be XML escaped
 
string toastXmlString =
$@"<toast launch='{argsLaunch}'>
    {toastVisual}
    {toastActions}
</toast>";
 
// Parse to XML
XmlDocument toastXml = new XmlDocument();
toastXml.LoadXml(toastXmlString);
 
// Generate toast
var toast = new ToastNotification(toastXml);
```


## <a name="resources"></a>资源

* [GitHub 上的完整代码示例](https://github.com/WindowsNotifications/quickstart-sending-local-toast)
* [toast 内容文档](adaptive-interactive-toasts.md)
* [ToastNotification 类](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)
* [ToastNotificationActivatedEventArgs 类](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)
