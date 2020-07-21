---
ms.assetid: 333f67f5-f012-4981-917f-c6fd271267c6
description: 此案例研究以书店提供的信息为基础，以 LongListSelector 中显示分组数据的 Windows Phone Silverlight 应用开始。
title: Windows Phone Silverlight 到 UWP 案例研究，Bookstore2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1d1440bf3cfded6b50eb58feffd322ea484e488a
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260107"
---
# <a name="windowsphone-silverlight-to-uwp-case-study-bookstore2"></a>Windows Phone Silverlight 到 UWP 案例研究： Bookstore2


此案例研究（在[Bookstore1](wpsl-to-uwp-case-study-bookstore1.md)中提供的信息的基础上）以在**LongListSelector**中显示分组数据的 Windows Phone Silverlight 应用开始。 在视图模型中，类 **Author** 的每个实例都表示一组由该作者创作的书籍，而在 **LongListSelector** 中，我们可以按作者查看分组书籍的列表，或者可以缩小到可以看到包含作者的跳转列表。 与在书籍列表中上下滚动相比，跳转列表提供了更快速的浏览方式。 我们逐步介绍如何将应用程序移植到 Windows 10 通用 Windows 平台（UWP）应用程序。

**请注意**   在 Visual studio 中打开 Bookstore2Universal\_10 时，如果看到消息 "需要 Visual studio 更新"，请按照在[TargetPlatformVersion](w8x-to-uwp-troubleshooting.md)中设置目标平台版本的步骤进行操作。

## <a name="downloads"></a>下载

[下载 Bookstore2WPSL8 Windows Phone Silverlight 应用程序](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2WPSL8)。

[下载 Bookstore2Universal\_10 Windows 10 应用](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10)。

##  <a name="the-windowsphone-silverlight-app"></a>Windows Phone Silverlight 应用

下图显示了 Bookstore2WPSL8（我们要移植的应用）的外观。 它是垂直滚动的 **LongListSelector**，其中包含按作者进行分组的书籍。 你可以缩小到跳转列表，并且可以从该列表导航回任一组。 此应用包含两个主要部分：提供分组数据源的视图模型，以及绑定到该视图模型的用户界面。 正如我们所看到的，这两个部分都可以轻松地从 Windows Phone Silverlight 技术到通用 Windows 平台（UWP）。

![Bookstore2WPSL8 的外观](images/wpsl-to-uwp-case-studies/c02-01-wpsl-how-the-app-looks.png)

##  <a name="porting-to-a-windows10-project"></a>移植到 Windows 10 项目

可快速完成以下任务：在 Visual Studio 中创建新项目、将文件从 Bookstore2WPSL8 复制到其中并将已复制的文件包含在新项目中。 首先创建一个新的空白应用程序（Windows 通用）项目。 将其命名为 Bookstore2Universal\_10。 这些是要从 Bookstore2WPSL8 复制到 Bookstore2Universal\_10 的文件。

-   复制包含书籍封面图像 PNG 文件的文件夹（文件夹 \\资产\\CoverImages）。 复制该文件夹后，在 **“解决方案资源管理器”** 中，请确保将 **“显示所有文件”** 切换为打开。 右键单击你复制的文件夹，然后单击**包括在项目中**。 该命令的意思是将文件或文件夹“包括”在某个项目中。 每次你复制文件或文件夹时，请在**解决方案资源管理器**中单击**刷新**，然后将文件或文件夹包括在项目中。 无需为你将在目标位置替换的文件执行此操作。
-   复制包含视图模型源文件的文件夹（文件夹为 \\ViewModel）。
-   复制 MainPage.xaml 并替换目标位置中的文件。

我们可以在 Windows 10 项目中保留 Visual Studio 为我们生成的 App.xaml.cs 和应用程序。

编辑刚刚复制的源代码和标记文件，并将对 Bookstore2WPSL8 命名空间的所有引用更改为 Bookstore2Universal\_10。 执行此操作的快速方法是使用**在文件中替换**功能。 在视图模型源文件的强制性代码中，需要进行以下移植更改。

