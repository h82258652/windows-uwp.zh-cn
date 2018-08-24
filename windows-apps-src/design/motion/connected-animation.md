---
author: mijacobs
description: 连贯动画让你可以通过为一个元素在两种不同视图之间的转换创建动画来创建动态和引入注目的导航体验。
title: 连贯动画
template: detail.hbs
ms.author: jimwalk
ms.date: 10/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: a789a8f082192b79b3e96990827f9a4f6a0eacbc
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "2834637"
---
# <a name="connected-animation-for-uwp-apps"></a>适用于 UWP 应用的连贯动画

## <a name="what-is-connected-animation"></a>什么是连贯动画？

连贯动画让你可以通过为一个元素在两种不同视图之间的转换创建动画来创建动态和引入注目的导航体验。 这有助于用户维持其上下文并提供不同视图之间的连贯性。
在连贯动画中，当 UI 内容发生变化时，元素似乎在两种不同视图之间保持“连贯性”，从其在源视图中的位置掠过屏幕，到达其在新视图中的目标位置。 这强调了不同视图之间的共同内容，并创建了转换过程中美观且动态的效果。

## <a name="see-it-in-action"></a>查看实际操作

在这段简短的视频中，应用使用连贯动画来为一个正在“继续”变成下一页标题中一部分的项目图像制作动画。 该效果有助于在转换过程维持用户上下文。

![UI 连续运动示例](images/continuous3.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>视频摘要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>连贯动画和 Fluent 设计系统

 Fluent 设计系统可帮助你创建包含光线、深度、动画、材料和比例的现代、粗体 UI。 连贯动画是 Fluent 设计系统的一个组成部分，它将动画添加到你的应用。 要了解详细信息，请参阅 [UWP 的 Fluent 设计概述](../fluent-design-system/index.md)。

## <a name="why-connected-animation"></a>为何选择连贯动画？

在页面之间导航时，很重要的一点是让用户了解导航过后会出现哪些新内容，以及这些新内容与他们在导航时的意图有何关联。 连贯动画提供了一个强大的视觉隐喻，通过将用户的注意力转移到两个视图之间共享的内容，强调了二者之间的关系。 此外，连贯动画为页面导航增添了视觉效果和润色，这可以帮助让你的应用的动态设计与众不同。

## <a name="when-to-use-connected-animation"></a>何时使用连贯动画

连贯动画通常在更改页面时使用，但它们可被应用于你在更改 UI 中的内容时希望用户维持上下文的任何体验。 每当源视图和目标视图之间有共享的图像或其他 UI 元素时，你应该考虑使用连贯动画而不是[导航转换中的钻取](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx)。

## <a name="how-to-implement"></a>如何实施

设置连贯动画涉及两个步骤：

1.  *准备*源页面上的动画对象，这向系统表明源元素将参与连贯动画 
2.  *启动*目标页面上的动画，将参考传递到目标元素

在这两个步骤之间，源元素将以冻结状态显示在应用中的其他 UI 上方，让你可以同时执行任何其他转换动画。 出于此原因，你在两个步骤之间不应等待超过 250 毫秒，因为源元素的存在可能会让人分心。 如果你准备一个动画且并未在三秒内启动它，则系统将释放该动画，且任何对 **TryStart** 的后续调用将失败。

你可以通过调用 **ConnectedAnimationService.GetForCurrentView** 来访问当前的 ConnectedAnimationService 实例。 要准备动画，请在此实例上调用 **PrepareToAnimate**，将参考传递到你想用在转换中的唯一密钥和 UI 元素。 该唯一密钥让你可以稍后检索动画，例如在不同页面上检索。

```csharp
ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("image", SourceImage);
```

要启动该动画，请调用 **ConnectedAnimation.TryStart**。 你可以通过使用你在创建动画时提供的唯一密钥调用 **ConnectedAnimationService.GetAnimation**，来检索正确的动画实例。

```csharp
ConnectedAnimation imageAnimation = 
    ConnectedAnimationService.GetForCurrentView().GetAnimation("image");
if (imageAnimation != null)
{
    imageAnimation.TryStart(DestinationImage);
}
```

下面是使用 ConnectedAnimationService 创建两页之间的转换的完整示例。

*SourcePage.xaml*

```xaml
<Image x:Name="SourceImage"
       Width="200"
       Height="200"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

*SourcePage.xaml.cs*

```csharp
private void NavigateToDestinationPage()
{
    ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("image", SourceImage);
    Frame.Navigate(typeof(DestinationPage));
}
```

*DestinationPage.xaml*

```xaml
<Image x:Name="DestinationImage"
       Width="400"
       Height="400"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

*DestinationPage.xaml.cs*

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation imageAnimation = 
        ConnectedAnimationService.GetForCurrentView().GetAnimation("image");
    if (imageAnimation != null)
    {
        imageAnimation.TryStart(DestinationImage);
    }
}
```

## <a name="connected-animation-in-list-and-grid-experiences"></a>列表和网格体验中的连贯动画

通常，你会想要以列表或网格控件为源或目标创建连贯动画。 您可以使用 **ListView** 和 **GridView**、**PrepareConnectedAnimation** 和 **TryStartConnectedAnimationAsync** 这两种新方法来简化此过程。
例如，如果你有一个在其数据模板中包含一个名为“PortraitEllipse”的元素的 **ListView**。

```xaml
<ListView x:Name="ContactsListView" Loaded="ContactsListView_Loaded">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="vm:ContactsItem">
            <Grid>
                …
                <Ellipse x:Name="PortraitEllipse" … />
            </Grid>
        </DataTemplate> 
    </ListView.ItemTemplate>
