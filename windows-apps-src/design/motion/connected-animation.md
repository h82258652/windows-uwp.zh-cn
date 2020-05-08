---
description: 连贯的动画让你可以通过为一个元素在两种不同视图之间的转换创建动画来创建动态且引人入胜的导航体验。
title: 连贯的动画
template: detail.hbs
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 385c11e48695c2486fd5a2b72633923454e2f8ea
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970632"
---
# <a name="connected-animation-for-windows-apps"></a>适用于 Windows 应用的连接动画

连贯的动画让你可以通过为一个元素在两种不同视图之间的转换创建动画来创建动态且引人入胜的导航体验。 这有助于用户维持其上下文并提供不同视图之间的连贯性。

在连接的动画中，在 UI 内容的更改过程中，在两个视图之间出现 "继续" 元素，在屏幕上，从其在源视图中的位置到新视图的目标位置。 这强调了视图间的通用内容，并在转换过程中创建了漂亮而动态的效果。

> **重要 api**： [ConnectedAnimation 类](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)、 [ConnectedAnimationService 类](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)


## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果已安装了<strong style="font-weight: semi-bold">XAML 控件库</strong>应用程序，请单击此处<a href="xamlcontrolsgallery:/item/ConnectedAnimation">打开应用程序，并查看连接动画的操作</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

在这个简短的视频中，应用使用连接的动画来对项图像进行动画处理，因为它 "继续" 变为下一页的标题。 该效果有助于在转换过程维持用户上下文。

![连贯动画](images/connected-animations/example.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>视频摘要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>连贯动画和 Fluent 设计系统

 Fluent Design System 可帮助你创建包含光线、深度、动画、材料和比例的现代粗体 UI。 连贯动画是 Fluent 设计系统的一个组成部分，它将动画添加到你的应用。 若要了解详细信息，请参阅 "[熟知设计概述](/windows/apps/fluent-design-system)"。

## <a name="why-connected-animation"></a>为何选择连贯动画？

在页面之间导航时，很重要的一点是让用户了解导航过后会出现哪些新内容，以及这些新内容与他们在导航时的意图有何关联。 连贯动画提供了一个强大的视觉隐喻，通过将用户的注意力转移到两个视图之间共享的内容，强调了二者之间的关系。 此外，连贯动画为页面导航增添了视觉效果和润色，这可以帮助让你的应用的动态设计与众不同。

## <a name="when-to-use-connected-animation"></a>何时使用连贯动画

连贯动画通常在更改页面时使用，但它们可被应用于你在更改 UI 中的内容时希望用户维持上下文的任何体验。 每当源视图和目标视图之间有共享的图像或其他 UI 元素时，你应该考虑使用连贯动画而不是[导航转换中的钻取](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)。

## <a name="configure-connected-animation"></a>配置连接动画

> [!IMPORTANT]
> 此功能要求应用的目标版本为 Windows 10 版本1809（[SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)）或更高版本。 配置属性在早期 Sdk 中不可用。 使用自适应代码或条件 XAML 可将低于 SDK 17763 的最低版本作为目标。 有关详细信息，请参阅[版本自适应应用](/windows/uwp/debug-test-perf/version-adaptive-apps)。

从 Windows 10 版本1809开始，连接的动画通过提供专为向前和向后翻页导航定制的动画配置，进一步体现的流畅设计。

可以通过在 ConnectedAnimation 上设置配置属性来指定动画配置。 （我们将在下一部分中显示此内容的示例。）

下表介绍了可用配置。 有关这些动画中应用的运动原则的详细信息，请参阅[方向性和重心](index.md)。

| [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) |
| - |
| 这是默认配置，建议用于向前导航。 |
当用户在应用中向前导航（A 到 B）时，连接的元素将显示为 "以物理方式拖离页面"。 在此过程中，元素显示为在 z 空间中向前移动，并将某个位删除为重力保持效果。 为了克服重心的影响，元素会获得速度并加速到其最终位置。 结果为 "规模和 dip" 动画。 |

| [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) |
| - |
| 当用户在应用中向后导航（B 到 A）时，动画更直接。 连接的元素以线性方式从 B 转换为，使用减速三次方缓动函数。 向后的视觉对象 affordance 尽可能快地将用户恢复到以前的状态，同时仍保持导航流的上下文。 |

| [BasicConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.basicconnectedanimationconfiguration) |
| - |
| 这是 Windows 10 版本1809（[SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)）之前的版本中使用的默认动画（仅限）。 |

### <a name="connectedanimationservice-configuration"></a>ConnectedAnimationService 配置

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)类具有两个适用于各个动画的属性，而不是整个服务。

