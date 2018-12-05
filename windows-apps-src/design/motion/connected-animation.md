---
description: 连贯动画让你可以通过为一个元素在两种不同视图之间的转换创建动画来创建动态和引入注目的导航体验。
title: 连贯动画
template: detail.hbs
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10，uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ce639faac66e93b65a398e6d9cdc700546fc68ab
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8706365"
---
# <a name="connected-animation-for-uwp-apps"></a>适用于 UWP 应用的连贯动画

连贯动画让你可以通过为一个元素在两种不同视图之间的转换创建动画来创建动态和引入注目的导航体验。 这有助于用户维持其上下文并提供不同视图之间的连贯性。

连贯动画，在某一元素似乎以期间更改 UI 内容，在屏幕上从源视图中的位置掠过到达其在新视图中的目标中的两个视图之间"继续"。 这强调了不同视图之间的共同内容，并创建了转换过程中美观且动态的效果。

> **重要 Api**: [ConnectedAnimation 类](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)、 [ConnectedAnimationService 类](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

## <a name="see-it-in-action"></a>查看实际操作

在此简短视频中，应用使用连贯的动画来项目图像制作它正在"继续"变成下一页标题中一部分。 该效果有助于在转换过程维持用户上下文。

![连贯动画](images/connected-animations/example.gif)

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

## <a name="configure-connected-animation"></a>配置连贯的动画

> [!IMPORTANT]
> 此功能需要你的应用的目标版本为 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更高版本。 配置属性不是早期的 Sdk 中提供的。 你可以指定目标低于 SDK 17763 最低版本使用自适应代码或条件 XAML。 有关详细信息，请参阅[版本自适应应用](/debug-test-perf/version-adaptive-apps)。

在 Windows 10 版本 1809，启动连贯的动画进一步体现 Fluent design 通过提供动画配置定制专门为向前和向后页面导航。

通过在 ConnectedAnimation 上设置配置属性指定的动画配置。 （我们将介绍这方面的示例在下一节。）

此表介绍了可用的配置。 有关这些动画在应用的运动原则的详细信息，请参阅[方向性和引力的影响](index.md)。

| [GravityConnectedAnimationConfiguration]() |
| - |
| 这是默认配置，并适合向前导航。 |
用户前进 (A 到 B) 在应用中，会出现连接的元素以物理方式"提取页面关闭"。 在执行此操作，元素似乎在 z 空间向前移动，并为执行保留的重力效果有点丢弃。 若要克服引力的影响，元素获得速度，并加快了到其最终位置。 结果是"缩放和 dip"动画。 |

| [DirectConnectedAnimationConfiguration]() |
| - |
| 在用户向后导航应用 (从 B 到 A) 中，动画将更为直接。 连接的元素线性转换从 B 到使用减速三次方贝塞尔缓动函数。 向后可视化提示使用户返回到其之前的状态尽快同时仍维护上下文的导航流程。 |

| [BasicConnectedAnimationConfiguration]() |
| - |
| 这是默认值 （和仅） 在 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 之前的版本中使用的动画。 |

### <a name="connectedanimationservice-configuration"></a>ConnectedAnimationService 配置

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)类具有适用于单个动画，而不是整个服务的两个属性。

