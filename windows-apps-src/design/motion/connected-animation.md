---
description: 连贯动画让你可以通过为一个元素在两种不同视图之间的转换创建动画来创建动态和引入注目的导航体验。
title: 连贯的动画
template: detail.hbs
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: conrwi
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 901aa1fa5c37c18a815e5e70becdf15001ed74c4
ms.sourcegitcommit: cc0ef75f314658b14376eb60ef8e5bb4d7726e04
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2019
ms.locfileid: "65444239"
---
# <a name="connected-animation-for-uwp-apps"></a>适用于 UWP 应用的连贯动画

连贯动画让你可以通过为一个元素在两种不同视图之间的转换创建动画来创建动态和引入注目的导航体验。 这有助于用户维持其上下文并提供不同视图之间的连贯性。

在已连接动画中，元素似乎"继续"期间在用户界面内容中，在屏幕上从源视图中的位置飞到其目标的新视图中更改两个视图之间。 这强调在视图之间的常见内容，并为转换的一部分创建美观和动态效果。

> **重要的 API**：[ConnectedAnimation 类](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)， [ConnectedAnimationService 类](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)


## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果有<strong style="font-weight: semi-bold">XAML 控件库</strong>应用程序安装，请单击此处<a href="xamlcontrolsgallery:/item/ConnectedAnimation">打开应用并在操作中看到连接动画</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

在此简短视频中，应用使用连接的动画进行动画处理的项图像，随着它"继续"成为下一个页面的标头的一部分。 该效果有助于在转换过程维持用户上下文。

![连贯动画](images/connected-animations/example.gif)

<!-- 
<iframe width=640 height=360 src='https://microsoft.sharepoint.com/portals/hub/_layouts/15/VideoEmbedHost.aspx?chId=552c725c%2De353%2D4118%2Dbd2b%2Dc2d0584c9848&amp;vId=b2daa5ee%2Dbe15%2D4503%2Db541%2D1328a6587c36&amp;width=640&amp;height=360&amp;autoPlay=false&amp;showInfo=true' allowfullscreen></iframe>
-->

## <a name="video-summary"></a>视频摘要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev005/player]

## <a name="connected-animation-and-the-fluent-design-system"></a>连贯动画和 Fluent 设计系统

 Fluent 设计系统可帮助你创建包含光线、深度、动画、材料和比例的现代、粗体 UI。 连贯动画是 Fluent 设计系统的一个组成部分，它将动画添加到你的应用。 要了解详细信息，请参阅 [UWP 的 Fluent Design 概述](../fluent-design-system/index.md)。

## <a name="why-connected-animation"></a>为何选择连贯动画？

在页面之间导航时，很重要的一点是让用户了解导航过后会出现哪些新内容，以及这些新内容与他们在导航时的意图有何关联。 连贯动画提供了一个强大的视觉隐喻，通过将用户的注意力转移到两个视图之间共享的内容，强调了二者之间的关系。 此外，连贯动画为页面导航增添了视觉效果和润色，这可以帮助让你的应用的动态设计与众不同。

## <a name="when-to-use-connected-animation"></a>何时使用连贯动画

连贯动画通常在更改页面时使用，但它们可被应用于你在更改 UI 中的内容时希望用户维持上下文的任何体验。 每当源视图和目标视图之间有共享的图像或其他 UI 元素时，你应该考虑使用连贯动画而不是[导航转换中的钻取](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.navigationthemetransition.aspx)。

## <a name="configure-connected-animation"></a>配置连接的动画

> [!IMPORTANT]
> 此功能需要应用的目标版本是 Windows 10，版本 1809年 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更高版本。 配置属性不是在较早的 Sdk 中可用的。 可以针对最小值的版本低于 SDK 17763 使用自适应代码或条件 XAML。 有关详细信息，请参阅[版本自适应应用](/windows/uwp/debug-test-perf/version-adaptive-apps)。

从 Windows 10，版本 1809，开始进一步连接的动画体现 Fluent 设计动画，从而配置定制专门为向前和向后页面导航。

通过在 connectedanimation 的位置上设置配置属性指定动画配置。 （我们将介绍此方面的示例中的下一节。）

下表介绍可用的配置。 有关应用于这些动画的动作原则的详细信息，请参阅[方向性和重力](index.md)。

| [GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration) |
| - |
| 这是默认的配置，并建议向前导航。 |
当用户导航正向 (A 到 B) 在应用中的，已连接的元素将显示物理上"将离开页面"。 这样，元素似乎在 z 轴空间向前移动，有点删除作为重力采用保留的效果。 若要克服受重力影响，该元素获得速度和加速到其最终位置。 结果是一个"规模和 dip"动画。 |

| [DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration) |
| - |
| 当用户在应用程序 (B 到 A) 中向后导航，动画是更直接。 已连接的元素以线性方式将转换从 B 到使用在减速三次方贝塞尔缓动函数。 向后 visual 功能可见性： 使用户返回到其以前的状态尽可能快同时仍维护的上下文导航流。 |

| [BasicConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.basicconnectedanimationconfiguration) |
| - |
| 这是默认值 （仅和） 在 Windows 10，版本 1809年之前的版本中使用的动画 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk))。 |

### <a name="connectedanimationservice-configuration"></a>ConnectedAnimationService 配置

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)类具有两个属性，将应用于单个动画而不是整个服务。

- [DefaultDuration](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaultduration)
- [DefaultEasingFunction](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.defaulteasingfunction)

若要实现的不同效果，某些配置忽略 ConnectedAnimationService 的这些属性，并改为此表中所述使用他们自己的值。