- [DefaultDuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

为了实现各种效果，某些配置将忽略 ConnectedAnimationService 上的这些属性，并改为使用自己的值，如此表中所述。

| Configuration | 是否尊重 DefaultDuration？ | 是否尊重 DefaultEasingFunction？ |
| - | - | - |
| 引力 | 是 | 是* <br/> **从 A 到 B 的基本转换使用此缓动函数，但 "重力 dip" 具有其自己的缓动函数。*  |
| 直接 | 否 <br/> *在150ms 上进行动画处理。*| 否 <br/> *使用减速缓动函数。* |
| 基本 | 是 | 是 |

## <a name="how-to-implement-connected-animation"></a>如何实现连接动画

设置连贯动画涉及两个步骤：

1. 在 "源" 页上*准备*一个动画对象，该对象向系统指示源元素将参与连接的动画。
1. 在 "目标" 页上*启动*动画，并向目标元素传递引用。

在源页中导航时，请调用[ConnectedAnimationService GetForCurrentView](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview)获取 ConnectedAnimationService 的实例。 若要准备动画，请在此实例上调用[PrepareToAnimate](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate) ，并传入要在转换中使用的唯一键和 UI 元素。 可以在 "目标" 页上稍后检索动画。

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

导航发生时，在 "目标" 页中启动动画。 要启动该动画，请调用 [ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart)。 你可以通过使用你在创建动画时提供的唯一密钥调用 [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation)，来检索正确的动画实例。

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>前进导航

此示例演示如何使用 ConnectedAnimationService 为两个页面（Page_A 到 Page_B）之间的向前导航创建过渡。

为向前导航推荐的动画配置为[GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration)。 这是默认设置，因此，除非要指定不同的配置，否则不需要设置[配置](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration)属性。

在 "源" 页中设置动画。

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

在 "目标" 页中启动动画。

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

对于后退导航（Page_B Page_A），请遵循相同的步骤，但会反转源页和目标页。

当用户向后导航时，他们希望应用程序尽快返回到以前的状态。 因此，推荐的配置为[DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration)。 此动画更快、更直接，并使用减速缓动。

在 "源" 页中设置动画。

```csharp
// Page_B.xaml.cs

protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    if (e.NavigationMode == NavigationMode.Back)
    {
        ConnectedAnimation animation = 
            ConnectedAnimationService.GetForCurrentView().PrepareToAnimate("backAnimation", DestinationImage);

        // Use the recommended configuration for back animation.
        animation.Configuration = new DirectConnectedAnimationConfiguration();
    }
}
```

在 "目标" 页中启动动画。

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

在动画的设置时间和开始时间之间，源元素显示在应用中的其他 UI 之上。 这使您可以同时执行任何其他转换动画。 出于此原因，不应在两个步骤之间等待大约250毫秒，因为存在源元素可能会造成混乱。 如果你准备一个动画且并未在三秒内启动它，则系统将释放该动画，且任何对 [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) 的后续调用将失败。

## <a name="connected-animation-in-list-and-grid-experiences"></a>列表和网格体验中的连贯动画

通常，你会想要以列表或网格控件为源或目标创建连贯动画。 可以在[ListView](/uwp/api/windows.ui.xaml.controls.listview)和[GridView](/uwp/api/windows.ui.xaml.controls.gridview)（ [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation)和[TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)）上使用这两个方法来简化此过程。

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

若要使用对应于给定列表项的椭圆准备连接的动画，请使用唯一键、项和名称 "PortraitEllipse" 调用[PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation)方法。

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

若要使用此元素作为目标来启动动画（如从详细信息视图向后导航时），请使用[TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)。 如果你刚为 ListView 加载了数据源，TryStartConnectedAnimationAsync 将会等到相应的项目容器已被创建时才启动动画。

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

*协调动画*是一种特殊类型的入口动画，其中元素与连接的动画目标同时出现，并在屏幕上移动连接的动画元素时与连接的动画元素一起进行动画处理。 协调动画可以向一个转换添加多个视觉效果，并进一步将用户的注意力转移到源视图和目标视图之间共享的上下文。 在这些图像中，该项目的标题 UI 使用协调动画创建动画。

当协调动画使用重力配置时，重心将应用于连接的动画元素和协调的元素。 协调的元素将与连接的元素 "一次性"，使元素保持真正的协调。

使用 **TryStart** 的双参数过载将协调元素添加至连贯动画。 此示例演示名为 "DescriptionRoot" 的网格布局的协调动画，该布局与名为 "CoverImage" 的连接动画元素一起进入。

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
- 使用[GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration)进行前向导航。
- 使用[DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration)进行后退导航。
- 请不要等待网络请求或其他长时间运行的异步操作在准备和启动连接的动画之间。 你可能需要预加载必要信息以提前运行转换，或在将高分辨率图像加载到目标视图中时使用低分辨率占位符图像。
- 如果你使用的是**ConnectedAnimationService**，则可以使用[SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo)来防止**帧**中的转换动画，因为连接的动画不应同时与默认导航转换一起使用。 请参阅 [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) 详细了解如何使用导航转换。

## <a name="related-articles"></a>相关文章

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)
