---
author: stevewhims
description: 以声明性 XAML 标记的形式定义 UI 的做法非常好地将通用 8.1 应用转换为通用 Windows 平台 (UWP) 应用。
title: 将 Windows 运行时 8.x XAML 和 UI 移植到 UWP
ms.assetid: 78b86762-7359-474f-b1e3-c2d7cf9aa907
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0b5a3425c49a30ddb96fcb7a8a2c8b83fbb6dff3
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6277748"
---
# <a name="porting-windows-runtime-8x-xaml-and-ui-to-uwp"></a>将 Windows 运行时 8.x XAML 和 UI 移植到 UWP


上一主题是[疑难解答](w8x-to-uwp-troubleshooting.md)。

以声明性 XAML 标记的形式定义 UI 的做法非常好地将 Universal 8.1 App 转换为 Universal Windows Platform (UWP) App。 你会发现，你的大多数标记是兼容的，尽管你可能需要针对正在使用的系统资源键或自定义模板对其作相应调整。 视图模型中的强制性代码只需稍作更改或无需更改。 操纵 UI 元素的表示层中的许多强制性代码（甚至是大部分代码）也应易于移植。

## <a name="imperative-code"></a>强制性代码

如果你只是希望转到项目构建阶段，你可以注释或去掉任何非必要的代码。 然后一次一个问题进行迭代，并参考本部分中的以下主题（和上一主题：[疑难解答](w8x-to-uwp-troubleshooting.md)），直到消除所有生成和运行时问题并且完成移植。

## <a name="adaptiveresponsive-ui"></a>自适应/响应式 UI

由于你的应用可以在种类可能很广泛的设备上运行（每种设备都具有自己的屏幕大小和分辨率），你不仅需要完成移植应用的最少步骤，而且还需要定制你的 UI 以使其在这些设备上具有最佳的外观。 你可以使用自适应视觉状态管理器功能来动态检测窗口大小并更改布局作为响应，还可以使用 Bookstore2 案例研究主题中的[自适应 UI](w8x-to-uwp-case-study-bookstore2.md) 部分中所示的有关如何执行此操作的示例。

## <a name="back-button-handling"></a>后退按钮处理

对于通用 8.1 应用、 Windows 运行时 8.x 应用和 Windows Phone 应用商店应用具有不同的方法，向你显示的 UI 和为后退按钮处理的事件。 但是，对于 windows 10 应用，你可以在应用中使用一种方法。 在移动设备上，该按钮作为设备上的电容性按钮或外壳中的按钮向你提供。 在桌面设备上，只要你的应用内可进行后退导航，你便可以向该应用的镶边添加一个按钮，它将显示在窗口化的应用的标题栏中或平板电脑模式下的任务栏中。 后退按钮事件是所有设备系列的通用概念，并且硬件或软件中实现的按钮会引发相同的 [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596) 事件。

以下示例适用于所有设备系列，并且对将相同的处理操作应用于所有页面的情形以及不需要确认导航的情形（例如，就未保存的更改发出警告）十分有用。

```csharp
   // app.xaml.cs

    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        [...]

        Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
        rootFrame.Navigated += RootFrame_Navigated;
    }

    private void RootFrame_Navigated(object sender, NavigationEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        // Note: On device families that have no title bar, setting AppViewBackButtonVisibility can safely execute 
        // but it will have no effect. Such device families provide back button UI for you.
        if (rootFrame.CanGoBack)
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Visible;
        }
        else
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Collapsed;
        }
    }

    private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        if (rootFrame.CanGoBack)
        {
            rootFrame.GoBack();
        }
    }
```

还有一个适用于所有设备系列的以编程方式退出应用的方法。

```csharp
   Windows.UI.Xaml.Application.Current.Exit();
```

## <a name="charms"></a>超级按钮

你无需更改任何与超级按钮，集成的代码，但你需要将某些 UI 添加到你的应用需要不属于 windows 10 外壳的超级按钮栏的位置。 Windows 10 上运行的通用 8.1 应用具有其自己的替换 UI 由应用的标题栏中的系统呈现的镶边提供。

## <a name="controls-and-control-styles-and-templates"></a>控件、控件样式和模板