- [DefaultDuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

若要实现不同的效果，某些配置忽略这些属性上 ConnectedAnimationService，并改为此表中所述使用他们自己的值。

| 配置 | 方面 DefaultDuration？ | 方面 DefaultEasingFunction？ |
| - | - | - |
| 引力 | 是 | 是* <br/> **从 A 到 B 的基本转换使用此缓动函数，但是"重力 dip"具有其自己的缓动函数。*  |
| 直接 | 否 <br/> *进行动画处理超过 150 毫秒。*| 否 <br/> *使用减速缓动函数。* |
| 基本 | 是 | 是 |

## <a name="how-to-implement-connected-animation"></a>如何实现连贯的动画

设置连贯动画涉及两个步骤：

1. *准备*向系统表明源元素将参与连贯动画源页面上的动画对象。
1. *开始菜单*目标页上的动画将传递到目标元素的引用。

当从源页面导航时，调用[ConnectedAnimationService.GetForCurrentView](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview)获取的 ConnectedAnimationService 实例。 若要准备动画，在此情况下，调用[PrepareToAnimate](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate)并传入的唯一密钥和你想要在转换中使用的 UI 元素。 唯一的密钥，可以检索动画稍后在目标页。

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

导航发生时，请在目标页中启动动画。 要启动该动画，请调用 [ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart)。 你可以通过使用你在创建动画时提供的唯一密钥调用 [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation)，来检索正确的动画实例。

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>向前导航

此示例显示了如何使用 ConnectedAnimationService 创建两个页面 (Page_A 到 Page_B) 之间的前进导航的转换。

向前导航的推荐的动画配置是[GravityConnectedAnimationConfiguration]()。 这是默认值，因此无需设置[配置](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration)属性，除非你想要指定不同的配置。

设置源页中的动画。

```xaml
<!-- Page_A.xaml -->

<Image x:Name="SourceImage"
       HorizontalAlignment="Left" VerticalAlignment="Top"
       Width="200" Height="200"
       Stretch="Fill"
       Source="Assets/StoreLogo.png"
       PointerPressed="SourceImage_PointerPressed"/>
```

```csharp
// Page_A.xaml.cs

private void SourceImage_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    // Navigate to detail page.
    // Suppress the default animation to avoid conflict with the connected animation.
    Frame.Navigate(typeof(Page_B), null, new SuppressNavigationTransitionInfo());
}

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    ConnectedAnimationService.GetForCurrentView()
        .PrepareToAnimate("forwardAnimation", SourceImage);
    // You don't need to explicitly set the Configuration property because
    // the recommended Gravity configuration is default.
    // For custom animation, use:
    // animation.Configuration = new BasicConnectedAnimationConfiguration();
}
```

在目标页中启动动画。

```xaml
<!-- Page_B.xaml -->

<Image x:Name="DestinationImage"
       Width="400" Height="400"
       Stretch="Fill"
       Source="Assets/StoreLogo.png" />
```

```csharp
// Page_B.xaml.cs

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation animation =
        ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
    if (animation != null)
    {
        animation.TryStart(DestinationImage);
    }
}
```

### <a name="back-navigation"></a>后退导航

对于后退导航 (Page_B 到 Page_A)，请按照相同的步骤中，但反转，源和目标页面。

当用户导航回来时，他们预期应用尽快返回到之前的状态。 因此，建议的配置是[DirectConnectedAnimationConfiguration]()。 此动画更快、 更直接，并使用减速缓动。

设置源页中的动画。

```csharp
// Page_B.xaml.cs

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    if (e.NavigationMode == NavigationMode.Back)
    {
        ConnectedAnimationService.GetForCurrentView()
            .PrepareToAnimate("backAnimation", DestinationImage);

        // Use the recommended configuration for back animation.
        animation.Configuration = new DirectConnectedAnimationConfiguration();
    }
}
```

在目标页中启动动画。

```csharp
// Page_A.xaml.cs

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    ConnectedAnimation animation =
        ConnectedAnimationService.GetForCurrentView().GetAnimation("backAnimation");
    if (animation != null)
    {
        animation.TryStart(SourceImage);
    }
}
```

之间的时间的动画设置，并启动时，源元素将显示在应用中的其他 UI 上方冻结。 这允许你同时执行任何其他转换动画。 出于此原因，你不应等待超过 250 毫秒，两个步骤之间，因为源元素的存在可能会让人分心。 如果你准备一个动画且并未在三秒内启动它，则系统将释放该动画，且任何对 [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) 的后续调用将失败。

## <a name="connected-animation-in-list-and-grid-experiences"></a>列表和网格体验中的连贯动画

通常，你会想要以列表或网格控件为源或目标创建连贯动画。 可以在[ListView](/uwp/api/windows.ui.xaml.controls.listview)和[GridView](/uwp/api/windows.ui.xaml.controls.gridview)、 [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation)和[TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)，使用两种方法来简化此过程。

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

若要使用对应于给定的列表项目的椭圆准备一个连贯的动画，调用一个唯一密钥、 该项目和名称"PortraitEllipse" [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation)方法。

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

若要启动一个动画与此元素作为目标，如当导航后退详细信息视图使用[TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)。 如果你刚为 ListView 加载了数据源，TryStartConnectedAnimationAsync 将会等到相应的项目容器已被创建时才启动动画。

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

*协调的动画*是一种特殊类型的进入动画，其中某一元素似乎在屏幕上移动一同连贯的动画元素创建动画的连贯的动画目标以及。 协调动画可以向一个转换添加多个视觉效果，并进一步将用户的注意力转移到源视图和目标视图之间共享的上下文。 在这些图像中，该项目的标题 UI 使用协调动画创建动画。

当协调的动画使用了引力配置时，重力会连贯的动画元素和协调的元素。 协调的元素将"swoop"旁边的连接的元素使元素保持真正协调。

使用 **TryStart** 的双参数过载将协调元素添加至连贯动画。 此示例演示了一个名为"DescriptionRoot"且与名为"CoverImage"的连贯的动画进入配合使用的网格布局的协调的动画。

```xaml
<!-- DestinationPage.xaml -->
<Grid>
    <Image x:Name="CoverImage" />
    <Grid x:Name="DescriptionRoot" />
</Grid>
```

```csharp
// DestinationPage.xaml.cs
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
- 使用[GravityConnectedAnimationConfiguration]()向前导航。
- [DirectConnectedAnimationConfiguration]()用于后退导航。
- 请勿等待的网络请求或其他长时间运行准备和启动连贯的动画之间的异步操作。 你可能需要预加载必要信息以提前运行转换，或在将高分辨率图像加载到目标视图中时使用低分辨率占位符图像。
- 使用[SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo)阻止**帧**中的转换动画，如果你使用**ConnectedAnimationService**，因为连贯的动画不应与默认导航同时使用转换。 请参阅 [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) 详细了解如何使用导航转换。

## <a name="download-the-code-samples"></a>下载代码示例

请参阅 [WindowsUIDevLabs](https://github.com/Microsoft/WindowsUIDevLabs) 示例库中的[连贯动画示例](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2014393/ConnectedAnimationSample)。

## <a name="related-articles"></a>相关文章

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)
