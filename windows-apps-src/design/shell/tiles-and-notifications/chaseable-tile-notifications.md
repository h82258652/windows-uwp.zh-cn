---
author: andrewleader
Description: Use chaseable tile notifications to find out what your app displayed on its Live Tile when the user clicked it.
title: 可追踪的磁贴通知
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tile notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 06/13/2017
ms.topic: article
keywords: windows 10, uwp, 可追踪的磁贴, 动态磁贴, 可追踪的磁贴通知
ms.localizationpriority: medium
ms.openlocfilehash: 8126755dfb6f5f0e117d10daef85a83e8a171f1f
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5819861"
---
# <a name="chaseable-tile-notifications"></a>可追踪的磁贴通知

使用可追踪的磁贴通知可以由你决定用户单击磁贴时，应用的动态磁贴显示哪个磁贴通知。  
例如，新应用可以使用此功能来决定用户启动应用时其动态磁贴显示哪一篇新闻报道，这样可以确保文章突出显示，以便用户找到它。 

> [!IMPORTANT]
> **需要周年更新**：若要通过 C#、C++ 或基于 VB 的 UWP 应用使用可追踪的磁贴通知，必须面向 SDK 14393 且必须运行版本 14393 或更高版本。 对于基于 JavaScript 的 UWP 应用，必须面向 SDK 17134 且必须运行版本 17134 或更高版本。 


> **重要 API**：[LaunchActivatedEventArgs.TileActivatedInfo 属性](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo)、[TileActivatedInfo 类](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)


## <a name="how-it-works"></a>工作原理

若要启用可追踪的磁贴通知，请使用磁贴通知有效负载上的 **Arguments** 属性（类似于 toast 通知有效负载的 launch 属性）嵌入磁贴通知中的内容信息。

当应用通过动态磁贴启动时，系统将从当前/最近显示的磁贴通知返回参数列表。


## <a name="when-to-use-chaseable-tile-notifications"></a>何时使用可追踪磁贴通知

当你在动态磁贴上使用通知队列时（说明你在循环使用多达 5 个不同通知），通常使用可追踪磁贴通知。 另外，当动态磁贴上的内容可能与应用中的最新内容不同步时，可追踪磁贴通知也很有帮助。 例如，“新闻”应用每 30 分钟刷新一次动态磁贴，但当该应用启动时，它会加载最新的新闻（可能不会包括上次轮询间隔中磁贴上的内容）。 发生这种情况时，用户可能会因为无法找到他们在动态磁贴上看见的新闻而感到沮丧。 这就是可追踪磁贴通知的用处所在，借助它可以确保用户能够轻松找到他们在磁贴上看到的内容。

## <a name="what-to-do-with-a-chaseable-tile-notifications"></a>对可追踪磁贴通知的操作

请注意，最重要的是，在大多数情况下，当用户单击磁贴时，**不应直接导航到磁贴上的特定通知**。 动态磁贴用作的是应用程序的入口点。 当用户单击动态磁贴时可能有两种情况：(1) 他们希望正常启动应用或 (2) 他们希望看到有关动态磁贴上特定通知的详细信息。 由于无法让用户明确表示他们所需的是哪种行为，所以理想的体验是**正常启动应用，并确保用户可以轻松找到其看到的通知**。

例如，单击“MSN 资讯”应用的动态磁贴可正常启动应用：它会显示主页或用户之前阅读的文章。 但是，在主页上，该应用可确保动态磁贴上显示的新闻可以轻松找到。 这样即可支持以下两种情况：仅想要启动/继续使用该应用的情况，以及想要查看具体新闻的情况。


## <a name="how-to-include-the-arguments-property-in-your-tile-notification-payload"></a>如何在磁贴通知有效负载中包含 Arguments 属性

在通知有效负载中，Arguments 属性让应用能够提供以后可用来识别通知的数据。 例如，参数可能包括新闻 ID，这样，当应用启动时，即可检索并显示该新闻。 该属性接受字符串，根据需要可以进行序列化（查询字符串、JSON 等），但通常建议采用查询字符串格式，因为这是轻量的，采用 XML 进行精细编码。