Windows 10 上运行的通用 8.1 应用将保留 8.1 的外观和行为的控件。 但是，当该应用移植到 windows 10 应用，有外观和行为需要注意的一些差异。 体系结构和控件设计未本质上适用于 windows 10 应用，因此更改主要围绕[设计语言](#design-language-in-windows-10)、 简化和可用性改进。

**请注意** PointerOver 视觉状态是与中自定义样式/模板在 windows 10 应用和 Windows 运行时 8.x 应用，而不是在 Windows Phone 应用商店应用。 出于此原因 （以及由于 windows 10 应用支持的系统资源键），我们建议你重新使用自定义样式/模板从 Windows 运行时 8.x 应用移植到 windows 10 应用时。
如果你想要在特定的自定义样式/模板使用最新的视觉状态集是否受益于对默认样式/模板，所做的性能改进，然后编辑新的 windows 10 默认模板的副本和重新应用你自定义。 性能改进的一个示例是，以前包含 **ContentPresenter** 或面板的任何 **Border** 已被删除，而子元素现在可呈现边框。

下面是对控件所做的更改的一些更具体的示例。

| 控件名称 | 更改 |
|--------------|--------|
| **AppBar**   | 如果你正在使用**AppBar**控件 （[**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927)建议改为），则它不隐藏默认情况下，在 windows 10 应用中。 你可以使用 [**AppBar.ClosedDisplayMode**](https://msdn.microsoft.com/library/windows/apps/dn633872) 属性对其进行控制。 |
| **AppBar**、[**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | 在 windows 10 应用中，**应用栏**和[**命令栏**](https://msdn.microsoft.com/library/windows/apps/hh701927)会**看到多**按钮 （省略号）。 |
| [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | 在 Windows 运行时 8.x 应用中， [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927)的辅助命令始终是可见的。 在 Windows Phone 应用商店应用中，并在 windows 10 应用中，不会在命令栏打开后才显示。 |
| [**CommandBar（命令栏）**](https://msdn.microsoft.com/library/windows/apps/hh701927) | 对于 Windows Phone 应用商店应用，[**CommandBar.IsSticky**](https://msdn.microsoft.com/library/windows/apps/hh701944) 的值不影响该栏是否可通过轻触消除。 对于 windows 10 应用中，如果**IsSticky**设置为 true，则**CommandBar**忽略轻型消除手势。 |
| [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) | 在 windows 10 应用中，[**命令栏**](https://msdn.microsoft.com/library/windows/apps/hh701927)不会处理[**EdgeGesture.Completed**](https://msdn.microsoft.com/library/windows/apps/hh701622) ，也不会[**UIElement.RightTapped**](https://msdn.microsoft.com/library/windows/apps/br208984)事件。 同时也不会响应点击或向上轻扫操作。 你仍可以选择处理这些事件并设置 [**IsOpen**](https://msdn.microsoft.com/library/windows/apps/hh701939)。 |
| [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/dn298584)、[**TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280) | 通过从视觉上更改 [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/dn298584) 和 [**TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280)，查看你的应用的外观。 对于在移动设备上运行的 windows 10 应用，这些控件不会再导航到选择页面，但改为使用轻触消除弹出窗口。 |
| [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/dn298584)、[**TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280) | 在 windows 10 应用中，不能将[**DatePicker**](https://msdn.microsoft.com/library/windows/apps/dn298584)或[**TimePicker**](https://msdn.microsoft.com/library/windows/apps/dn299280)放在浮出控件内。如果你想要在弹出式控件中显示这些控件，然后你可以使用[**DatePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn625013)和[**TimePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn608313)。 |
| **GridView**、**ListView** | 有关 **GridView**/**ListView**，请参阅 [GridView 和 ListView 更改](#gridview-and-listview-changes)。 |
| [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) | 在 Windows Phone 应用商店应用中，[**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) 控件从最后一部分环绕到第一部分。 在 Windows 运行时 8.x 应用中，并在 windows 10 应用中，中心区域不会环绕。 |
| [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) | 在 Windows Phone 应用商店应用中，[**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) 控件的背景图像相对于中心区域在视差中移动。 在 Windows 运行时 8.x 应用中，并在 windows 10 应用中，不使用视差。 |
| [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843)  | 在通用 8.1 应用中，[**HubSection.IsHeaderInteractive**](https://msdn.microsoft.com/library/windows/apps/dn251917) 属性会导致区域标头（和呈现在它旁边的 V 型字型）变得具有交互性。 在 windows 10 应用中，"查看更多"提供可交互的旁边标头，但该标头本身不可交互。 **IsHeaderInteractive** 仍用于确定交互是否引发 [**Hub.SectionHeaderClick**](https://msdn.microsoft.com/library/windows/apps/dn251953) 事件。 |
| **MessageDialog** | 如果你使用的是 **MessageDialog**，请考虑改用更加灵活的 [**ContentDialog**](https://msdn.microsoft.com/library/windows/apps/dn633972)。 另请参阅 [XAML UI 基础知识](http://go.microsoft.com/fwlink/p/?linkid=619992)示例。 |
| **ListPickerFlyout**、**PickerFlyout**  | **ListPickerFlyout**和**PickerFlyout**已弃用适用于 windows 10 应用中。 对于单选浮出控件，请使用 [**MenuFlyout**](https://msdn.microsoft.com/library/windows/apps/dn299030)；对于更复杂的体验，请使用 [**Flyout**](https://msdn.microsoft.com/library/windows/apps/dn279496)。 |
| [**PasswordBox**](https://msdn.microsoft.com/library/windows/apps/br227519) | 在 windows 10 应用中，已弃用[**PasswordBox.IsPasswordRevealButtonEnabled**](https://msdn.microsoft.com/library/windows/apps/hh702579)属性和设置它不起作用。 相反，它默认为**速览**（在其中眼睛标志，如中显示的 Windows 运行时 8.x 应用） 使用[**PasswordBox.PasswordRevealMode**](https://msdn.microsoft.com/library/windows/apps/dn890867) 。 另请参阅[密码框指南](https://msdn.microsoft.com/library/windows/apps/dn596103)。 |
| [**Pivot**](https://msdn.microsoft.com/library/windows/apps/dn608241) | [**Pivot**](https://msdn.microsoft.com/library/windows/apps/dn608241) 控件现在是通用控件，它的使用不再限于移动设备。 |
| [**SearchBox**](https://msdn.microsoft.com/library/windows/apps/dn252771) | 尽管已在通用设备系列中实现了 [**SearchBox**](https://msdn.microsoft.com/library/windows/apps/dn252803)，但它无法在移动设备上正常运行。 请参阅[弃用 SearchBox 以支持 AutoSuggestBox](#searchbox-deprecated-in-favor-of-autosuggestbox)。 |
| **SemanticZoom** | 有关 **SemanticZoom**，请参阅 [SemanticZoom 更改](#semanticzoom-changes)。 |
| [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527)  | [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527) 的某些默认属性已更改。 [**HorizontalScrollMode**](https://msdn.microsoft.com/library/windows/apps/br209549) 已更改为 **Auto**，[**VerticalScrollMode**](https://msdn.microsoft.com/library/windows/apps/br209589) 已更改为 **Auto**，而 [**ZoomMode**](https://msdn.microsoft.com/library/windows/apps/br209601) 已更改为 **Disabled**。 如果新的默认值不适合你的应用，你可以使用样式更改它们，或对控件本身的本地值进行更改。  |
| [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) | 在 Windows 运行时 8.x 应用中，拼写检查默认是关闭的[**文本框**](https://msdn.microsoft.com/library/windows/apps/br209683)。 在 Windows Phone 应用商店应用中，并在 windows 10 应用中，它是在默认情况下。 |
| [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) | [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 的默认字体大小已从 11 更改为 15。 |
| [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) | [**TextBox.TextReadingOrder**](https://msdn.microsoft.com/library/windows/apps/dn252859) 的默认值已从 **Default** 更改为 **DetectFromContent**。 如果不适用，则使用 **UseFlowDirection**。 **Default** 已弃用。 |
| 各种 | 主题色适用于 Windows Phone 应用商店应用和 windows 10 应用，但不适用于 Windows 运行时 8.x 应用。  |

有关 UWP 应用控件的详细信息，请参阅[按功能列出的控件](https://msdn.microsoft.com/library/windows/apps/mt185405)、[控件列表](https://msdn.microsoft.com/library/windows/apps/mt185406)和[控件指南](https://msdn.microsoft.com/library/windows/apps/dn611856)。

##  <a name="design-language-in-windows10"></a>在 windows 10 中的设计语言

有一些细小但很重要设计语言差异通用 8.1 应用和 windows 10 应用之间。 有关所有详细信息，请参阅[设计](http://dev.windows.com/design)。 不考虑设计语言更改，我们的设计原则始终保持一致：关注细节却又力求简洁（专注于内容而不是外观），显著减少视觉元素，始终忠实于数字领域；使用可视化层次结构（尤其是版式）；基于网格进行设计；通过流畅的动画带给你生动的体验。

## <a name="effective-pixels-viewing-distance-and-scale-factors"></a>有效像素、观看距离和比例因子

以前，视图像素是从设备的实际物理大小和分辨率抽象表示 UI 元素的大小和布局的方法。 视图像素现在已发展为有效像素，下面是对该术语、该术语的意义以及它所提供的额外价值的说明。

术语“分辨率”是指像素密度的度量，而不是通常认为的像素计数。 “有效分辨率”是构成图像或字形的物理像素对肉眼解析的方法，因为设备的观看距离和物理像素大小之间有差异（像素密度是物理像素大小的倒数）。 有效分辨率是构建周围体验的良好指标，因为它是以用户为中心的。 通过了解所有因素并控制 UI 元素的大小，即可优化用户的体验。

不同设备的宽度的有效像素是不同的，范围从最小设备的 320 像素到中等大小监视器的 1024 像素，甚至更高宽度的有效像素。 你只需像往常那样继续使用可自动调整大小的元素和动态布局面板。 在某些情况下，你还需要将 XAML 标记中的 UI 元素的相关属性设置为固定大小。 根据应用运行所在的设备和用户所设置的显示设置，比例因子将自动应用于应用。 并且，该比例因子可使具有固定大小的任何 UI 元素在各种尺寸的屏幕上都能向用户显示一些大小恒定的触摸（和阅读）目标。 通过与动态布局结合使用，你的 UI 不仅仅在不同设备上进行视觉上的缩放。 它还会执行任何必要的操作来将相应的内容量纳入到可用空间中。

这样，应用便可在所有屏幕上提供最佳体验。我们建议你针对各种屏幕大小创建每个位图资源，其中每个资源均适用于特定的比例因子。 在大多数情况下，提供 100% 缩放、200% 缩放和 400% 缩放的资源（按优先级顺序）能在采用所有中间比例系数时均可提供极佳效果。

**请注意**如果出于任何原因，无法创建资源的多个大小，则创建 100%缩放的资源。 在 Microsoft Visual Studio 中，UWP 应用的默认项目模板仅使用一个大小提供品牌标识资源（磁贴图像和徽标），但这些资源并非 100% 缩放。 为自己的应用编写资源时，请按照本部分中的指南进行编写、提供 100%、200% 和 400% 尺寸，并使用资源包。

如果具有繁复的图案，则可能希望在更多尺寸中提供资源。 如果要从矢量图像开始，则生成采用任意比例系数的高质量资源相对容易。

我们不建议你尝试支持所有比例系数，但适用于 windows 10 应用的比例系数的完整列表为 100%、 125%、 150%、 200%、 250%、 300%和 400%。 如果你支持这些比例系数，应用商店将针对每台设备选取大小适合的资源，然后将仅下载这些资源。 应用商店将根据设备的 DPI 选择要下载的资源。 你可以重新使用诸如 140%和 220%等比例因子 Windows 运行时 8.x 应用中的资源，但你的应用将运行某一新比例因子并且使得某些位图缩放无法避免。 在各种设备上测试你的应用，以查看你是否满意相应的结果。

你可能重复使用 Windows 运行时 8.x 应用中的 XAML 标记 （可能用于大小形状或其他元素，也可能用于版式） 在标记中使用了文本维度值。 但是，在某些情况下，较大的比例系数用于比为 windows 10 应用在设备上的通用 8.1 应用 （例如，150%使用其中 140%而之前，而 180%而使用 200%）。 因此，如果你发现这些文本值现在在 windows 10 中过大，则尝试它们乘以 0.8。 有关详细信息，请参阅[适用于 UWP 应用的响应式设计基础知识](https://msdn.microsoft.com/library/windows/apps/dn958435)。

## <a name="gridview-and-listview-changes"></a>GridView 和 ListView 更改

已对 [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) 默认样式资源库进行多个更改，以使控件垂直滚动（而不是像之前默认的那样水平滚动）。 如果你编辑过你的项目中的默认样式副本，则副本将不会具有这些更改，因此你将需要手动进行更改。 下面是这些更改的列表。

-   [**ScrollViewer.HorizontalScrollBarVisibility**](https://msdn.microsoft.com/library/windows/apps/br209547) 的资源库已从 **Auto** 更改为 **Disabled**。
-   [**ScrollViewer.VerticalScrollBarVisibility**](https://msdn.microsoft.com/library/windows/apps/br209587) 的资源库已从 **Disabled** 更改为 **Auto**。
-   [**ScrollViewer.HorizontalScrollMode**](https://msdn.microsoft.com/library/windows/apps/br209549) 的资源库已从 **Enabled** 更改为 **Disabled**。
-   [**ScrollViewer.VerticalScrollMode**](https://msdn.microsoft.com/library/windows/apps/br209589) 的资源库已从 **Disabled** 更改为 **Enabled**。
-   在 [**ItemsPanel**](https://msdn.microsoft.com/library/windows/apps/br242826) 的资源库中，[**ItemsWrapGrid.Orientation**](https://msdn.microsoft.com/library/windows/apps/dn298907) 的值已从 **Vertical** 更改为 **Horizontal**。

如果上一个更改（对 **Orientation** 的更改）看起来矛盾，请记住我们讨论的是包围式网格。 水平方向的包围式网格（新值）与文本水平流动的写入系统类似，并且在页面末尾中断到下一行。 这样的文本页面会垂直滚动。 相反，垂直方向的包围式网格（上一个值）与文本垂直流动的写入系统类似，因而水平滚动。

下面是方面的[**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705)和[**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878)已更改，或在 windows 10 中不受支持。

-   [**IsSwipeEnabled**](https://msdn.microsoft.com/library/windows/apps/hh702518)属性 （仅 Windows 运行时 8.x 应用） 的 windows 10 应用不支持。 API 仍存在，但设置它不起任何作用。 以前的所有选择手势都受支持，向下轻扫（它不受支持是因为数据显示其不容易被发现）和右键单击（为显示上下文菜单而保留）除外。
-   [**ReorderMode**](https://msdn.microsoft.com/library/windows/apps/dn625099)属性 （仅适用于 Windows Phone 应用商店应用） 的 windows 10 应用不支持。 API 仍存在，但设置它不起任何作用。 请改为将你的 **GridView** 或 **ListView** 的 [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/br208912) 和 [**CanReorderItems**](https://msdn.microsoft.com/library/windows/apps/br242882) 设置为 true，以便用户能够使用长按（或单击并拖动）手势重新排序。
-   当开发适用于 windows 10，使用[**ListViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/dn298500)而不是[**GridViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/dn279298)在项容器样式中，针对[**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878)和[**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705)的。 如果你编辑了默认项容器样式的副本，你将获得正确的类型。
-   适用于 windows 10 应用发生了更改，选择视觉效果。 如果你将 [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/br242915) 设置为 **Multiple**，则在默认情况下，将为每个项都呈现一个复选框。 **ListView** 项的默认设置意味着复选框在项旁边以内联方式布局，因此，该项的其余部分所占用的空间将稍微减少并进行移动。 对于 **GridView** 项，复选框默认叠加在该项上方。 但是，在任何一种情况下，你都可以通过项容器样式内的 [**ListViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.aspx) 元素，控制复选框的布局方式（内联或叠加，通过 [**CheckMode**](https://msdn.microsoft.com/library/windows/apps/dn913923) 属性控制）以及是否完整显示它们（通过 [**SelectionCheckMarkVisualEnabled**](https://msdn.microsoft.com/library/windows/apps/dn298541) 属性），如以下示例所示。
-   在 windows 10， [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/dn298914)引发该事件两次每个项目在 UI 虚拟化期间： 一次用于回收，并且一次用于重复使用。 如果 [**InRecycleQueue**](https://msdn.microsoft.com/library/windows/apps/dn279443) 的值是 **true**，并且没有特定回收工作要执行，可立即退出事件处理程序，并可确保在重复使用该相同项时（此时 **InRecycleQueue** 将会是 **false**），将重新进入事件处理程序。

```xml
<Style x:Key="CustomItemContainerStyle" TargetType="ListViewItem|GridViewItem">
    ...
    <Setter.Value>
        <ControlTemplate TargetType="ListViewItem|GridViewItem">
            <ListViewItemPresenter CheckMode="Inline|Overlay" ... />
        </ControlTemplate>
    </Setter.Value>
    ...
</Style>
```

![带有内联复选框的 ListViewItemPresenter](images/w8x-to-uwp-case-studies/ui-listviewbase-cb-inline.jpg)

带有内联复选框的 ListViewItemPresenter

![带有叠加复选框的 ListViewItemPresenter](images/w8x-to-uwp-case-studies/ui-listviewbase-cb-overlay.jpg)

带有叠加复选框的 ListViewItemPresenter

-   在删除用于选择的向下轻扫和右键单击手势时（由于上述原因），交互模型已更改，其结果之一是，[**ItemClick**](https://msdn.microsoft.com/library/windows/apps/br242904) 和 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) 事件将不再互相排斥。 Windows 10 应用，查看你的方案并确定是否要采用"选择"调用"交互模型。 有关详细信息，请参阅[如何更改交互模式](https://msdn.microsoft.com/library/windows/apps/xaml/hh780625)。
-   用于设置 [**ListViewItemPresenter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.listviewitempresenter.aspx) 样式的属性进行了一些更改。 新属性包括：[**CheckBoxBrush**](https://msdn.microsoft.com/library/windows/apps/dn913905)、[**PressedBackground**](https://msdn.microsoft.com/library/windows/apps/dn913931)、[**SelectedPressedBackground**](https://msdn.microsoft.com/library/windows/apps/dn913937) 和 [**FocusSecondaryBorderBrush**](https://msdn.microsoft.com/library/windows/apps/dn898370)。 适用于 windows 10 应用忽略的属性都是[**填充**](https://msdn.microsoft.com/library/windows/apps/dn424775)（改为使用[**ContentMargin**](https://msdn.microsoft.com/library/windows/apps/dn424773) ）， [**CheckHintBrush**](https://msdn.microsoft.com/library/windows/apps/dn298504)、 [**CheckSelectingBrush**](https://msdn.microsoft.com/library/windows/apps/dn298506)、 [**PointerOverBackgroundMargin**](https://msdn.microsoft.com/library/windows/apps/dn424778)、 [**ReorderHintOffset**](https://msdn.microsoft.com/library/windows/apps/dn298528)、 [**SelectedBorderThickness**](https://msdn.microsoft.com/library/windows/apps/dn298533)，并[**SelectedPointerOverBorderBrush**](https://msdn.microsoft.com/library/windows/apps/dn298539)。

下表描述了对 [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/br242919) 和 [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/hh738501) 控件模板中的视觉状态和视觉状态组的更改。

| 8.1                 |                         | Windows 10        |                     |
|---------------------|-------------------------|-------------------|---------------------|
| CommonStates        |                         | CommonStates      |                     |
|                     | Normal                  |                   | Normal              |
|                     | PointerOver             |                   | PointerOver         |
|                     | Pressed                 |                   | Pressed             |
|                     | PointerOverPressed      |                   | [不可用]       |
|                     | Disabled                |                   | [不可用]       |
|                     | [不可用]           |                   | PointerOverSelected |
|                     | [不可用]           |                   | Selected            |
|                     | [不可用]           |                   | PressedSelected     |
| [不可用]       |                         | DisabledStates    |                     |
|                     | [不可用]           |                   | Disabled            |
|                     | [不可用]           |                   | Enabled             |
| SelectionHintStates |                         | [不可用]     |                     |
|                     | VerticalSelectionHint   |                   | [不可用]       |
|                     | HorizontalSelectionHint |                   | [不可用]       |
|                     | NoSelectionHint         |                   | [不可用]       |
| [不可用]       |                         | MultiSelectStates |                     |
|                     | [不可用]           |                   | MultiSelectDisabled |
|                     | [不可用]           |                   | MultiSelectEnabled  |
| SelectionStates     |                         | [不可用]     |                     |
|                     | Unselecting             |                   | [不可用]       |
|                     | Unselected              |                   | [不可用]       |
|                     | UnselectedPointerOver   |                   | [不可用]       |
|                     | UnselectedSwiping       |                   | [不可用]       |
|                     | Selecting               |                   | [不可用]       |
|                     | Selected                |                   | [不可用]       |
|                     | SelectedSwiping         |                   | [不可用]       |
|                     | SelectedUnfocused       |                   | [不可用]       |

如果你有一个自定义 [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/br242919) 或 [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/hh738501) 控件模板，请按照上述更改查看它。 我们建议你通过编辑新默认模板的副本并向其重新应用你的自定义来重新开始。 如果出于任何原因而无法执行此操作，并且你需要编辑现有模板，下面是有关可以如何执行此操作的一些常规指南。

-   添加新的 MultiSelectStates 视觉状态组。
-   添加新的 MultiSelectDisabled 视觉状态。
-   添加新的 MultiSelectEnabled 视觉状态。
-   添加新的 DisabledStates 视觉状态组。
-   添加新的 Enabled 视觉状态。
-   在 CommonStates 视觉状态组中，删除 PointerOverPressed 视觉状态。
-   将 Disabled 视觉状态移动到 DisabledStates 视觉状态组中。
-   添加新的 PointerOverSelected 视觉状态。
-   添加新的 PressedSelected 视觉状态。
-   删除 SelectedHintStates 视觉状态组。
-   在 SelectionStates 视觉状态组中，将 Selected 视觉状态移动到 CommonStates 视觉状态组中。
-   删除整个 SelectionStates 视觉状态组。

## <a name="localization-and-globalization"></a>本地化和全球化

在 UWP App 项目中，你可以重新使用通用 8.1 项目中的 Resources.resw 文件。 复制完该文件后，将其添加到项目，然后将 **“生成操作”** 设置为**PRIResource**，并将 **“复制到输出目录”** 设置为 **“不复制”**。 [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/br206071) 主题介绍了如何基于设备系列资源选择规格加载特定于设备系列的资源。

## <a name="play-to"></a>播放到

适用于 windows 10 应用，以[**Windows.Media.Casting**](https://msdn.microsoft.com/library/windows/apps/dn972568) Api 支持弃用[**Windows.Media.PlayTo**](https://msdn.microsoft.com/library/windows/apps/br207025)命名空间中的 Api。

## <a name="resource-keys-and-textblock-style-sizes"></a>资源键和 TextBlock 样式大小

设计语言已针对 windows 10 进行开展，因此某些系统样式已发生更改。 在某些情况下，你需要重新访问视图的视觉设计，以查看它们是否能与已更改的样式属性协调运行。

在其他情况下，资源键将不再受支持。 Visual Studio 中的 XAML 标记编辑器突出显示对无法解析的资源键的引用。 例如，XAML 标记编辑器将使用红色波形曲线为对样式键 `ListViewItemTextBlockStyle` 的引用加下划线。 如果未更正该错误，则应用将在你尝试将其部署到模拟器或设备时立即终止。 因此，请务必留意 XAML 标记的正确性。 而且你将发现 Visual Studio 是捕获此类问题的绝佳工具。

对于仍受支持的键，设计语言的更改意味着由某些样式设置的属性已更改。 例如，`TitleTextBlockStyle`将**FontSize**设置为在 Windows 运行时 8.x 应用中的 14.667px 和 18.14px 在 Windows Phone 应用商店应用。 但是，相同的样式将**FontSize**设置为大于 24px 在 windows 10 应用中。 查看你的设计和布局，并在合适的位置上使用适当的样式。 有关详细信息，请参阅[字体指南](https://msdn.microsoft.com/library/windows/apps/hh700394.aspx)和[设计 UWP 应用](http://dev.windows.com/design)。

下面是不再受支持的键的完整列表。

-   CheckBoxAndRadioButtonMinWidthSize
-   CheckBoxAndRadioButtonTextPaddingThickness
-   ComboBoxFlyoutListPlaceholderTextOpacity
-   ComboBoxFlyoutListPlaceholderTextThemeMargin
-   ComboBoxHighlightedBackgroundThemeBrush
-   ComboBoxHighlightedBorderThemeBrush
-   ComboBoxHighlightedForegroundThemeBrush
-   ComboBoxInlinePlaceholderTextForegroundThemeBrush
-   ComboBoxInlinePlaceholderTextThemeFontWeight
-   ComboBoxItemDisabledThemeOpacity
-   ComboBoxItemHighContrastBackgroundThemeMargin
-   ComboBoxItemMinHeightThemeSize
-   ComboBoxPlaceholderTextBlockStyle
-   ComboBoxPlaceholderTextThemeMargin
-   CommandBarBackgroundThemeBrush
-   CommandBarForegroundThemeBrush
-   ContentDialogButton1HostPadding
-   ContentDialogButton2HostPadding
-   ContentDialogButtonsMinHeight
-   ContentDialogContentLandscapeWidth
-   ContentDialogContentMinHeight
-   ContentDialogDimmingColor
-   ContentDialogTitleMinHeight
-   ControlContextualInfoTextBlockStyle
-   ControlHeaderContentPresenterStyle
-   ControlHeaderTextBlockStyle
-   FlyoutContentPanelLandscapeThemeMargin
-   FlyoutContentPanelPortraitThemeMargin
-   GrabberMargin
-   GridViewItemMargin
-   GridViewItemPlaceholderBackgroundThemeBrush
-   GroupHeaderTextBlockStyle
-   HeaderContentPresenterStyle
-   HighContrastBlack
-   HighContrastWhite
-   HubHeaderCharacterSpacing
-   HubHeaderFontSize
-   HubHeaderMarginThickness
-   HubSectionHeaderCharacterSpacing
-   HubSectionHeaderFontSize
-   HubSectionHeaderMarginThickness
-   HubSectionMarginThickness
-   InlineWindowPlayPauseMargin
-   ItemTemplate
-   LeftFullWindowMargin
-   LeftMargin
-   ListViewEmptyStaticTextBlockStyle
-   ListViewItemContentTextBlockStyle
-   ListViewItemContentTranslateX
-   ListViewItemMargin
-   ListViewItemMultiselectCheckBoxMargin
-   ListViewItemSubheaderTextBlockStyle
-   ListViewItemTextBlockStyle
-   MediaControlPanelAudioThemeBrush
-   MediaControlPanelPhoneVideoThemeBrush
-   MediaControlPanelVideoThemeBrush
-   MediaControlPanelVideoThemeColor
-   MediaControlPlayPauseThemeBrush
-   MediaControlTimeRowThemeBrush
-   MediaControlTimeRowThemeColor
-   MediaDownloadProgressIndicatorThemeBrush
-   MediaErrorBackgroundThemeBrush
-   MediaTextThemeBrush
-   MenuFlyoutBackgroundThemeBrush
-   MenuFlyoutBorderThemeBrush
-   MenuFlyoutLandscapeThemePadding
-   MenuFlyoutLeftLandscapeBorderThemeThickness
-   MenuFlyoutPortraitBorderThemeThickness
-   MenuFlyoutPortraitThemePadding
-   MenuFlyoutRightLandscapeBorderThemeThickness
-   MessageDialogContentStyle
-   MessageDialogTitleStyle
-   MinimalWindowMargin
-   PasswordBoxCheckBoxThemeMargin
-   PhoneAccentBrush
-   PhoneBackgroundBrush
-   PhoneBackgroundColor
-   PhoneBaseBlackColor
-   PhoneBaseHighColor
-   PhoneBaseLowColor
-   PhoneBaseLowSolidColor
-   PhoneBaseMediumHighColor
-   PhoneBaseMediumMidColor
-   PhoneBaseMediumMidSolidColor
-   PhoneBaseMidColor
-   PhoneBaseWhiteColor
-   PhoneBorderThickness
-   PhoneButtonBasePressedForegroundBrush
-   PhoneButtonContentPadding
-   PhoneButtonFontWeight
-   PhoneButtonMinHeight
-   PhoneButtonMinWidth
-   PhoneChromeBrush
-   PhoneChromeColor
-   PhoneControlBackgroundColor
-   PhoneControlDisabledColor
-   PhoneControlForegroundColor
-   PhoneDisabledBrush
-   PhoneDisabledColor
-   PhoneFontFamilyLight
-   PhoneFontFamilySemiBold
-   PhoneForegroundBrush
-   PhoneForegroundColor
-   PhoneHighContrastSelectedBackgroundThemeBrush
-   PhoneHighContrastSelectedForegroundThemeBrush
-   PhoneImagePlaceholderColor
-   PhoneLowBrush
-   PhoneMidBrush
-   PhonePageBackgroundColor
-   PhonePivotLockedTranslation
-   PhonePivotUnselectedItemOpacity
-   PhoneRadioCheckBoxBorderBrush
-   PhoneRadioCheckBoxBrush
-   PhoneRadioCheckBoxCheckBrush
-   PhoneRadioCheckBoxPressedBrush
-   PhoneStrokeThickness
-   PhoneTextHighColor
-   PhoneTextLowColor
-   PhoneTextMidColor
-   PhoneTextOverAccentColor
-   PhoneTouchTargetLargeOverhang
-   PhoneTouchTargetOverhang
-   PivotHeaderItemPadding
-   PlaceholderContentPresenterStyle
-   ProgressBarHighContrastAccentBarThemeBrush
-   ProgressBarIndeterminateRectagleThemeSize
-   ProgressBarRectangleStyle
-   ProgressRingActiveBackgroundOpacity
-   ProgressRingElipseThemeMargin
-   ProgressRingElipseThemeSize
-   ProgressRingTextForegroundThemeBrush
-   ProgressRingTextThemeMargin
-   ProgressRingThemeSize
-   RichEditBoxTextThemeMargin
-   RightFullWindowMargin
-   RightMargin
-   ScrollBarMinThemeHeight
-   ScrollBarMinThemeWidth
-   ScrollBarPanningThumbThemeHeight
-   ScrollBarPanningThumbThemeWidth
-   SliderThumbDisabledBorderThemeBrush
-   SliderTrackBorderThemeBrush
-   SliderTrackDisabledBorderThemeBrush
-   TextBoxBackgroundColor
-   TextBoxBorderColor
-   TextBoxDisabledHeaderForegroundThemeBrush
-   TextBoxFocusedBackgroundThemeBrush
-   TextBoxForegroundColor
-   TextBoxPlaceholderColor
-   TextControlHeaderMarginThemeThickness
-   TextControlHeaderMinHeightSize
-   TextStyleExtraExtraLargeFontSize
-   TextStyleExtraLargePlusFontSize
-   TextStyleMediumFontSize
-   TextStyleSmallFontSize
-   TimeRemainingElementMargin

## <a name="searchbox-deprecated-in-favor-of-autosuggestbox"></a>弃用搜索框以支持 AutoSuggestBox

尽管已在通用设备系列中实现了 [**SearchBox**](https://msdn.microsoft.com/library/windows/apps/dn252803)，但它无法在移动设备上正常运行。 将 [**AutoSuggestBox**](https://msdn.microsoft.com/library/windows/apps/dn633874) 用于通用搜索体验。 下面介绍通常如何使用 **AutoSuggestBox** 实现搜索体验。

用户开始键入后，将引发 **TextChanged** 事件，描述为 **UserInput**。 然后填充建议列表，并设置 [**AutoSuggestBox**](https://msdn.microsoft.com/library/windows/apps/dn633874) 的 **ItemsSource**。 在用户导航列表时，将引发 **SuggestionChosen** 事件（并且若已设置 **TextMemberDisplayPath**，文本框将自动填充指定的属性）。 当用户使用 Enter 键提交选择时，将引发 **QuerySubmitted** 事件，此时可对该建议执行相应操作（在此情况下，最有可能是导航至具有有关指定内容的更多详细信息的另一个页面）。 请注意，**SearchBoxQuerySubmittedEventArgs** 的 **LinguisticDetails** 和 **Language** 属性不再受支持（有支持该功能的等效 API）。 并且 **KeyModifiers** 也不再受支持。

[**AutoSuggestBox**](https://msdn.microsoft.com/library/windows/apps/dn633874) 还支持输入法编辑器 (IME)。 如果希望显示“查找”图标，也可以那样做（与图标交互将引发 **QuerySubmitted** 事件）。

```xml
   <AutoSuggestBox ... >
        <AutoSuggestBox.QueryIcon>
            <SymbolIcon Symbol="Find"/>
        </AutoSuggestBox.QueryIcon>
    </AutoSuggestBox>
```

另请参阅 [AutoSuggestBox 移植示例](http://go.microsoft.com/fwlink/p/?linkid=619996)。

## <a name="semanticzoom-changes"></a>SemanticZoom 更改

[**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) 的缩小手势已在 Windows Phone 模型上进行了融合，该手势是点击或单击组标题（因此，在桌面计算机上，不再显示提供的用于缩小的减号按钮）。 现在，我们可以随意在所有设备上获取相同且一致的行为。 与 Windows Phone 模型相比的一个外观区别是缩小视图（跳转列表）替换放大视图，而不是覆盖它。 出于此原因，你可以从缩小视图中删除任何半透明背景。

在 Windows Phone 应用商店应用中，为屏幕的应缩小 viewexpands。 在 Windows 运行时 8.x 应用中，并在 windows 10 应用中，缩小视图的大小限制**SemanticZoom**控件的边界内。

在 Windows Phone 应用商店应用中，如果缩小视图的背景中有任何透明度，则缩小视图后面的内容将（采用 z 顺序）显示出来。 在 Windows 运行时 8.x 应用中，并在 windows 10 应用中，所有内容均可见缩小视图后面。

在 Windows 运行时 8.x 应用中，当应用已停用和重新激活，缩小视图将消失 （如果它正处于显示），并改为显示在放大视图。 在 Windows Phone 应用商店应用中，并在 windows 10 应用中，缩小视图将保持显示，如果它正处于显示。

在 Windows Phone 应用商店应用中，并在 windows 10 应用中，缩小视图将消失时按下后退按钮。 对于 Windows 运行时 8.x 应用，没有任何内置后退按钮处理，因此该问题适用。

## <a name="settings"></a>设置

Windows 运行时 8.x **SettingsPane**类不适用于 windows 10。 除了生成“设置”页面，还应为用户提供从应用内访问它的方式。 我们建议你在最高级别公开此应用“设置”页面来作为导航窗格上的最后一个固定项，但下面依然提供完整的选项集。

-   导航窗格。 “设置”应为选项的导航列表中最后一个项，并固定到底部。
-   应用栏/工具栏（在选项卡视图或透视布局内）。 “设置”应为应用栏或工具栏菜单浮出控件中的最后一个项。 不建议将“设置”作为导航内的顶级项之一。
-   中心。 “设置”应位于菜单浮出控件内部（可以在应用栏菜单或中心布局的工具栏菜单内）

同样不建议将“设置”隐藏在大纲细节窗格内。

“设置”页面应填充整个应用窗口，并且“设置”页面也是“关于”和“反馈”所在的位置。 有关“设置”页面的设计指南，请参阅[应用设置指南](https://msdn.microsoft.com/library/windows/apps/hh770544)。

## <a name="text"></a>文本

文本（或版式）是 UWP 应用的重要方面，并且在移植时，你可能希望回顾你的视图的视觉设计，以便它们与新设计语言相协调。 使用这些图示查找可用的通用 Windows 平台 (UWP) **TextBlock** 系统样式。 查找对应于你使用的 WindowsPhone Silverlight 样式。 或者，你可以创建自己的通用样式并将从 WindowsPhone Silverlight 系统样式的属性复制到这些。

![适用于 Windows 10 应用的 TextBlock 系统样式](images/label-uwp10stylegallery.png) <br/>Windows 10 应用的 TextBlock 系统样式

在 Windows 运行时 8.x 应用和 Windows Phone 应用商店应用中，默认字体系列是 Global User Interface。 在 windows 10 应用中，默认字体系列是 Segoe UI。 因此，你的应用中的字体指标可能看起来不同。 如果你希望重新生成 8.1 文本的外观，可以使用 [**LineHeight**](https://msdn.microsoft.com/library/windows/apps/br209671) 和 [**LineStackingStrategy**](https://msdn.microsoft.com/library/windows/apps/br244362) 等属性来设置你自己的指标。

在 Windows 运行时 8.x 应用和 Windows Phone 应用商店应用中，文本的默认语言设置为版本语言或 en-我们。 在 windows 10 应用中，默认语言设置为最常使用的应用语言 （字体回退）。 你可以显式设置 [**FrameworkElement.Language**](https://msdn.microsoft.com/library/windows/apps/hh702066)，不过如果你未设置该属性的值，你将能够体验到更好的字体回退行为。

有关详细信息，请参阅[字体指南](https://msdn.microsoft.com/library/windows/apps/hh700394.aspx)和[设计 UWP 应用](http://go.microsoft.com/fwlink/p/?LinkID=533896)。 有关对文本控件更改的信息，另请参阅上面的[控件](#controls-and-control-styles-and-templates)部分。

## <a name="theme-changes"></a>主题更改

对于通用 8.1 应用，默认主题为深色。 对于 windows 10 设备，默认主题已更改，但你可以控制使用声明所请求的主题在 App.xaml 中的主题。 例如，若要在所有设备上都使用深色主题，请将 `RequestedTheme="Dark"` 添加到根 Application 元素。

## <a name="tiles-and-toasts"></a>磁贴和 Toast

对于磁贴和 toast，当前正在使用的模板将继续在 windows 10 应用中工作。 但有新的自适应模板可供你使用，它们在[通知、磁贴、Toast 和锁屏提醒](https://msdn.microsoft.com/library/windows/apps/mt185606)中有相关说明。

以前在台式机上，Toast 通知是暂时的消息。 一旦丢失或被忽略，它将消失且无法再检索。 在 Windows Phone 上，如果 Toast 通知被忽略或暂时消除，它将转到操作中心。 现在，操作中心不再局限于移动设备系列。

若要发送 Toast 通知，不再需要声明一个功能。

## <a name="window-size"></a>窗口大小

对于 Universal 8.1 应用，将使用 [**ApplicationView**](https://msdn.microsoft.com/library/windows/apps/dn391667) 应用清单元素声明最小窗口宽度。 在 UWP 应用中，你可以使用命令式代码指定最小大小（宽度以及高度）。 默认的最小大小为 500x320 epx，这也是可接受的最小大小的最小值。 可接受的最小大小的最大值为 500x500epx。

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

下一主题是[针对 I/O、设备和应用模型进行移植](w8x-to-uwp-input-and-sensors.md)。