-   将 `System.ComponentModel.DesignerProperties` 更改为 `DesignMode`，然后对其使用 **Resolve** 命令。 删除 `IsInDesignTool` 属性并使用 IntelliSense 添加正确的属性名称：`DesignModeEnabled`。
-   对 **使用**Resolve`ImageSource` 命令。
-   对 **使用**Resolve`BitmapImage` 命令。
-   删除 `using System.Windows.Media;` 和 `using System.Windows.Media.Imaging;`。
-   将**Bookstore2Universal\_BookstoreViewModel**属性返回的值从 "BOOKSTORE2WPSL8" 更改为 "Bookstore2Universal"。
-   更新 [BookSku.CoverImage](wpsl-to-uwp-case-study-bookstore1.md) 属性的实现，正如我们对 **Bookstore1** 所执行的操作一样（请参阅[将图像绑定到视图模型](wpsl-to-uwp-case-study-bookstore1.md)）。

在 MainPage.xaml 中，需要进行以下初始移植更改。

-   将 `phone:PhoneApplicationPage` 更改为 `Page`（包括出现在属性元素语法中的相应项）。
-   删除 `phone` 和 `shell` 命名空间前缀声明。
-   在其余的命名空间前缀声明中将“clr-namespace”更改为“using”。
-   删除 `SupportedOrientations="Portrait"` 和 `Orientation="Portrait"`，然后在新项目的应用包清单中配置 **“纵向”** 。
-   删除 `shell:SystemTray.IsVisible="True"`。
-   跳转列表项目转换器的类型（在标记中以资源形式存在）已移动到 [**Windows.UI.Xaml.Controls.Primitives**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives) 命名空间。 因此，请将命名空间前缀声明 Windows\_UI\_Xaml\_控件添加\_基元，并将其映射到**Windows。** 在跳转列表项目转换器资源上，将该前缀从 `phone:` 更改为 `Windows_UI_Xaml_Controls_Primitives:`。
-   将对 [](wpsl-to-uwp-case-study-bookstore1.md)TextBlock`PhoneTextExtraLargeStyle` 样式的所有引用替换为对  ** 的引用、将 ** 替换为 `SubtitleTextBlockStyle`、将 `PhoneTextSubtleStyle` 替换为 `SubtitleTextBlockStyle`，然后将 `PhoneTextNormalStyle` 替换为 `CaptionTextBlockStyle`，正如我们对 `PhoneTextTitle1Style`Bookstore1`HeaderTextBlockStyle` 所执行的操作一样。
-   `BookTemplate` 中存在一个例外。 第二个 **TextBlock** 的样式应引用 `CaptionTextBlockStyle`。
-   从 **内的**TextBlock`AuthorGroupHeaderTemplate` 中 删除 FontFamily 属性，并将 **Border** 的 Background 设置为引用 `SystemControlBackgroundAccentBrush` 而非 `PhoneAccentBrush`。
-   由于[更改与视图像素有关](wpsl-to-uwp-porting-xaml-and-ui.md)，请检查标记并将所有大小尺寸（边距、宽度、高度等）均乘以 0.8。

## <a name="replacing-the-longlistselector"></a>替换 LongListSelector


