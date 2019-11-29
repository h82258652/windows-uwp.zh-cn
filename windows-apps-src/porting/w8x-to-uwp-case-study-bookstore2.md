---
ms.assetid: 333f67f5-f012-4981-917f-c6fd271267c6
description: 此案例研究（基于 Bookstore1 中提供的信息生成）首先研究通用 8.1 应用，该应用可在 SemanticZoom 控件中显示分组数据。
title: Windows 运行时 8.x 到 UWP 案例研究：Bookstore2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fb3fdd324caa54c6965ae63485b5b4f264980d7e
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259122"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore2"></a>Windows 运行时 8.x 到 UWP 案例研究：Bookstore2


此案例研究（基于 [Bookstore1](w8x-to-uwp-case-study-bookstore1.md) 中提供的信息生成）首先研究通用 8.1 应用，该应用可在 [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 控件中显示分组数据。 在视图模型中，类 **Author** 的每个实例都表示一组由该作者创作的书籍，而在 **SemanticZoom** 中，我们可以按作者查看分组书籍的列表，或者可以缩小到可以看到包含作者的跳转列表。 与在书籍列表中上下滚动相比，跳转列表提供了更快速的浏览方式。 我们逐步介绍如何将应用程序移植到 Windows 10 通用 Windows 平台（UWP）应用程序。

**请注意**，在 Visual studio 中打开 Bookstore2Universal 时  \_10，如果看到消息 "需要 Visual studio 更新"，请按照[TargetPlatformVersion](w8x-to-uwp-troubleshooting.md)中的步骤操作。

## <a name="downloads"></a>下载

[下载 Bookstore2\_81 通用8.1 应用](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2_81)。

[下载 Bookstore2Universal\_10 Windows 10 应用](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10)。

## <a name="the-universal-81-app"></a>通用 8.1 应用

下面是 Bookstore2\_81 （我们要移植的应用）的内容。 它是水平滚动（在 Windows Phone 上垂直滚动）的 [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom)，按作者分组显示书籍。 你可以缩小到跳转列表，并且可以从该列表导航回任一组。 此应用包含两个主要部分：提供分组数据源的视图模型，以及绑定到该视图模型的用户界面。 正如我们所看到的，这两个部分都可以轻松地从 WinRT 8.1 技术移植到 Windows 10。

![windows 上的 bookstore2\-81，已放大视图](images/w8x-to-uwp-case-studies/c02-01-win81-zi-how-the-app-looks.png)

Windows 上的 Bookstore2\_81，已放大视图
 

![windows 上的 bookstore2\-81，缩小视图](images/w8x-to-uwp-case-studies/c02-02-win81-zo-how-the-app-looks.png)

Windows 上的 Bookstore2\_81，缩小视图

![windows phone 上的 bookstore2\-81，已放大视图](images/w8x-to-uwp-case-studies/c02-03-wp81-zi-how-the-app-looks.png)

Windows Phone 上的 Bookstore2\_81，放大视图

![windows phone 上的 bookstore2\-81，缩小视图](images/w8x-to-uwp-case-studies/c02-04-wp81-zo-how-the-app-looks.png)

Windows Phone 上的 Bookstore2\_81，缩小视图

##  <a name="porting-to-a-windows10-project"></a>移植到 Windows 10 项目

Bookstore2\_81 解决方案是8.1 通用应用程序项目。 Bookstore2\_81. Windows 项目生成 Windows 8.1 的应用包，而 Bookstore2\_81. WindowsPhone 项目为 Windows Phone 8.1 生成应用程序包。 Bookstore2\_81。共享是包含源代码、标记文件以及其他两个项目都使用的其他资产和资源的项目。

与上一案例研究一样，我们将采用的选项（如果你使用的是[通用8.1 应用](w8x-to-uwp-root.md)，在中介绍的选项）是将共享项目的内容移植到面向通用设备系列的 Windows 10。

首先创建新的空白应用程序（Windows 通用）项目。 将其命名为 Bookstore2Universal\_10。 这些文件是要从 Bookstore2\_81 到 Bookstore2Universal\_10 复制的文件。

**从共享项目**

-   复制包含书籍封面图像 PNG 文件的文件夹（文件夹 \\资产\\CoverImages）。 复制该文件夹后，在 **“解决方案资源管理器”** 中，请确保将 **“显示所有文件”** 切换为打开。 右键单击你复制的文件夹，然后单击**包括在项目中**。 该命令的意思是将文件或文件夹“包括”在某个项目中。 每次你复制文件或文件夹、每个副本时，请在**解决方案资源管理器**中单击**刷新**，然后将文件或文件夹包括在项目中。 无需为你将在目标位置替换的文件执行此操作。
-   复制包含视图模型源文件的文件夹（文件夹为 \\ViewModel）。
-   复制 MainPage.xaml 并替换目标位置中的文件。

**从 Windows 项目**

-   复制 BookstoreStyles.xaml。 我们将此文件用作良好的起点，因为此文件中的所有资源键都将在 Windows 10 应用中解析;那些在等效的 WindowsPhone 文件中的部分将不会。
-   复制 SeZoUC.xaml 和 SeZoUC.xaml.cs。 我们将从此视图的 Windows 版本开始（此版本适用于宽窗口），然后我们将使其适应较小的窗口，从而适应较小的设备。

编辑刚刚复制的源代码和标记文件，并将对 Bookstore2\_81 命名空间的所有引用更改为 Bookstore2Universal\_10。 执行此操作的快速方法是使用**在文件中替换**功能。 视图模型中和任何其他强制性代码中都不需要更改任何代码。 但是，为了更轻松地查看正在运行的应用程序版本，请将 **\_Bookstore2Universal**返回的值从 "Bookstore2\_81" 更改为 "Bookstore2Universal\_10"。

现在，你可以执行生成和运行操作。 下面是我们的新 UWP 应用在尚未完成将其移植到 Windows 10 后的外观。

![在桌面设备上运行的初始源代码发生更改的 Windows 10 应用，放大视图](images/w8x-to-uwp-case-studies/c02-05-desk10-zi-initial-source-code-changes.png)

Windows 10 应用程序，其初始源代码更改在桌面设备上运行，放大视图

![在桌面设备上运行的初始源代码发生更改的 Windows 10 应用，缩小视图](images/w8x-to-uwp-case-studies/c02-06-desk10-zo-initial-source-code-changes.png)

Windows 10 应用程序，其初始源代码更改在桌面设备上运行，缩小视图

视图模型与放大和缩小视图正确协作，尽管存在一些问题使其难以体现出来。 一个问题是，[**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 无法滚动。 这是因为，在 Windows 10 中， [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)的默认样式使其垂直布局（windows 10 设计指南建议在新的和移植的应用中使用它）。 但是，我们从 Bookstore2\_81 项目（专为8.1 应用）复制的 "自定义项" 面板模板中的水平滚动设置与 Windows 10 默认样式中的垂直滚动设置冲突，因为我们已移植到 Windows 10 应用程序的结果。 第二个问题是，该应用尚未调整其用户界面以在不同大小的窗口和小型设备中提供最佳体验。 第三，尚未使用正确的样式和画笔，从而导致许多文本不可见（包括你可以通过单击缩小的组标题）。 因此在接下来的三个部分（[SemanticZoom 和 GridView 设计更改](#semanticzoom-and-gridview-design-changes)、[自适应 UI](#adaptive-ui) 和[通用样式](#universal-styling)）中，我们将修复这三个问题。

## <a name="semanticzoom-and-gridview-design-changes"></a>SemanticZoom 和 GridView 设计更改

在 SemanticZoom 的 "[更改](w8x-to-uwp-porting-xaml-and-ui.md)" 一节中介绍了 Windows 10 中对[**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom)控件的设计更改。 在此部分中，我们无需为响应这些更改而进行任何工作。

[GridView/ListView 设计更改**部分中描述了对** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)GridView[](w8x-to-uwp-porting-xaml-and-ui.md) 的更改。 为了适应这些更改，我们需要进行一些非常微小的调整，如下所述。

-   在 SeZoUC.xaml 中，在 `ZoomedInItemsPanelTemplate` 中设置 `Orientation="Horizontal"` 和 `GroupPadding="0,0,0,20"`。
-   在 SeZoUC.xaml 中，从缩小视图中删除 `ZoomedOutItemsPanelTemplate` 并移除 `ItemsPanel` 属性。

就这么简单！

## <a name="adaptive-ui"></a>自适应 UI

进行该更改后，SeZoUC.xaml 向我们提供的 UI 布局最适用于应用在宽窗口（只可能出现在带有大屏幕的设备上）中运行的情况。 但是，当应用的窗口较窄时（会出现在小型设备上，但也可能出现在大型设备上），Windows Phone 应用商店应用中所具有的 UI 可以说是最合适的。

我们可以使用自适应视觉状态管理器功能来实现此目的。 我们将在视觉元素上设置属性，以便默认使用我们在 Windows Phone 应用商店应用中所使用的较小模板，将 UI 的布局设置为较窄的状态。 然后，我们将检测到应用窗口大于或等于特定大小（以[有效像素](w8x-to-uwp-porting-xaml-and-ui.md)为测量单位）的情况，并更改视觉元素的属性作为回应，以获取更大且更宽的布局。 我们将这些属性更改置于视觉状态中，并且将使用自适应触发器持续监视并确定是否要应用该视觉状态，具体取决于窗口的宽度（以有效像素为单位）。 在此情况下，我们既可以针对窗口宽度进行触发，也可以针对窗口高度进行触发。

最小窗口宽度 548 epx 适用于此用例，因为这是我们希望在其上显示宽布局的最小设备大小。 手机通常小于 548 epx，因此在诸如此类的小型设备上，我们将保留默认的较窄布局。 在电脑上，默认情况下窗口将在足够宽的状态下启动，以触发向较宽状态的切换。 你将能够从此处将窗口拖动到最窄宽度以显示两列 250x250 大小的项。 如果比这更窄一些，则触发器将停用、宽视觉状态将被删除，并且默认的窄布局将生效。

因此，为了实现这两个不同的布局，我们需要设置（和更改）哪些属性？ 有两个替代项，每一项都需要不同的方法。

1.  我们可以在标记中放置两个 [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 控件。 其中一种是我们在 Windows 运行时3.x 应用程序中使用的标记的副本（在其中使用[**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)控件），默认情况下折叠。 另一个将是我们在 Windows Phone 应用商店应用中所使用的标记的副本（使用其内部的 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 控件），并且在默认情况下处于可见状态。 视觉状态将切换两个 **SemanticZoom** 控件的可见性属性。 这只需非常少的工作即可实现，但一般情况下不是一个高性能技术。 因此，如果你要使用它，你应该分析你的应用并确保它仍然满足你的性能目标。
2.  我们可以使用包含 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 控件的单个 [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)。 若要实现这两个布局，在宽视觉状态中，我们将更改 **ListView** 控件的属性（包括应用于它们的模板），以使它们采用与 [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) 相同的方式进行布局。 这可能会提高性能，但是在 **GridView** 和 **ListView** 的各种样式和模板之间以及它们的各种项类型之间有许多细小的差异，因此这是一个更难实现的解决方案。 此解决方案此时还与默认样式和模板的设计方式紧密耦合，因此该解决方案容易受到将来对默认值的任何更改的影响。

在此案例研究中，我们将选择第一个替代项。 但如果你愿意，你可以尝试第二个替代项然后看它是否更适合你。 下面是实现第一个替代项需要采取的步骤。

-   在新项目的标记中的 [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 上，设置 `x:Name="wideSeZo"` 和 `Visibility="Collapsed"`。
-   返回到 Bookstore2\_WindowsPhone 项目并打开 SeZoUC。 从该文件复制 [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 元素标记并将其粘贴在紧挨着新项目中的 `wideSeZo` 后的位置。 在刚粘贴的元素上设置 `x:Name="narrowSeZo"`。
-   但是 `narrowSeZo` 需要一些我们尚未复制的样式。 再次在 Bookstore2 中，将两个样式（`AuthorGroupHeaderContainerStyle` 和 `ZoomedOutAuthorItemContainerStyle`）从 SeZoUC 复制到中的\_，并将其粘贴到新项目的 BookstoreStyles 中。
-   现在在新的 SeZoUC.xaml 中有两个 [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 元素。 将这两个元素包裹在 **Grid** 中。
-   在新项目的 BookstoreStyles.xaml 中，将单词 `Wide` 附加到以下三个资源键（并附加到它们在 SeZoUC.xaml 中的引用，但仅附加到 `wideSeZo` 内的引用）：`AuthorGroupHeaderTemplate`、`ZoomedOutAuthorTemplate` 和 `BookTemplate`。
-   在 Bookstore2\_WindowsPhone 项目中，打开 BookstoreStyles。 在此文件中，复制上述三个资源（如上所述）和两个跳转列表项转换器，命名空间前缀声明 Windows\_UI\_Xaml\_控制\_基元，并将其全部粘贴到新项目中的 BookstoreStyles。
-   最后，在新项目的 SeZoUC.xaml 中，将相应的视觉状态管理器标记添加到你在上面添加的 **Grid**。

```xml
    <Grid>
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

    </Grid>
```

## <a name="universal-styling"></a>通用样式设置

现在，让我们来修复某些样式设置问题（包括上面我们在从旧项目中进行复制时介绍的问题）。

-   在 MainPage.xaml 中，将 `LayoutRoot` 的 Background 更改为 `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`。
-   在 BookstoreStyles.xaml 中，将资源 `TitlePanelMargin` 的值设置为 `0`（或你认为合适的任意值）。
-   在 SeZoUC.xaml 中，将 `wideSeZo` 的 Margin 设置为 `0`（或你认为合适的任意值）。
-   在 BookstoreStyles.xaml 中，从 `AuthorGroupHeaderTemplateWide` 中删除 Margin 属性。
-   从 `AuthorGroupHeaderTemplate` 和 `ZoomedOutAuthorTemplate` 中删除 FontFamily 属性。
-   Bookstore2\_81 使用 `BookTemplateTitleTextBlockStyle`、`BookTemplateAuthorTextBlockStyle`和 `PageTitleTextBlockStyle` 资源键作为间接寻址，以便单个密钥在两个应用中具有不同的实现。 我们不再需要该间接寻址；我们可以直接引用系统样式。 因此分别使用 `TitleTextBlockStyle`、`CaptionTextBlockStyle` 和 `HeaderTextBlockStyle` 在整个应用中替换这些引用。 你可以使用 Visual Studio的 **“在文件中替换”** 功能快速且准确地执行此操作。 然后，你可以删除这三个未使用的资源。
-   在 `AuthorGroupHeaderTemplate` 中，使用 `PhoneAccentBrush` 替换 `SystemControlBackgroundAccentBrush`，然后在 `Foreground="White"`TextBlock**上设置**，以便它在移动设备系列上运行时外观正确。
-   在 `BookTemplateWide` 中，将第二个 **TextBlock** 中的 Foreground 属性复制到第一个。
-   在 `ZoomedOutAuthorTemplateWide` 中，将对 `SubheaderTextBlockStyle` 的引用（现在有点过大）更改为对 `SubtitleTextBlockStyle` 的引用。
-   缩小视图（跳转列表）不再覆盖新平台中的放大视图，因此我们可以从 `Background` 的缩小视图中删除 `narrowSeZo` 属性。
-   因此，所有样式和模板位于一个文件中，将 `ZoomedInItemsPanelTemplate` 移出 SeZoUC.xaml 并移入 BookstoreStyles.xaml 中。

在样式设置操作的最后一步中，应用的外观如下所示。

![在桌面设备上运行的已移植的 Windows 10 应用，放大视图，两个窗口大小](images/w8x-to-uwp-case-studies/c02-07-desk10-zi-ported.png)

在桌面设备上运行的、放大视图、两种大小的窗口的端口

![在桌面设备上运行的已移植的 Windows 10 应用，缩小视图，两个窗口大小](images/w8x-to-uwp-case-studies/c02-08-desk10-zo-ported.png)

在桌面设备上运行的 "端口上的" Windows 10 应用程序，缩小视图，两种大小的窗口

![在移动设备上运行的已移植的 Windows 10 应用，放大视图](images/w8x-to-uwp-case-studies/c02-09-mob10-zi-ported.png)

在移动设备上运行的、放大的 Windows 10 应用程序

![在移动设备上运行的已移植的 Windows 10 应用，缩小视图](images/w8x-to-uwp-case-studies/c02-10-mob10-zo-ported.png)

在移动设备上运行的已移植 Windows 10 应用，缩小

## <a name="conclusion"></a>总结

此案例研究涉及了一个比上一个用户界面更为大胆的用户界面。 和上一个案例研究一样，此特定视图模型不需要进行任何工作，我们的主要工作集中在重构用户界面。 某些更改是将两个项目组合为一个项目同时仍然支持许多外形规格（事实上，比我们以前所能支持的多很多）的必然结果。 一些更改与对平台进行的更改有关。

下一个案例研究是 [QuizGame](w8x-to-uwp-case-study-quizgame.md)，我们将从中了解有关访问和显示分组数据的信息。