| 配置 | 尊重 DefaultDuration？ | 尊重 DefaultEasingFunction？ |
| - | - | - |
| 引力 | 是 | 是* <br/> **从 A 到 B 的基本转换使用此缓动函数，但是"重力 dip"具有其自己的缓动函数。*  |
| 直接 | 否 <br/> *超过 150ms年之间进行动画处理。*| 否 <br/> *使用缓动函数在减速。* |
| 基本 | 是 | 是 |

## <a name="how-to-implement-connected-animation"></a>如何实现连接的动画

设置连贯动画涉及两个步骤：

1. *准备*动画对象在源页面上，向系统指示源元素将参与连接动画。
1. *启动*动画的目标页上，将传递到目标元素的引用。

当从源页中导航，调用[ConnectedAnimationService.GetForCurrentView](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getforcurrentview)来获取 ConnectedAnimationService 的实例。 若要准备动画，请调用[PrepareToAnimate](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.preparetoanimate)对此实例，并传入唯一键和你想要的转换中使用的 UI 元素。 唯一键，可以检索动画更高版本在目标页。

```csharp
ConnectedAnimationService.GetForCurrentView()
    .PrepareToAnimate("forwardAnimation", SourceImage);
```

当发生导航时，在目标页中启动动画。 要启动该动画，请调用 [ConnectedAnimation.TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart)。 你可以通过使用你在创建动画时提供的唯一密钥调用 [ConnectedAnimationService.GetAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice.getanimation)，来检索正确的动画实例。

```csharp
ConnectedAnimation animation =
    ConnectedAnimationService.GetForCurrentView().GetAnimation("forwardAnimation");
if (animation != null)
{
    animation.TryStart(DestinationImage);
}
```

### <a name="forward-navigation"></a>向前导航

此示例演示如何使用 ConnectedAnimationService 创建两个页面 (Page_A 到 Page_B) 之间的向前导航的转换。

建议的动画配置为前进导航[GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration)。 这是默认值，因此不需要设置[配置](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.configuration)属性除非想要指定不同的配置。

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

后退导航 (Page_B 到 Page_A)，你遵循相同的步骤，但源和目标页面进行了互换。

当用户导航回时，它们期望该应用程序尽可能快地返回到以前的状态。 因此，建议的配置是[DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration)。 此动画更快、 更直接，并使用在减速的缓动。

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

之间的时间设置动画，并会在启动时，源元素出现上面的应用中其他 UI 冻结。 这样可以同时执行任何其他过渡动画。 出于此原因，您不应等待多个 ~ 250 毫秒之间的两个步骤，因为源元素存在可能会变得让人分散注意力。 如果你准备一个动画且并未在三秒内启动它，则系统将释放该动画，且任何对 [TryStart](/uwp/api/windows.ui.xaml.media.animation.connectedanimation.trystart) 的后续调用将失败。

## <a name="connected-animation-in-list-and-grid-experiences"></a>列表和网格体验中的连贯动画

通常，你会想要以列表或网格控件为源或目标创建连贯动画。 您可以使用两种方法在[ListView](/uwp/api/windows.ui.xaml.controls.listview)并[GridView](/uwp/api/windows.ui.xaml.controls.gridview)， [PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation)并[TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)，若要简化此过程。

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

若要使用与给定的列表项相对应的椭圆准备连接的动画，调用[PrepareConnectedAnimation](/uwp/api/windows.ui.xaml.controls.listviewbase.prepareconnectedanimation)方法与唯一键、 项和"PortraitEllipse"的名称。

```csharp
void PrepareAnimationWithItem(ContactsItem item)
{
     ContactsListView.PrepareConnectedAnimation("portrait", item, "PortraitEllipse");
}
```

若要启动动画与此元素为目标，例如当导航回从详细信息视图中，使用[TryStartConnectedAnimationAsync](/uwp/api/windows.ui.xaml.controls.listviewbase.trystartconnectedanimationasync)。 如果只是您已加载 ListView 数据源，TryStartConnectedAnimationAsync 将等待，直到已创建相应的项容器启动这个动画。

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

一个*协调动画*是一种特殊的出场动画元素以及连接的动画目标，在屏幕上移动动画结合使用的连接的动画元素的显示位置。 协调动画可以向一个转换添加多个视觉效果，并进一步将用户的注意力转移到源视图和目标视图之间共享的上下文。 在这些图像中，该项目的标题 UI 使用协调动画创建动画。

当协调的动画使用重力配置时，重力被应用于连接的动画元素和协调的元素。 协调的元素将"swoop"旁边的连接元素使元素保持真正协调。

使用 **TryStart** 的双参数过载将协调元素添加至连贯动画。 此示例演示一个协调的动画的名为"DescriptionRoot"具有名为"CoverImage"的连接的动画元素中一前一后输入的网格布局。

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
- 使用[GravityConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration)向前导航。
- 使用[DirectConnectedAnimationConfiguration](/uwp/api/windows.ui.xaml.media.animation.directconnectedanimationconfiguration)重新导航。
- 不等待网络请求或在准备和开始连接的动画之间其他长时间运行的异步操作。 你可能需要预加载必要信息以提前运行转换，或在将高分辨率图像加载到目标视图中时使用低分辨率占位符图像。
- 使用[SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo)以防止在转换动画**帧**如果你使用的**ConnectedAnimationService**，因为连接动画不应该同时使用的默认导航过渡。 请参阅 [NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition) 详细了解如何使用导航转换。

## <a name="related-articles"></a>相关文章

[ConnectedAnimation](/uwp/api/windows.ui.xaml.media.animation.connectedanimation)

[ConnectedAnimationService](/uwp/api/windows.ui.xaml.media.animation.connectedanimationservice)

[NavigationThemeTransition](/uwp/api/Windows.UI.Xaml.Media.Animation.NavigationThemeTransition)