将 **LongListSelector** 替换为 [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 控件需要几个步骤，让我们开始吧。 **LongListSelector** 将直接绑定到分组的数据源，但 **SemanticZoom** 中包含 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 或 [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) 控件，它们将通过 [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) 适配器间接绑定到数据。 **CollectionViewSource** 必须以资源形式存在于标记中，因此，让我们先将其添加到 `<Page.Resources>` 内 MainPage.xaml 的标记中。

```xml
    <CollectionViewSource
        x:Name="AuthorHasACollectionOfBookSku"
        Source="{Binding Authors}"
        IsSourceGrouped="true"/>
```

请注意，**LongListSelector.ItemsSource** 上的绑定将成为 **CollectionViewSource.Source** 的值，而 **LongListSelector.IsGroupingEnabled** 上的绑定将成为 **CollectionViewSource.IsSourceGrouped** 的值。 **CollectionViewSource** 具有一个名称（请注意：不是你可能期望的键），因此我们可以绑定到它。

接下来，用此标记来替换 `phone:LongListSelector`，这将向我们提供一个可使用的初步 **SemanticZoom**。

```xml
    <SemanticZoom>
        <SemanticZoom.ZoomedInView>
            <ListView
                ItemsSource="{Binding Source={StaticResource AuthorHasACollectionOfBookSku}}"
                ItemTemplate="{StaticResource BookTemplate}">
                <ListView.GroupStyle>
                    <GroupStyle
                        HeaderTemplate="{StaticResource AuthorGroupHeaderTemplate}"
                        HidesIfEmpty="True"/>
                </ListView.GroupStyle>
            </ListView>
        </SemanticZoom.ZoomedInView>
        <SemanticZoom.ZoomedOutView>
            <ListView
                ItemsSource="{Binding CollectionGroups, Source={StaticResource AuthorHasACollectionOfBookSku}}"
                ItemTemplate="{StaticResource ZoomedOutAuthorTemplate}"/>
        </SemanticZoom.ZoomedOutView>
    </SemanticZoom>
```

包含简单列表和跳转列表模式的 **LongListSelector** 概念将在包含放大和缩小视图的 **SemanticZoom** 概念中分别解答。 放大视图是一个属性，你可以将该属性设置为 **ListView** 的一个实例。 在此情况下，缩小视图也设置为 **ListView**，并且两个 **ListView** 控件都绑定到我们的 **CollectionViewSource**。 放大的视图与 **LongListSelector** 的简单列表使用相同的项目模板、组标题模板和 **HideEmptyGroups** 设置（现在称为 **HidesIfEmpty**）。 而缩小的视图所使用的项目模板十分类似于 **LongListSelector** 的跳转列表样式 (`AuthorNameJumpListStyle`) 内的项目模板。 此外请注意，缩小的视图将绑定到名为 **CollectionGroups** 的 **CollectionViewSource** 的特殊属性，这是一个包含组而不是项目的集合。

我们不再需要 `AuthorNameJumpListStyle`，至少不需要它的所有部分。 我们只需要缩小视图中的组（在此应用中即作者）的数据模板。 因此，我们删除 `AuthorNameJumpListStyle` 样式并将其替换为此数据模板。

```xml
   <DataTemplate x:Key="ZoomedOutAuthorTemplate">
        <Border Margin="9.6,0.8" Background="{Binding Converter={StaticResource JumpListItemBackgroundConverter}}">
            <TextBlock Margin="9.6,0,9.6,4.8" Text="{Binding Group.Name}" Style="{StaticResource SubtitleTextBlockStyle}"
            Foreground="{Binding Converter={StaticResource JumpListItemForegroundConverter}}" VerticalAlignment="Bottom"/>
        </Border>
    </DataTemplate>
```

请注意，由于此数据模板中的数据上下文是一个组而不是一个项目，因此我们将绑定到名为 **Group** 的特殊属性。

现在，你可以生成并运行该应用。 下面是它在移动仿真器上所呈现的外观。

![移动设备上初始源代码发生更改的 UWP 应用](images/wpsl-to-uwp-case-studies/c02-02-mob10-initial-source-code-changes.png)

视图模型与放大和缩小视图正确协作，不过，我们面临着需再多做一些样式设置和模板方面的工作这一问题。 例如，尚未使用正确的样式和画笔，因此，在可以单击缩小的组标题上不会显示文本。如果在桌面设备上运行该应用程序，则会看到另一个问题，即应用程序尚未调整其用户界面，以便在更大的设备上获得最佳体验并使用空间，在这种情况下，windows 可能会远远大于移动设备的屏幕。 因此，在后面的几个部分（[初始样式设置和模板](#initial-styling-and-templating)、[自适应 UI](#adaptive-ui) 和 [最终样式设置](#final-styling)）中我们将解决这些问题。

## <a name="initial-styling-and-templating"></a>初始样式设置和模板

若要很好地分隔开组标题，请编辑 `AuthorGroupHeaderTemplate` 并在 **Border** 上设置 `"0,0,0,9.6"` 的 **Margin**。

若要很好地分隔开书籍项，请编辑 `BookTemplate` 并在 **TextBlock** 上将 `"9.6,0"`Margin**设置为**。

若要将应用名称和页面标题的布局设置得更美观一些，则在 `TitlePanel` 内，通过该值设置为 **来删除第二个**TextBlock**上的顶部**Margin`"7.2,0,0,0"`。 并在 `TitlePanel` 上将边距设置为 `0`（或你认为合适的任意值）

将 `LayoutRoot` 的 Background 更改为 `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`。

## <a name="adaptive-ui"></a>自适应 UI

因为我们是从手机应用入手，所以你不必惊讶于在此阶段我们已移植应用的 UI 布局的确只能适应较小的设备和较窄的窗口。 不过，我们真正希望的是，UI 布局能在应用在较宽的窗口中运行时自行适应并能很好地利用空间（这只能在具有大屏幕的设备上实现），而在应用的窗口较窄时仅使用我们当前拥有的 UI（这种情况出现在较小的设备上，也可能会出现在较大的设备上）。

我们可以使用自适应视觉状态管理器功能来实现此目的。 我们将在视觉元素上设置属性，以便默认使用我们正在使用的模板以较窄的状态设置 UI 的布局。 然后，我们将检测到应用窗口大于或等于特定大小（以[有效像素](wpsl-to-uwp-porting-xaml-and-ui.md)为测量单位）的情况，并更改视觉元素的属性作为回应，以获取更大且更宽的布局。 我们将这些属性更改置于视觉状态中，并且将使用自适应触发器持续监视并确定是否要应用该视觉状态，具体取决于窗口的宽度（以有效像素为单位）。 在此情况下，我们既可以针对窗口宽度进行触发，也可以针对窗口高度进行触发。

最小窗口宽度 548 epx 适用于此用例，因为这是我们希望在其上显示宽布局的最小设备大小。 手机通常小于 548 epx，因此在诸如此类的小型设备上，我们将保留默认的较窄布局。 在电脑上，默认情况下窗口将在足够宽的状态下启动，以触发向较宽状态的切换，这将显示大小为 250x250 的项。 你可在此处将窗口拖动到最窄宽度，以显示两列最小大小为 250x250 的项。 只要比这更窄一些，触发器便会停用，宽视觉状态将被删除，默认的窄布局将生效。

在处理自适应视觉状态管理器这一部分之前，我们首先需要设计宽状态，这意味着需要向我们标记中添加一些新的视觉元素和模板。 以下步骤将介绍如何执行此操作。 依据视觉元素和模板的命名约定，我们将单词“宽”纳入到适用于宽状态的任意元素或模板中。 如果元素或模板不包含单词“宽”，你可以假定它属于窄状态（这是默认状态），且在页面的可视元素上其属性值已设为本地值。 在标记中，仅宽状态的属性值才能通过实际的视觉状态进行设置。

-   在标记中，创建 [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 控件的副本并对其设置 `x:Name="narrowSeZo"`。 在原始副本上，依次设置 `x:Name="wideSeZo"` 和 `Visibility="Collapsed"`，以便在默认情况下使宽状态不可见。
-   在 `wideSeZo` 中，针对放大视图和缩小视图将 **ListView** 更改为 **GridView**。
-   创建 `AuthorGroupHeaderTemplate`、`ZoomedOutAuthorTemplate` 和 `BookTemplate` 这三个资源的副本，并将单词 `Wide` 附加到这些副本的相关键。 此外，还需更新 `wideSeZo`，以便它可引用这些新资源的相关键。
-   将 `AuthorGroupHeaderTemplateWide` 中的内容替换为 `<TextBlock Style="{StaticResource SubheaderTextBlockStyle}" Text="{Binding Name}"/>`。
-   将 `ZoomedOutAuthorTemplateWide` 中的内容替换为：

```xml
    <Grid HorizontalAlignment="Left" Width="250" Height="250" >
        <Border Background="{StaticResource ListViewItemPlaceholderBackgroundThemeBrush}"/>
        <StackPanel VerticalAlignment="Bottom" Background="{StaticResource ListViewItemOverlayBackgroundThemeBrush}">
          <TextBlock Foreground="{StaticResource ListViewItemOverlayForegroundThemeBrush}"
              Style="{StaticResource SubtitleTextBlockStyle}"
            Height="80" Margin="15,0" Text="{Binding Group.Name}"/>
        </StackPanel>
    </Grid>
```

-   将 `BookTemplateWide` 中的内容替换为：

```xml
    <Grid HorizontalAlignment="Left" Width="250" Height="250">
        <Border Background="{StaticResource ListViewItemPlaceholderBackgroundThemeBrush}"/>
        <Image Source="{Binding CoverImage}" Stretch="UniformToFill"/>
        <StackPanel VerticalAlignment="Bottom" Background="{StaticResource ListViewItemOverlayBackgroundThemeBrush}">
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}"
                Foreground="{StaticResource ListViewItemOverlaySecondaryForegroundThemeBrush}"
                TextWrapping="NoWrap" TextTrimming="CharacterEllipsis"
                Margin="12,0,24,0" Text="{Binding Title}"/>
            <TextBlock Style="{StaticResource CaptionTextBlockStyle}" Text="{Binding Author.Name}"
                Foreground="{StaticResource ListViewItemOverlaySecondaryForegroundThemeBrush}" TextWrapping="NoWrap"
                TextTrimming="CharacterEllipsis" Margin="12,0,12,12"/>
        </StackPanel>
    </Grid>
```

-   对于宽状态，在放大视图中的组的周围将需要多留出一些垂直空间。 创建和引用项目面板模板将为我们提供想要的结果。 下面是标记的外观。

```xml
   <ItemsPanelTemplate x:Key="ZoomedInItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" GroupPadding="0,0,0,20"/>
    </ItemsPanelTemplate>
    ...

    <SemanticZoom x:Name="wideSeZo" ... >
        <SemanticZoom.ZoomedInView>
            <GridView
            ...
            ItemsPanel="{StaticResource ZoomedInItemsPanelTemplate}">
            ...
```

-   最后，添加相应的视觉状态管理器标记作为 `LayoutRoot` 的第一个子项。

```xml
    <Grid x:Name="LayoutRoot" ... >
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="WideState">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="548"/>
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Target="wideSeZo.Visibility" Value="Visible"/>
                        <Setter Target="narrowSeZo.Visibility" Value="Collapsed"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

    ...
```

## <a name="final-styling"></a>最终样式设置

所有这些仍是某些最终样式设置的调整。

-   在 `AuthorGroupHeaderTemplate` 的 `Foreground="White"`TextBlock**上设置**，以便它在移动设备系列上运行时显示正确的外观。
-   在 `FontWeight="SemiBold"` 和 **中，将** 添加到 `AuthorGroupHeaderTemplate`TextBlock`ZoomedOutAuthorTemplate`。
-   在 `narrowSeZo` 中，缩小视图中的组标题和作者是左对齐而不是拉伸的，那么让我们开始吧。 我们通过将 [**HorizontalContentAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.groupstyle.headercontainerstyle) 设置为 [，为放大视图创建HeaderContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment)`Stretch`。 并且将为包含同一 [**Setter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle) 的缩小视图创建 [**ItemContainerStyle**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter)。 其外观如下所示。

```xml
   <Style x:Key="AuthorGroupHeaderContainerStyle" TargetType="ListViewHeaderItem">
        <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
    </Style>

    <Style x:Key="ZoomedOutAuthorItemContainerStyle" TargetType="ListViewItem">
        <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
    </Style>

    ...

    <SemanticZoom x:Name="narrowSeZo" ... >
        <SemanticZoom.ZoomedInView>
            <ListView
            ...
                <ListView.GroupStyle>
                    <GroupStyle
                    ...
                    HeaderContainerStyle="{StaticResource AuthorGroupHeaderContainerStyle}"
                    ...
        <SemanticZoom.ZoomedOutView>
            <ListView
                ...
                ItemContainerStyle="{StaticResource ZoomedOutAuthorItemContainerStyle}"
                ...
```

在样式设置操作的最后一步中，应用的外观如下所示。

![在桌面设备上运行的已移植的 Windows 10 应用，放大视图，两个窗口大小](images/w8x-to-uwp-case-studies/c02-07-desk10-zi-ported.png)

在台式计算机上运行的端口、放大视图、两种大小的窗口  
![在桌面设备上运行的端口、缩小视图、两种大小的窗口](images/w8x-to-uwp-case-studies/c02-08-desk10-zo-ported.png)

在桌面设备上运行的 "端口上的" Windows 10 应用程序，缩小视图，两种大小的窗口

![在移动设备上运行的已移植的 Windows 10 应用，放大视图](images/w8x-to-uwp-case-studies/c02-09-mob10-zi-ported.png)

在移动设备上运行的、放大的 Windows 10 应用程序

![在移动设备上运行的已移植的 Windows 10 应用，缩小视图](images/w8x-to-uwp-case-studies/c02-10-mob10-zo-ported.png)

在移动设备上运行的已移植 Windows 10 应用，缩小

## <a name="making-the-view-model-more-flexible"></a>使视图模型更灵活

本部分包含多种设施的示例，这些设施是由于移动我们的应用以使用 UWP 而对我们开放的。 下面介绍一些可选步骤，你可以利用它们在通过 **CollectionViewSource** 访问视图模型时使之更灵活。 从 Windows Phone Silverlight app Bookstore2WPSL8 中移植的视图模型（源文件位于 ViewModel\\BookstoreViewModel.cs）包含一个名为 Author 的类，该类派生自**List&lt;t&gt;** ，其中**t**为 BookSku。 这意味着，Author 类*是一*组 BookSku。

当我们将 **CollectionViewSource.Source** 绑定到 Authors 时，我们要传达的唯一信息是 Authors 中的每个 Author 都是一个包含*某些内容*的组。 我们将其保留为 **CollectionViewSource**，以确定在此情况下 Author 确实是一组 BookSku。 该方法可用：但是不灵活。 如果我们希望 Author *既属于*一组 BookSku *又属于*一组作者的住址呢？ Author 不可能同时*属于*这两个组。 但 Author 可以*包含*任意数量的组。 而这就是解决方案：使用*包含组*模式而不是我们当前在使用的*属于组*模式。 以下是操作方法：

-   更改 Author，这样它就不再从 **List&lt;T&gt;** 派生。
-   将此字段添加到 
-   将此属性添加到 
-   当然，我们可以重复上述两个步骤，向 Author 添加所需数量的组。
-   将 AddBookSku 方法的实现更改为 `this.BookSkus.Add(bookSku);`。
-   既然 Author *包含*至少一个组，我们需要向 **CollectionViewSource** 表明它应该使用其中的哪个组。 为此，请将以下属性添加到**CollectionViewSource**： `ItemsPath="BookSkus"`

这些更改将使此应用在功能上保持不变，但现在你已知道可以如何扩展 Author 和 **CollectionViewSource**（如果需要）。 让我们对 Author 做出最后一项更改，以便如果我们在*没有*指定 **CollectionViewSource.ItemsPath** 的情况下使用它，我们将使用一个包含所选项的默认组：

```csharp
    public class Author : IEnumerable<BookSku>
    {
        ...

        public IEnumerator<BookSku> GetEnumerator()
        {
            return this.BookSkus.GetEnumerator();
        }
        System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
        {
            return this.BookSkus.GetEnumerator();
        }
    }
```

现在，如果我们乐意，我们可以选择删除 `ItemsPath="BookSkus"`，而该应用仍将正常运行。

## <a name="conclusion"></a>总结

此案例研究涉及了一个比上一个用户界面更为大胆的用户界面。 已发现 Windows Phone Silverlight **LongListSelector**的所有功能和概念，如**SemanticZoom**、 **ListView**、 **GRIDVIEW**和**CollectionViewSource**的形式提供给 UWP 应用。 我们展示了如何在 UWP 应用中重复使用、或复制并编辑强制性代码和标记，以实现为适合最窄和最宽以及介于这两者之间的所有大小的 Windows 设备外形规格而定制的功能、UI 和交互。