该属性可对 **TileVisual** 和 **TileBinding** 元素进行设置并向下级联。 如果希望每个磁贴大小采用相同的参数，只需对 **TileVisual** 设置参数。 如果需要对特定磁贴大小使用特定参数，可对个别 **TileBinding** 元素设置参数。

本示例创建使用 Arguments 属性的通知有效负载，以便以后识别通知。 

```csharp
// Uses the following NuGet packages
// - Microsoft.Toolkit.Uwp.Notifications
// - QueryString.NET
 
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        // These arguments cascade down to Medium and Wide
        Arguments = new QueryString()
        {
            { "action", "storyClicked" },
            { "story", "201c9b1" }
        }.ToString(),
 
 
        // Medium tile
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                // Omitted
            }
        },
 
 
        // Wide tile is same as Medium
        TileWide = new TileBinding() { /* Omitted */ },
 
 
        // Large tile is an aggregate of multiple stories
        // and therefore needs different arguments
        TileLarge = new TileBinding()
        {
            Arguments = new QueryString()
            {
                { "action", "storiesClicked" },
                { "story", "43f939ag" },
                { "story", "201c9b1" },
                { "story", "d9481ca" }
            }.ToString(),
 
            Content = new TileBindingContentAdaptive() { /* Omitted */ }
        }
    }
};
```


## <a name="how-to-check-for-the-arguments-property-when-your-app-launches"></a>在应用启动时如何检查参数属性

大部分应用都有一个包含 [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) 方法的重写方法的 App.xaml.cs 文件。 顾名思义，应用在启动时调用此方法。 它使用一个参数，即 [LaunchActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs) 对象。

LaunchActivatedEventArgs 对象有一个属性支持可追踪通知，那就是 [TileActivatedInfo 属性](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo)，它提供对 [TileActivatedInfo 对象](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)的访问。 当用户从应用磁贴（而不是应用列表、搜索或任何其他入口点）启动应用时，它初始化此属性。

[TileActivatedInfo 对象](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)包含一个名为 [RecentlyShownNotifications](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo.RecentlyShownNotifications) 的属性，其中包含在前 15 分钟内在磁贴上显示的通知的列表。 该列表中的第一项表示磁贴的当前通知，后面的项表示用户在当前通知之前看过的通知。 如果已清除磁贴，则该列表为空。

每个 ShownTileNotificationhas Argumentsproperty。 Argumentsproperty 将初始化 argumentsstring 从磁贴通知负载，或者，如果负载不包括 argumentsstring。

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs args)
{
    // If the API is present (doesn't exist on 10240 and 10586)
    if (ApiInformation.IsPropertyPresent(typeof(LaunchActivatedEventArgs).FullName, nameof(LaunchActivatedEventArgs.TileActivatedInfo)))
    {
        // If clicked on from tile
        if (args.TileActivatedInfo != null)
        {
            // If tile notification(s) were present
            if (args.TileActivatedInfo.RecentlyShownNotifications.Count > 0)
            {
                // Get arguments from the notifications that were recently displayed
                string[] allArgs = args.TileActivatedInfo.RecentlyShownNotifications
                .Select(i => i.Arguments)
                .ToArray();
 
                // TODO: Highlight each story in the app
            }
        }
    }
 
    // TODO: Initialize app
}
```


## <a name="raw-xml-example"></a>原始 XML 示例

如果使用原始 XML 而不是通知库，请查看下面的 XML。

```xml
<tile>
  <visual arguments="action=storyClicked&amp;story=201c9b1">
 
    <binding template="TileMedium">
       
      <text>Kitten learns how to drive a car...</text>
      ... (omitted)
     
    </binding>
 
    <binding template="TileWide">
      ... (same as Medium)
    </binding>
     
    <!--Large tile is an aggregate of multiple stories-->
    <binding
      template="TileLarge"
      arguments="action=storiesClicked&amp;story=43f939ag&amp;story=201c9b1&amp;story=d9481ca">
   
      <text>Can your dog understand what you're saying?</text>
      ... (another story)
      ... (one more story)
   
    </binding>
 
  </visual>
</tile>
```



## <a name="related-articles"></a>相关文章

- [LaunchActivatedEventArgs.TileActivatedInfo 属性](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs#Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_TileActivatedInfo_)
- [TileActivatedInfo 类](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)