</ListView>
```

要使用对应于给定列表项目的椭圆准备一个连贯动画，请使用唯一密钥、该项目和名称“PortraitEllipse”调用  **PrepareConnectedAnimation** 方法。

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

或者，若要以该元素为目标启动一个动画，例如从一个详细信息视图返回时，请使用 **TryStartConnectedAnimationAsync**。 如果你刚为 **ListView** 加载了数据源，**TryStartConnectedAnimationAsync** 将会等到相应的项目容器已被创建时才启动动画。

```csharp
private void ContactsListView_Loaded(object sender, RoutedEventArgs e)
{
    ContactsItem item = GetPersistedItem(); // Get persisted item
    if (item != null)
    {
        ContactsListView.ScrollIntoView(item);
        ConnectedAnimation animation = 
            ConnectedAnimationService.GetForCurrentView().GetAnimation("portrait");
        if (animation != null)
        {
            await ContactsListView.TryStartConnectedAnimationAsync(
                animation, item, "PortraitEllipse");
        }
    }
}
```

## <a name="coordinated-animation"></a>协调动画

![协调动画](images/connected-animations/coordinated_example.gif)

<!--
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=9066bbbe%2Dcf58%2D4ab4%2Db274%2D595616f5d0a0&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

一个*协调动画*是一种特殊类型的进入动画，其中一个元素将与连贯动画目标一并显示，当连贯动画元素在屏幕上移动时创建动画。 协调动画可以向一个转换添加多个视觉效果，并进一步将用户的注意力转移到源视图和目标视图之间共享的上下文。 在这些图像中，该项目的标题 UI 使用协调动画创建动画。

使用 **TryStart** 的双参数过载将协调元素添加至连贯动画。 该示例演示了一个名为“DescriptionRoot”且将与名为“CoverImage”的连贯动画一同进入的网格布局的协调动画。

*DestinationPage.xaml*

```xaml
<Grid>
    <Image x:Name="CoverImage" />
    <Grid x:Name="DescriptionRoot" />
</Grid>
```

*DestinationPage.xaml.cs*

```csharp
void OnNavigatedTo(NavigationEventArgs e)
{
    var animationService = ConnectedAnimationService.GetForCurrentView();
    var animation = animationService.GetAnimation("coverImage");
    
    if (animation != null)
    {
        // Don’t need to capture the return value as we are not scheduling any subsequent
        // animations
        animation.TryStart(CoverImage, new UIElement[] { DescriptionRoot });
     }
}
```

## <a name="dos-and-donts"></a>应做事项和禁止事项

- 在源页面和目标页面之间有共享元素的页面转换中使用连贯动画。
- 请勿等待准备和启动连贯动画之间的网络请求或其他长时间运行的异步操作。 你可能需要预加载必要信息以提前运行转换，或在将高分辨率图像加载到目标视图中时使用低分辨率占位符图像。
- 如果你正在使用 **ConnectedAnimationService**，请使用 [SuppressNavigationTransitionInfo](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) 来阻止**帧**中的转换动画，因为连贯动画不应与默认的导航转换同时使用。 请参阅 [NavigationThemeTransition](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx) 详细了解如何使用导航转换。


## <a name="download-the-code-samples"></a>下载代码示例

请参阅 [WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs) 示例库中的[连贯动画示例](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/ConnectedAnimationSample)。

## <a name="related-articles"></a>相关文章

- [ConnectedAnimation](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimation)
- [ConnectedAnimationService](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.connectedanimation.aspx)
- [NavigationThemeTransition](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx)
